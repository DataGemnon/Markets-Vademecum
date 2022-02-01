### Volatility and Implied Volatility

#### Beta

The beta of a stock is its indicator of volatility regarding market risk (systematic risk). The market portfolio has a beta of 1. A stock with a beta superior to 1 is a cyclical stock and below 1 a non-cyclical stock. 


 <img src="https://render.githubusercontent.com/render/math?math=\beta = \frac{Cov(r_{S}, r_{M})}{Var(r_{M})}"> 
 
 The following code allows you to download stock prices and to compute for each stock its beta, here from 01.09.2021 to 31.12.2021. The analyzed stocks here are Walmart, Robinhood, Apple and IBM.
 
 ```python
import yfinance as yf
import numpy as np
import pandas as pd

tickers = ['WMT', 'HOOD', 'AAPL', 'IBM']
benchmark=['SPY']
data = yf.download(tickers, start='2021-09-01', end='2021-12-31')["Adj Close"] 
bench_data = yf.download(benchmark, start='2021-09-01', end='2021-12-31')["Adj Close"] 

#computing the returns for the stocks
for i in range(len(data.columns)):
    data.iloc[:,i]= data.iloc[:,i].pct_change()
data.dropna(inplace = True)

bench_data=bench_data.pct_change() #computing the returns for the market index
bench_data.dropna(inplace = True)

betas= []
for i in range(len(data.columns)):
    betas.append(np.cov(data.iloc[:,i], 
              bench_data)[0][1] / np.var(bench_data)) 
betas = pd.DataFrame(betas, index = data.columns)
```
We get the following results:
AAPL:	1.17,
HOOD:	1.61,
IBM:	0.41,
WMT:	0.37. Without big surprise, we notice that Walmart has the lowest and Robinhood the highest beta.

