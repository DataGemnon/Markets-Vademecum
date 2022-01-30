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
