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

<img src="https://render.githubusercontent.com/render/math?math=\(\frac{\delta F}{\delta t}%2b\frac{1}{2}\frac{\delta ^{2}F}{\delta S^{2}}\sigma ^{2}S^{2})dt = r(-F%2b\frac{\delta F}{\delta S}S)dt"> <=> <img src="https://render.githubusercontent.com/render/math?math=\rf=\frac{\delta F}{\delta t}%2brs\frac{\delta F}{\delta S}%2b\frac{1}{2}\sigma ^{2}S^{2}\frac{\delta ^{2}F}{\delta S^{2}}">

This last equation is called the Black Scholes partial differential equation and allows many solutions as the value of the derivatives are multiple. 
This is why we need to define the boundaries at maturity T:
* <img src="https://render.githubusercontent.com/render/math?math=\max (S_{T}-K, 0)"> for a European call
* <img src="https://render.githubusercontent.com/render/math?math=\max (K-S_{T}, 0)"> for a European put

The most known solutions are the Black Scholes Formulas

<img src="https://render.githubusercontent.com/render/math?math=\c=S_{0}N(d_{1})-Ke^{-rT}N(d_{2})"> for European calls

<img src="https://render.githubusercontent.com/render/math?math=\p=Ke^{-rT}N(-d_{2})-S_{0}N(-d_{1})"> for European puts

with <img src="https://render.githubusercontent.com/render/math?math=\d_{1}=\frac{ln(S_{0}/K)%2b(r%2b\sigma ^{2}/2)T}{\sigma \sqrt{T}}"> and <img src="https://render.githubusercontent.com/render/math?math=\d_{2}=\frac{ln(S_{0}/K)%2b(r-\sigma ^{2}/2)T}{\sigma \sqrt{T}}=d_{1}-\sigma \sqrt{T}">

N(x) represents the cumulative density function for x, a normally distributed variable. 

The following code provides two functions to calculate the price of a European call et a European put. 

```python
import numpy as np
from scipy.stats import norm

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
```

