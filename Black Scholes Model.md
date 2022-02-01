### Black Scholes Model

#### Before starting

The reader should remember two things from the previous github page:

* <img src="https://render.githubusercontent.com/render/math?math=\dS=\mu S\cdot dt%2b\sigma S\cdot \varepsilon \sqrt{dt}"> with <img src="https://render.githubusercontent.com/render/math?math=\varepsilon \sqrt{dt}">
being a Wiener process we can replace by dz.

* Itô's Lemma

With Itô's Lemma we can easily imagine the price of an option whose underlying asset is the stock S. We can represent the price of this option with the variable F

<img src="https://render.githubusercontent.com/render/math?math=\dF = (\frac{\delta F}{\delta S}\mu S %2b \frac{\delta F}{\delta t} %2b \frac{1}{2}\frac{\delta ^{2} F}{\delta S^{2}}\sigma ^{2}S^{2})dt %2b \frac{\delta F}{\delta S}\sigma S\cdot \varepsilon \sqrt{dt} ">

#### Construction of a riskfree portfolio

The Black Scholes Model takes into account an argument that will help us to develop until the final equation: it is possible to build a riskless portfolio using one position on a European call and one position in its underlying stock (both having the same source of volatility, namely changes in the stock price).
If the portfolio is built correctly, the position in the stock will balance the changes in the price of the call (and vice versa). 

Using Itô's formula just above, we can build a portfolio composed of -1 call (short position) and  <img src="https://render.githubusercontent.com/render/math?math=\frac{\delta F}{\delta S}"> units of the underlying stock S to eliminate the Wiener Process dz (it is the same Wiener Process for S and F). 

<img src="https://render.githubusercontent.com/render/math?math=\Pi = -F+%2b\frac{\delta F}{\delta S}S"> <=> <img src="https://render.githubusercontent.com/render/math?math=\Pi = -\Delta F%2b\frac{\delta F}{\delta S}{\Delta S}"> with <img src="https://render.githubusercontent.com/render/math?math=\Pi"> representing the value of the portfolio.  

Pluging in the formulas for dF and dS we get:

<img src="https://render.githubusercontent.com/render/math?math=\d\Pi =(-\frac{\delta F}{\delta t}-\frac{1}{2}\frac{\delta ^{2}F}{\delta S^{2}}\sigma ^{2}S^{2})dt">

With this equation we got rid of the "uncertainty" represented bu dz. If there is no arbitrage opportunity, this portfolio should earn as much as the risk-free rate. For a small time icrement, we can deduce the following:

<img src="https://render.githubusercontent.com/render/math?math=\d\Pi = r \Pi\cdot dt"> 

or written differently:

<img src="https://render.githubusercontent.com/render/math?math=\(\frac{\delta F}{\delta t}%2b\frac{1}{2}\frac{\delta ^{2}F}{\delta S^{2}}\sigma ^{2}S^{2})dt = r(-F%2b\frac{\delta F}{\delta S}S)dt"> <=> <img src="https://render.githubusercontent.com/render/math?math=\rF=\frac{\delta F}{\delta t}%2brs\frac{\delta F}{\delta S}%2b\frac{1}{2}\sigma ^{2}S^{2}\frac{\delta ^{2}F}{\delta S^{2}}">

This last equation is called the Black Scholes partial differential equation and allows many solutions as the value of the derivatives are multiple. 
This is why we need to define the boundaries at maturity T:
* <img src="https://render.githubusercontent.com/render/math?math=\max (S_{T}-K, 0)"> for a European call
* <img src="https://render.githubusercontent.com/render/math?math=\max (K-S_{T}, 0)"> for a European put

The most known solutions are the Black Scholes Formulas

<img src="https://render.githubusercontent.com/render/math?math=\c=S_{0}N(d_{1})-Ke^{-rT}N(d_{2})"> for European calls

<img src="https://render.githubusercontent.com/render/math?math=\p=Ke^{-rT}N(-d_{2})-S_{0}N(-d_{1})"> for European puts

with <img src="https://render.githubusercontent.com/render/math?math=\d_{1}=\frac{ln(S_{0}/K)%2b(r%2b\sigma ^{2}/2)T}{\sigma \sqrt{T}}"> and <img src="https://render.githubusercontent.com/render/math?math=\d_{2}=\frac{ln(S_{0}/K)%2b(r-\sigma ^{2}/2)T}{\sigma \sqrt{T}}=d_{1}-\sigma \sqrt{T}">

N(x) represents the cumulative density function for x, a normally distributed variable. N(d2) ist the prbability that S will be at or above the K (the call would be at-the-money or in-the-money) at maturity T.

The following code provides two functions to calculate the price of a European call et a European put. The variable prices gives you the price for a call and a put with the same parameters. The code further below provides 3-dimensional representations for calls and puts.

```python
import numpy as np
from scipy.stats import norm
import matplotlib.pyplot as plt

def bs_call(S, K, r, sigma, T):
    d1=(np.log(S/K)+(r+(sigma**2)/2)*T)/(sigma*np.sqrt(T))
    d2=d1-sigma*np.sqrt(T)
    c = S*norm.cdf(d1)-K*np.exp(-r*T)*norm.cdf(d2)
    return c

def bs_put(S, K, r, sigma, T):
    d1=(np.log(S/K)+(r+(sigma**2)/2)*T)/(sigma*np.sqrt(T))
    d2=d1-sigma*np.sqrt(T)
    p = K*np.exp(-r*T)*norm.cdf(-d2)-S*norm.cdf(-d1)
    return p

prices=[bs_call(100, 105, 0.01, 0.2, 1),bs_put(100, 105, 0.01, 0.2, 1)]

# plot for calls
strikes = np.linspace(50, 150, 100)
sigmas=np.linspace(0.01, 1, 100)
X, Y = np.meshgrid(sigmas, strikes)
Z=bs_call(100, Y, 0.01, X, 1)
fig = plt.figure(figsize = (10,7))
ax = plt.axes(projection='3d')
ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap='jet', edgecolor='none')
ax.set_title("3-dimensional plot of call prices", fontsize = 13)
ax.set_xlabel('sigma', fontsize = 11)
ax.set_ylabel('strike', fontsize = 11)
ax.set_zlabel('call price', fontsize = 10)

# same for puts
Z2=bs_put(100, Y, 0.01, X, 1)
fig2 = plt.figure(figsize = (10,7))
ax2 = plt.axes(projection='3d')
ax2.plot_surface(X, Y, Z2, rstride=1, cstride=1, cmap='viridis', edgecolor='none')
ax2.set_title("3-dimensional plot of put prices", fontsize = 13)
ax2.set_xlabel('sigma', fontsize = 11)
ax2.set_ylabel('strike', fontsize = 11)
ax2.set_zlabel('put price', fontsize = 10)
```

First representation for calls:

![3d for calls](https://user-images.githubusercontent.com/76557960/151881096-51d78aa9-e49b-4ccc-9fea-3226d32fad41.png)

Second representation for puts:

![3d put price](https://user-images.githubusercontent.com/76557960/151881392-f8769da2-0574-4260-bb3b-5e8481f43b92.png)

