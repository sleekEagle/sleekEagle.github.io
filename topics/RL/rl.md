---
layout: page
title: Introduction to Reinforcement Learning
permalink: /topics/RL/rl
exclude: true
---

We learn through interacting with environment. An infant plays with environment to learn. Cause and effect. What actions to take to achieve goals. RL analyze this in a computational manner.

RL - maps situations to actions.  The environment give a reward for some ofour actions (mostly this is represented as a scalar).  The learner should discoverwhat  actions  to  take  in  situations  to  maximize  the  reward.   We  do  not  tellwhat actions to take.  The reward might be a delayed one.  i.e it might occurafter  a  long  time  since  the  action  is  taken.   And  the  agent  some  times  takestrial and error approaches.  These two are some of the defining characteristicsof  RL.  The  learning  agent  is  in  the  environment.   It  is  said  to  be  in  a  state.The agent can take actions to change the state.  Also the agent has a goal.  Ittries to achieve this through taking good actions at the given state.  Here I caninclude the catch game and define the states and actions in that.  Why RL isdifferent from supervised learning.  In SL we should give the agent examples ofdesired behaviour.  But in RL, the agent must learn by itself by interacting withthe environment.  The problem of balancing exploration and exploitation.  Theagent  may  not  be  a  complete  organism  like  a  robot.   It  can  be  some  moduleinside a robot for example.  The battery level sensing example.

RL is one of the most general principles of AI.

Exmaple of RL: A robot collecting garbage decides either to go on exploringto find new garbage or to go to the battery charging station.  the decision isbased on how easy it was to find garbage in the past, how far is the chargingstation ...etc.

The agent usually knows when they are achieving its goal.  the robot knowswhen it picks up garbage.  or runs out of battery.  Agent can improve perfor-mance with experience.  interact with the environment and learn.policy - mapping from states to actions.  determines behaviour reward signal -defines the goal.  Agent gets a reward when a good event occurs.  less rewardat a bad event.  the objective of the agent is to maximize the total reward overlong run.  value function - how good or bad to be in a certain state.  takes intoaccount the expected rewards starting from a given state.  This takes into ac-count the likely states that will be visited in the future and the rewards.  rewardsignals are more near sighted and values functions think about the long run.  thereward we get when going to a state may be low.  but if that state is likely tobe followed by other states with high rewards, the value of that state becomeshigh.  one of main problems of RL - how to estimate values of states ?  model -of the environment may help us in planning in RL. a model might give us whichstate we will end up when we take an action starting from a certain state. state - how the environment is at some time.  This depends on the amount of dataavailable for the agent.






