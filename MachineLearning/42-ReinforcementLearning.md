# Reinforcement Learning

For some applications such as controlling robots what is required is a mapping from a state, $s$, to an action, $a$, that helps the application achieve its goal. However, for these same applications it is difficult for an expert to provide the exact action, $a$, required for every state, $s$. Therefore supervised learning does not work well in this case.

Reinforcement learning in contrast facilitates the creation of the state to action mapping by providing a reward function that tells the application whether it is doing well or poorly and by how much. Reinforcement learing algorithms use the reward function to learn the appropriate mapping of states to actions in order to achieve the goal of the application.

## The Return

The return in reinforcement learning is the sum of the rewards that the system gets, weighted by the discount factor, where rewards in the far future are weighted by the discount factor raised to a higher power. This reward is calculated by adding all the rewards obtained while moving toward the terminal state, where each reward is multiplied by some power of the choosen discount factor, $\gamma$, (usually a number close to but less than 1.) The discount factor has the effect of tuning the reinforcement learning algorithm toward saving steps or time to the terminal state, as opposed to just caring about the total reward without the discount factor.

![Reinforcement Learning Return](/ReinforcementLearningReturn.PNG 'Reinforcement learning return')

The return depends on the actions taken in any particular state. Below is an example of how the return varies based on the starting state and the action taken.

![Reinforcement Learning Return Example](/ReinforcementLearningReturnExample.PNG 'Reinforcement learning return example')

The fact that the return discounts rewards in the future has an interesting effect when you have systems with negative rewards. If there are negative rewards, then the discount factor incentivizes the system to push out the negative rewards as far into the future as possible.

## Policy

For any system there are usually many possible sets of actions that can be taken in any given set of states. In reinforcement learning, the goal is to come up with a function which is called a policy $\pi$, whose job it is to take as input any state $s$ and map it to some action $a$.

![Reinforcement Learning Policy](/ReinforcementLearningPolicy.PNG 'Reinforcement learning policy')

The goal of reinforcement learning is to find a policy $\pi$ or $\pi(s)$ that tells you what action to take in every state so as to maximize the return.

## Markov Decision Process

The formulism of reinforcement learning can be summarized as learning a policy, $\pi$, from the return that is generated from a set of states, $s$, and a set of actions, $a$, from those states, and wherby there is a reward associated with each state that is discounted by a factor, $\gamma$, when the return is calculated. This formulism is described as a Markov Decision Process. This is a process whereby the future depends only on the current state and not on anything that has occurred previously.

![Markov Decision Process](/MarkovDecisionProcess.PNG 'Markov decision process')

## State Action Value Function

A key component of reinforcement learning algorithms is the state action value function or Q function. This function takes the current state and an action and returns the return if the action is taken, and then afterward the optimal set of actions is taken thereafter. Although this definition of the function may appear circular, reinforcement algorithms resolve this in their implementation.

![State Action Value Function](/StateActionValueFunction.PNG 'State action value function')

Once the Q function has been calculated for each state, it can be used to pick the best action to take. The best action to take from a state $s$ is the maximum value of the Q function at that state.

![Picking Actions](/ReinforcementLearningPickingActions.PNG 'Reinforcement learning picking actions')

If you can compute $Q(s,a)$, for every state and every action, then that gives us a good way to
compute the optimal policy $\pi(s)$.

## Bellman Equation

Another key component of reinforcement learning algorithms is the Bellman equation. 

![Bellman Equation](/BellmanEquation.PNG 'Bellman equation')

The Bellman equation states that the best next action to take from the current state $s$ is the action that maximizes the $Q$ function return for the next state.

![Bellman Equation Example](/BellmanEquationExample.PNG 'Bellman equation example')

![Bellman Equation Explanation](/BellmanEquationExplanation.PNG 'Bellman equation explanation')

The intuition that this captures is if you're starting from state $s$ and you're going to take action $a$ and then act
optimally after that, then you're going to see some sequence of rewards over time. In particular, the return will be computed from the
reward at the first step, plus $\gamma$ times the reward at the second step plus $\gamma^2$ times the reward at the third step, and so on until you get to the terminal state. What the Bellman equation says is this sequence of rewards can be broken down into two components. First, there is $R(s)$, that's the reward you get right away. In the reinforcement learning literature, this is sometimes called the immediate reward, but that's what $R_1$ in the sequence of rewards is. It's the reward you get for starting out in some state $s$. The second term then is the following. After you start in state $s$ and take action $a$, you get to some new state $s'$. The definition of $Q(s,a)$ assumes we're going to behave optimally after that. After we get to $s'$, we are going to behave optimally and get the best possible return from the state $s'$. The maximum of $Q(s',a')$, is the return from behaving optimally, starting from the state $s'$. That is exactly what we had written up here, which is the best possible return for when you start from state $s'$.

## Random (Stochastic) Environment

Because the agent that is implementing a reinforcement learning algorithm might be in a stochastic environment or not always execute a choosen action well, there is a generalization to the Bellman equation that depends on the expected value (average) of the returns for a given state and action.

![Reinforcement Learning Stochastic Expected Return](/RLExpectedReturn.PNG 'Expected return when reinforcement learning environment is stochastic')

In the generalized Bellman equation it is the maximum expected return for the the next state and action that is choosen.

![Bellman Equation Stochastic](/BellmanEquationStochastic.PNG 'Bellman equation stochastic')

## Continuous State Space

Many applications, such as controlling a robot, require modelling the state as a set of continuous values, as oppposed to a set of discrete values. So in a reinforcement learning problem or a Markov decision process that involves continuous states, the state of the
problem isn't just one of a small number of possible discrete values, like a number from 1-6. Instead, it's a vector of numbers, any of which could take any of a large number of values.

![Continuous State Space](/ContinuousStateSpace.PNG 'Continuous state space')

## Learing The State Action Value Function (Q)

To learn the state action value function or $Q(s,a)$ one method is to train a neural network to calculate the value of $Q(s,a)$ for every possible action in a particular state and then choose the action that maximizes $Q(s,a)$. Note that in the example below, the input vector is a set of continuous state values and a set of values for representing the action, where the actions have been one-hot encoded.

![Deep Reinforcement Learning](/DeepReinforcementLearning.PNG 'Deep reinforcemen learning')

In order to train the nerual network a training set of state and action vectors will be needed. Then using this training set supervised learning can be used to train the neural network. The Bellman equation can be used to create an artificial training set of state and action vectors, $x$ and target outputs $y$.

![Bellman Equation Training Set](/BellmanEquationTrainingSet.PNG 'Using Bellman equation to create deep reinforcement learning training set')

To generate a (artificial) training set using the Bellman equation, take a bunch of random actions and calculate the result of those actions using the Bellman equation. The state and the action taken in that state is the vector $x$ and the result calculated using the Bellman equation is the target $y$. Since the left term, $Q(s',a')$ in the Bellman equation is unknown, a random guess will be used and this will turn out to work fine in the learning algorithm.


![Deep Reinforcement Learning Algorithm](/DeepReinforcementLearningAlgorithm.PNG 'Deep reinforcement learning algorithm')

The values for $Q(s,a)$ and $Q(s',a')$ are initialized to random guesses. After repeated training sessions these guesses should get better and result in better application performance. However, it might take a long time for the algorithm to converge. This algorithm is sometimes refered to as the DQN algorithm.

## Improved Deep Reinforcement Learning Architecture

![Deep Reinforcement Learning Better Architecture](/DeepReinforcementLearningBetterArch.PNG 'Deep reinforcement learning better neural network architecture')

The original neural network architecture took as input all of the state values and a one-hot encoded set of values for the action and outputted a value for $Q(s,a)$ for the single action encoded in the input. The new architecture above only takes as input the state values and outputs the value of $Q(s,a)$ for all values of $a$. This is much more efficient as the inference for all possible actions in a particular state $s$ is only done once and it is easy to pick the maximum value from the output. This also makes it easier to calculate the Bellman equation.

## $\epsilon$-greedy Policy

One of the steps in the deep reinforcemen learning algorithm is to take an action. In general there are 2 options on how to choose which action to take. As it turns out always choosing the action that maximizes $Q(s,a)$ often does not work well. The reason is that the intial values of $Q(s,a)$ are set to random guesses. If a value for $Q(s,a)$ just so happens to be set low for some action $a$, then it the algorithm might never pick that action to take, even though it could be the best action to take for some particular state. To overcome this, the other option is to sometimes randomly choose an action to take. The value that determines how often to randomly choose an action is called $\epsilon$. One variation to using a constant value of $\epsilon$ is to start with a very low value and then gradually increase the value as the algorithm progresses.

![Epsilon Greedy Policy](/EpsilonGreedyPolicy.PNG 'Epsilon greedy policy')

## Mini-Batch And Soft Updates

Another refinement that works for both supervised learning and reinforcement learning is to use mini-batch instead of all of the training examples at once during the learning process. In the supervised learning case, if the number of training examples is very large then for each iteration of the gradient descent algorithm the mean squared error needs to be calculated over all of the training examples which can be computationally very expensive. Instead what can be done is that for each iteration only a small subset of the training examples are used.

![Mini-Batch Motiviation](/MiniBatchMotiviation.PNG 'Mini-batch refinement motivation')

![Mini-Batch Examplke](/MiniBatchExample.PNG 'Mini-batch refinement example')

Unlike regular batch gradient descent, mini-batch gradient descent will not smoothly head toward a global minimum. Instead it will take a much more irregular path and may not get as close to a global minimum as batch gradient descent. However, for very large training sets, mini-batch on average does pretty well at getting close to a global minimum.

![Mini-Batch Gradient Descent](/MiniBatchGradientDescent.PNG 'Mini-batch gradient descent')

Applying the mini-batch technique to reinforcement learning means that instead of learning on the entire set of training examples generated (using the Bellman equation) a small subset is used for each iteration.

![Mini-Batch Reinforcement Learing](/MiniBatchReinforcementLearning.PNG 'Mini-batch reinforcement learning')

As it turns out the mini-batch refinement helps to make the reinforcemen learning algorithm run faster.

Another refinment is to update the paramenters of the deep reinforment learning neural network more gradually than in the original algorithm which updated them to the new learned values all at once. The problem with updating the parameters completely to the new learned parameters is that by chance the new neural network may be worse than the previous one. Therefore what can be done is a soft update which means adding a small weighted value of the new parameter to the old parameter value that is weighted more heavily. As it turns out this soft update refinement makes it more likely that the reinforcement learning algorithm will converge instead of oscillating or diverging.

![Reinforcement Learning Soft Update](/ReinforcementLearningSoftUpdate.PNG 'Soft update of neural network parameters in reinforcement learning')

## State Of Reinforcement Learning

For some applications such as robotic control reinforcement learning may be appropriate. However, it tends to work better in simulations than in the real world.

![Limitations Of Reinforcement Learning](/LimitationsReinforcementLearning.PNG 'Limitations of reinforcement learning')


























