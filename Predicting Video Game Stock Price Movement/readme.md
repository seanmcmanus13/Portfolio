# Predicting the movement of video game stock price movement using multivariable time series analyses and semi-supervised learning

## Introduction:
Prediction of stock price movement is a notoriously difficult problem. The price of stocks depend on a multitude of complex variables. They tend to be a product of future projections but they also depend on near term events. Unexpected moves often introduce volatility and accordingly, stock price trends assume the patterns of stochastic processes. Furthermore, stock prices can be said to abide by the 'Predictability Paradox'. The Predictability Paradox in simple terms states that:

'Predicting the predictable causes the predictable to become unpredictable'

Events which abide by this rule, can also be described by another term: A Level 2 Chaos Event (L2CE). L2CEs are events that react according to predictions made on them. Take for example of a given stock, a prediction made that alters the buy/sell order flow of the stock in turns impacts the stock movement. This can be of consequence at small consumer scale purchasing of stock due to advent of high frequency trading and selling of order flow [2] and of course at larger scales that move the market.

Nonetheless, many people make a living out of making strategic predictions on stock price movement. Usually, the individuals who are successful in this regard usually have some intrinsic knowledge or insight into the business or the larger economy that isn’t widely available or due to sheer luck. For the purpose’s of this introduction let’s refer to this intrinsic knowledge or insight as ‘hidden information’. This project aims to utilize publically available data to uncover hidden information that may inform video game stock price movement.

It is important to first explore the base case for ownership of video game stocks, as most informed stock trading strategy should account for the potential ownership of the underlying security. Here are some key reasons to believe the video game industry should see continued growth and expansion:

-   Highly profitable businesses
-   Produce a product that is extremely sticky (read: addictive)
-   Companies have a considerable moat due to the increasing cost in producing Triple AAA games
-   Overall market expected to increase at an annual rate of about 5-6% until 2021.

The first critical question in this regard is where can one derive hidden information that may inform the movement of video game stocks. It is of note that, games that are produced by these companies are increasingly played in the public realm thanks in part to the advent of modern streaming platforms like Youtube and Twitch. The games also enjoy significant coverage and discussion in public forums like Twitter.These stocks are one of the first, where the use of the companies product can be estimated, if not accurately tracked, in real time. I suspect that this offers an opportunity to make informed decisions on ownership and trading of these companies and further to this, probably impacts the price behavior of these stocks.

![Fig 1: Games are increasingly being played and tracked online. This may be a lucrative trove for informed investors](https://github.com/seanmcmanus13/Portfolio/blob/master/Predicting%20Video%20Game%20Stock%20Price%20Movement/Images/twitchtrends.png?raw=true)

**Fig 1: Games are increasingly being played and tracked online. This may be a lucrative trove for informed investors**

This capstone will focus on predicting the price action of video game stocks based on time series analyses of both online streaming data and derived financial indictors. Due to the provenance of large amounts of data in both of these domains, we will initially build a model on one indicator company and see if this model can be applied to other companies, first without reteaching based on that model and if required, with reteaching. Based on the provenance of available data, the following companies may be considered using this methodology:

<h1><center> Companies of interest: </center></h1>

| Company     | Market Cap (in $, rounded) | Country of Origin         |
|-------------|----------------------------|---------------------------|
| EA          | 28 Billion                 | US                        |
| Take Two    | 11 Billion                 | US                        |
| Activision  | 36 Billion                 | US                        |
| Ubisoft     | 9 Billion                  | France/Operates worldwide |
| Square Enix | 4 Billion                  | Japan                     |
| Nintendo    | 45 Billion                 | Japan                     |
| Konami      | 7 Billion                  | Japan                     |
| Capcom      | 3 Billion                  | Japan                     |


## Data:

The historical stock price data will be accessed using pandas datareader functionality. The streaming data was scraped by monitoring network traffic from sullygnome.com and pulling the resultant json files. The json files were then munged to pull the data from the files.

From these data sources a number of features will be generated. Including rolling averages of viewer data. As streaming viewership tends to peak on the weekend, while the stock market exchanges are closed, generating rolling averages of the data is a necessity as it will allow us to generate features for the streaming data that include weekend streaming data

All other features will be manually generated using mathematically formula previously described in the literature [1]. A selection of these features are displayed below:

![Fig 2: Financial Indicators utilized as part of this analysis.](https://github.com/seanmcmanus13/Portfolio/blob/master/Predicting%20Video%20Game%20Stock%20Price%20Movement/Images/variables.png?raw=true)
**Fig 2: Financial Indicators utilized as part of this analysis.**

## Methodology:
The methodology employed here can broadly be considered a kind of meta-modelling. A broad range of financial indicators, including indices and oscillators are fed into both Random Forest and Long Short Term Memory models for both auto-regressive prediction of future stock price and classification of stock price directional movement, where 1 equals an increase and 0 a decrease.

The prediction of stock price is a notably difficult problem and has been a frequent area of interest in popular Data Science [3, 4]. The majority of these models are however *not* predicting future stock price, they are simply projecting the current day stock price 1 day into the future:

![Fig 3: Simple Neural Networks cannot predict stock price this accurately. This is simply a misinterpretation and misplotting of a model predicting the current day price from the current day value.](https://miro.medium.com/max/800/1*YkfE7NRM2qJVRN2HCv1hhw.png)

**Fig 3: Simple Neural Networks cannot predict stock price this accurately. This is simply a misinterpretation and misplotting of a model predicting the current day price from the current day value.**

## References:

1.  [https://arxiv.org/abs/1605.00003](https://arxiv.org/abs/1605.00003)
2. [https://www.bloomberg.com/news/articles/2018-10-15/robinhood-gets-almost-half-its-revenue-in-controversial-bargain-with-high-speed-traders](https://www.bloomberg.com/news/articles/2018-10-15/robinhood-gets-almost-half-its-revenue-in-controversial-bargain-with-high-speed-traders)
3. [https://medium.com/p/30505541d877/responses/show](https://medium.com/p/30505541d877/responses/show)
4. [https://towardsdatascience.com/neural-networks-to-predict-the-market-c4861b649371](https://towardsdatascience.com/neural-networks-to-predict-the-market-c4861b649371)
