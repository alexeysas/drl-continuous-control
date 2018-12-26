[//]: # (Image References)

[image1]: images/policy.png "Policy"
[image2]: images/deterministic-policy.png "Deterministic-policy"
[image3]: images/bellman.png "Bellman"
[image4]: images/mseloss.png "Loss"
[image5]: images/policy2.png "Policy"
[image6]: images/algorithm.png "Algorithm"
[image7]: images/noise.png "Noise"

#  Continuous Control

### Goal

The goal of the project is to train agent to Solve "Reacher" environment. You can find detailed description of the environment following the [link](README.md) 

### Solution Summary

To solve the environment we are going to use Deep deterministic policy gradient (DDPG) algorithm published in the [the Deepmind paper](https://arxiv.org/pdf/1509.02971.pdf). The idea of the algorithm is to combine [Policy gradient methods](http://www.scholarpedia.org/article/Policy_gradient_methods) to learn optimal policy together with [DQN algorithm](https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf) to learn Q function. The problem with base DQN algorithm is that it can solve environments with discrete action space. However, most of the real-world tasks especially in the physical control domain has continues and high dimensional action spaces. DDPG algorithm was designed to solve these issues and learn policies in high-dimensional, continuous action spaces.

We have standard reinforcement learning setup where agent is interacting with environment in discrete timesteps. Environment has high-dimensional continues state space and high-dimensional continuous action spaces. We need to learn optimal policy:

![Policy][image1] 

In our case we are going to learn deterministic policy:

![Policy][image2] 

As usual algorithm is based on the reclusive Bellman equation presented in this way below:

![Bellman][image3] 

Similarly, as for the DQN algorithm, we use function approximator (Neural network) to learn Q function and need to optimize MSE loss below:

![MSE][image4] 

Additionally, we need to use function approximator (Neural network) to approximate optimal policy function:

![Policy][image5]

To find an optimal policy we need our policy function learned in a way to maximize Q value from Bellman equation above. So we use standard method for policy gradient methods - gradient accent. Full algorithm described in a [paper](https://arxiv.org/pdf/1509.02971.pdf) is below:

![Algorithm][image6]

Please note that there are two additional ideas used to improve algorithm convergence

- As mentioned in the paper to favor additional exploration, noise was added to the actions defined by the policy:
![Noise][image7]

- The weights of target networks are softly updated during each steps in contrast to [original DQN paper](https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf) where weights of target networks are updated only periodically


### Network archtiecture

The overall architectures we come up with which provide good training results for the Reacher environment are presented on the images below:

Policy network (Actor)

![network architecture][image8]

Q network (Critic) 

![network architecture][image8]

Relu is used as activation function and dropout as regularization technique.


### Next Steps

1. There are possible additional improvements might be tried described in the [Rainbow paper](https://arxiv.org/pdf/1710.02298.pdf)

   - [Prioritized experience relay](https://arxiv.org/abs/1511.05952)
   - [Distributional RL](https://arxiv.org/abs/1707.06887)
   - [Noisy nets](https://arxiv.org/abs/1706.10295)


2. Also additional improvements might be related to the environment state itself.  The idea is related to some observed behavior during training process when agent stack between black bananas. It might be beneficial to add additional value to state vector which indicates how much time left to complete the episode. The intuition behind that is that same states in the beginning of the episode and at the end of episode have different values. The most vivid example is when you surrounded by black bananas - in the beginning of the episode it makes sense just go and collect banana to get out of trap at cost of the -1 points and compensate later. As at the end of the episode it might have more sense to just stay and wait while episode completed.

3. And the final step was to try to learn agent from the raw pixel data using Convolutional network as originally proposed in the DeepMind paper.  Also there might be good idea to use Recurrent Net or long sequence of frames with Convolutional Net to "remember" what agent seen couple of seconds ago – which is especially useful when agent is rotated at 180 degrees and this information is lost.

### Sample 

Here is sample of performance of the Dueling DDQN agent with averege score = 17:  https://github.com/alexeysas/drl-navigation/blob/master/video/test.mp4

