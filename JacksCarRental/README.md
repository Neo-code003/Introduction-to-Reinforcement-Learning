# Jack’s car rent

Owner: Tian Huang

linear

problem statement:

reward:

- rent car: 10
- move car: -2

constraint: 

- move at most 5 cars from one place to the other
- each place have at most 20 cars

distribution of request and return in location A & B:

$$
\lambda_{request\_A} = 3,\lambda_{request\_B}=4,\lambda_{return\_A}=3,\lambda_{return\_B}=2
$$

$$
P(n) = {\lambda^n\over n!}e^{-\lambda}
$$

![截屏2023-06-25 20.32.52.png](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/%25E6%2588%25AA%25E5%25B1%258F2023-06-25_20.32.52.png)

policy iteration:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled.png)

three part:

- initialization
  
    value matrix, policy matrix:
    
    ![value matrix[i][j] store the function value corresponding to state, policy matrix[i][j] store the number of cars moving from A to B (+) or B to A (-)](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/%25E6%2588%25AA%25E5%25B1%258F2023-06-25_20.17.19.png)
    
    value matrix[i][j] store the function value corresponding to state, policy matrix[i][j] store the number of cars moving from A to B (+) or B to A (-)
    
- policy evaluation
    - expected reward
      
        using Bellman equation:
        
        $$
        \begin{align*}
        v_\pi(s)&\doteq \mathbb{E}_\pi[G_t|S_t=s]\\
        &=\mathbb{E}_\pi[R_{t+1}+\gamma G_{t+1}|S_t=s]\\
        &=\mathbb{E}_\pi[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s]\\
        &=\sum_a \pi(a|s)\sum_{s',r}p(s',r|s,a)[r+\gamma v_\pi(s')]
        \end{align*}
        $$
        
    
    $$
    V(s)=\sum_{s',r}p(s',r|s,\pi(s))[r+\gamma V(s')]
    $$
    
    since in location A, B request and return are independent, $p(s',r|s,a) = p(A_{request})\cdot p(A_{return})\cdot p(B_{request})\cdot p(B_{return})$
    
    - $\theta$ a small positive value, start with 50, divided by 10 in every iteration
- policy improvement
  
    $$
    V(s)=\arg\max_a\sum_{s',r}p(s',r|s,a)[r+\gamma V(s')]
    $$
    

iteration 1:

![state value](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%201.png)

state value

![policy](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%202.png)

policy

iteration 2:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%203.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%204.png)

iteration 3:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%205.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%206.png)

iteration 4:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%207.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%208.png)

iteration 5:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%209.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2010.png)

$$
\Delta = 0.0047
$$

![截屏2023-06-25 22.20.21.png](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/%25E6%2588%25AA%25E5%25B1%258F2023-06-25_22.20.21.png)

adding non-linearities to the original rental problem (exercise 4.7)

reward:

- move from A to B for the first time free
- each location more than 10 cars cost 4 per night

change:

constraint 1

```python
phi = 0 #reward
new_state = apply_action(state, action)
    
# adding reward for moving cars from one location to another (which is negative) 
phi = phi + jcp.moving_reward() * abs(action)
```

```python
phi = 0 #reward
new_state = apply_action(state, action)    

# adding reward for moving cars from one location to another (which is negative) 

if action <= 0:
    phi = phi + jcp.moving_reward() * abs(action)
else:
    phi = phi + jcp.moving_reward() * (action - 1)    #one car is moved by one of Jack's employees for free
```

constraint 2

```python
if new_state[0] > 10:
    phi = phi + jcp.second_parking_lot_reward()
    
if new_state[1] > 10:
    phi = phi + jcp.second_parking_lot_reward()
```

iteration 1:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2011.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2012.png)

iteration 2:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2013.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2014.png)

iteration 3:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2015.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2016.png)

iteration 4:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2017.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2018.png)

iteration 5:

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2019.png)

![Untitled](RL%20experiment%20Jack%E2%80%99s%20car%20rent%20416508573d184d5a9fcf4d60661280fa/Untitled%2020.png)

$$
\Delta = 0.0049
$$