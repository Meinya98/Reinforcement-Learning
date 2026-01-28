# Lunar Lander, Comparing DQN and DDQN
A reinforcement learning exercises to compare the result of Deep Q-Network and Double Deep Q-Network. Environment used was from OpenAI's Gymnasium.
## Environment Description
The agent's observation space consist of a state vector with 8 variables:
1. _(x,y)_ coordinates. The landing pad is at _(0,0)_.
2. Linear velocities _(ẋ,ẏ)_.
3. Angle _θ_.
4. Angular velocity _θdot_.
5. Booleans to represent whether each leg is in contact with the ground or not (_l_ and _r_).
\
Each action taken by the agent has a corresponding value:
1. Do nothing = 0
2. Fire right engine = 1
3. Fire main engine = 2
4. Fire left engine = 3
\
After every step, a reward is granted for the agent. The total reward of an episode is the sum for all the steps within that episode (already stated within the environment):

1. +100 for landing safely.
2. -100 for crashing.
3. -0.3 for each frame the main engine is firing.
4. -0.03 for each frame the side engine is firing.
5. +10 for each leg in contact with the ground.
6. Further decreased the more the lander is tilted.
7. Further increased/decreased if it took too long.
8. Further increased/decreased the closed/further the lander is to the landing pad.

## Workflow
Workflow: 
1. Import packages
2. Setting up the environment
3. Setting up replay buffer
4. Setting up, train, and evaluate the DQN agent
5. Setting up, train, and evaluate the DDQN agent
6. Compare both

## Conclusion
### **1. DQN was an essential first step**
Deep Q-Networks enable reinforcement learning in high dimensional and continuous state spaces by introducing 3 critical mechanisms:
1. Experience replay -> decorrelate samples
2. Target networks -> stabilize DQN learning by separating to learning network (updated constantly) and target network (updated every xxx step)  
3. ϵ greedy exploration -> balance exploration and exploitation

These mechanisms allow DQN to learn meaningful control policies. However, during longer training, it was observed that DQN:
1. Exhibits high variance in performance
2. Collapse and worsen after early success spike

Why? Because in DQN, the target value is overestimated. Regular DQN uses the same network for both choose which action is best AND estimate how good that action is. This overestimation bias can lead to:
1. Picking actions with noisy Q-value estimates
2. Overconfident but unstable (learning bad over time)

### **2. DDQN to overcome the DQN limitations**
Double Deep Q-Networks fixes this by splitting the network:
1. Main network -> picks which action looks best
2. Target network -> evaluate how good that action actually is (imagine having a second opinion), reducing overestimation bias

### **3. Role of hyperparameter adjustments**
The goal is to stabilizing the learning process, rather than speeding up:
- Slower ϵ decay -> ensure sufficient exploration before exploitation
- Higher minimum ϵ -> prevent premature convergence
- Less frequent target network update -> stabilizing
- Training every N steps -> reduce noise
- Huber loss and value clipping -> prevent extreme updates (MSE => square average => extreme score)

As the environments become more complex, stability is more important than speed.

Lunar lander environment => complex physics
