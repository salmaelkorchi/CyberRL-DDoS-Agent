# Introduction : 

Network attack detection is crucial to ensure the security of computer systems. With the increasing sophistication of attackers and the diversity of threats, it has become imperative to develop advanced detection mechanisms to protect networks against malicious intrusions. Among the many forms of attacks, Distributed Denial of Service (DDoS) attacks are one of the most concerning threats, as they can cripple online infrastructures by overwhelming servers with malicious requests. In this context, the use of techniques such as Reinforcement Learning (RL) for network attack detection holds particular importance.

In this report, we present the application of a Reinforcement Learning (RL) model for network attack detection, focusing on identifying DDoS attacks. RL offers a promising approach to this problem by enabling an agent to learn to make sequential decisions by interacting with an environment. Unlike traditional methods of attack detection based on signatures or predefined rules, RL allows the agent to acquire knowledge about network behavior over time, enabling it to adapt to complex and evolving attack scenarios.

The application of RL to network attack detection has several advantages, including its ability to handle dynamic environments and adapt to unknown attacks. By modeling the detection process as a reinforcement learning problem, we can enable the agent to make informed decisions based on rewards received for its actions, allowing it to develop effective strategies to identify and counter attacks. This approach also offers some flexibility, as it can be tailored to different types of attacks and network topologies.

In the following sections, we will detail the implementation of our RL-based attack detection environment, as well as the agent training process. We will then present the results of our experiments and discuss the performance of our model, before concluding with perspectives on future developments in RL-based network attack detection.

# Labellisation des données : 

In the context of data labeling, the initial dataset consisted of 7 columns and 9999 rows. These columns were composed of the following features:

1. Source IP
2. Source Port
3. Destination IP
4. Destination Port
5. Timestamp
6. Total Forward Packets
7. Total Backward Packets

To enrich this dataset, we added three new features:

- Flow IAT Mean: the mean of inter-arrival times between packets in the flow.
- Flow IAT Max: the maximum value of inter-arrival times between packets in the flow.
- Label: This feature is crucial for training the detection agent. It divides the data into two distinct classes:

  * Portman: Indicates observations associated with a simulated DDoS attack.
  * Benign: Corresponds to observations that do not pertain to an attack in the simulated environment.

The correct assignment of labels is essential to enable the agent to distinguish DDoS attacks from normal network activities. This labeling thus facilitates the training of the model and improves its ability to detect and respond effectively to threats.

# Creating the Environment :

We created a simulation environment using the OpenAI Gym library to simulate DDoS attacks. The environment, called DDoSEnv, takes a dataset as input during initialization. This dataset must contain information about network traffic, including source IP addresses and labels indicating whether the traffic is normal or part of a DDoS attack.

The environment has a discrete action space with two possible actions:
- 0 for normal
- 1 for DDoS
The observation space is a continuous vector of length 11, representing the one-hot encoded source IP address of the current traffic.

The reset method sets the current step to 0, indicating the start of a new episode, and returns the first observation from the dataset.

The _next_observation method creates the observation by extracting the source IP address from the current data point and converting it into a one-hot encoded vector.

The step method increments the current step, calculates the reward based on the action and label of the current data point, and returns the next observation, the reward, and a boolean indicator indicating whether the current episode is finished or not.

The result shows that the observation space is a continuous vector of length 11 with values ranging from 0 to 1, and the action space is a discrete space with two possible actions.

This means that the agent can perform one of the two following actions, and the observation it receives is a vector of 11 values representing the one-hot encoded source IP address. The reward is calculated based on the action and label of the current data point.

# Integration of Reinforcement Learning:

In this code, we have implemented an agent using the Q-learning algorithm for learning in a gym-like environment, specifically designed for DDoS attack detection.

In the `__init__` constructor, the environment is initialized with the provided dataset. Action and observation spaces are defined, where the action space is discretized into two options: normal or DDoS, and the observation space is an 11-dimensional box.

The `reset` method is used to reset the environment at the beginning of each new episode. It returns the initial observation from the `_next_observation` method.

The `_next_observation` method retrieves the current observation from the dataset, encodes relevant features, and returns the observation.

The `step` method is used to perform an action in the environment. It compares the action taken with the correct action defined in the dataset to calculate the reward. It also updates the current state and returns the next state, the reward, an indication of termination, and additional information. Then, the code loads the dataset from a CSV file and creates an instance of the DDoSEnv environment.

The parameters of the Q-learning algorithm are defined, including the learning rate (`alpha`), the discount factor (`gamma`), and the initial exploration rate (`epsilon`).

A Q-table is initialized with zero values for each possible state-action in the environment. Training parameters such as the number of episodes and the maximum number of steps per episode are defined.

The Q-learning algorithm is then executed for the specified number of episodes. For each episode, the agent interacts with the environment, takes actions based on its policy (exploration or exploitation), updates the Q-table, and adjusts the exploration rate.

At the end of training, a message indicates that the training is complete, and episode statistics are printed to track the agent's performance over time.

# Training the agent with RL:

The results below indicate that the agent learns and improves over episodes by adjusting its action policy through exploration and exploitation of information stored in the Q-table.

The decrease in the exploration rate reflects a transition towards a policy more focused on exploiting acquired knowledge.

Here is an explanation of three episodes:

* Episode 1: The agent achieved a total reward of 104 in the first episode, with an exploration rate of 0.99.
* Episode 2: Improvement is observed with a total reward of 198, while the exploration rate decreases slightly to 0.9801.
* Episode 3: The total reward is negative (-38), indicating a less satisfactory performance for this episode. The exploration rate continues to decrease to 0.9702989999999999.

# Conclusion:

In conclusion, our study demonstrates that the application of Reinforcement Learning (RL) to network attack detection holds significant potential. While our model has shown promising results, especially in effectively identifying DDoS attacks, some challenges persist. Among these challenges, managing the balance between exploration and exploitation as well as designing adequate state representations remain crucial aspects to address to improve the performance and generalization of our model.

Our work paves the way for new possibilities in the field of cybersecurity, offering an innovative and adaptive approach to address cyber threats. By combining RL principles with the unique challenges posed by network attack detection, we are able to design smarter and more resilient security systems capable of adapting to the constantly evolving cyber threat landscape.

# Perspectives:

To further advance, we plan to explore advanced Reinforcement Learning (RL) techniques, including the use of deep neural networks (Deep Q-Networks), to handle complex state spaces. Deep neural networks have demonstrated their ability to capture highly nonlinear data structures and generalize models from complex input data. By integrating these neural architectures into our network attack detection system, we may be able to better process information from diverse sources and detect more subtle and evolving attack patterns.

Additionally, implementing online learning policies could enhance the responsiveness and efficiency of our network attack detection system. Rather than relying solely on static datasets for training, online learning policies allow our system to continue learning and improving as it interacts with its environment. This could be particularly beneficial in scenarios where attacks evolve rapidly and new behavioral patterns need to be quickly identified and adapted to.

By embracing these perspectives and continuing to explore new techniques and approaches in the field of Reinforcement Learning applied to network attack detection, we may be able to develop more robust, responsive, and adaptive cybersecurity systems capable of addressing the increasing complexity of cyber threats.
