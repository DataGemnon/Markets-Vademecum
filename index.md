# Markets Vade Mecum
Gathering mathematical knowledge about financial markets.

This work is done by Master in Finance students.

### Stock path modeling
#### Short introduction to options

Options are financial derivatives that give their holder the right (not the obligation) to buy or sell the underlying asset (stocks, currencies …) for a given price at a given maturity. Calls options give their holders the right to buy and put options the right to sell. The payoff of a call is given by max(0 ; St-K) and the payoff of a put is given by max(0 ; K-St) with K being the strike price and St the spot price. But before going into the option's pricing theory, we must see how to modelize stock paths. 

#### Markov Process & Wiener Process

In options pricing, the value of the underlying evolves in an uncertain way and is, therefore, a continuous-time stochastic process. To be more precise, the price of the UA (underlying asset, like stocks) is a Markovian process, a specific type of stochastic process in which only the latest value is helpful to provide the future one.

A Wiener process is one kind of Markov process with mean 0 and variance 1. A variable z follows a Wiener process as follows:

 <img src="https://render.githubusercontent.com/render/math?math=\Delta z = \varepsilon \sqrt{\Delta t} "> 

*  with <img src="https://render.githubusercontent.com/render/math?math=\Delta z "> being the change of variable z, <img src="https://render.githubusercontent.com/render/math?math=\Delta t "> being a small time increment and <img src="https://render.githubusercontent.com/render/math?math=\varepsilon "> being a normally distributed variable (<img src="https://render.githubusercontent.com/render/math?math=\Phi \left ( 0,1 \right ) ">). <img src="https://render.githubusercontent.com/render/math?math=\Delta z "> is also normally distributed with mean 0, variance <img src="https://render.githubusercontent.com/render/math?math=\Delta t "> and standard deviation <img src="https://render.githubusercontent.com/render/math?math=\sqrt{\Delta t}">.

* <img src="https://render.githubusercontent.com/render/math?math=\Delta z ">for different time increments are independent.

<img src="https://render.githubusercontent.com/render/math?math=\ z\left ( T \right )-z\left ( 0 \right )"> gives us the change of the variable z betwenn time 0 and time T. If we consider that there are N time increments <img src="https://render.githubusercontent.com/render/math?math=\Delta t "> between time 0 and time T (<img src="https://render.githubusercontent.com/render/math?math=\N = \frac{T}{\Delta t} ">), we can  deduce following equation:

<img src="https://render.githubusercontent.com/render/math?math=\z\left ( T \right )-z\left ( 0 \right )=\sum_{i}^{N} e_{i}\sqrt{\Delta t} ">

<img src="https://render.githubusercontent.com/render/math?math=\ z\left ( T \right )-z\left ( 0 \right )"> follows a normal distribution with mean 0 and standard deviation <img src="https://render.githubusercontent.com/render/math?math=\sqrt{\T}">.

![3 wiener processes](https://user-images.githubusercontent.com/76557960/151622627-232c15dd-061a-499c-97ed-bc491edfebc6.png)

This chart represents 3 Wiener Processes with T=10 and N=1000 (you can use the following code to get the results). 

```python
import numpy as np
import matplotlib.pyplot as plt
def wiener(T, N):
    Z0 = [0]
    dt = T/float(N)
    dz = np.random.normal(0, 1*np.sqrt(dt), N)
    Z = Z0 + list(np.cumsum(dz))
    return Z

N = 1000
T = 10
dt = T / float(N)
t = np.linspace(0.0, N*dt, N+1)
plt.figure(figsize=(15,10))
for i in range(3):
    W = wiener(T, N)
    plt.plot(t, W)
    plt.xlabel('time')
    plt.ylabel('Z')
    plt.grid(True)
```

#### Generalized Wiener Process

At the moment, our variable z has a drift rate (average change per unit of time) equal to 0, meaning that the expected value at a future point in time is equal to the current value, and a variance rate of 1. A Generalized Wiener Process for variable x using z would look like this :

<img src="https://render.githubusercontent.com/render/math?math=\Delta x = a\cdot dt%2bb\cdot dz "> or <img src="https://render.githubusercontent.com/render/math?math=\Delta x = a\cdot dt %2b b\cdot \varepsilon \sqrt{dt}"> with a and b being two constants. Constant a represents the drift rate and <img src="https://render.githubusercontent.com/render/math?math=\b\cdot dz "> represents the "uncertain" part in the <img src="https://render.githubusercontent.com/render/math?math=\Delta x">. Here dx follows a normal distribution wth mean <img src="https://render.githubusercontent.com/render/math?math=\Delta x = a\cdot dt"> and standard deviation <img src="https://render.githubusercontent.com/render/math?math=\b\cdot \varepsilon \sqrt{dt}">.

In a time interval T (as seen previously), dx would be normally distributed with mean <img src="https://render.githubusercontent.com/render/math?math=\a\cdot T"> and standard deviation <img src="https://render.githubusercontent.com/render/math?math=\sqrt{T}">.

In reality both constants a and b are not sufficient because investors ask for a specific return (variable a) independently of the current stock price (they will ask 10% whether the price is 50$ or 100$). Hence we need to replace the drift rate a with the expected return <img src="https://render.githubusercontent.com/render/math?math=\mu S"> (with the constant expected rate of return of the stock being <img src="https://render.githubusercontent.com/render/math?math=\mu">). 

If we take out the uncertainty term we have:
<img src="https://render.githubusercontent.com/render/math?math=\Delta S = \mu S\cdot dt"> and, if we develop just a bit, <img src="https://render.githubusercontent.com/render/math?math=\frac{\Delta S}{S}=\mu \cdot dt">. This means that without uncertainty during a small time increment dt, the stock's percent of change is of <img src="https://render.githubusercontent.com/render/math?math=\mu \cdot dt">.

Taking again a longer time interval T, the above equation could be developped as follows: <img src="https://render.githubusercontent.com/render/math?math=S_{T}=S_{0}e^{\mu T}"> with <img src="https://render.githubusercontent.com/render/math?math=\S_{0}"> being the stock price at time 0.

Problem with this equation is that there is no uncertainty factor. Whatever the stock price, we believe that the variability of the stock price remains the same (in a short period of time, the standard deviation in the change of the price should be proportional to the stock price).

<img src="https://render.githubusercontent.com/render/math?math=\dS=\mu S\cdot dt %2b \sigma S\cdot \varepsilon \sqrt{dt}">

We recognize in here two parameters: <img src="https://render.githubusercontent.com/render/math?math=\mu"> (the expected rate of return for that stock) and <img src="https://render.githubusercontent.com/render/math?math=\sigma"> (the volatility of that stock).

This model is called a generalized Wiener Process and we can easily deduct the following:

<img src="https://render.githubusercontent.com/render/math?math=\frac{dS}{S}=\mu \cdot dt%2b\sigma\cdot\varepsilon \sqrt{dt}">

Here <img src="https://render.githubusercontent.com/render/math?math=\frac{dS}{S}"> is normally distributed with mean <img src="https://render.githubusercontent.com/render/math?math=\mu \cdot dt"> and standard deviation <img src="https://render.githubusercontent.com/render/math?math=\sigma\cdot\varepsilon \sqrt{dt}">.


The following code allows you to simulate several (here 10) paths of a stock (in this case with an expected return of 8%, volatility of 20%, initial price 100$ and during 1000 time increments).

```python
import numpy as np
import matplotlib.pyplot as plt

def GBM(s0, mu, sigma, T, N):
    dt=T/N
    ds=np.zeros(shape=N)
    ds[0]=0
    allS=np.zeros(shape=N)
    allS[0]=s0
    for i in range(1, N):
        ds[i]=mu*allS[i-1]*dt+sigma*allS[i-1]*np.random.normal(0,1)*np.sqrt(dt)
        allS[i]=allS[i-1]+ds[i]
    return(allS)
    
N = 1000
T = 1
dt = T / float(N)
t = np.linspace(0.0, N*dt, N)
plt.figure(figsize=(15,10))
for i in range(10):
    W = GBM(100, 0.08, 0.2, T, N)
    plt.plot(t, W)
    plt.xlabel('time')
    plt.ylabel('Stock Price')
    plt.grid(True)
```

![GBM](https://user-images.githubusercontent.com/76557960/151659322-25413d9f-0648-49c3-8b11-0931210f91c7.png)

#### Itô's lemma and the lognormal property of stock prices

We remember the equation <img src="https://render.githubusercontent.com/render/math?math=\dS=\mu S\cdot dt %2b \sigma S\cdot \varepsilon \sqrt{dt}"> seen above. If we have a variable G dependent on both S and t, Itô's lemma helps us to determine the following:

<img src="https://render.githubusercontent.com/render/math?math=\dG = (\frac{\delta G}{\delta S}\mu S %2b \frac{\delta G}{\delta t} %2b \frac{1}{2}\frac{\delta ^{2} G}{\delta S^{2}}\sigma ^{2}S^{2})dt %2b \frac{\delta G}{\delta S}\sigma S\cdot \varepsilon \sqrt{dt} ">

To show you the lognormal property of stock prices, we define <img src="https://render.githubusercontent.com/render/math?math=\G = ln(S)">

We can compute the derivatives one by one:

<img src="https://render.githubusercontent.com/render/math?math=\frac{\delta G}{\delta S}=\frac{1}{S}">

<img src="https://render.githubusercontent.com/render/math?math=\frac{\delta G}{\delta t}=0">

<img src="https://render.githubusercontent.com/render/math?math=\frac{\delta^{2} G}{\delta S^{2}}= -\frac{1}{S^{2}}">

Now we can plug that in:

<img src="https://render.githubusercontent.com/render/math?math=\dG=(\frac{1}{S}\cdot \mu S %2b \frac{1}{2}\cdot -\frac{1}{S^{2}}\cdot \sigma ^{2}S^{2})dt %2b \frac{1}{S}\cdot \sigma S\cdot \varepsilon \sqrt{dt}"> <=> <img src="https://render.githubusercontent.com/render/math?math=\dG=(\mu -\frac{1}{2}\sigma ^{2})dt %2b \sigma \cdot \varepsilon \sqrt{dt}">

The change of G between time 0 and time T is normally distributed with mean <img src="https://render.githubusercontent.com/render/math?math=\(\mu -\frac{1}{2}\sigma ^{2})T"> and variance <img src="https://render.githubusercontent.com/render/math?math=\sigma ^{2}T">.

<img src="https://render.githubusercontent.com/render/math?math=\ln(S_{T})-ln(S_{0})\sim \Phi ((\mu -\frac{1}{2}\sigma ^{2})T, \sigma ^{2}T)"> <=> <img src="https://render.githubusercontent.com/render/math?math=\ln(S_{T})\sim \Phi (ln(S_{0})%2b(\mu -\frac{1}{2}\sigma ^{2})T, \sigma ^{2}T)">

<img src="https://render.githubusercontent.com/render/math?math=\S_{T}"> has a lognormal distribution because its logarithm <img src="https://render.githubusercontent.com/render/math?math=\ln(S_{T})"> is normally distributed. 

Knowing this, it is possible for us to compute an interval for <img src="https://render.githubusercontent.com/render/math?math=\S_{T}">. The following code provides an interval for a stock with price at time 0 100$, expected return 12%, variance 18% for T=0.75 (9 months). In this case, there is a 95% chance that the price will be between 82.06$ and 151.19$.

```python
import numpy as np
import matplotlib.pyplot as plt

def price_interval(S0, mu, sigma, T):
    mid = np.log(S0)+(mu - (0.5 * sigma **2) * T)
    var = sigma ** 2 * T
    plt.hist(np.random.normal(loc = np.exp(mid),
                              scale = var,
                              size =1000))
    plt.xlabel('ST')
    plt.show()
    interval = [np.exp(mid-np.sqrt(var)*1.96), np.exp(mid+np.sqrt(var)*1.96)]
    plt.show()
    print("There is a 95% chance that the stock price will be between {} and {}.".format(interval[0], interval[1]))
price_interval(100, 0.12, 0.18, 0.75)
```

![lognormal distrib of prices](https://user-images.githubusercontent.com/76557960/151696897-f8d2190e-258c-4a2b-9cf1-8e492a12c30e.png)

We can run 100 simulations to have a glimpse at the distribution at T=0.75 using the GBM function provided earlier.

![100 simulations distribution](https://user-images.githubusercontent.com/76557960/151697385-a3200870-776c-4148-a2af-e8a35aa00f99.png)

S has a lognormal distribution and we can infer from the propoerties of a lognormal distribution the following:

<img src="https://render.githubusercontent.com/render/math?math=\E(S_{T})=S_{0}e^{\mu T}"> for the expected value of <img src="https://render.githubusercontent.com/render/math?math=\S_{T}"> and <img src="https://render.githubusercontent.com/render/math?math=\var(S_{T})=S_{0}^{2}e^{2\mu T}(e^{\sigma ^{2}T}-1)"> concerning its variance. 

#### The distribution of returns

Considering both previous equations, we can have a glimpse on the distribution of the continiously compounded rate of return we will consider as x:

<img src="https://render.githubusercontent.com/render/math?math=\E(S_{T})=S_{0}e^{x T}"> <=> <img src="https://render.githubusercontent.com/render/math?math=\x=\frac{1}{T}\cdot ln(\frac{S_{T}}{S_{0}})">

We remember the distribution of <img src="https://render.githubusercontent.com/render/math?math=\ln(\frac{S_{T}}{S_{0}})"> and can deduct the following:

<img src="https://render.githubusercontent.com/render/math?math=\x\sim \Phi (\mu -\frac{1}{2}\sigma ^{2}, \sigma ^{2}T)">

x is normally distributed with mean <img src="https://render.githubusercontent.com/render/math?math=\mu -\frac{1}{2}\sigma ^{2}"> and variance <img src="https://render.githubusercontent.com/render/math?math=\sigma ^{2}T)">

The following code gives us the distribution of the average rate of return x for 1.5 years, an expected return of 10% and volatility of 16%.

```python
import numpy as np
import matplotlib.pyplot as plt

def return_distrib(mu, sigma, T):
    mid = mu-0.5*sigma**2
    var = (sigma ** 2) / T
    plt.hist(np.random.normal(loc = mid,
                              scale = var,
                              size =1000))
    interval = [mid-np.sqrt(var)*1.96, mid+np.sqrt(var)*1.96]
    plt.xlabel('Average rate of return')
    plt.show()
    print("There is a 95% chance that the stock price will be between {} and {}.".format(interval[0], interval[1]))

return_distrib(0.1, 0.16, 1.5)
```
![rate of return distribution](https://user-images.githubusercontent.com/76557960/151703233-c727643f-1c38-499e-a860-98cd2027bf15.png)

Caution: <img src="https://render.githubusercontent.com/render/math?math=\mu"> and the average rate of return are not the same. 

The indicator of volatility of the stock is the standard deviation. The following code allows to extract stock data from Yahoo Finance's API and to compute from it the standard deviation:

```python
import yfinance as yf

tickers = ['MSFT', 'AAPL', 'FB']
data = yf.download(tickers, start='2020-01-01', end='2020-05-31')["Adj Close"] 
all_std=data.std(axis = 0, skipna = True)
```




