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


