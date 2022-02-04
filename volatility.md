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

#### Close-to-close historical volatility

This is probably the most basic historical volatility measure existing. 

 <img src="https://render.githubusercontent.com/render/math?math=\CCHV=\sqrt{\frac{1}{N}\sum_{i}^{N}(x_{i}-\bar{x})^{2}}"> 

We use here the log returns of the closing prices to compute this indicator. The following code downloads the data for Walmart and Apple and computes their CCHV based on the last 22 lags from 01.2015 to 02.2020. N represents here the amount of trading days during the period (22 here).

```python
import yfinance as yf
import numpy as np
import matplotlib.pyplot as plt

tickers = ['AAPL', 'WMT']
data = yf.download(tickers, start='2015-01-01', end='2020-03-31')["Adj Close"] 

if len(tickers) <= 1:
    data=np.log(data)-np.log(data.shift(1)) #computing the returns for the market index
    data=data.replace([np.inf, -np.inf], np.nan).dropna(axis=0)
else:
    for i in range(len(data.columns)):
        data.iloc[:,i]= np.log(data.iloc[:,i])-np.log(data.iloc[:,i].shift(1))
        data=data.replace([np.inf, -np.inf], np.nan).dropna(axis=0)

sums=[] #contains the sum of the last lags elements
lags = 22
for i in range(0,len(data)):
    if i <lags:
        sums.append(data[:i+1])
    else:
        sums.append(data[i - lags+1:i+1])
sums=[x for x in sums if len(x)>=lags] # we only keep lists with lenght >= lags

sigmas = []
for j in range(len(sums)):
    sigmas.append(np.std(sums[j] * np.sqrt(22)))
    
plt.figure(figsize=(15,10))
plt.plot(data.index[lags-1:], sigmas)
if len(tickers) <=1:
    plt.legend(tickers)
else:    
    plt.legend(sigmas[0].index)
plt.xlabel('time')
plt.ylabel('CCHV')
plt.show()
```

![CCHV](https://user-images.githubusercontent.com/76557960/152562448-4a6cf46a-0471-42f3-b0c6-64a4a2d6256b.png)


This volatility metric is pretty simple to compute but doesn't take into account intraday high and low levels.

#### Parkinson Historical Volatility

The PHV is an enhancement compared to the CCHV because it takes into account the lowest and highest value during the trading day for a period of N days. Here we compare th data for BlackRock, Microsoft and Costco.

 <img src="https://render.githubusercontent.com/render/math?math=\PHV=\sqrt{\frac{1}{N}\sum_{i}^{N}ln(\frac{H_{i}}{L_{i}})^{2}\cdot \frac{1}{4\cdot ln(2)}}"> 


``` python
import yfinance as yf
import numpy as np
import matplotlib.pyplot as plt

tickers = ['BLK', 'MSFT', 'COST']
data = yf.download(tickers, start='2015-01-01', end='2020-03-31')[['Low', 'High']]

if len(tickers) <= 1:
    data['Ratio']=np.log(data['High']/data['Low'])*(1/(4*np.log(2)))
else:
    for ticker in tickers:
        data[('Ratio', ticker)]=np.log(data[('High', ticker)]/data[('Low', ticker)])        
data=data.replace([np.inf, -np.inf], np.nan).dropna(axis=0)

sums=[] #contains the sum of the last lags elements
lags = 22
for i in range(0,len(data)):
    if i <lags:
        sums.append(data[:i+1])
    else:
        sums.append(data[i - lags+1:i+1])
sums=[x for x in sums if len(x)>=lags]

for df in sums:
    df.drop(['Low', 'High'], axis=1, inplace=True)
    
sums=[np.sqrt(np.mean(x)) for x in sums]
                            
plt.figure(figsize=(15,10))
plt.plot(data.index[lags-1:], sums)
plt.legend(tickers)
plt.xlabel('time')
plt.ylabel('Parkinson')
plt.show()
```

![Parkinson](https://user-images.githubusercontent.com/76557960/152563440-0a04be18-8989-4298-99d9-7a8af7bc90d4.png)

Since Parkisnon doesn't take into account opening prices (and therefore price jumps), it tends to underestimate the volatility.
