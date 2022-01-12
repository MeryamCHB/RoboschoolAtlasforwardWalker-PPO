# 
[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/97554187/149199891-d2601385-1973-4161-b994-ef846059ffc4.png "Atlas Robot"
[vidéo1]: https://gym.openai.com/videos/2019-09-24--SqODtxVFEQ/roboschool:RoboschoolAtlasForwardWalk-v1/original.mp4 "Roboschool"

# Project 1: RoboschoolAtlasforwardWalker-PPO

### Introduction

**Atlas**: Atlas robot is a bipedal humanoid robot, mainly developed by Boston Dynamics. At present, it can complete a series of complex movements such as walking, running and somersaults.

**Roboschool**: Roboschool is a physics simulation engine based on the OpenAI Gym reinforcement learning simulation package. Roboschool ships with twelve environments, including tasks familiar to Mujoco users as well as new challenges, such as harder versions of the Humanoid walker task wich is going to be our environment for this project.

In this project, we will train the Atlas Robot agent to run in a racetrack using the PPO algorithm. 


![Trained Agent][image1]

![Robot Agent][vidéo1]

### Installation

If the environment runs on CPU, use CPU as device for faster training. Box-2d and Roboschool run on CPU and training them on GPU device will be significantly slower because the data will be moved between CPU and GPU often.

```bash
use_cuda = torch.cuda.is_available()
device = torch.device("cuda:0" if use_cuda else "cpu")
```
#### Get the environment

```bash
!pip install roboschool==1.0.48 gym==0.15.4
!pip install box2d-py 
!pip install pybullet

```
 
Now import the roboschoolAtlasforwardWalker environement from OpenAI Gym

```bash
import gym
import roboschool
env = gym.make("RoboschoolAtlasForwardWalk-v1")
```
## Simulation
#### Action space , Observation space
Let's introduce RoboschoolAtlas' action and observation space :
```
print('action space=',env.action_space)
print('observation space=',env.observation_space)
```
Output
```
action space=box(30,)
observation space=box(70,)
```
RoboschoolAtlas environmnet combine action dimension 30 ( the robot has 30 degrees of freedom) , and observation dimention 70 (the observation vector).

The simulation stops when the agent falls down (final state).
The logFile contains reward, episode and timesteps:
*  **Reward** :the reward is computed from various components that make the whole reward function :
 * * alive
 * * progress
 * * electricity_cost
 * * joints_at_limit_cost
 * * feet_collision_cost

*  **Episode** :a sequence of states, actions and rewards, which ends with the fall down of the Atlas agent.

*  **Time steps**:an agent interacts with an environment in (discrete) time steps, which are incremented after the agent takes an action, receives a reward and the "system" (the environment and the agent) moves to a new state. In our case the train stops when reaching a limit time steps.
  
### Train the agent
In order to train our agent we have to:

1. Initialize the agent.
2. Evaluate state and action space.
3. Train the agent using Proximal Policy Optimization (PPO). 
4. Iterate until agent reaches 10 million timesteps.

You can train the agent following the instructions in the notebook RoboschoolAtalsforwardWalker.ipynb

