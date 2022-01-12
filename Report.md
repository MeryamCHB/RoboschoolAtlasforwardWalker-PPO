# **Environnement Description**
### Atlas
Atlas is a bipedal humanoid robot primarily developed by the American robotics company Boston Dynamics with funding and oversight from the U.S. Defense Advanced Research Projects Agency (DARPA). The robot was initially designed for a variety of search and rescue tasks

Our environnement represents a 3-D humanoid robot similar to Boston Dynamics ATLAS robot. The task is to make the robot run as fast as possible.


https://user-images.githubusercontent.com/68785689/149093559-4605f83d-26fa-4cd6-b2a1-c43f57f23600.mp4

# **Algorithm Description**


PPO is motivated by the same question as TRPO: how can we take the biggest possible improvement step on a policy using the data we currently have, without stepping so far that we accidentally cause performance collapse? Where TRPO tries to solve this problem with a complex second-order method, PPO is a family of first-order methods that use a few other tricks to keep new policies close to old. PPO methods are significantly simpler to implement, and empirically seem to perform at least as well as TRPO.

PPO can be used for environments with either discrete or continuous action spaces (in our environnement action space is continious). It trains a stochastic policy in an on-policy way. Also, it utilizes the actor critic method. The actor maps the observation to an action and the critic gives an expectation of the rewards of the agent for the observation given. Firstly, it collects a set of trajectories for each epoch by sampling from the latest version of the stochastic policy. Then, the rewards-to-go and the advantage estimates are computed in order to update the policy and fit the value function. The policy is updated via a stochastic gradient ascent optimizer, while the value function is fitted via some gradient descent algorithm. This procedure is applied for many epochs until the environment is solved.

![image](https://user-images.githubusercontent.com/68785689/149186899-345afe14-c92f-413d-93b1-f1f5e6ec782d.png)




