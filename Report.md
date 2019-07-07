
# Banana Navigation Project Report

This Report provides a description of the Banana Navigation Project implementation.  

This is an Environment where a small, first person robot is tasked with cruzing around a "field of play" to collect Yellow bananas. The robot collects a point for every Yellow banana it collects. There are also Blue bananas in the "field of play", and if the robot hits one of those I loose a point. I will refer to the Robot as an Agent.

The "field of play" is a State space made of 37 dimensions. With this we can see the Agent's velocity because a 2-dimentional view of whatever the Agent sees in front of it is provided by the Environment.

The Environment contains a brain. The brain holds the policy for the Agent. The brain also receives the Observations and the Rewards from the Environment and returns the Actions for the Agent to take. My Python code will interact with the Environment via this brain.

There are only four Actions that the Agent can take:

- Move forward
- Move backward
- Turn right
- Turn left

The Environment is considered "solved" once the Agent can acquire an average score of +13 over 100 episodes. Just to ensure that I maximize my chances of winning, I continue to train until the average score reaches +15 over 100 episodes, and only then capture the trained weights.


### The Learning Algorithm

The learning algorithm here is a Deep Q-Network (DQN). Naive Q-Networks suffer from stability and short-term memory.  But, if we utilize Fixed Q-Targets and Experience Replay, we can take things to a whole other level.  

Experience Replay randomizes the data.  By doing this, it removes correlations and smooths changes in the data distribution.

During learning, we apply Q-Learning updates on samples (or mini-batches) of experience, drawn uniformly at random from the pool of stored samples.

With a DQN we:
- Use stochastic gradient descent
- Use a policy that selects Actions uniformily at random
- Then include:
    - Replay Memory
    - Separate Target Q-Network
    - Deep Convolutional Network Architecture

There are two main processes that are interleaved into this algorithm:
1. Sample
    - we sample the environment by performing actions  
    - we store away the observed experience tuples in a weak link memory
2. Learn
    - select a small batch of tuples fro this memory randomly
    - learn from that batch using a gradient descent update step
    
And the rest of the algorithm does these steps:

- In the beginning you need to initialize an empty weakly linked memory.  Remember, memory is finite, so use a circular queue that retains the "n" most recent tuples.
- You need to initialize the parameters or weights of your neural network.
    - One common way is to sample the weights randomly from a normal distribution with weigh-ins equal to two by the number of inputs to each neuron (a common thing in Keras and TensorFlow)
- To use the Fixed Q-Targets Technique; You need a second set of parameters "w-" which you can initialize a set of parameters "w-" which you can initialize to "w".
- Because the original was meant to work with video games:
    - For each episode and each time step t within that episode, you observe a raw screen image or input frame x(t), that you need to convert to grey scale and crop to a square size, etc.
- To capture temporal relationships, you can stack a few input frames to build each state vector.
- φ = the pre-processing and stacking operations mentioned in the two steps above.
    - Takes a sequence of frames and produces a combined representation
    - For the first frame set, we don't have any previous frames to use.  So we can either leave them blank, or make copies of the first frame.  Or, we can skip storing experience tuples until we get a complete sequence.
        - In practice, you won't be able to run the learning step immediately anyway
- We do NOT clear out the memory after each episode.  This allows us to build batches from across episodes.
- Other techniques and optimizations can be used such as Reward Clipping, Error Clipping, storing past actions as part of the state vector, dealing with terminal states, decaying Epsilon over time, etc.


### The Chosen Hyperparameters

I didn't spend any time playing with the hyperparameters on this project.  I pulled an Agent and associated Model from a previous OpenAI project called TaxiCab.  I started out with the exact same parameters that where used before, and BOOM, my Banana Navigation solved.  Soooooo ... if it aint broke, don't fix it.  These hyperparameters include:

- Number of Episodes = 1000
- Number of game plays in a training episode = 1000
- Epsilon Start = 1.0
- Epsilon End = 0.01
- Epsilon Decay Rate = 0.995


### The Model Architecture

The details of the model architecture can be seen in the Python code "model.py" located [here](https://github.com/the-john/Deep_Q_Network_Project/blob/master/model.py).  I'm using an Actor (Policy) Model for my Q-Network. It consists of three Fully Connected (FC) networks where the first two utilize ReLU functions.  The environment states come into the first FC layer which consists of 64 nodes, and connects to the second FC layer that also has 64 nodes.  The final FC layer brings the output nodes down to the number of actions that the Agent can take.  In this case, the number of aactions is four (left, right, forward, backward).   


### Rewards per Episode Plot

The figure below shows the scores during training.  Included is a line that shows the average of the scores over episodes.  I was able to "solve" the environment in 572 episodes.  A "solved" environment is defined as reaching an average score of +13.  But I went ahead and continued training the Agent until I got to an average score of +15, just to add some buffer in my trained Agent's ability to play.  I was able to reach a score of +15 in 740 episodes.

![image.png](https://github.com/the-john/Deep_Q_Network_Project/blob/master/Learning_Rate.png)

Once the Agent was trained, I saved its Weights in the "checkpoint.pth" file which can be found [here](https://github.com/the-john/Deep_Q_Network_Project/blob/master/checkpoint.pth).  Training the Agent up until it averaged a score of +15 ensured that I never dipped below the requirment of scoring +13, as can be seen in the plot below.  The "mean score" is the average score of the trained Agent over 15 episodes of play.

![image.png](https://github.com/the-john/Deep_Q_Network_Project/blob/master/Trained_Score.png)


### Ideas on Agent Improvement

There are a couple of things that I can do to increase performance.  

##### Enchance the Model and Hyperparameters

I can spend a LOT of time developing alternative model architectures to help improve my performance.  I can also write some "automating" code that can run training sessions through a multitude of hyperparameter settings to see which settings have the best impact.

##### Enhance the Q-Network

There are six published baselines that I can incorporate into this Deep Q-Network (DQN) to improve the Agent.  Each of these has the potential of enhancing performance.  The UBER setup would be to incorporate all six.  Perhaps for another time.  These options include:

- Double Deep Q-Network (DDQN)
- Prioritized Double Deep Q-Network
- Dueling Double Deep Q-Network
- Asynchronous Advantage Actor-Critic (A3C)
- Distributional Deep Q-Network
- Noisy Deep Q-Network


