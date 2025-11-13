# Solving a Real-World Problem Using Reinforcement Learning

## Project Description

The project implements a reinforcement learning (RL) algorithm to solve a real-world inspired control task. 
A warehouse robot must move from a loading bay to a target shelf while avoiding hazards (holes). The robot moves on a slippery surface. Therefore, it cannot rely on a fixed plan, as the robot’s movement can slip unpredictably. 

The project uses the FrozenLake-v1 environment from Gymnasium, which simulates the slippery grid world. The goal is for the robot to learn a safe and efficient route over the course of multiple episodes.

## Environment

**The FrozenLake-v1 from Gymnasium is used to train the warehouse robot:** Gymnasium is a Python library for Reinforcement Learning (RL) environments: https://gymnasium.farama.org/environments/toy_text/frozen_lake/


**Actions -** The robot can take 4 actions:
| Action | Description      |
|--------|------------------|
| 0      | Move left        |
| 1      | Move down        |
| 2      | Move right       |
| 3      | Move up          |


**Example Initialization (4x4 Grid):**
```
env_4 = gym.make(
        "FrozenLake-v1",
        is_slippery = True, 
        render_mode ="rgb_array",
        success_rate=1.0/3.0,
        reward_schedule=(1, 0, 0),
        desc = generate_random_map(
            size = 4, p = 0.9, seed = 123 
        ),
    )
```

Key Parameters:
- ```is_slippery = True```: Robot actions may not always go as intended (slippery surface).
- ```p = 0.9```: Probability that a cell is frozen/safe.
- ```size = 4```: Grid size (4×4 -> 16 total cells).
- ```success_rate=1.0/3.0```: probability of moving as intended. 
- ```reward_schedule=(1, 0, 0)```: 
    - Reach Goal: +1
    - Reach Hazard: 0
    - Reach Frozen: 0 


**Example 4x4 Map:**
```
S F F F
F F H F
F F F F
F F F G
```
- S: Starting cell. 
- F: Frozen cell, where robot can move.
- H: Hazard cell, episode terminates.
- G: Goal cell.

Each episode starts with the agent in state S ([0,0]). For a 4x4 environment, there are 16 possible states. 
The goal is to reach the state G ([3,3] for 4x4 environment).

**Episode Termination:**
- Termination: 
    - Robot moves into a Hazard.
    - Robot reaches the Goal.
- Truncation: Time Limit reached (100 episodes for FrozenLake4x4, 200 for FrozenLake8x8).


## Notebook/ Implementation Overview

The Notebook contains the following sections:

### 1. Initialize and Explore Three FrozenLake-v1 Environments

 Environments: 4x4, 6x6, 8x8 FrozenLake-v1 maps:    
- ```is_slippery = True```: Robot's moves may not go as intended. 
- ```p = 0.9 ```: Probability that a tile is frozen.
- Fixed seed for reproducibility.
- Default time limits, default success rate, default rewards.

This section plots the initial state of these environments.



### 2. Reinforcement Learning Algorithm
The project uses the WarehouseRobot class to implement Q-Learning and SARSA,

Methods:
1. Initialization: 
    - Hyperparameters:
        - Learning Rate: Influences the speed of the learning process. Range (0,1).
        - Discount Factor: Balances the importance of future rewards relative to immediate rewards. Range (0, 1).
        - Start Epsilon, Final Epsilon, Epsilon Decay: Influences the policy/ actions.
    - Type of Learning: Q Learning or SARSA. Default value is "Q Learning".

2. Reset Q Table: 
    - Q Table stores expected rewards for (state, action) pairs. Size: (number of states, number of actions).
    - Initialized to 0 (for each cell).

3. Action Selection - Uses Epsilon-Greedy policy:
    - With probability Epsilon, pick random action: exploration.
    - Otherwise, pick action with highest Q value: exploitation.

4. Decay Epsilon:
    - Linear of exponential decay. 
    - Ensures exploration at the beginning of a run, and exploitation later on.

5. Update Q Values:
    - Q Learning: 
        - Off-policy learning: update Q values using the maximum possible reward in the next state.
        - Update rule: Q(s,a) = Q(s,a) + alpha*(r + gamma*max Q(s',a') - Q(s,a))
            - Where :
                    - Q(s,a): current Q value for (state, action) pair
                    - alpha: learning rate
                    - r: immediate reward received
                    - gamma: discout factor for future rewards
                    - max Q(s',a'): maximum expected reward in the next state
    - SARSA:
        - On policy learning: update Q values based on the robot's next state and action.
        - Update rule: Q(s,a) = Q(s,a) + alpha*(r + gamma*Q(s',a') - Q(s, a))
            - Where:
                    - Q(s,a): current Q value for (state, action) pair
                    - alpha: learning rate
                    - r: immediate reward received
                    - gamma: discout factor for future rewards
                    - Q(s',a'): Q value for the (next state, next action) pair

6. Run Method:
    - Main method of the class. 
    - Resets environment to initial state for each episode.
    - Tracks total rewards, states, actions and steps. 



### 3. Helper Functions
- ```run_algorithm```: Run the RL algorithm over multiple runs and episodes. 
- ```plot_rewards```: Plot mean cumulative rewards and standard deviation. Plot success rate. Print mean successs rate and standard deviation.
- ```plot_steps```: Plot steps per episode with standard deviation.
- ```plot_state_action_dist```: Show distribution of states visited and actions taken.
- ```plot_qval_map```: Visualize last state of environment and final Q-values as a heatmap with arrows indicating the best actions.

### 4. Experiments
For each environment (4x4, 6x6, 8x8): 
1. **Baseline Q Learning:**
    - Learning rate: 0.1
    - Discount factor: 0.95
    - Initial Epsilon: 1.0
    - Final Epsilon: 0.1
    - Type of Epsilon Decay: Linear
    - Number of runs: 20
    - Number of episodes: 10 000
    - Epsilon Decay Factor = Initial Epsilon / Number of Episodes / 2

2. **Slippery Rate Variation:**
    - Change the slippery probability to 3/4 to test different dynamics.

3. **Deterministic (No Slippery):**
    - Set ```is_slippery``` to ```False``` to compare with performance in deterministic environment.

4. **Hazard Rate Variation:**
    - Change p (probability of frozen tile) to 0.8 for more hazards.

5. **Exponential Epsilon Decay:**
    - ```epsilon_decay = (epsilon_final / epsilon_init) ** (2 / episodes_num)```

6. **Test Hyperparameter Combinations:**
    - ```discount_factors = [0.1, 0.5, 0.95]```
    - ```learning_rates = [0.01, 0.1, 0.5]```
    - Linear decays: 
        - ```epsilon_decays = [epsilon_init / (episodes_num / 2), epsilon_init / (episodes_num)]```
    - Exponential decays: 
        - ```epsilon_decays = [(epsilon_final / epsilon_init) ** (2/episodes_num), (epsilon_final / epsilon_init) ** (1/episodes_num)]```

### 5. Random Policy Experiment
For each environment, agent chooses action at random.

### 6. Simple heuristic/ Deterministic Plan
For each environment:
- A deterministic plan has been constructed.
- Show each environment state, for each planned step, on non-slippery environment.
- Show each environment state, for each planned step, on slippery environment.

This section shows why planning does not work in non-deterministic environments.

### 7. SARSA Experiments
For each environment, test linear and exponential decay with SARSA update rule.

### 8. Improve More Hazarduous Environments with SARSA
Since Q Learning struggled in 6x6 and 8x8 environments, this section uses SARSA and exponential epsilon decay.

For the 6x6 map, switching to SARSA was sufficient.

For the 8x8 environment, the reward structure and the number of episodes were also adjusted.


# How to Run
1. Open the "Reinforcement_Learning.ipynb" notebook.
2. Select Run All to execute all cells.

**Python version:** 3.12.4

**Libraries used:**
- matplotlib
- numpy
- seaborn
- pandas
- gymnasium
- itertools

**How to install the libraries:**
- pip install matplotlib numpy seaborn pandas gymnasium

