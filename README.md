# Mean Reversion Market Neutral Trading Strategy

### Description
My objective was to design, implement and evaluate the performance of a mean reversion market neutral trading strategy for a 1500 stock basket. This involved identifying cointegrated stock pairs, generating trading signals, and then backtesting my strategy.
</br></br>
I used yfinance to collect the stock data, specifically the daily returns (which I later converted to log returns) for the last 15 years. However, downloading the data proved to be very computationally expensive, so to improve time efficiency, I downloaded the daily returns for each stock from yfinance and stored this as a csv file. The csv file was much faster to load.
</br></br>
To identity conintegrated pairs, I first filtered out stock pairs based on correlation. I only kept stock pairs with a correlation between 0.9 and 0.99. I excluded extremely high correlation (e.g., GOOGL and GOOG) as this caused numerical precision issues, leading to errors in the cointegration test. Then on the filtered stock pairs, I applied the Granger test to test for cointegration. Then for significantly cointegrated stocks, I used the VECM model to calculate a linear relationship between the cointegrated stocks.
</br></br>
With cointegrated stocks, we believe that in the long run the stocks will revert back to the mean, so we make profit by trading if either stock deviates from the mean. We do this by going long on one stock and short on the other. To standardise spreads, I used a rolling mean and standard deviation to compute a Z-score for each pair. 
Trading signals were based on: 
- Go long when Z-score < -1 
- Go short when Z-score > 1 
- Exit when Z-score returns near zero

### Strategy Performance
Performance metrics:
- 70% return over 11 years 
- Annualised Return: 4.83% (Decent, but lower than S&P 500 ~9-10%)
- Sharpe Ratio: 1.74 (Strong risk-adjusted return - above 1 is good, above 2 is excellent)
- Max Drawdown: -4.24% (Very low, much better than S&P 500’s worst case ~50%)
- Volatility: 2.63% (Extremely low - S&P 500 typically has ~15-20%)

Compared to the S&P 500, this strategy had:
- Much lower volatility and drawdowns (good for risk-averse traders)
- A strong Sharpe Ratio, meaning returns were well-compensated for risk.
- Lower absolute returns than the S&P 500, meaning long-term passive investing might still be preferable. 

### Final Thoughts
This strategy is profitable, relatively safe, and has strong risk-adjusted 
returns. However, it lags behind passive investing in raw performance. Future 
enhancements could boost profitability without significantly increasing risk.
</br>Some potential improvements:
- Transaction Cost Considerations – Incorporating slippage and commissions to make backtests more realistic. 
- More Computational Power – With higher processing power, we could test additional cointegration methods like Johansen and optimize pair selection further. 
- Data Optimisation – Instead of using yearly data, we could explore rolling windows to make pair selection more adaptive. 

### Tools Used
- Python: pandas, yfinance, numpy, matplotlib, statsmodels (Granger cointegration test & VECM model), google.colab
