# neurontrading
NeuronTrading
https://medium.com/@neurontrading/cryptocurrencies-trading-based-on-1m-timeframe-using-ml-and-getting-more-than-1-of-profit-daily-c84ef3269b3a
Cryptocurrencies trading based on 1m timeframe using ML and getting more than 1% of profit daily
The title may sound dream-like to you, though the findings surprised our team too. But first things first!

#1. Datasource
As a datasource, we decided to go with Binance Spot Market financial quotes on MATICUSDT currency pair.
Binance allows you to receive data for 1m chart at free access, you don’t even need to get the API key.
Data about MATICUSDT starts from January 12, 2019. At the time of this article, there are about 1 400 000 strings already.
The following code allows you to download all the financial quotes in about 10–20 minutes and it will create a CSV file with the needed source data.
@Тут идет вставка кода@
Код

As a result, we will get the following DataFrame:
@Тут идет вставка результата кода@
#2. Building up the strategy of your dream
Each trader tries to come up with their own strategy and backtest it based on the historical data. At the same time, each trader is willing his strategy to provide the highest profit possible.
In other words, the strategy should be able to define the best time to buy the asset, and the best moments to sell it.
In this particular article, we are speaking about the Spot market and we don’t consider short positions.
So, are you ready to create your dream strategy? Right now we are going to show you how you can trade MATICUSDT on a 1m timeframe and achieve the highest profit.
For our calculations, we will only consider the price of closed 1m candle.
In this case, our graph for February 1 will look as follows:
@Тут вставка графика@
When you start building up the ideal strategy, you should find the local maximum and minimum points of the asset price function.
In order to find them, let’s apply the double exponential smoothing to our function as the first step.
@тут вставка кода@
@вставка графика@
After that, we should find the local extrema on the graph.
The Binance Spot fee is equal to about 0.1% peЭr each deal.
Accordingly, in order to have the profit deal, the selling price should be greater than the purchase price for at least 0.2%.
Let’s take the 0.5% parameter as the minimum delta between the peaks in our profitable strategy.
@вставка кода@
@вставка графика@
That’s how the daily graph of a trader’s dream could look like.
We have the ideal entering to stock position, and ideal moments to close it (i.e. to sell the asset).

#3.Signal definition — setting up the classification task
As soon as we defined the local extremums, let’s set the signal value to 0 or 1 in each line of our dataset (next to each candle).
If it’s the local minimum point, we set 1 until we reach the local maximum point.
When we get to the local maximum point, we set 0 and we do this for as long as we reach the local minimum point again.
Basically, it means that when we have 1 — we should either buy it, or keep the asset until we get 0.
As soon as it’s equal to 0 — we should sell and don’t buy it until we get 1.
This code will set the signals for the whole dataset:
@вставка кода@
@скрин нашего фдатасета@
Now let’s see what profit our ideal strategy could bring us based on the latest 20 percent of historical data (approximately 200 days).
@вставляем код@
As you can see, the strategy would make a little bit more than 2000 deals per chosen timeline (approx. 200 days) and it would earn about 2900% exclusive of the compound interest — i.e. the system always starts with the same position size. We considered 0.2% as the stock exchange fee in each deal.
Now you don’t need to be a math genius to calculate that this dream strategy gives us more than 14% of profit per day! Such profit looks really fantastic and that’s how the ideal strategy should look like.
Even if we define the position opening and closing points 10 times worse than the ideal one — the result is still impressive.
Adding technical indicators values
People have used technical analysis for many decades now — there are thousands of indicators that should help traders to forecast the behavior of any given asset, according to stock analysts.
We are using the TA Python library — it contains more than 80 of the most popular technical indicators. Let’s add the values of all these indicators to our dataset:
@код вставляем@
As a result, we got the dataset with more than 1400 000 rows and almost 100 columns. We will use it for learning and identification of trends of the considered asset.
@вставка куска фдатасета@

#4.Learning and testing
Now let’s divide the dataset into two parts: 80% will be used for learning and the rest 20% will be used for testing needs.
Let’s apply the best known machine learning algorithm to solve the classification task — the random forest.
In the next article we will compare the different machine learning algorithms in terms of their effectiveness in solving our task (xgboost and RNN LSTM seem the most interesting to me).
@ всталвяем код @
We’ve got the model, and now let’s test it on the 20% of the whole data array test set.
@вставляем код@
We’ve got the dataset with signals that match with the ideal ones in 85% of cases.
If we test the strategy suggested by the algorithm, we get this statistics:
@ вставляем код @
@вставляем результат@
The number of deals per test timespan (200 days) equals more than 5000!
The profit is more than 1600 percent!
Concurrently, there are some negative losing trades appearing (almost half of all deals), though the average loss per strategy is drastically lower than the average profit per deal.

So, let’s summarize:
At first sight, the result is really stunning, the accuracy of signals predicting (when you should buy and sell the asset) gives you an opportunity to create quite a profitable strategy.
The random forest algorithm proved itself as one of the best algorithms for classification tasks, and the technical indicators we rely upon — are the most known and proven ones. The learning dataset contains more than a million of strings — so in terms of machine learning, the result of 85% accuracy is quite real.
Undoubtedly, there is a room for improvement:
In the current article, the ideal model is built up pretty roughly. The ideal strategy can be much more profitable. For example, we can give up using double exponential smoothing and try the different methods instead — e.g. polynomial approximation method;
We took all 80 most famous technical indicators — surely, the formula for success lies in compilation of various indicators with the different parameters;
We only considered a single currency pair and used one 1m time frame — though we can try out all kinds of assets and use different timeframes;
We can use different machine learning algorithms and compare the results (particularly, we can try to go with xgboost and RNN LSTM);
It’s possible to retrain the system once per some periodic interval;
In order to make sure that the system is actually working, we’ve implemented a module to predict the signal for every new candle that the system retrieves from the exchange each minute.
When the system gets data about another one-minute candle — it calculates the values of technical indicators (using N = 100 previous strings) and makes the signal prediction according to the model.
Data about signals is sent and published in the Telegram channel we created — you can subscribe and receive the signals notifications in real time.
The full code and saved datasets can be found in our git repository -
neurontrading.io
Please feel free to send us any questions or ideas on how to improve our algorithm — our email is
team@email.com
We are always open to new connections and potential cooperation!
