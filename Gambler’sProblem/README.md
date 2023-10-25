# Gambler’s Problem
***
Owner: Yu Yang
It is counterintuitive to think of a gambler could win when the $p_h<0.5$, it's also very interesting to figure out the each capital meaning in the fig.1 below.

![Fig1](Gambler's_Problem/final_policy.png)

* state: the current cash in the gambler
* reward: 0, each decision to next state doesn't cost or get anything
* action: call or all in

![Fig2](Gambler's_Problem/value_iteration.png)

```python
 for a in range(1, min(s, 100 - s) + 1):
    # every state i can call or the num to win at least
    # for these all reasonable actions
    v[a] = 0
    if a + s <= 100:
        v[a] += ph * (0 + V[s + a]) + (1 - ph) * (0 + V[s - a]) 
        # sigma for success and drop
        # 0 is for r 
```

This code is for $V(s)\leftarrow max_a\Sigma_{s',r}p(s',r|s,a)[r+\gamma V(s')]$, attention the desicion part: *if a + s <= 100*, which seems redundant.
But the point of this code is the equal decision. We must make the V[100] to be 1 in advance.

Let alone the other obviously naive parts and let us analysis the results which is the essence of this problem.

![Fig3](ph%3D0.25.png)
Totally the same as the one in the book(Fig 1), capital == 50 is the first point we start to explain, just one all in can make us a winner, so why not try? Even the possibility is only a quarter!
Just as I mentioned at the beganning, 
>It is counterintuitive to think of a gambler could win when the $p_h<0.5$.

So as the capital == 25 and capital == 75, they have the same reason as capital == 50.

>Thinking capital of 51 as 50 plus 1. Of course we can bet all when we have 51, but the best policy is to see if we can earn much from the extra 1 dollar. If this return g is positive, we can say we have extra g money and bet it again until 75, where the sudden win chance is coming. On the contrary, if we bet 50 out of 51 first, our chance of win is only ph and we lose the chance to reach 75. Instead, we will have to try our best to reach 25 first with 1 dollar if we lose the bet, a much worse condition.

Upon is the enough good reason I have found to explain the other capital values. Forgive me don't give the preference, it's a long time from I put it on my notion. Kick me if you know the oringin editor and I'll really appreciate it.

![Fig4](ph%3D0.55.png)
![Fig5](ph%3D1.0.png)
A slight possibility will make the gambler calm down, he only put one coin no matter how many he holds.
Why not just all in every time even the ph=1.0?
My mentor thinks the gambler doesn't know the exact ph all the time, so he chooses the most stablely way to get his goal.
I think she has some points.
