# Markets-Vademecum
Gathering mathematical knowledge about financial markets.

This work is done by Master in Finance students.

### Stock path modeling
#### Short introduction to options

Options are financial derivatives that give their holder the right (not the obligation) to buy or sell the underlying asset (stocks, currencies …) for a given price at a given maturity. Calls options give their holders the right to buy and put options the right to sell. The payoff of a call is given by max(0 ; St-K) and the payoff of a put is given by max(0 ; K-St) with K being the strike price and St the spot price. But before going into the option's pricing theory, we must see how to modelize stock paths. 

#### Markov Process & Wiener Process

In options pricing, the value of the underlying evolves in an uncertain way and is, therefore, a continuous-time stochastic process. To be more precise, the price of the UA (underlying asset, like stocks) is a Markovian process, a specific type of stochastic process in which only the latest value is helpful to provide the future one.

A Wiener process is one kind of Markov process with mean 0 and variance 1. A variable z follows a Wiener process if both following conditions are satisfied:


$$
M = \left( \begin{array}{ccc}
x_{11} & x_{12} & \ldots \\
x_{21} & x_{22} & \ldots \\
\vdots & \vdots & \ldots \\
\end{array} \right)
$$

