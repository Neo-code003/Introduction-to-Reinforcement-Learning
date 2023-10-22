# Windy GridWorld

Owner: Tian Huang

![截屏2023-07-10 17.19.00.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/%25E6%2588%25AA%25E5%25B1%258F2023-07-10_17.19.00.png)

problem statement:

from start to goal, the middle region exists wind, the strength of which varies from column to column, as is shown in the picture.

- state: position x, y, wind strength
- reward: -1 until reach goal, undiscounted episodic task $\gamma =1$
- action: N, S, E, W

![Untitled](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/Untitled.png)

modules:

- environment:
    - gridworld size: 7*10
    - wind: hard [6, 7], soft [3,4,5,8] (columns)
    - state (x,y)
    - action
    - reward: -1 (feasible region), -100 (infeasible region), 0 (goal position)
    - check if hit the boundary
    - check if reach goal
    - update each step: reach end → reach infeasible region → take action →hit boundary → wind → hit boundary →return reward and new state
      
        ```python
        def move_conditions(self, state, action):
        
            if action == 'N':
                temp_s = (state[0] - 1, state[1])
            elif action == 'S':
                temp_s = (state[0] + 1, state[1])
            elif action == 'W':
                temp_s = (state[0], state[1] - 1)
            elif action == 'E':
                temp_s = (state[0], state[1] + 1)
            elif action == 'NE':
                temp_s = (state[0] - 1, state[1] + 1)
            elif action == 'NW':
                temp_s = (state[0] - 1, state[1] - 1)
            elif action == 'SE':
                temp_s = (state[0] + 1, state[1] + 1)
            elif action == 'SW':
                temp_s = (state[0] + 1, state[1] - 1)
            else:
                raise Exception("Invalid action")
            
            # testify whether hit the border
            temp_s = self.wall_move(temp_s)
            # move according to wind strength
            temp_s = self.wind_move(temp_s)
            # testify whether hit the border
            temp_s = self.wall_move(temp_s)
        
            return temp_s
        ```
    
- agent
    - give possible actions (’N’, ‘S’, ‘E’, ‘W’)
- algorithm
  
    ![截屏2023-07-10 19.34.38.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/%25E6%2588%25AA%25E5%25B1%258F2023-07-10_19.34.38.png)
    
    ![截屏2023-07-10 19.35.20.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/%25E6%2588%25AA%25E5%25B1%258F2023-07-10_19.35.20.png)
    

result：

Sarsa:

total time: 8893.94s

![截屏2023-07-12 10.40.49.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/%25E6%2588%25AA%25E5%25B1%258F2023-07-12_10.40.49.png)

![windy_gridworld_Sarsa.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/windy_gridworld_Sarsa.png)

Q-learning:

total time: 706.64s

$\varepsilon$-greedy: $\varepsilon=0.6$

![截屏2023-07-12 10.40.37.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/%25E6%2588%25AA%25E5%25B1%258F2023-07-12_10.40.37.png)

![windy_gridworld_Q-learning.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/windy_gridworld_Q-learning.png)

---

windy gridworld with king’s moves

change actions: right, left, up, down, diagonal

result:

Sarsa king’s move

total time: 4009.03s

![截屏2023-07-12 10.40.24.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/%25E6%2588%25AA%25E5%25B1%258F2023-07-12_10.40.24.png)

![windy_gridworld_Sarsa_King.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/windy_gridworld_Sarsa_King.png)

Q-learning king’s move:

total time: 321.58s

$\varepsilon$-greedy: $\varepsilon=0.99$

![截屏2023-07-12 10.39.43.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/%25E6%2588%25AA%25E5%25B1%258F2023-07-12_10.39.43.png)

![windy_gridworld_Q-learning_King.png](Windy%20GridWorld%20f93ed095670b4c18be63546bb1133ec7/windy_gridworld_Q-learning_King.png)


💡
  1. if use $\varepsilon$-greedy, the goal will be reached more quickly over time
  2. can’t use MC, cause can’t make sure always reach goal




---

TODO：

stochastic wind

change state:

wind strength sometimes varies by 1 from