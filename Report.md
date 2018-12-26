[//]: # (Image References)

[image1]: images/q_formula.png "Action-value function"
[image2]: images/n1.png "Q-Network"
[image3]: images/n2.png "Dueling Q-Network"

#  Continuous Control

### Goal

The goal of the project is to train agent to Solve "Reacher" environemnt. You can found detailed description of the environemnt following the [link](README.md) 


### Solution Summary

To solve the environment we are going to use Deep deterministic policy gradient (DDPG) algorithm published in the [the Deepmind paper](https://arxiv.org/pdf/1509.02971.pdf). The ieda of the algorithm is to combine [Policy gradient methods](http://www.scholarpedia.org/article/Policy_gradient_methods) to learn optimal policy together with [DQN algorithm](https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf) to learn Q function. The problem with base DQN algorithm is that it can solve environmetns with descrete action space. However, most of the real-world tasks expecially in the physical control domain has continius and high dimentional action spaces. DDPG algorith was designed to solve these issues and learn policies in high-dimensional, continuous action spaces.

We have standard reinforcement learning setup where agent is iteracting with environemnt in discrete timesteps. Environemnt has high-dimentional continius state space and high-dimentional continius action spaces. We need to learn optimal policy [[inssert policy image there]] (in our case we are going to learn deterministic policy) [[inssert deterministic policy image there]] having reward function r(s, a).

So in our case we can use Bellman equation presented in this way below:

[[insert Bellamn image]]

In the same way as in DQN we use function aproximator (Neural network) to learn Q function  and need to optimize MSE loss below:

[[insert MSE loss image]]

Additinaly we need to use function aproximator (Neural network) to aproximate optimal policy function 

[[insert Policy 2 image there]]
![Action-value function][image1]

To find an optimal policy we need our policy function to maximize Q value from bellman equasion above. So we use standard gradient accesnt method. 

The problem is that our state space is continius  with 37 dimensions so we can not use traditional temporal-difference method like SARSA or [Q-learning](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.80.7501&rep=rep1&type=pdf). Of course this task can be solved with descritisations techniques like: Tile Coding or Course Coding. However as described in the paper the better results can be archived with function approimation aproach and using of Neural Network as a universal function aproximator for this purpose.  So we approaximating true action-value function q(S,a) with function q(S,a,w). Our goal is to optimize parameters w to make approximatein as good as possible.

It was a known fact that reinfercement learning is unstable when a Q function is represented with newral network. Autwors introduced two additional ideas:

    - Expirience replay - the mecjanomt to store observed tuples of (state, action, next_state, is_terminal) in the special buffer and randomly sample these tuples during 
    - Target values are stored in the separate network with same archtiecture and only periodicaly updated reducing correlation with target
    
 So the final idea of the algorithm is to parameterize an approximate value function

### Network archtiecture

![network architecture][image2]


### Variations 

1. The Double DQN

2. Dueling DQN: 

![ Dueling DQN][image3]

