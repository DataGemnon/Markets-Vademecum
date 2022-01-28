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

<img src="https://render.githubusercontent.com/render/math?math=\Delta x = a\cdot dt%2bb\cdot dz "> or <img src="https://render.githubusercontent.com/render/math?math=\Delta x = a\cdot dt %2b b\cdot \varepsilon \sqrt{dt}"> with a and b being two constants. Constant a represents the drift rate and <img src="https://render.githubusercontent.com/render/math?math=\b\cdot dz "> represents the "uncertain" part in the <img src="https://render.githubusercontent.com/render/math?math=\Delta x">.



