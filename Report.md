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

So full algorithm described in a [paper](https://arxiv.org/pdf/1509.02971.pdf) is below:

[[Use algorithm image]]

Please note that there are two additinal ideas used to improve algorithm convergence

- As mentioned in the paper to favor additional exploration noise was added to the actions defined by policy:
[[noise image]]

- The weights of target networks are softly updated during each steps in contrast to [original DQN paper](https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf) where weights of target networks are updated only periodically


### Network archtiecture

![network architecture][image2]


### Variations 

1. The Double DQN

2. Dueling DQN: 

![ Dueling DQN][image3]

