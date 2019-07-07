# Deep_Q_Network_Project

My first DQN

This is an Environment where a small, first person robot is tasked with cruzing around a "field of play" to collect Yellow bananas. The robot collects a point for every Yellow banana it collects. There are also Blue bananas in the "field of play", and if the robot hits one of those I loose a point. I will refer to the Robot as an Agent.

The "field of play" is a State space made of 37 dimensions. With this we can see the Agent's velocity because a 2-dimentional view of whatever the Agent sees in front of it is provided by the Environment.

The Environment contains a brain. The brain holds the policy for the Agent. The brain also receives the Observations and the Rewards from the Environment and returns the Actions for the Agent to take. My Python code will interact with the Environment via this brain.

There are only four Actions that the Agent can take:

- Move forward
- Move backward
- Turn right
- Turn left

The Environment is considered "solved" once the Agent can acquire an average score of +13 over 100 episodes. Just to ensure that I maximize my chances of winning, I continue to train until the average score reaches +15 over 100 episodes, and only then capture the trained weights.

Details on how I went about this process can be seen in my code [here](https://github.com/the-john/Deep_Q_Network_Project/blob/master/Banana_Navigation_Project.ipynb) in a file named "Banana_Navigation_Project.ipynb".  
1. I load the necessary packages
2. I instantiate the Environment and the Agent
3. I train the Agent with the Deep Q-Network
    - The Agent is defined in the Python code [here](https://github.com/the-john/Deep_Q_Network_Project/blob/master/dqn_agent.py) in a file named "dqn_agent.py"
    - The Model is defined in the Python code [here](https://github.com/the-john/Deep_Q_Network_Project/blob/master/model.py) in a file named "model.py"
4. And I save the Deep Q-Network Weights which can be found [here](https://github.com/the-john/Deep_Q_Network_Project/blob/master/checkpoint.pth) in a file named "checkpoint.pth"

Now, anybody can load these Weights and run the Game.  How to do this can be seen at the end of the Python Notebook [here](), under the last section "Now I go ahead and run the trained Agent" at the end of the Notebook.
