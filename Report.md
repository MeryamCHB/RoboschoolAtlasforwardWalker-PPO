<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#Environnement description">Environnement description</a>
      <ul>
        <li><a href="#Atlas">Atlas</a></li>
      </ul>
    </li>

   <li><a href="#Algorithm description">Algorithm description</a></li>
           <li><a href="#Algorithm description">Agent Hyperparameters</a></li>

       <li><a href="#Algorithm description">Model Hyperparameters</a></li>
           <li><a href="#Results">Model Hyperparameters</a></li>


    

  </ol>
</details>

# **Environnement Description**
### Atlas
Atlas is a bipedal humanoid robot primarily developed by the American robotics company Boston Dynamics with funding and oversight from the U.S. Defense Advanced Research Projects Agency (DARPA). The robot was initially designed for a variety of search and rescue tasks

Our environnement represents a 3-D humanoid robot similar to Boston Dynamics ATLAS robot. The task is to make the robot run as fast as possible.


https://user-images.githubusercontent.com/68785689/149093559-4605f83d-26fa-4cd6-b2a1-c43f57f23600.mp4

# **Algorithm Description**


PPO is motivated by the same question as TRPO: how can we take the biggest possible improvement step on a policy using the data we currently have, without stepping so far that we accidentally cause performance collapse? Where TRPO tries to solve this problem with a complex second-order method, PPO is a family of first-order methods that use a few other tricks to keep new policies close to old. PPO methods are significantly simpler to implement, and empirically seem to perform at least as well as TRPO.

PPO can be used for environments with either discrete or continuous action spaces (in our environnement action space is continious). It trains a stochastic policy in an on-policy way. Also, it utilizes the actor critic method. The actor maps the observation to an action and the critic gives an expectation of the rewards of the agent for the observation given. Firstly, it collects a set of trajectories for each epoch by sampling from the latest version of the stochastic policy. Then, the rewards-to-go and the advantage estimates are computed in order to update the policy and fit the value function. Adam optimiser was used for both actor and critic. This procedure is applied for many epochs until convergence.

![image](https://user-images.githubusercontent.com/68785689/149186899-345afe14-c92f-413d-93b1-f1f5e6ec782d.png)


Sampling actions for a continious action space needs to add: 
 * A constant/decayed standard deviation for the output action distribution (multivariate normal with diagonal covariance matrix) It is a hyperparameter and NOT a trainable parameter. However, it can be linearly decayed.

 * It uses simple monte-carlo estimate for calculating advantages and NOT Generalized Advantage Estimate 



# **Agent Hyperparameters**

- ### For training
* * max_ep_len = 1000  : is the max timesteps in one episode
* * max_training_timesteps = int(2000000) : break training loop if timeteps > max_training_timesteps
* * action_std = 0.1 : starting std for action distribution (Multivariate Normal)
* * update_timestep = max_ep_len * 4  : update policy every n timesteps
* * K_epochs = 80  :update policy for K epochs in one PPO update The idea in PPO is that you want to reuse the batch many times to * update the current policy.This means you repeat your training' k. epoch amount of times for the same batch of trajectories.
* * eps_clip = 0.2  : clip parameter for PPO  
* * gamma = 0.99            # discount factor

- ### For tracking , logging and checkpoint
* * print_freq = max_ep_len * 10  : print avg reward in the interval (in num timesteps)
* * log_freq = max_ep_len * 2 : log avg reward in the interval (in num timesteps)
* * save_model_freq = 10000  : save model frequency (in num timesteps)

# **Model Hyperparameters**
* ACTOR architecture :

The actor network maps each state to a corresponding action.
In our implementation, the Actor Network is a simple network consisting of 2 densely connected layers with the Tanh activation function. Each hidden layer has 64 hidden units. 

For optimization we have used Adam optimizer with a learning rate =0.001
MSE loss function
* CRITIC architecture : 

The critic network maps each state to its corresponding Q-value
Also the critic  Network is a simple network consisting of 2 densely connected layers with the Tanh activation function. Each hidden layer has 64 hidden units.  

For optimization we have used Adam optimizer as well with a learning rate =0.001.
MSE loss function

# **Results**


| PPO Continuous RoboschoolAtlasForwardWalk-v1  | PPO Continuous RoboschoolAtlasForwardWalk-v1 |
| :-------------------------:|:-------------------------: |
| ![](https://user-images.githubusercontent.com/68785689/149205405-acc39538-83b5-4c7a-a0cd-91a1d2a27a06.gif) |  ![](https://user-images.githubusercontent.com/68785689/149204794-35a70647-1468-49da-8822-4796ad77d89d.png) |


</p>
The video was taken from [HERE](https://github.com/chainer/chainerrl/blob/8c42022361d7013e7ed5535874a5ff71c7686698/examples/atlas/assets/atlas.gif) it doesn't reflect our results, just for give an idea about the learning objectives.


We train the Proximal Policy Optimization agent until it can reach the maximum score of 115 for 30000 consecutive episodes with 1000 max timesteps for each episode. The agent didn't converge yet and we didn't found any baseline with the same algorithm to compare our results with but we think that adding some improvements can help the agent converge faster :
 * using learning rate scheduler 
 * using a standard deviation decaying 
 * normalizing rewards 












