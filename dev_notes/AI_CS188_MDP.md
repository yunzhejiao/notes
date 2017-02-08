

MDP : the way of formalizing the idea of non-deterministic search which is search when your actions outcomes are uncertain.

why would we be unsure what the outcomes of our actions is going to be ?  Well, maybe we've got a robot on a ledge and we take the action across the ledge, what's gonna happen ? Maybe we'll cross the ledge ,maybe we will fall into the fire pit. We're not sure we can commit to the action. But of course the outcome is entirely under our control. 

maybe you're a can opener robot and you take the can and you open it , and what's underneath ? More can! 



## Example : Grid World 

Noisy movement: for example : North action

Reward : 2 kinds of rewards.  one are the terminal utilities, shown as the plus 1 and minus 1(also the exit to game end). Another kind of reward which is every step take comes along with a little tiny reward , and this is sometimes called a living reward or a living penalty based on whether it's positive or negative. 

Goal: in general the agent is going to involve getting to a big reward and taking it. 

## Grid World Actions

you're not sure what the results gonna be. 

if you take north , you may move left, and it's bad.  So when you plan you're gonna have to take these outcomes into account. 


## Markov Decision Processes

MDP is a lot like a search problem. 

unlike : successor function . unlike in search , we're going to take the successor function and break it into a few pieces. We're going to have an idea of actions which are the actions you take like north ,south, east, west. We're then gonna have a transition function . 

T(s,a,s')  :  in some state s , you take some action a , s' is a possible result. The function T(s,a,s')  tells you how likely that result is and in that sense it's a conditional probability. 

The transition function is basically the successor function. The differences is now there are lots of differents s's that can happen and they all have various probabilities t associated with them. 

Reward function (unlike)  R(s,a,s')  means you get a reward that depends on the state you are in , the action you took, and the outcome. You might not know your actual reward until you see whether or not you fell into the pit.  In some formulations R will only depend on s and s'. What is this ?  This is basically the cost function from search. In seach the cost would be small and in the case of MDPs in general we want the rewards to be big. 

Terminal state :   another import difference between MDPs and search problems is  MDP's very ofren go on forever .

MDP is basically taking search that we know and love , and adding the necessary machinery to support the idea that actions can have multiple outcomes. 

## What is Markov about MDPs ?

MDP means the probability distribution over your outcomes depends only on the current state and action , not on the whole histroy of how you got there.

So this is important property in MDP is to make sure that you define your transition function and your state in such a way that the transition probabilities depend only on the current state and action. 

## Policies

1 not work for MDPs. because we don't know what actions are gonna to do.  The relevant idea is not a plan now but a policy. Policy is a mapping from states to actions and tells  in each state what action to take. 

Expectimax didn't really compute an explicit policy in this sense. What expectimax did for these kinds of problems is from a given state it did a forward-thinking computation that produced one entry of the policy which you then took and wherever you land and you run expectimax again. 

So on one hand expectimax is a way of solving these problems and on the other hand it doesn't compute an explicit policy. That chould be a good thing or bad thing. It can be bad because you might redo a lot of work if you keep in the same state. It could be good because there are so many states you coundn't write down an explicit policy anyway. 

## Optimal Policies

The funny thing is the square left to "-1" squre. It's going into the wall. Why is it going into the wall?  This is an example of we feed the rules of the game in , we compute the optimal behavior , sometimes the behavior is not what we expected but it's still optimal. In this example, if you did anything else you would risk the pit. The agaent is pressing its check against the wall and justing waiting waiting waiting until it gets out of this scary situation safely. So this agent is very very conservative because each time step cost very little. 

What happens if we make this living penalty more severe. 

## Example: Racing

### Racing Search Tree

Any MDP is defining a search tree. So if you're in some particular state , for example if you're in the state where the car is cool, you have 2 actions: slow or fast. 

It's very like an expectimax tree but we'll see very shortly why we might not want to use expectimax to solve it. You can already see hints of it : it's a big tree but it's kinds of same blue and red stuff over and over again. There's just not that many states. This is the case where expectimax will do a lot of work. 


## MDP Search Trees

Queue State: when I'm in a state and I take an action I end up in a queue state , which you can think of as kind of the pair of the state and the action where I've committed to the action but I haven't done it yet. 
 
That is kind of expectimax tree except ...
 
 1. the probabilities are given to you by the transition function and 
 2. the rewards instead of being at the bottom are smeared throughout the tree, they come to you step-by-step.

## Utilities of Sequences

In a MDP the rewards come to you step-by-step , we need to figure out what our unility function acutally is. In the simplest case you just add up the rewards but it can be little more subtle than that.

For example shown here 


you might care whether or not you get these 4 gems step-by-step or all at the end in one big prize. This raises a general question for MDPs: what preferences or utilities should an agent have for rewards sequences. 

## Discounting 

## Stationary Preferences

What we want from an agent that looks at sequences of rewards in order to consider it kind of reasonable. 

stick the same reward in front of both 

if I liked A better than B now I should like it better shifted into the future as well and vice versa. 

## Infinite Utilities ?!

Here are multiple possible solutions, in general we'er gonna have discounts that usually saves us. 

## Recap: Defining MDPs

## Solving MDPs

### Optimal Quantities 

what the star means is  this is the value under optimal action.

![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_mdp_Optimal_Quantities_example.png)

Look at the left-bottom square with 0.49. The arrow is the policy.  What's that 0.49?  That is if you started in this state and you ran this game over and over again and sometimes you slipped, and sometimes you didn't , and you added up all of those utilities on average you would get 0.49, and you will achieve it by trying to go north whenever you're in the state. 

You can see it's better to be at 0.85 square by the exit that it is to be over here 0.49 square. Why ? Because if you're over here you have to pay that living reward and some discount to even get to the exit. The best thing is to actually be in the exit. 

Similarly you can see if you're essentially 2 steps from the exit, it's better to be at 0.74 square than at 0.57 next to the pit , because when something goes wrong , .74  is not a big deal, while .57 you fall into the pit and lose.

![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_mdp_Optimal_Quantities_example_qstate.png)

This shows the Q values. From each state , except for the exit state , you got 4 choices of actions . If you are in the 0.57 state next to the pit, and the action you've committed to is north , then you get that same .57. 

## Values of States 

So we want to be able to compute these values. We'd like to be able to take an MDP and compute these expectimax values for a state and actully we usually do with these algorithms we compute the values for all of the states. We'll see that there are ways to save time by doing all the states at once provided your MDP is small enough that you can actually go through all the states. 

### Racing Search Tree

## Time-Limited Values

What's a time step ? It's a reward. 

when you achieve the exit, you must take extra "exit" action to end game and get the reward.

iteration 2:  from the squre between wall and pit, I have time to do very stupid things -- going to the pit and receive a negative -- but that's not the optimal.  The optimal thing is kind of anything else. So I have zero. But if it allowed 3 steps I can get some rewards even from there. 



iteration 6: from left-bottom square , I am now possible to  get to a positive reward.
iteration 7: from left-bottom square , I can not only get there in the lucky way where nothing goes wrong I can also get there in various ways where something goes wrong once. 

iteration 100: most of the states are pretty good. 

![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_grid_world_it1.png)
![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_grid_world_it2.png)
![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_grid_world_it3.png)
![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_grid_world_it5.png)
![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_grid_world_it6.png)
![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_grid_world_it7.png)

![](https://raw.githubusercontent.com/mebusy/notes/master/imgs/cs188_grid_world_it100.png)


## Computing Time-Limited Values

assuming it's a whole tree.

in the bottom , there's a lot of repeat status. It's terminal state, nothing else happens  so no more rewards. That means I got lots of copies of V₀ -- lots of depth-0 expectimax , and in fact we know it be 0, 0 for any state.

Now what's in this layer upon the bottom ?  Each one of these computations is a depth-1 expectimax computation. Which means in fact all we have is a whole bunch of copies of V₁ in this layer. 

Then at the very top where only computing one value but conceptually this top layer is well represented by the collection of V₄ .

So you see at the bottom even though the tree has grown immensely it's still only 3 values, and each layer is V<sub>k</sub> for K . So this actually gives us an idea of how we can compute these values in an efficient way where we don't get the explosion in depth because you can see at the bottom it's no worse than at the top. That's now going called value iteration.

## Value Iteration

You can think of this as basically building your computation from bottom up all the way up to the top where you will receive the computation that expectimax would have done but with a whole lot less work assuming we have a small number of states.

V₀(s)   what's this actually look like in code ?  It's a vector. There is 0 for every state in this vector. This is different than expectimax which you think about is computing for one state -- this is all states , the vector of zeroes. 

To compute V<sub>k+1</sub> , we will recurse into a expectimax tree V<sub>k</sub> that we've already computed, We already have that entire vector for every state. 

So what does this actually do ? It goes from 0 to 1 , to 2 , and we keep going until we decide to stop. When are we gonna stop ? What is that magic value K ? Because we're going bottom-up we can keep doing this until it converges. 

Complexity of each iteration: O(S²A).  It's good in that it doesn't grow with the number of iterations like expectimax grows with the depth. It's bad in that expectimax doesn't have to touch every state if it doesn't go to deep, this always touches every state.  So it's all about the trade-off of how many states you have and how connected they are and how deep you need to into the tree. 

### Example: Value Iteration 

...

So what we see ?  

 1. As I go further up the numbers are increasing and that make sense becaues as I have more time steps in this MDP I can get more rewards
 2. I also see that every layer it's better to be in the cool state than to be in the warm state.
    - of course it's better because you can then safely go fast. 

Now you can look at this you can probably figure out the optimal policy. The optimal policy is if you're cool go fast and once you warm up you go slow and you never risk overheating. Now that optimal policy is actually already found at V₁ . These number 2,1,0 already reflect it. But it takes longer for the values to actually figure out how good that is . In general , V₁ is not enough to find the vest policy and in particular in this MDP because there's no discount the Vs aren't going to converge. 
 
## Convergence 

How do we know ?

First of all we don't. Because if there's no discount and the rewards are all positive and the game never going to end like for racing the values are infinite and you're never going to get there.  But there are cases where we can show that it will converge. 

Case 1: 



 