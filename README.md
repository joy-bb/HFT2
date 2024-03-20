# PAIR TRADING STRATEGY FOR ETHERIUM & BITCOIN
## About

This project includes two different methods in pair trading, which is considered as the most important part in Algorithmic and High Frequency Trading. The first part will introduce the strategy and the model fitted, following by finding optimal trading weights and optimal wealth process. The centric method is extracting real-time data on the trading date of 14 Apr 2023 to feed in the model for the estimation results. There will be also the comparison between the optimal and the static control pair.

### Data downloading
Price movement of both ETH and BTC on the 14 Apr is plotted in the following figure. Since there is quite significant gap between the prices of ETH and BTC – about 2.100 and about 30.000, the chart is using 2 different vertical axes with different measurements to pull these price movements closer; in order to see the correlation pattern between the two assets. 
![image](https://github.com/joy-bb/HFT2/assets/71431452/0bba9b81-1af8-4d4f-ade5-0f0af06367d9)

### Assume the δ=0.8, estimate the parameter ρ .
In pair trading, the delta refers to the hedge ratio between two assets in a pair trade. The hedge ratio is the ratio of the notional value of the two assets in the pair trade that must be held to achieve a market-neutral position. The delta value is calculated using a statistical method such as regression analysis, where the historic prices of the two assets are used to estimate the optimal ratio of the two assets in the pair trade. The delta value is usually a fractional value, and it represents the sensitivity of the pair trade to movements in the market.
Phi, on the other hand, is a measure of the mean-reversion speed of the spread between the two assets in the pair trade. The phi value is typically calculated using a mean-reverting model, such as the Ornstein-Uhlenbeck process, which describes the spread between the two assets as a mean-reverting process.
With the value of delta = 0.8, to estimate the value of phi using the following steps:
•	Calculate the spread between the two assets in the pair trade by taking the difference between the prices of the two assets at each time step.
•	Calculate the log returns of the spread by taking the natural logarithm of the spread at each time step and taking the difference between consecutive time steps.
•	Regress the log returns of the spread on the lagged spread values using a linear regression model. The slope coefficient of this regression model is the negative of the mean-reversion speed or -phi.
•	Use the delta value that already have to scale the slope coefficient to obtain the estimate of phi.
The intuition behind this approach is that the spread between the two assets in the pair trade is expected to revert to its long-term mean at a rate proportional to the deviation of the spread from its mean. The slope coefficient of the regression model represents the speed at which this reversion occurs, and the negative of this coefficient represents the mean-reversion speed or phi.
In this case, the value phi will be estimated from value of rho – correlation between the log return of the asset 1 ETH and the asset 2 BTC (from the previous day – 13 Apr). 

![image](https://github.com/joy-bb/HFT2/assets/71431452/76104704-cad9-4c7f-bbfb-8b431df17c83)

Hence the phi ρ=0.758.

### Estimate a reasonable β and explain the model you fitted.
Beta is typically estimated using linear regression analysis, where the historical returns of the asset 1 – ETH are regressed on the returns of the asset 2 - BTC. The slope coefficient of this regression model represents the beta of these assets.
From the above code, Beta can be estimated as β=0.198.
![image](https://github.com/joy-bb/HFT2/assets/71431452/7ac26220-6ea9-45e3-b265-8fda05e5d56c)

Assumed that ETH and BTC are both correlated and co-integrated, the pair trading strategy involves identifying a pair of assets that are highly correlated and have diverged from their long-term relationship, and then taking opposite positions in the assets based on their expected convergence. Since ETH and BTC have historically moved together and if one of them is currently trading at a discount to the other, taking a long position in the underpriced asset and a short position in the overpriced one, with the expectation that the spread between them will eventually converge back to its long-term mean.
The success of the pair trading strategy depends on the accuracy of the correlation and mean-reversion estimates, as well as the ability to accurately hedge the positions to eliminate market risk. Typically, the strategy involves continuously monitoring the prices and correlations of the two assets, and adjusting the positions as needed to maintain a market-neutral stance and capture profits from the spread. This means the pair trading strategy for ETH and BTC in this report will adapt the theory to set up model for optimal trading weights, with the real-time data fed in every minute on the trading date of 14 Apr 2023 – known as feedback control form. Together with the co-integrated factor, where instantaneous co-integrating vector z(t) - stationary Ornstein-Uhlenbeck process, which is defined by:
![image](https://github.com/joy-bb/HFT2/assets/71431452/4645420d-bcf0-48b7-a3ef-472deff795ab)

### Discuss your inputs. (It should include S, μ and σ)
The model is built to using real time data for the feedback form control process, hence the code below is set to extract data on the trading day of 14 Apr 2023. Hence, the value of S1 and S2 respectively should be the price of ETH and BTC on the trading day of 14 Apr 2023, where:
•	S1_0 and S2_0 will be FIRST Open price of ETH and BTC
•	S1_1 and S2_1 will be the FIRST Adj Close price of ETH an BTC
•	S1_t and S2_t will be fed in continuously by the Adj Close price of ETH and BTC on the next coming minutes (with the real-time data)

Take two assets whose spread, that is the difference between their prices, exhibits a marked pattern and deviations from it are temporary. Then, pairs trading algorithms profit from betting on the empirical fact that spread deviations tend to return to their historical or predictable level. This is the reason why the data from previous date (13 Apr) is used to calibrate co-efficient factors: Mu μ, Sigma σ, Beta β, Rho ρ
The python code is already used to calculate these values in the previous session.

Summary, the model with parameters: (symbol 1 is used for ETH and 2 is for BTC)
![image](https://github.com/joy-bb/HFT2/assets/71431452/cca9b33c-7f7d-4847-9873-d8b16d197933)

### Find your optimal trading weights
Consider two co-integrated and correlated stocks which trade at some spread. For a given period, we need to maximise the agent’s terminal utility of wealth subject to budget constraints. Unique solution (W, S1, S2) for any t ∈ [0,T] and (π1, π2, S1, S2) satisfy the integrability condition:
![image](https://github.com/joy-bb/HFT2/assets/71431452/8753e8f5-a19a-47db-abff-461933815d4b)

![image](https://github.com/joy-bb/HFT2/assets/71431452/7d8b41be-d3f8-4dc7-a216-70fc8f92c3fe)
The code automatedly calculate the trading weights of ETH and BTC, then plotting respectively in the figure below.

![image](https://github.com/joy-bb/HFT2/assets/71431452/e1bc0784-76fe-4282-a0ff-5a2f79dfcbca)

With this trading strategy, both assets ETH and BTC will be traded with the optimal weights calculated in every minute from the random of co-integrated factor z and the real-time data from S1 and S2 at minute t. 



