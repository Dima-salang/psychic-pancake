# Introduction to Reinforcement Learning
## Chapter 1 Lecture: Foundations of Learning from Interaction

---

## 📚 Lecture Overview

**Welcome to Reinforcement Learning!** Today, we embark on a journey into one of the most fascinating areas of artificial intelligence—learning from interaction to achieve goals. This lecture introduces you to the fundamental concepts that make reinforcement learning unique and powerful.

**Learning Objectives:**
- Understand what distinguishes reinforcement learning from other machine learning paradigms
- Grasp the exploration-exploitation dilemma—a fundamental challenge in RL
- Learn the four key elements of any reinforcement learning system
- Understand the mathematical foundation through value functions and temporal difference learning
- Appreciate the rich history and interdisciplinary nature of RL

---

## 1. What is Reinforcement Learning?

### The Core Definition

**Reinforcement Learning** is a computational approach to learning from interaction to achieve a goal. Unlike other machine learning methods, RL is defined by **what** is learned rather than **how** it is learned.

> **Key Insight:** In reinforcement learning, an agent learns to map situations to actions by trying them and observing the outcomes, with the goal of maximizing a numerical reward signal.

### The Two Distinguishing Features

Reinforcement learning is characterized by two essential features that separate it from all other approaches to learning:

#### **1. Trial-and-Error Search**

The agent is **not told** which actions to take. Instead, it must discover which actions yield the most reward by trying them and seeing what happens. This is fundamentally different from:

- **Supervised Learning:** Where a teacher provides explicit labels or correct answers
- **Unsupervised Learning:** Where the goal is to find hidden structure in data

#### **2. Delayed Reward**

In RL, the consequences of an action are not always immediate. An action taken now may affect not only the immediate reward but also the next state and, through that, all subsequent rewards down the line.

> **Mathematical Representation:** The agent's goal is to maximize the expected cumulative reward:
>
> $$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$
>
> Where:
> - $G_t$ is the **return** (total discounted reward from time $t$)
> - $R_{t+k}$ is the reward received at time step $t+k$
> - $\gamma$ is the **discount factor** ($0 \leq \gamma \leq 1$), which determines how much we value future rewards

---

## 2. The Exploration-Exploitation Dilemma

### The Fundamental Trade-off

One of the most critical challenges in reinforcement learning is balancing **exploration** with **exploitation**. This is not just a technical detail—it's a fundamental problem that every RL system must solve.

#### **Exploitation**
- **Definition:** Choosing the action that the agent believes will yield the highest immediate reward
- **Metaphor:** Going to your favorite restaurant because you know the food is good
- **Risk:** You might miss out on discovering an even better option

#### **Exploration**
- **Definition:** Trying a new or suboptimal action to gather more information about the environment
- **Metaphor:** Trying a new restaurant to see if it might be better than your favorite
- **Risk:** You might waste time and receive lower rewards temporarily

### The Mathematical Challenge

The dilemma can be formalized as follows:

$$\max_{\pi} \mathbb{E}_\pi\left[\sum_{t=0}^{\infty} \gamma^t r_t\right]$$

Where we must learn the optimal policy $\pi^*$ while simultaneously exploring the environment to gather the data needed to estimate it.

> **Critical Point:** If we always exploit, we might get stuck in a local optimum. If we always explore, we never reap the benefits of what we've learned. The art of RL is finding the right balance.

### Example: The Casino Bandit Problem

Imagine you're at a casino with 10 slot machines (often called **multi-armed bandits**). Each machine pays out with a different probability, but you don't know what those probabilities are.

- **Exploitation:** Pull the lever of the machine that has paid out the most so far
- **Exploration:** Try other machines to see if they might have higher payout rates

**Question:** How do you maximize your total winnings over 1000 pulls?

This simple problem captures the essence of the exploration-exploitation dilemma and will be our first detailed case study in the next chapter.

---

## 3. The Three Paradigms of Machine Learning

Reinforcement learning is not just another machine learning technique—it represents a **third paradigm**, fundamentally different from the other two:

### **1. Supervised Learning**
- **Learning from a teacher:** The agent is provided with labeled examples (input-output pairs)
- **Passive:** The agent doesn't interact with the environment; it just observes and learns patterns
- **Limitation:** Can't handle "uncharted territory" where no training data exists

**Example:** Classifying images as cats or dogs using labeled training images

### **2. Unsupervised Learning**
- **Finding structure:** The goal is to discover hidden patterns or structures in unlabeled data
- **No objective feedback:** There's no correct answer or reward signal
- **Goal:** Clustering, dimensionality reduction, generative modeling

**Example:** Grouping customers into market segments based on purchasing behavior

### **3. Reinforcement Learning (Our Focus)**
- **Learning from interaction:** The agent actively interacts with the environment
- **Goal-directed behavior:** There's a clear objective: maximize cumulative reward
- **Trial-and-error:** The agent must discover good actions through experimentation
- **Delayed consequences:** Actions affect not just immediate rewards but all future rewards

**Key Distinction:**
> "Supervised learning is like learning from a textbook. Reinforcement learning is like learning to ride a bicycle—you have to try it, fall down, get back up, and figure out what works through direct experience."

---

## 4. Elements of a Reinforcement Learning System

Every reinforcement learning system can be understood in terms of four key sub-elements:

### **1. Policy (π)**
The policy is the agent's **strategy**—the mapping from perceived states to actions. It defines the agent's behavior at a given time.

**Mathematical Definition:**
$$\pi(a|s) = P(A_t = a | S_t = s)$$

Where:
- $\pi(a|s)$ is the probability of taking action $a$ in state $s$
- For deterministic policies: $\pi(s) = a$ (one specific action per state)

**Types of Policies:**
- **Deterministic:** Same action always chosen for a given state
- **Stochastic:** Actions chosen according to a probability distribution

**Analogy:** Think of the policy as your "rules of thumb" or habits—what you do in different situations.

---

### **2. Reward Signal (r)**
The reward signal is the **goal** of the problem—a single number sent by the environment at each time step that tells the agent how well it's doing.

**Mathematical Formulation:**
$$R_t = \text{reward received at time step } t$$

**Key Properties:**
- **Primary basis for learning:** All changes to the policy are ultimately motivated by maximizing total reward
- **Immediate feedback:** Rewards are provided at each time step
- **Design choice:** In many problems, designing the right reward function is more art than science

> **Critical Insight:** The reward signal is what defines the agent's objective. If you want your agent to do something, you must define a reward function that makes that behavior rewarding.

**Example:**
- **Training a dog:** Treats and praise are the reward signal
- **Playing chess:** Winning (+1), losing (-1), drawing (0) might be the rewards
- **Robot navigation:** +1 for reaching goal, -0.01 per time step (to encourage efficiency)

---

### **3. Value Function (V)**
The value function is arguably the **most important** concept in reinforcement learning. It specifies what is good in the long run.

**Definition:** The value of a state is the total amount of reward an agent can expect to accumulate over the future, starting from that state.

**Mathematical Definition:**
$$v_\pi(s) = \mathbb{E}_\pi\left[G_t | S_t = s\right]$$

Where:
- $v_\pi(s)$ is the **state-value function** under policy $\pi$
- $G_t$ is the return (cumulative discounted reward) starting from time $t$
- The expectation is taken over all possible trajectories following policy $\pi$

**State-Value vs. Action-Value:**

1. **State-Value Function** $v_\pi(s)$: How good is it to be in state $s$?
   $$v_\pi(s) = \mathbb{E}_\pi\left[\sum_{k=0}^{\infty} \gamma^k R_{t+k+1} \mid S_t = s\right]$$

2. **Action-Value Function** $q_\pi(s,a)$: How good is it to take action $a$ in state $s$?
   $$q_\pi(s,a) = \mathbb{E}_\pi\left[\sum_{k=0}^{\infty} \gamma^k R_{t+k+1} \mid S_t = s, A_t = a\right]$$

> **Fundamental Insight:** **Rewards are immediate; values are foresight.** Rewards are like immediate pleasure or pain. Values are like wisdom—they represent the long-term consequences of being in a state or taking an action.

**Example:** In chess, moving your queen to attack might give an immediate reward (capturing a piece), but the value of the resulting position might be low because you've left your king exposed.

---

### **4. Model (Optional)**
A model is the agent's **internal representation** of the environment. It mimics how the environment behaves and is used for **planning**.

**Types of Models:**

1. **Transition Model** $P(s' | s, a)$: Given a state $s$ and action $a$, what is the probability of transitioning to state $s'$?

2. **Reward Model** $R(s, a, s')$: What reward will be received after taking action $a$ in state $s$ and transitioning to state $s'$?

**Model-Based vs. Model-Free Learning:**

| Approach | Uses Model? | Learning Method | Planning? |
|----------|-------------|-----------------|-----------|
| **Model-Free** | No | Learn directly from experience | No |
| **Model-Based** | Yes | Learn a model of the environment, then plan | Yes |

**Example:**
- **Model-Free:** Learning to play a video game by trial and error
- **Model-Based:** A chess computer that simulates future moves before making a decision

> **Note:** The model is optional in RL. Model-free methods are simpler and more widely applicable, while model-based methods can be more efficient when a good model can be learned.

---

## 5. Extended Example: Tic-Tac-Toe

To solidify these concepts, let's walk through a complete example: building a reinforcement learning agent to play Tic-Tac-Toe.

### The Problem Setup

- **Environment:** A 3×3 grid where two players (X and O) take turns marking cells
- **Goal:** Get three of your marks in a row, column, or diagonal
- **State:** The current configuration of the board
- **Actions:** Placing your mark in any empty cell
- **Rewards:** +1 for winning, 0 for drawing, -1 for losing

### The Value Function Approach

Instead of having the agent memorize what to do in every possible situation (which would be complex), we'll have it learn a **value function** that estimates the probability of winning from each state.

**The Value Update Rule:**

After each move, the agent updates the value of the state it just left:

$$V(S_t) \leftarrow V(S_t) + \alpha [V(S_{t+1}) - V(S_t)]$$

Where:
- $S_t$ is the state before the move
- $S_{t+1}$ is the state after the move
- $\alpha$ is the **step-size parameter** (learning rate), typically $0 < \alpha \leq 1$
- $[V(S_{t+1}) - V(S_t)]$ is the **temporal difference (TD) error**

**Understanding the Update:**
1. **TD Error:** The difference between our current estimate and the estimate of the next state
2. **Learning:** If $V(S_{t+1}) > V(S_t)$, we increase $V(S_t)$ (that state is better than we thought)
3. **Credit Assignment:** The value "flows" backward from the end of the game through all states visited

### How the Agent Learns

**Initialization:**
- Set $V(s) = 0.5$ for all states (initially, we assume a 50% chance of winning from any state)

**Exploration (ε-greedy policy):**
- With probability $\epsilon$ (e.g., 0.1), choose a random move (exploration)
- With probability $1-\epsilon$, choose the move that leads to the state with the highest value (exploitation)

**During Each Game:**
1. Agent receives current state $S_t$
2. Either explores (random move) or exploits (best-value move)
3. Makes move, observes new state $S_{t+1}$
4. Updates $V(S_t)$ using the rule above
5. Continue until game ends
6. Update the values of all visited states based on the final outcome (win/loss/draw)

**After Many Games:**
- The value function converges to the true probability of winning from each state
- The agent develops a strong policy by always moving to the state with the highest value

### Key Insight: Why This Works

Unlike evolutionary methods that only care about the final outcome, this RL method:

1. **Learns from every state:** Each state gets updated, not just the final winner
2. **Handles credit assignment:** The value propagates backward, giving credit to intermediate decisions
3. **Handles intermediate losses:** Even if you lose the game, you learn which intermediate positions were bad
4. **Sets traps:** A learned value function can help you set up traps for a shortsighted opponent

---

## 6. Real-World Applications of RL

To give you a sense of the breadth of reinforcement learning, here are some exciting applications:

### **1. Game Playing**
- **Chess:** AlphaGo Zero learned to play Go at superhuman level entirely from self-play
- **Dota 2:** OpenAI Five defeated world champions in team-based strategy
- **Atari Games:** DeepMind's DQN learned to play 49 Atari games from raw pixels

### **2. Robotics**
- **Manipulation:** Robots learning to grasp objects of various shapes and textures
- **Locomotion:** Teaching robots to walk, run, and jump (similar to baby animals!)
- **Navigation:** Autonomous vehicles learning to drive in complex environments

### **3. Finance**
- **Portfolio Management:** Optimizing investment strategies over time
- **Algorithmic Trading:** Learning to execute trades to maximize returns
- **Risk Management:** Balancing exploration (new investments) with exploitation (proven strategies)

### **4. Healthcare**
- **Treatment Planning:** Personalizing treatment sequences for chronic diseases
- **Drug Discovery:** Optimizing molecular structures for drug development
- **Medical Diagnosis:** Learning to interpret medical images and make diagnoses

### **5. Energy and Environment**
- **Smart Grids:** Optimizing power distribution and demand response
- **Climate Control:** Managing building heating and cooling systems
- **Environmental Monitoring:** Optimizing sensor placement for pollution detection

---

## 7. Historical Context: Three Threads Converging

Reinforcement learning didn't emerge from a single research tradition. It's the convergence of three distinct threads of thought:

### **Thread 1: Trial-and-Error Learning**

**Origins in Psychology:**
- **Edward Thorndike (1898):** The Law of Effect
  > "Actions that produce a satisfying effect in a particular situation become more likely to occur again in that situation, and actions that produce an uncomfortable effect become less likely."

**Modern Revival:**
- **A. Harry Klopf (1972):** "Hedonistic Neuron" hypothesis
  - Proposed that neurons learn based on rewards
  - Revived interest in biological trial-and-error learning

---

### **Thread 2: Optimal Control**

**Mathematical Foundations:**
- **Richard Bellman (1957):** Dynamic Programming
  - The **Bellman Equation** provides the foundation for value functions
  - Introduced the concept of optimizing sequential decisions

**Markov Decision Processes (MDPs):**
- Formal framework for decision-making under uncertainty
- Provides the mathematical language for RL

**Key Contribution:**
> The optimal value function $V^*$ satisfies the **Bellman Optimality Equation**:
>
> $$V^*(s) = \max_{a} \mathbb{E}\left[R_{t+1} + \gamma V^*(S_{t+1}) \mid S_t = s, A_t = a\right]$$

---

### **Thread 3: Temporal-Difference Learning**

**Early Pioneers:**
- **Arthur Samuel (1959):** Checkers-playing program
  - First program to learn from self-play
  - Used a value function and TD-like updates

**Modern Formalization:**
- **Sutton & Barto (1980s):** Formalized TD learning
  - Provided a theoretical foundation
  - Showed how to learn predictions from experience without a model

**Key Insight:**
> TD learning combines the strengths of Monte Carlo methods (learning from complete episodes) and Dynamic Programming (bootstrapping from estimated values):
>
> $$V(S_t) \leftarrow V(S_t) + \alpha [R_{t+1} + \gamma V(S_{t+1}) - V(S_t)]$$
>
> The target is $R_{t+1} + \gamma V(S_{t+1})$—a blend of actual reward and estimated future value.

---

### The Credit Assignment Problem

**Marvin Minsky (1961)** identified a fundamental challenge:

> "How do you distribute credit for success among the many decisions that may have been involved in producing it?"

**RL's Solution:**
- **Value functions** estimate long-term consequences
- **TD learning** propagates credit backward through time
- The agent learns which actions truly contribute to long-term success

---

## 8. Summary: Key Takeaways

### **Core Concepts:**

1. **Reinforcement Learning** is learning from interaction to maximize cumulative reward
2. **Trial-and-error search** is essential—the agent must discover good actions
3. **Delayed rewards** mean actions affect not just immediate but all future rewards
4. **Exploration vs. exploitation** is a fundamental trade-off that must be balanced

### **The Four Elements:**

| Element | Role | Mathematical Notation |
|---------|------|---------------------|
| **Policy** | Agent's strategy | $\pi(a|s)$ |
| **Reward Signal** | Goal definition | $R_t$ |
| **Value Function** | Long-term evaluation | $v_\pi(s), q_\pi(s,a)$ |
| **Model** | Environment representation | $P(s'|s,a), R(s,a,s')$ |

### **Key Equations to Remember:**

1. **Return (Cumulative Reward):**
   $$G_t = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$

2. **State-Value Function:**
   $$v_\pi(s) = \mathbb{E}_\pi\left[\sum_{k=0}^{\infty} \gamma^k R_{t+k+1} \mid S_t = s\right]$$

3. **TD Update (Tic-Tac-Toe Example):**
   $$V(S_t) \leftarrow V(S_t) + \alpha [V(S_{t+1}) - V(S_t)]$$

4. **Bellman Optimality Equation:**
   $$V^*(s) = \max_{a} \mathbb{E}\left[R_{t+1} + \gamma V^*(S_{t+1}) \mid S_t = s, A_t = a\right]$$

### **The Big Picture:**

> Reinforcement learning is about **decision making under uncertainty**. An agent interacts with an unknown environment, learns from the consequences of its actions, and gradually develops a policy that maximizes long-term reward. It's a powerful framework that unifies ideas from psychology, optimal control, and machine learning.

---

## 9. Looking Ahead

Now that we've established the foundations, what's next?

**In the upcoming lectures, we'll explore:**

1. **Multi-Armed Bandits:** The exploration-exploitation dilemma in its purest form
2. **Tabular RL Methods:** Q-learning, SARSA, and the algorithms that power modern RL
3. **Function Approximation:** How to handle problems with too many states to store in a table
4. **Deep Reinforcement Learning:** Neural networks meet reinforcement learning (AlphaGo, etc.)
5. **Advanced Topics:** Policy gradients, actor-critic methods, and beyond

---

## 🎯 Practice Problems

To solidify your understanding, try these problems:

### **Problem 1: Exploration vs. Exploitation**

You're testing 5 different website designs to maximize click-through rate. Each design has an unknown true click-through rate. You have 1000 visitors to test on.

**Question:** How would you design an algorithm to balance exploration and exploitation? What happens if you only explore? Only exploit?

---

### **Problem 2: Value Function Intuition**

Consider a simple maze where an agent starts at position A and wants to reach the goal at position G. Moving to the goal gives a reward of +10, and each step costs -1.

**Question:** 
- What would be a reasonable value function for positions close to the goal?
- What about positions far from the goal?
- How would you interpret a negative value for a state?

---

### **Problem 3: Tic-Tac-Toe Analysis**

In the Tic-Tac-Toe example, why do we update the value of the state $S_t$ based on $V(S_{t+1})$ instead of just using the final game outcome?

**Question:** Explain the advantage of this approach compared to only updating states based on wins and losses.

---

## 📖 Recommended Reading

1. **Chapter 1:** Sutton & Barto, *Reinforcement Learning: An Introduction* (2nd edition)
2. **Chapter 2:** Multi-Armed Bandits (the exploration-exploitation dilemma in depth)
3. **Paper:** Samuel, A. L. (1959). "Some Studies in Machine Learning Using the Game of Checkers"
4. **Paper:** Bellman, R. (1957). *Dynamic Programming*

---

## 💡 Closing Thoughts

Reinforcement learning is not just another machine learning technique—it's a **computational theory of goal-directed behavior**. It provides a framework for understanding how agents can learn optimal behavior through interaction with their environment.

As you progress through this course, remember:

> **Every complex RL problem can be understood in terms of:**
> - **What** the agent can do (actions)
> - **Where** the agent can be (states)
> - **What** the agent wants (rewards)
> - **How** the agent learns (value functions and policies)

**Next time:** We'll dive deep into the exploration-exploitation dilemma through the lens of multi-armed bandits—where we'll develop concrete algorithms and mathematical tools for balancing this fundamental trade-off.

---

*End of Chapter 1 Lecture*

**Professor's Note:** This lecture provides the conceptual foundation for everything that follows. Make sure you understand the four elements of RL and the exploration-exploitation dilemma before moving forward. These ideas will recur throughout the entire course!

---

## Chapter 2 Lecture: Markov Decision Processes

---

## 📚 Lecture Overview

**Welcome to Chapter 2!** Having introduced the fundamental concepts of reinforcement learning, we now dive into the mathematical framework that formalizes sequential decision-making under uncertainty. This lecture introduces **Markov Decision Processes (MDPs)**—the mathematical foundation upon which almost all reinforcement learning problems are built.

**Learning Objectives:**
- Understand the **Markov Property** and why it's essential for RL
- Master the progression from Markov Chains → Markov Reward Processes → Markov Decision Processes
- Learn to define and compute **value functions** for different problem types
- Derive and apply the **Bellman Equations**—the recursive structure that makes RL tractable
- Understand the difference between **policy evaluation** and **policy optimization**

---

## 1. The Markov Property

### The Foundation of Sequential Decision-Making

Before diving into Markov processes, we must understand the **Markov Property**—the mathematical cornerstone of reinforcement learning.

### Formal Definition

A stochastic process satisfies the **Markov Property** if the future state depends **only** on the current state, not on the entire history of states that preceded it.

$$P(S_{t+1} | S_t, S_{t-1}, S_{t-2}, \ldots, S_0) = P(S_{t+1} | S_t)$$

> **Intuitive Understanding:** "The future is independent of the past given the present."

### Why This Matters

The Markov Property is crucial for reinforcement learning because:

1. **Memorylessness:** We don't need to remember the entire history—we only need to know where we are now
2. **Tractability:** It allows us to use powerful mathematical tools (Bellman equations) to solve sequential problems
3. **State Representation:** The state $S_t$ must capture **all relevant information** from the history needed to predict the future

### Real-World Example

Imagine a robot navigating a maze:

- **Non-Markov representation:** Remembering every position visited, every obstacle encountered, every decision made—this quickly becomes unmanageable
- **Markov representation:** Current position (grid cell) + goal direction—this is sufficient to make optimal future decisions

> **Key Insight:** A good state representation is the art of capturing enough history to be Markov, without capturing so much that the state space becomes intractable.

---

## 2. Markov Processes (Markov Chains)

### The Simplest Framework

A **Markov Process** (or **Markov Chain**) is the simplest sequential decision framework. It's a memoryless random process defined by:

$$\mathcal{MP} = (S, P)$$

Where:
- $S$ is a finite set of **states**
- $P$ is the **state transition probability matrix**: $P_{ss'} = P(S_{t+1} = s' | S_t = s)$

### Properties

1. **Episodic:** We assume the process eventually terminates
2. **Stochastic:** Transitions are probabilistic, not deterministic
3. **Stationary:** The transition probabilities don't change over time

### Example: The Student's Day

Consider a student moving through their daily activities:

**States:** {Class 1, Class 2, Class 3, Pass, Pub, Facebook, Sleep}

**Transitions:**
- From Class 1: 50% → Class 2, 50% → Facebook
- From Facebook: 90% → Facebook, 10% → Class 1
- From Class 3: 60% → Sleep, 40% → Pass
- From Pub: 20% → Class 1, 40% → Class 2, 40% → Class 3
- From Sleep: 100% → Sleep (terminal state)
- From Pass: 100% → Sleep (terminal state)

**Transition Matrix:**

$$
P = \begin{bmatrix}
P_{C1,C1} & P_{C1,C2} & \ldots \\
P_{C2,C1} & P_{C2,C2} & \ldots \\
\vdots & \vdots & \ddots
\end{bmatrix}
$$

> **Visualization:** Draw a directed graph where nodes are states and edges represent possible transitions with probabilities.

### Key Characteristics

1. **Sparsity:** Most entries in the transition matrix are zero (most states don't transition directly to each other)
2. **Row Summation:** Each row sums to 1 (probability of transitioning somewhere from a state is 1)
3. **Terminal States:** States like "Sleep" have $P_{Sleep, Sleep} = 1$ (absorbing state)

---

## 3. Markov Reward Processes (MRPs)

### Adding Value to Markov Chains

A **Markov Reward Process** extends a Markov Chain by associating **rewards** with states. This introduces the concept of value—we can now ask: "How good is it to be in a particular state?"

$$\mathcal{MRP} = (S, P, R, \gamma)$$

Where:
- $R$ is the **reward function**: $R_s = \mathbb{E}[R_{t+1} | S_t = s]$
- $\gamma \in [0, 1]$ is the **discount factor**

### The Return

The most important quantity in RL is the **return** (also called the **goal** or **cumulative discounted reward**):

$$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$

**Understanding the Discount Factor:**

$$
\gamma = \begin{cases}
\text{If } \gamma = 0: & \text{Only care about immediate reward (myopic)} \\
\text{If } \gamma = 1: & \text{Value all future rewards equally} \\
\text{If } 0 < \gamma < 1: & \text{Prefer sooner rewards to later ones}
\end{cases}
$$

**Why Discount?**
1. **Uncertainty:** Future is uncertain—we should discount it
2. **Mathematical Convenience:** Ensures convergence of infinite sums
3. **Human Behavior:** We naturally prefer sooner rewards (present bias)

### Example: Student MRP Rewards

For our student example, let's assign rewards:

- $R_{Pass} = +10$ (graduating is great!)
- $R_{Class 1} = -2$ (studying is work)
- $R_{Class 2} = -2$
- $R_{Class 3} = -2$
- $R_{Pub} = +1$ (socializing is fun)
- $R_{Facebook} = -1$ (procrastinating feels guilty)
- $R_{Sleep} = 0$ (terminal state)

### The Value Function

The **state-value function** gives the **expected return** starting from a state:

$$v(s) = \mathbb{E}[G_t | S_t = s]$$

> **Interpretation:** "If I start in state $s$, what's the average total (discounted) reward I'll accumulate?"

### The Bellman Equation for MRPs

The **Bellman Equation** is arguably the most important equation in reinforcement learning. It provides a **recursive decomposition** of the value function:

$$v(s) = R_s + \gamma \sum_{s' \in S} P_{ss'} v(s')$$

We can formulate this into a matrix equation:

$$v = R + \gamma Pv$$


**Breaking it Down:**

1. **$R_s$**: Immediate reward for being in state $s$
2. **$\gamma \sum_{s' \in S} P_{ss'} v(s')$**: Discounted expected future reward
3. **Recursion**: The value of $s$ depends on the values of all states it can transition to

> **Key Insight:** This equation says: "The value of a state equals the immediate reward plus the discounted average value of all possible next states."

### Solving the Bellman Equation

The Bellman equation can be solved in two ways:

#### Method 1: Direct Solution (Linear Algebra)

$$
v = R + \gamma P v
$$

Rearranging:

$$
(I - \gamma P) v = R
$$

$$
v = (I - \gamma P)^{-1} R
$$

Where $I$ is the identity matrix.

**Complexity:** $O(|S|^3)$ for matrix inversion—only practical for small state spaces.

#### Method 2: Iterative Solution

Initialize $v(s) = 0$ for all $s$, then repeatedly apply:

$$
v_{k+1}(s) = R_s + \gamma \sum_{s' \in S} P_{ss'} v_k(s')
$$

Continue until convergence: $\max_{s \in S} |v_{k+1}(s) - v_k(s)| < \epsilon$

**Advantage:** Works for large state spaces, provides intuition.

### Example Calculation

Let's compute values for a simple MRP with $\gamma = 0.9$:

**States:** {A, B, C} with C as terminal

**Rewards:** $R_A = -1, R_B = -1, R_C = 0$

**Transitions:**
- $P_{AB} = 0.5, P_{AC} = 0.5$
- $P_{BC} = 1.0$
- $P_{CC} = 1.0$

**Step 1:** Initialize $v(A) = v(B) = v(C) = 0$

**Step 2:** First update
- $v(A) = -1 + 0.9(0.5 \cdot 0 + 0.5 \cdot 0) = -1$
- $v(B) = -1 + 0.9(1.0 \cdot 0) = -1$
- $v(C) = 0$ (unchanged)

**Step 3:** Second update
- $v(A) = -1 + 0.9(0.5 \cdot (-1) + 0.5 \cdot 0) = -1.45$
- $v(B) = -1 + 0.9(1.0 \cdot 0) = -1$
- $v(C) = 0$

**Step 4:** Third update
- $v(A) = -1 + 0.9(0.5 \cdot (-1) + 0.5 \cdot 0) = -1.45$
- $v(B) = -1 + 0.9(1.0 \cdot 0) = -1$

Converged! Final values: $v(A) = -1.45, v(B) = -1, v(C) = 0$

---

## 4. Markov Decision Processes (MDPs)

### The Full Reinforcement Learning Framework

A **Markov Decision Process** is what we get when we add **decision-making** to Markov Reward Processes. This is the complete framework for reinforcement learning!

$$\mathcal{MDP} = (S, A, P, R, \gamma)$$

Where $A$ is the **finite set of actions**.

**Key Difference from MRP:**
- In MRP: Transitions happen automatically
- In MDP: The **agent chooses actions** to influence transitions

### Dynamics

Now transitions depend on both the current state **and** the chosen action:

$$
P_{ss'}^a = P(S_{t+1} = s' | S_t = s, A_t = a)
$$

The reward may also depend on the action:

$$
R_s^a = \mathbb{E}[R_{t+1} | S_t = s, A_t = a]
$$

### Example: Student MDP with Actions

Now the student can **choose** what to do in each state:

**States:** {Class 1, Class 2, Class 3, Pass, Pub, Facebook, Sleep}

**Actions:**
- **Study:** In class, move to next class
- **Facebook:** Go to Facebook
- **Pub:** Go to the pub
- **Sleep:** Go to sleep (only possible from Pass)
- **Quit:** Give up (from class)

**Visualization:**
```
        Class 1
        /     \
   Study/     \Facebook
     /           \
Class 2   →   Facebook
   /   \
Study/    \Pub
  /         \
Class 3  →   Pub
  |           \
Study|Quit    Sleep
  \           /
  Pass  ─────→  Sleep
                (terminal)
```

### Policies

A **policy** $\pi$ is a complete mapping from states to actions. It defines the agent's behavior.

$$
\pi(a|s) = P(A_t = a | S_t = s)
$$

**Types:**
1. **Deterministic Policy:** $\pi(s) = a$ (one action per state)
2. **Stochastic Policy:** $\pi(a|s)$ gives probability distribution over actions

**Key Property:** Policies are **stationary**—they don't change over time (though the agent may update its policy as it learns).

> **Important:** A policy completely defines the agent's behavior. Once we have $\pi$, the agent's decision-making is fully specified.

---

## 5. Value Functions for MDPs

### Two Types of Value Functions

In MDPs, we have two distinct value functions:

#### State-Value Function: $v_\pi(s)$

"How good is it to be in state $s$ if I follow policy $\pi$?"

$$
v_\pi(s) = \mathbb{E}_\pi[G_t | S_t = s]
$$

#### Action-Value Function: $q_\pi(s, a)$

"How good is it to take action $a$ in state $s$, **and then** follow policy $\pi$?"

$$
q_\pi(s, a) = \mathbb{E}_\pi[G_t | S_t = s, A_t = a]
$$

### The Relationship Between $v$ and $q$

$$
v_\pi(s) = \sum_{a \in A} \pi(a|s) q_\pi(s, a)
$$

**Interpretation:** "The value of a state is the average value of all actions I might take from that state, weighted by how likely I am to take each action."

### The Bellman Expectation Equations

For a fixed policy $\pi$, both value functions satisfy recursive relationships.

#### For State-Value Function:

$$
v_\pi(s) = \sum_{a \in A} \pi(a|s) \sum_{s' \in S} P_{ss'}^a [R_s^a + \gamma v_\pi(s')]
$$

**Breaking it Down:**
1. Average over all actions (based on policy)
2. For each action, average over all possible next states
3. Immediate reward plus discounted future value

#### For Action-Value Function:

$$
q_\pi(s, a) = \sum_{s' \in S} P_{ss'}^a [R_s^a + \gamma v_\pi(s')]
$$

**Breaking it Down:**
1. No action averaging—we're evaluating a specific action
2. Average over all possible next states
3. Immediate reward plus discounted future value

### Vector Form (for Efficiency)

For large state spaces, we can vectorize:

$$
v_\pi = R^\pi + \gamma P^\pi v_\pi
$$

Where:
- $R^\pi$ is the reward vector under policy $\pi$
- $P^\pi$ is the transition matrix under policy $\pi$

$$
P_{ss'}^\pi = \sum_{a \in A} \pi(a|s) P_{ss'}^a
$$

$$
R_s^\pi = \sum_{a \in A} \pi(a|s) R_s^a
$$

### Example: Computing $v_\pi$

Consider a simple MDP with:
- States: {A, B}
- Actions: {L, R}
- Policy: $\pi(L|A) = \pi(R|A) = 0.5$ (random)
- Transitions from A with L: 80% → B, 20% → A
- Transitions from A with R: 100% → B
- Rewards: $R_A^L = -1, R_A^R = 0, R_B = +1$
- $\gamma = 0.9$

**Compute $v_\pi(A)$:**

$$
v_\pi(A) = \pi(L|A) \left[ R_A^L + \gamma (0.8 \cdot v_\pi(B) + 0.2 \cdot v_\pi(A)) \right] + \pi(R|A) \left[ R_A^R + \gamma (1.0 \cdot v_\pi(B)) \right]
$$

$$
v_\pi(A) = 0.5 \left[ -1 + 0.9 (0.8 v_\pi(B) + 0.2 v_\pi(A)) \right] + 0.5 \left[ 0 + 0.9 (1.0 v_\pi(B)) \right]
$$

This creates a system of equations that we can solve iteratively!

---

## 6. Optimal Value Functions

### The Ultimate Goal: Finding the Best Policy

Given an MDP, we want to find the **optimal policy** $\pi^*$ that maximizes the expected return from **every** state.

$$
\pi^* = \arg\max_\pi v_\pi(s), \forall s \in S
$$

### Optimal State-Value Function: $v^*(s)$

"The best possible expected return starting from state $s$"

$$
v^*(s) = \max_\pi v_\pi(s)
$$

### Optimal Action-Value Function: $q^*(s, a)$

"The best possible expected return from taking action $a$ in state $s$, then acting optimally"

$$
q^*(s, a) = \mathbb{E}[R_{t+1} + \gamma v^*(S_{t+1}) | S_t = s, A_t = a]
$$

> **Critical Insight:** If you know $q^*$, you can extract the optimal policy: $\pi^*(s) = \arg\max_a q^*(s, a)$

If you know q*, then you have already solved the MDP.
### The Bellman Optimality Equations

These are the **most important equations** in reinforcement learning. They define the optimal value functions recursively.

#### Optimal State-Value Function:

$$
v^*(s) = \max_{a \in A} \sum_{s' \in S} P_{ss'}^a [R_s^a + \gamma v^*(s')]
$$

**Why This is Optimal:**
- We **choose** the best action (max over $a$)
- But we can't **control** what happens next, so we **average** over transitions (expectation)

#### Optimal Action-Value Function:

$$
q^*(s, a) = \sum_{s' \in S} P_{ss'}^a [R_s^a + \gamma v^*(s')]
$$

**Or equivalently:**

$$
q^*(s, a) = \sum_{s' \in S} P_{ss'}^a R_s^a + \gamma \max_{a' \in A} q^*(s', a')
$$

### The Two-Lookahead Structure

Notice the beautiful two-step lookahead:

$$
\underbrace{\max_a}_{\text{our choice}} \sum_{s'} \underbrace{P_{ss'}^a [R_s^a + \gamma \underbrace{\max_{a'} q^*(s', a')}_{\text{optimal future behavior}}]}_{\text{environment's choice}}
$$

1. **Max over actions:** We control what we do now
2. **Average over states:** The environment controls what happens next
3. **Max over future actions:** We'll behave optimally thereafter

### Example: Computing $v^*$

Consider a simple grid world:

```
S1 --(-1)--> S2 --(+1)--> Goal
 ^             |
 |            v
(-1)         (-1)
 |
 S3 --(+0)--> S4 --(-1)--> Goal
```

From S1:
- Action up: Goes to S3 with reward -1
- Action right: Goes to S2 with reward -1

**Terminal:** Goal has $v^*(Goal) = 0$

**Working backward (with $\gamma = 0.9$):**

**S2:** 
$$
v^*(S2) = \max\{ -1 + 0.9 \cdot 0, -1 + 0.9 \cdot v^*(S4) \}
$$
Assuming $v^*(S4) \approx -0.9$:
$$
v^*(S2) = \max\{ -1, -1 + 0.9 \cdot (-0.9) \} = \max\{ -1, -1.81 \} = -1
$$

**S1:**
$$
v^*(S1) = \max\{ -1 + 0.9 \cdot v^*(S3), -1 + 0.9 \cdot (-1) \}
$$

The optimal policy tells us which action achieves the max!

### Existence of Optimal Policy

**Theorem:** Every finite MDP has at least one optimal deterministic policy.

> **Implication:** We don't need to consider stochastic policies for optimality—a deterministic policy will always do at least as well.

---

## 7. Solving MDPs

### Policy Evaluation

**Problem:** Given an MDP and a policy $\pi$, compute $v_\pi$.

**Algorithm:**
1. Initialize $v_\pi(s) = 0$ for all $s$
2. Repeatedly apply Bellman expectation equation:
   $$v_\pi^{k+1}(s) = \sum_{a \in A} \pi(a|s) \sum_{s' \in S} P_{ss'}^a [R_s^a + \gamma v_\pi^k(s')]$$
3. Check convergence
4. Stop when $\max_s |v_\pi^{k+1}(s) - v_\pi^k(s)| < \epsilon$

**Guarantee:** This algorithm always converges to $v_\pi$.

### Policy Improvement

**Problem:** Given $v_\pi$, find a better policy $\pi'$.

**Algorithm:**
$$
\pi'(s) = \arg\max_a \sum_{s' \in S} P_{ss'}^a [R_s^a + \gamma v_\pi(s')]
$$

**Greedy Improvement:** Make the policy greedy with respect to the current value function.

**Theorem:** If $\pi'$ is greedy with respect to $v_\pi$, then $v_{\pi'}(s) \geq v_\pi(s)$ for all $s$.

> **Key Insight:** The new policy is at least as good as the old one!

### Policy Iteration

Combine evaluation and improvement:

```
Initialize π randomly
Repeat:
    1. Policy Evaluation: Compute v_π
    2. Policy Improvement: π' = greedy(v_π)
    3. If π' = π: stop (optimal found!)
       Else: π = π'
```

**Convergence:** Guaranteed to find an optimal policy $\pi^*$ for finite MDPs.

### Value Iteration

Instead of explicitly storing policies, we can directly compute $v^*$:

$$
v^{k+1}(s) = \max_{a \in A} \sum_{s' \in S} P_{ss'}^a [R_s^a + \gamma v^k(s')]
$$

**Intuition:** In each iteration, we perform one step of Bellman optimality from all states.

**Advantage:** No explicit policy evaluation needed—directly computes optimal values.

**Extracting the Policy:** Once $v^*$ converges:
$$
\pi^*(s) = \arg\max_a \sum_{s' \in S} P_{ss'}^a [R_s^a + \gamma v^*(s')]
$$

---

## 8. Summary: Key Takeaways

### Core Concepts

1. **Markov Property:** The future depends only on the present—state representation must capture all relevant information
2. **Markov Process:** Random sequence of states (S, P)—no rewards, no decisions
3. **Markov Reward Process:** MP with values (S, P, R, γ)—rewards but no decisions
4. **Markov Decision Process:** MRP with decisions (S, A, P, R, γ)—complete RL framework

### The Progression

$$
\underbrace{(S, P)}_{\text{MP}} \rightarrow \underbrace{(S, P, R, \gamma)}_{\text{MRP}} \rightarrow \underbrace{(S, A, P, R, \gamma)}_{\text{MDP}}
$$

### Key Equations

1. **Return:**
   $$G_t = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$

2. **Bellman Equation (MRP):**
   $$v(s) = R_s + \gamma \sum_{s'} P_{ss'} v(s')$$

3. **Bellman Expectation (MDP):**
   $$v_\pi(s) = \sum_a \pi(a|s) \sum_{s'} P_{ss'}^a [R_s^a + \gamma v_\pi(s')]$$

4. **Bellman Optimality:**
   $$v^*(s) = \max_a \sum_{s'} P_{ss'}^a [R_s^a + \gamma v^*(s')]$$

5. **Policy Improvement:**
   $$\pi'(s) = \arg\max_a \sum_{s'} P_{ss'}^a [R_s^a + \gamma v_\pi(s')]$$

### The Two Types of Value Functions

| Type | Question | Formula |
|------|-----------|----------|
| **State-Value** $v_\pi(s)$ | How good is state $s$ under $\pi$? | $\mathbb{E}_\pi[G_t \| S_t = s]$ |
| **Action-Value** $q_\pi(s, a)$ | How good is action $a$ in state $s$ under $\pi$? | $\mathbb{E}_\pi[G_t \| S_t = s, A_t = a]$ |

### The Big Picture

> Markov Decision Processes provide the **mathematical foundation** for sequential decision-making under uncertainty. The Bellman equations give us a way to break down complex, long-term decisions into simpler, immediate decisions. By solving these equations (through iteration or other methods), we can compute optimal behaviors for a wide range of problems.

---

## 9. Looking Ahead

Having established the mathematical framework, what's next?

**In the upcoming lectures, we'll explore:**

1. **Dynamic Programming:** Systematic methods for solving MDPs when we know the model
2. **Model-Free Prediction:** Learning value functions without knowing the model (Monte Carlo, TD learning)
3. **Model-Free Control:** Learning optimal policies without knowing the model (Q-learning, SARSA)
4. **Function Approximation:** Handling large state spaces with neural networks
5. **Deep RL:** Modern breakthroughs like AlphaGo and DQN

---

## 🎯 Practice Problems

### Problem 1: Markov Property Verification

Consider a chess-playing agent. Which of these state representations satisfies the Markov property?

1. Current board configuration only
2. Current board + last 5 moves
3. Current board + who moved last
4. All moves in the game so far

**Question:** Explain why option 1 is sufficient, and why options 2-4 add unnecessary information.

---

### Problem 2: Computing $v(s)$

Consider a simple MRP with states {A, B, C}:

- $P_{AB} = 0.6, P_{AC} = 0.4$
- $P_{BB} = 1.0$
- $P_{CC} = 1.0$
- $R_A = 1, R_B = 2, R_C = 0$
- $\gamma = 0.9$

**Question:** Compute $v(A), v(B), v(C)$ using the Bellman equation. Show at least two iterations.

---

### Problem 3: Policy Evaluation

Given an MDP with states {S1, S2} and actions {a, b}:

**Policy:** $\pi(a|S1) = 0.7, \pi(b|S1) = 0.3$

**Dynamics from S1:**
- Action a: 60% → S2 (reward +1), 40% → S1 (reward 0)
- Action b: 100% → S2 (reward -1)

**Question:** Write out the Bellman expectation equation for $v_\pi(S1)$. Compute it assuming $v_\pi(S1) = 2$ and $v_\pi(S2) = 3$.

---

### Problem 4: Policy Improvement

Given the values from Problem 3:

**Question:** Should the agent switch to a deterministic policy? If so, which action should it take in S1? Show your work.

---

## 📖 Recommended Reading

1. **Chapter 3:** Sutton & Barto, *Reinforcement Learning: An Introduction* (2nd edition) - Finite Markov Decision Processes
2. **Chapter 4:** Markov Decision Processes in Puterman's *Markov Decision Processes*
3. **Lecture Slides:** David Silver's Lecture 2 slides (available on UCL website)
4. **Chapter 17:** Russell & Norvig, *Artificial Intelligence: A Modern Approach* - Decision Making Under Uncertainty

---

## 💡 Closing Thoughts

Markov Decision Processes are the **mathematical backbone** of reinforcement learning. Everything we do in RL—whether it's Q-learning, policy gradients, or deep RL—is fundamentally working with MDPs, either explicitly or implicitly.

As you proceed through this course, remember:

> **Every RL algorithm you learn can be understood in terms of:**
> - **Estimating** value functions (prediction)
> - **Improving** policies (control)
> - **Balancing** exploration and exploitation
> - **All grounded** in the Markov Decision Process framework

**Next time:** We'll dive into **Dynamic Programming**—systematic methods for solving MDPs when we have complete knowledge of the environment. This will give us our first complete, working algorithms for reinforcement learning!

---

*End of Chapter 2 Lecture*

**Professor's Note:** This chapter provides the mathematical foundation for everything that follows. The Bellman equations you've learned here will appear in every RL algorithm we study. Make sure you're comfortable with:
1. The difference between $v$ and $q$ functions
2. The expectation vs. max in Bellman equations
3. How policy evaluation and improvement work together
4. The intuition behind the discount factor

These concepts are the bedrock of reinforcement learning—master them now, and everything else will be much easier!

---

## Chapter 3 Lecture: Planning by Dynamic Programming

---

## 📚 Lecture Overview

**Welcome to Chapter 3!** In our previous lecture, we formalized the Reinforcement Learning problem as a Markov Decision Process (MDP). Today, we move from defining the problem to solving it. We will explore **Dynamic Programming (DP)**, a powerful class of algorithms used for **Planning** in an MDP.

**Learning Objectives:**
- Understand the core requirements for applying Dynamic Programming to any problem.
- Distinguish between **Prediction** and **Control** in the context of planning.
- Master the **Iterative Policy Evaluation** algorithm.
- Learn how **Policy Iteration** alternates between evaluation and improvement to reach optimality.
- Understand **Value Iteration** as a way to solve for the optimal policy directly.
- Explore extensions like **Asynchronous DP** that make these methods more practical for large-scale problems.

---

## 1. What is Dynamic Programming?

### The Philosophy of Divide and Conquer

**Dynamic Programming** is a method for solving complex problems by breaking them down into simpler subproblems. The name itself is historical: "Dynamic" refers to the temporal or sequential nature of the problem, and "Programming" refers to optimizing a policy or mathematical program.

### Requirements for DP

To apply DP, a problem must satisfy two fundamental properties:
1. **Optimal Substructure:** The optimal solution to the overall problem can be decomposed into optimal solutions for its subproblems. the principle of optimality must apply
2. **Overlapping Subproblems:** Subproblems recur many times; their solutions can be cached (memoized) and reused.

### Why MDPs are Perfect for DP

Markov Decision Processes naturally satisfy both requirements:
- **Optimal Substructure:** The Bellman Equation provides a recursive decomposition of the value function. The value of a state is built from the values of its successor states.
- **Overlapping Subproblems:** The Value Function acts as a cache. It stores the solution to the subproblem "What is the value of starting in state $s$?" which is used repeatedly as the agent moves through the environment.

---

## 2. Planning in an MDP

### Definition of Planning

In Reinforcement Learning, **Planning** refers to the situation where we have **perfect knowledge** of the environment. Specifically, we know the MDP's dynamics ($P_{ss'}^a$) and reward function ($R_s^a$).

There are two main tasks in planning:

| Task | Input | Output |
|------|-------|--------|
| **Prediction** | MDP $(S, A, P, R, \gamma)$ and policy $\pi$ | Value function $v_\pi$ |
| **Control** | MDP $(S, A, P, R, \gamma)$ | Optimal value function $v_*$ and optimal policy $\pi_*$ |

> **Professor's Insight:** Planning is "offline" learning. The agent "thinks" about the MDP to compute a policy before ever interacting with the environment.

---

## 3. Iterative Policy Evaluation

### The Prediction Problem

Given a fixed policy $\pi$, how do we find the value function $v_\pi$? We use **Iterative Policy Evaluation**.

### The Algorithm

We start with an initial approximation $v_1$ (e.g., all zeros) and iteratively apply the **Bellman Expectation Backup** at every state:

$$v_{k+1}(s) = \sum_{a \in A} \pi(a|s) \left( R_s^a + \gamma \sum_{s' \in S} P_{ss'}^a v_k(s') \right)$$

**Key Characteristics:**
- **Synchronous Backups:** In each iteration $k+1$, we update the value of every state $s$ based on the values $v_k(s')$ from the *previous* iteration.
- **Convergence:** As $k \to \infty$, $v_k$ is guaranteed to converge to the true $v_\pi$ (provided $\gamma < 1$ or termination is guaranteed).

### Example: Small Grid World

Imagine a 4x4 grid. Two corners are terminal states (reward 0). All other transitions have a reward of -1.
- **Policy:** Uniform random ($\pi(a|s) = 0.25$ for all actions).
- **Iteration 0:** All $v(s) = 0$.
- **Iteration 1:** For any non-terminal state, $v_1(s) = -1 + 0 = -1$.
- **Iteration 2:** $v_2(s) = -1 + 0.25 \sum v_1(s')$. States near terminal states will start to "feel" the proximity of the reward 0, while middle states stay closer to -2.

By the time the values converge, we have the true expected cost to reach the goal from every square.

![[Pasted image 20260118235806.png]]

---

## 4. Policy Iteration

### The Control Problem

How do we move from evaluating a fixed policy to finding the **optimal** policy? We use **Policy Iteration**.

### The Two-Step Loop

Policy Iteration alternates between two processes:
1. **Policy Evaluation:** Compute $v_\pi$ for the current policy $\pi$ (using the iterative method above).
2. **Policy Improvement:** Update the policy to be greedy with respect to $v_\pi$.

$$\pi' = \text{greedy}(v_\pi)$$
$$\pi'(s) = \arg\max_{a \in A} q_\pi(s, a)$$

### Why It Works: The Policy Improvement Theorem

If we change the policy to choose the best action according to the current value function ($q_\pi(s, a)$) and then follow the old policy thereafter, the value of the state $s$ cannot decrease.

$$q_\pi(s, \pi'(s)) = \max_{a \in A} q_\pi(s, a) \geq q_\pi(s, \pi(s)) = v_\pi(s)$$

If the improvement stops (i.e., $\pi' = \pi$), then the Bellman Optimality Equation is satisfied, and we have reached $v_*$.

> **Professor's Tip:** In practice, you don't always need to wait for policy evaluation to converge perfectly. Even a few steps of evaluation can be enough to suggest a better policy!

---

## 5. Value Iteration

### Solving for Optimality Directly

While Policy Iteration works on the policy, **Value Iteration** works directly on the value function using the **Bellman Optimality Equation**.

### The Principle of Optimality

Any optimal policy can be subdivided into two components:
1. An optimal first action $a$.
2. Followed by an optimal policy from the resulting state $s'$.

### The Algorithm

We iteratively update the value function using the **Bellman Optimality Backup**:

$$v_{k+1}(s) = \max_{a \in A} \left( R_s^a + \gamma \sum_{s' \in S} P_{ss'}^a v_k(s') \right)$$

**Key Differences from Policy Evaluation:**
- There is **no explicit policy**. We take the `max` over actions.
- Each iteration is technically an update towards the *optimal* values, not the values of a specific policy.
- Once $v_k$ converges to $v_*$, the optimal policy $\pi_*$ is extracted as the greedy policy.

### Intuition: Working Backwards

Think of Value Iteration as finding the shortest path.
- In iteration 1, you find the states that can reach the goal in 1 step.
- In iteration 2, you find states that can reach the goal in 2 steps.
- Eventually, information propagates across the entire state space.

---

## 6. Summary of DP Algorithms

| Problem | Bellman Equation | Algorithm |
|---------|------------------|-----------|
| **Prediction** | Bellman Expectation Equation | Iterative Policy Evaluation |
| **Control** | Bellman Expectation Equation + Greedy Improvement | Policy Iteration |
| **Control** | Bellman Optimality Equation | Value Iteration |

All these algorithms are based on **State-Value Functions** $v(s)$. Complexity is $O(mn^2)$ per iteration where $n$ is states and $m$ is actions. If we used Action-Value Functions $q(s, a)$, the complexity would be $O(m^2n^2)$.

---

## 7. Extensions and Practicalities

### The Curse of Dimensionality

DP uses **Full-Width Backups**. To update one state, we must consider every possible action and every possible successor state.
- If we have $n$ states, the complexity grows exponentially with $n$ (Bellman called this the "Curse of Dimensionality").

### Asynchronous Dynamic Programming

To avoid updating the entire state space at once, we can use **Asynchronous DP**. These methods update states in any order, using whatever values are currently available. This significantly reduces computation and still guarantees convergence if all states are visited infinitely often.

Three common types:
1. **In-place DP:** Store only one copy of the value function and update it "in-place" rather than using $v_k$ to produce $v_{k+1}$.
2. **Prioritized Sweeping:** Update states with the largest Bellman error first (i.e., those whose values changed the most).
3. **Real-time DP:** Only update states that the agent actually visits during interaction.

---

## 8. The Mathematical Foundation: Contraction Mapping (Brief)

Why do these iterative processes converge? The Bellman operators are **contractions** in the space of value functions (under the $\infty$-norm).
- Applying the operator makes two different value functions closer to each other.
- According to the **Banach Fixed-Point Theorem**, there is a unique fixed point (the true value function), and iterative application will always reach it.

---

## 9. Summary: Key Takeaways

1. **DP is for Planning:** It requires a perfect model of the environment.
2. **Prediction vs. Control:** Evaluation vs. Optimization.
3. **Iterative Evaluation:** Repeatedly applies the Bellman Expectation Equation.
4. **Policy Iteration:** Evaluation → Improvement → Repeat.
5. **Value Iteration:** Direct optimization via the Bellman Optimality Equation.
6. **Efficiency:** Asynchronous methods allow us to focus computation where it matters most.

---

## 🎯 Practice Problems

### Problem 1: Grid World Step

In a 3x3 grid world, state $(2,2)$ is the goal (Value 0). All other steps give reward -1. Assume $\gamma = 1$.
Using **Value Iteration**, what is the value of $(2,1)$ after the first iteration where it "sees" the goal? What is the value of $(1,1)$ in that same iteration?

---

### Problem 2: Greedy Policy Improvement

Suppose you have evaluated a random policy $\pi$ in a 2-state MDP and found $v_\pi(S1) = 10$ and $v_\pi(S2) = 15$.
Action $A$ from $S1$ leads to $S2$ with probability 1.0 and reward 0.
Action $B$ from $S1$ leads to $S1$ with probability 1.0 and reward 2.
Assume $\gamma = 0.9$.
**Question:** Which action ($A$ or $B$) is selected by the greedy policy improvement step for state $S1$?

---

### Problem 3: DP Complexity

If an environment has 1,000,000 states and 10 actions, why might synchronous Policy Iteration be problematic? Suggest a more efficient DP approach.

---

## 📖 Recommended Reading

1. **Chapter 4:** Sutton & Barto, *Reinforcement Learning: An Introduction* (2nd edition) - Dynamic Programming.
2. **Paper:** Bellman, R. (1957). *Dynamic Programming*. Princeton University Press.
3. **Lecture Slides:** David Silver's Lecture 3 slides.

---

## 💡 Closing Thoughts

Dynamic Programming is the bridge between the theory of MDPs and practical algorithms. While "Full-Width" DP is rarely used in large-scale modern RL (due to the Curse of Dimensionality and the need for a perfect model), the concepts of **bootstrapping** (updating an estimate from other estimates) and **iteration** form the core of every modern RL algorithm, including those used in AlphaGo and autonomous driving.

**Next time:** We move into the "True" Reinforcement Learning territory—where we **don't have a model** of the environment and must learn directly from experience!

---

*End of Chapter 3 Lecture*

**Professor's Note:** Remember, the transition from Chapter 2 to Chapter 3 is the transition from *what* is an optimal policy to *how* do we compute it. Master the difference between Policy and Value iteration—they are the parents of almost all RL control algorithms!

---

## Chapter 4 Lecture: Model-Free Prediction

---

## 📚 Lecture Overview

**Welcome to Chapter 4!** Today we bridge the gap between theory and reality. Until now, we have focused on **Planning**—solving environments where the underlying dynamics (transitions $P$ and rewards $R$) are perfectly known. However, for most real-world problems—like controlling a factory, optimizing a commute, or playing complex games—the exact equations governing the environment are unknown.

We introduce **Model-Free Prediction**: methods that allow an agent to estimate the value function $v_\pi$ directly from its interactions with the environment.

**Learning Objectives:**
- Distinguish between **Planning** (known model) and **Reinforcement Learning** (unknown model).
- Master **Monte Carlo (MC) Learning** and its dependence on complete episodes.
- Understand the revolutionary concept of **Temporal Difference (TD) Learning** and **Bootstrapping**.
- Evaluate the **Bias-Variance Tradeoff** between MC and TD.
- Explore the **Unifying Spectrum** of $n$-step returns and **TD($\lambda$)**.

---

## 1. Introduction: From Planning to Learning

### The Reality of Unknown Dynamics
In many interesting problems, no one gives us the MDP. We don't know how pollution levels depend on engine torque or how traffic patterns shift in real-time. 

| Feature | Planning (Dynamic Programming) | Reinforcement Learning (Model-Free) |
| :--- | :--- | :--- |
| **Model** | Known (Transitions $P$, Rewards $R$) | Unknown |
| **Method** | Inner-loop computation (Bellman updates) | Interaction/Experience |
| **Goal** | Solve for $v_*$ and $\pi_*$ | Learn $v_\pi$ (Prediction) or $v_*$ (Control) |

### The Two-Step Progression
Just as in DP, we break the problem into two parts:
1. **Prediction (This Lecture):** Given a policy $\pi$, what is the value function $v_\pi$?
2. **Control (Next Lecture):** How do we find the optimal policy $\pi_*$?

---

## 2. Monte Carlo (MC) Learning

### Core Principle: Learning from Episodes
Monte Carlo methods learn directly from **complete episodes** of experience. The fundamental idea is simple: the value of a state is the expected return, so we estimate it using the **empirical mean return**.

$$v_\pi(s) \approx \text{average}(G_t) \text{ such that } S_t = s$$

**Key Requirements:**
- **Episodic Tasks:** The agent must reach a terminal state to calculate the return $G_t$.
- **No Model Required:** We only need samples of States, Actions, and Rewards.

It is model-free: no knowledge of MDP transitions / rewards
- MC learns from complete episodes: no bootstrapping
- MC uses the simplest possible idea: value = mean return
- Caveat: can only apply MC to episodic MDPs
	- all episodes must terminate

### First-Visit vs. Every-Visit MC
- **First-Visit MC:** We only consider the return from the *first* time a state $s$ is visited in an episode. By the Law of Large Numbers, the average converges to the true expectation as the number of episodes increases.
- **Every-Visit MC:** We consider the return from *every* visit to state $s$ in an episode.

> **Law of Large Numbers:** The empirical mean of independent and identically distributed random variables converges to the true expectation.

### Incremental Mean Updates
We can update the mean value function incrementally after each episode without storing all previous returns:
$$\mu_k = \mu_{k-1} + \frac{1}{k}(x_k - \mu_{k-1})$$

In RL terms, for state $S_t$ with return $G_t$:
$$N(S_t) \leftarrow N(S_t) + 1$$
$$V(S_t) \leftarrow V(S_t) + \frac{1}{N(S_t)} (G_t - V(S_t))$$

For **non-stationary** problems (where the environment or policy changes), we replace $\frac{1}{N}$ with a constant step-size $\alpha$:
$$V(S_t) \leftarrow V(S_t) + \alpha (G_t - V(S_t))$$

---

## 3. Temporal Difference (TD) Learning

### Core Principle: Bootstrapping
Temporal Difference methods are the most central idea in RL. Unlike MC, TD learns from **incomplete episodes** by looking only one step ahead. 

### The TD(0) Update
Instead of waiting for the actual return $G_t$, TD uses an **estimate** of the return:
$$V(S_t) \leftarrow V(S_t) + \alpha ( \underbrace{R_{t+1} + \gamma V(S_{t+1})}_{\text{TD Target}} - V(S_t) )$$

- **TD Target:** $R_{t+1} + \gamma V(S_{t+1})$ (Immediate reward + discounted value of the next state).
- **TD Error ($\delta_t$):** The difference between our new estimate and our previous estimate.

**Why "Bootstrapping"?**
We are updating a guess (the value of the current state) based on another guess (the value of the next state).

### The "Driving to Work" Example
- **Monte Carlo:** You drive, you almost crash, but you swerve and arrive safely. MC sees only the "safe arrival" and concludes the drive was fine. You only update your "value of the drive" once you reach the destination.
- **TD Learning:** As soon as you see the car hurtling toward you, your *prediction* of your survival drops. You immediately update your value function to say "that situation was dangerous," without needing to wait for a crash (or safe arrival). **You can't back up death.**

---

## 4. MC vs. TD: The Great Tradeoff

### Bias-Variance Analysis
- **Monte Carlo (Unbiased):** $G_t$ is an unbiased estimate of $v_\pi(s)$. It has **high variance** because it depends on many random transitions and rewards over the whole trajectory. It is robust to initial values but slow to converge.
- **TD Learning (Biased but Low Variance):** The TD target is biased because it depends on our (initially wrong) value function $V$. However, it has **low variance** because it only depends on one random step. It is much more efficient and usually converges faster than MC.

### The Markov Property
- **TD exploits the Markov Property:** It assumes the future depends only on the current state. It is highly efficient in Markov environments.
- **MC ignores the Markov Property:** It can be more effective in non-Markovian or partially observed environments (POMDPs) where the current "state" doesn't capture the full history.

### Convergence in Batch Settings
If we have a finite set of episodes and repeat the updates (Batch RL):
- **MC** converges to the values that minimize the **Mean Squared Error** with the sample returns.
- **TD(0)** converges to the solution of the **Maximum Likelihood MDP**—it builds the best possible model of what it has seen and solves it.

---

## 5. The Unified Spectrum: TD($\lambda$)

### $n$-Step Prediction
We can bridge the gap between TD (1-step) and MC ($\infty$-step) by looking $n$ steps ahead:
- $n=1$ (TD): $G_t^{(1)} = R_{t+1} + \gamma V(S_{t+1})$
- $n=2$: $G_t^{(2)} = R_{t+1} + \gamma R_{t+2} + \gamma^2 V(S_{t+2})$
- $n=\infty$ (MC): $G_t^{(\infty)} = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{T-t-1} R_T$

### The $\lambda$-Return
Instead of picking one $n$, we can take a weighted average. TD($\lambda$) uses the **$\lambda$-return**, which averages all $n$-step returns using geometric weights $(1-\lambda)\lambda^{n-1}$.
- $\lambda = 0$: Equivalent to TD(0).
- $\lambda = 1$: Equivalent to Monte Carlo.

### Forward vs. Backward Views
1. **Forward View:** Theoretical. To update a state, we must look into the future to compute the $\lambda$-return (requires complete episodes).
2. **Backward View (Eligibility Traces):** Mechanistic and online. We keep an **Eligibility Trace** for every state, combining **Frequency** (how often we visited) and **Recency** (how recently we visited). 
   - When a TD error $\delta_t$ occurs, we broadcast it back to all states proportional to their eligibility.
   - This allows us to learn online, step-by-step, while achieving the same results as the forward view.

---

## 📚 Summary: Key Takeaways

1. **Model-Free Prediction** allows learning $v_\pi$ from experience without knowing the rules of the environment.
2. **Monte Carlo** averages sample returns. It is unbiased but slow and requires termination.
3. **Temporal Difference** bootstraps from its own estimates. it is biased but low-variance, online, and more efficient.
4. **Bootstrapping** (TD/DP) vs. **Sampling** (MC/TD).
5. **Eligibility Traces** provide a practical way to implement the spectrum between TD and MC.

---

*End of Chapter 4 Lecture*

---

## 6. Deep Dive: Mathematical Foundations and Examples

### 6.1. The AB Example: Convergence to Different Solutions

The **AB Example** is the definitive illustration of why MC and TD(0) converge to different solutions in batch settings. 

**The Experience (8 Episodes):**
1. $A, 0, B, 0$
2. $B, 1$
3. $B, 1$
4. $B, 1$
5. $B, 1$
6. $B, 1$
7. $B, 1$
8. $B, 0$

**Results:**
- **$V(B)$:** Both methods agree. There were 8 visits to B, 6 of which ended in reward 1. Thus, $V(B) = 6/8 = 0.75$.
- **$V(A)$:**
    - **Monte Carlo:** $V(A) = 0$. MC looks at the complete return from state A. In the only episode where A appeared, the total return was 0.
    - **TD(0):** $V(A) = 0.75$. TD(0) observes that state A always transitions to state B with reward 0. It then uses the value of B ($0.75$) to bootstrap: $V(A) = R + \gamma V(B) = 0 + 1 \cdot 0.75 = 0.75$.

**Theoretical Implication:**
- **MC minimizes Mean Squared Error (MSE)** between the estimated values and the actual observed returns.
- **TD(0) converges to the Maximum Likelihood MDP**. It builds the most likely transition and reward model that explains the data and solves it exactly (Certainty Equivalence).

---

### 6.2. The Bias-Variance Trade-off: A Formal View

The choice between MC and TD is fundamentally a trade-off between bias and variance.

| Property | Monte Carlo (MC) | Temporal Difference (TD) |
| :--- | :--- | :--- |
| **Bias** | **Zero Bias.** The return $G_t$ is an unbiased estimator: $\mathbb{E}[G_t \mid S_t=s] = v_\pi(s)$. | **Biased.** The TD Target $R_{t+1} + \gamma V(S_{t+1})$ relies on an estimate $V$, making it biased early on. |
| **Variance** | **High Variance.** $G_t$ depends on many random transitions and rewards across the entire episode. | **Low Variance.** The TD target depends on only one random step ($R_{t+1}$ and $S_{t+1}$). |

---

### 6.3. The Spectrum of n-Step Returns

We can unify MC and TD by considering a spectrum of "lookahead" steps.

- **1-step Return (TD):** 
  $$G_t^{(1)} = R_{t+1} + \gamma V(S_{t+1})$$
- **2-step Return:** 
  $$G_t^{(2)} = R_{t+1} + \gamma R_{t+2} + \gamma^2 V(S_{t+2})$$
- **n-step Return:** 
  $$G_t^{(n)} = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{n-1} R_{t+n} + \gamma^n V(S_{t+n})$$
- **Infinite-step Return (MC):** 
  $$G_t^{(\infty)} = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{T-t-1} R_T$$

**The $\lambda$-Return:**
TD($\lambda$) takes a weighted average of all n-step returns using the decay parameter $\lambda \in [0, 1]$:
$$G_t^\lambda = (1 - \lambda) \sum_{n=1}^{\infty} \lambda^{n-1} G_t^{(n)}$$

---

### 6.4. Eligibility Traces: The Backward View

While the Forward View (using $G_t^\lambda$) is intuitive, it requires waiting for the end of the episode. The **Backward View** implements TD($\lambda$) online using **Eligibility Traces**.

**The Eligibility Trace Vector ($E_t$):**
For each state $s$, we maintain a trace $E_t(s)$ that tracks how recently and frequently a state was visited.

1. **Accumulating Traces:**
   $$E_t(s) = \gamma \lambda E_{t-1}(s) + \mathbb{1}(S_t = s)$$
2. **Replacing Traces:**
   $$E_t(s) = \begin{cases} 1 & \text{if } s = S_t \\ \gamma \lambda E_{t-1}(s) & \text{if } s \neq S_t \end{cases}$$

**The Update Rule:**
For every state $s$ in the state space:
$$V(s) \leftarrow V(s) + \alpha \delta_t E_t(s)$$
Where $\delta_t = R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$ is the **TD Error**.

---

### 6.5. Unified View of RL Backups

We can now categorize RL algorithms along two dimensions:

1. **Width of Backup (Sampling vs. Expectation):**
   - **Sampling:** MC, TD (look at one possible outcome).
   - **Expectation:** Dynamic Programming (look at all possible outcomes).
2. **Depth of Backup (Bootstrapping vs. Full Trajectory):**
   - **Bootstrapping:** TD, DP (update estimate from other estimates).
   - **Full Trajectory:** MC (update from actual outcome).

| Algorithm | Width | Depth | Bootstraps? | Samples? |
| :--- | :--- | :--- | :--- | :--- |
| **Dynamic Programming** | Full | Shallow | Yes | No |
| **TD Learning** | Sample | Shallow | Yes | Yes |
| **Monte Carlo** | Sample | Full | No | Yes |
| **Exhaustive Search** | Full | Full | No | No |

---

## 🎯 Practice Problems

### Problem 1: TD Update Calculation
Given: $V(S_1) = 10$, $V(S_2) = 15$, $R_2 = 2$, $\gamma = 0.9$, $\alpha = 0.1$.
**Question:** Calculate the new $V(S_1)$ after one TD(0) update.

**Solution:**
1.  **Target:** $Target = R_2 + \gamma V(S_2) = 2 + 0.9(15) = 2 + 13.5 = 15.5$
2.  **Error:** $\delta = Target - V(S_1) = 15.5 - 10 = 5.5$
3.  **Update:** $V(S_1) \leftarrow 10 + 0.1(5.5) = 10.55$

---

### Problem 2: MC vs TD in the Driving Example
Why does TD learn that "driving toward a car" is dangerous *before* the crash happens, while MC only learns it *after*?

**Solution:**
TD updates the value of the "approaching car" state as soon as it sees the "next state" (imminent collision) having a very low value. MC must wait for the actual crash (or swerve) to conclude the journey and assign a final return to the earlier states.

---

## 📖 Recommended Reading
1. **Chapter 12:** Sutton & Barto, *Reinforcement Learning: An Introduction* - Eligibility Traces.
2. **Lecture Notes:** David Silver's Lecture 4 slides (UCL).
3. **Paper:** Dayan, P. (1992). The convergence of TD($\lambda$) for general $\lambda$. *Machine Learning*.

---

*End of Chapter 4 Deep Dive*

**Professor's Note:** This deep dive solidifies the mathematical bridge between simple prediction and complex real-world learning. Understanding eligibility traces is the "secret sauce" that makes temporal difference learning truly efficient in the real world.


Here is a comprehensive, deep-dive lecture on **Model-Free Prediction**, based strictly on the mathematical frameworks and intuitions presented in David Silver’s RL Course (Lecture 4).

This lecture is formatted for **Obsidian** (using standard LaTeX syntax) and structured to move from intuition to formal rigor.

---

# Lecture 4: Model-Free Prediction

## 1. Introduction: The Move to "Real" Learning

In previous lectures, we discussed **Dynamic Programming (DP)**. DP is powerful, but it has a fatal flaw for real-world application: it requires a **Model**. To solve an MDP with DP, you must know the transition probabilities $P_{ss'}^a$ and the reward function $R_s^a$.

In this lecture, we abandon the model. We enter the realm of **Model-Free Prediction**.

- **No Model:** The agent does not know how the environment works. It doesn't know the physics of the robot or the rules of the game.
    
- **Experience-Based:** The agent learns purely by interacting with the environment—generating trajectories of states, actions, and rewards.
    
- **Goal:** Estimate the Value Function $V_\pi(s)$ for a given policy $\pi$.
    

This is "Prediction" (evaluating a policy). In the next lecture, we will discuss "Control" (finding the optimal policy).

---

## 2. Monte Carlo (MC) Learning

### The Intuition

Monte Carlo is the most intuitive approach. Imagine you want to know the average height of people in a city. You don't know the distribution (the "model"). Instead, you just stand on a street corner, measure 100 random people, and take the average.

In RL, "measuring a person" corresponds to running a complete episode.

MC methods learn directly from episodes of experience. They are "Model-Free" because they don't need to know the transitions; they just observe what happens.

**Crucial Constraint:** MC can only apply to **episodic MDPs**. The episode must terminate so we can calculate the total return.

### The Formalism

Our goal is to learn $v_\pi(s)$ from episodes of experience under policy $\pi$:

$$S_1, A_1, R_2, S_2, A_2, R_3, \dots, S_T \sim \pi$$

Recall that the return $G_t$ is the total discounted reward:

$$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots + \gamma^{T-1} R_T$$

The value function is simply the expected return:

$$v_\pi(s) = \mathbb{E}_\pi [ G_t | S_t = s ]$$

Monte Carlo uses the **empirical mean** return instead of the expected return.

### First-Visit vs. Every-Visit MC

When running an episode, you might visit state $s$ multiple times (e.g., passing through the same intersection twice).

1. **First-Visit MC:**
    
    - Check if state $s$ is visited for the **first time** in this episode.
        
    - Increment counter $N(s) \leftarrow N(s) + 1$.
        
    - Increment total return $S(s) \leftarrow S(s) + G_t$.
        
    - Estimate value $V(s) = S(s) / N(s)$.
        
    - _Theory:_ By the Law of Large Numbers, $V(s) \to v_\pi(s)$ as $N(s) \to \infty$.
        
2. **Every-Visit MC:**
    
    - Update the stats for **every** occurrence of $s$ in the episode.
        
    - _Theory:_ Also converges to $v_\pi(s)$.
        

### Incremental Mean Updates

We do not need to store every return to calculate an average. We can update the mean incrementally.

If $\mu_{k}$ is the mean of $k$ items, and we observe a new item $x_k$:

$$\mu_k = \mu_{k-1} + \frac{1}{k} (x_k - \mu_{k-1})$$

In RL, we replace $1/k$ with a step-size parameter $\alpha$. This gives us the standard non-stationary update rule:

$$V(S_t) \leftarrow V(S_t) + \alpha (G_t - V(S_t))$$

- $G_t$: The **Target** (the actual return).
    
- $G_t - V(S_t)$: The **Error** (how wrong our estimate was).
    

---

## 3. Temporal-Difference (TD) Learning

### The Intuition: Bootstrapping

Monte Carlo has a problem: you must wait until the episode **ends** before you can learn anything. If the episode is long (or infinite), you learn nothing.

**Temporal-Difference (TD)** learning breaks this dependency.

- **MC:** "I will tell you the value of this state once I see the final outcome."
    
- **TD:** "I will estimate the value of this state based on the _estimated_ value of the _next_ state."
    

This is called **Bootstrapping**: updating a guess towards another guess.

The Driving Analogy:

Imagine driving home.

- **MC Approach:** You predict the trip takes 30 mins. You drive. You arrive after 40 mins. _Only then_ do you update your prediction for next time: "Okay, it actually takes 40."
    
- **TD Approach:** You predict 30 mins. You drive 5 mins and hit unexpected rain. You immediately think, "Oh, now it's going to take 40 total." You updated your prediction _during_ the episode, before knowing the final result.
    

### The Math: TD(0)

The simplest form is TD(0). We update $V(S_t)$ toward the **estimated return** after one step.

$$V(S_t) \leftarrow V(S_t) + \alpha \underbrace{(R_{t+1} + \gamma V(S_{t+1}) - V(S_t))}_{\delta_t}$$

- **TD Target:** $R_{t+1} + \gamma V(S_{t+1})$
    
    - This acts as the "ground truth" we move toward. It comprises the immediate reward plus the discounted value of the next state.
        
- **TD Error ($\delta_t$):** $\delta_t = R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$
    
    - This measures the difference between our estimate _before_ taking the step and our improved estimate _after_ taking the step.
        

---

## 4. MC vs. TD: The Bias-Variance Trade-off

This is the most critical theoretical distinction in Model-Free Prediction.

### 1. Bias and Variance

- **Monte Carlo (MC):**
    
    - **Unbiased:** The return $G_t$ is a true sample of the value function. $E[G_t] = v_\pi(S_t)$.
        
    - **High Variance:** $G_t$ depends on _many_ random actions, transitions, and rewards until the end of the episode. All that noise accumulates.
        
- **Temporal Difference (TD):**
    
    - **Biased:** The target $R_{t+1} + \gamma V(S_{t+1})$ depends on our _current estimate_ $V(S_{t+1})$, which might be wrong.
        
    - **Low Variance:** The target depends on only _one_ new random transition and reward ($R_{t+1}$, $S_{t+1}$). It ignores the noise of the rest of the episode.
        

**Result:** TD is usually more efficient (converges faster) because it has lower variance, even though it is initially biased.

### 2. Markov Property

- **MC** does not exploit the Markov Property. It treats the episode as a black box black timeline. It minimizes the Mean Squared Error (MSE) between estimates and actual returns.
    
- **TD** exploits the Markov Property. It assumes that $V(s)$ should equal $R + \gamma V(s')$. It builds a solution that is consistent with the Markov structure.
    

Example (The AB Example):

Imagine two states, A and B.

- We see 1 episode: Start at A, go to B, get 0 reward, terminate. (A, 0, B, 0)
    
- We see 7 episodes: Start at B, get 1 reward, terminate. (B, 1)
    

**Question:** What is $V(A)$?

- **MC Answer:** $V(A) = 0$. Why? We saw A once, and the total return was 0.
    
- **TD Answer:** $V(A) \approx 0.75$. Why? TD learns $V(B) \approx 1$ (from the 7 episodes). Since A transitions to B, TD implies $V(A)$ should inherit value from $B$.
    

TD (Markov) tends to be better in standard environments because the Markov property usually holds.

---

## 5. $n$-Step TD Prediction

We don't have to choose between 1 step (TD) and $\infty$ steps (MC). We can look $n$ steps ahead.

- **1-Step Return (TD):** $G_t^{(1)} = R_{t+1} + \gamma V(S_{t+1})$
    
- **2-Step Return:** $G_t^{(2)} = R_{t+1} + \gamma R_{t+2} + \gamma^2 V(S_{t+2})$
    
- n-Step Return:
    
    $$G_t^{(n)} = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{n-1} R_{t+n} + \gamma^n V(S_{t+n})$$
    

We can define an $n$-step TD update:

$$V(S_t) \leftarrow V(S_t) + \alpha (G_t^{(n)} - V(S_t))$$

As $n \to \infty$, this becomes Monte Carlo.

---

## 6. TD($\lambda$)

Choosing the "best" $n$ is hard. It varies by task. Instead of choosing one $n$, let's average _all_ of them.

### The Forward View

The $\lambda$-return $G_t^\lambda$ is a geometric weighted average of all $n$-step returns.

$$\lambda \in [0, 1]$$

$$G_t^\lambda = (1-\lambda) \sum_{n=1}^{\infty} \lambda^{n-1} G_t^{(n)}$$

- If $\lambda = 0$: We only care about the 1-step return ($G_t^{(1)}$). This is TD(0).
    
- If $\lambda = 1$: We put all weight on the $\infty$-step return. This is Monte Carlo.
    

The Forward View update is:

$$V(S_t) \leftarrow V(S_t) + \alpha (G_t^\lambda - V(S_t))$$

**Problem:** Like MC, the Forward View is acausal. You have to wait until the end of the episode to calculate all the $n$-step returns (e.g., the 100-step return requires waiting 100 steps).

### The Backward View (Eligibility Traces)

We can achieve the _exact same result_ as the Forward View, but update **online**, at every step, using **Eligibility Traces**.

Intuition:

When a bad event happens (e.g., a robot crashes), which state is to blame?

1. **Frequency Heuristic:** The state we visited the most?
    
2. **Recency Heuristic:** The state we visited most recently?
    

Eligibility Traces ($E_t(s)$) combine both.

Every time we visit state $s$, we bump up its eligibility. Over time, eligibility decays.

$$E_0(s) = 0$$

$$E_t(s) = \gamma \lambda E_{t-1}(s) + \mathbb{1}(S_t = s)$$

- $\mathbb{1}(S_t = s)$: Adds 1 if we are currently in state $s$ (Frequency).
    
- $\gamma \lambda$: Decays the trace every step (Recency).
    

### The Backward Update

When we generate a TD error $\delta_t$ at the current step, we don't just update the current state. We shout the error backwards to **all** states, proportional to their eligibility $E_t(s)$.

$$\delta_t = R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$$

$$V(s) \leftarrow V(s) + \alpha \delta_t E_t(s)$$

**Theorem:** The sum of offline updates in the Backward View (using eligibility traces) is exactly equal to the sum of offline updates in the Forward View (using $\lambda$-returns).

This allows us to perform efficient, tunable bootstrapping online.

---

## 7. Summary of Relationships

|**Method**|**Update Target**|**Perspective**|
|---|---|---|
|**Monte Carlo**|$G_t$|Learns from full history. Zero bias, high variance.|
|**TD(0)**|$R_{t+1} + \gamma V(S_{t+1})$|Learns from next step. Biased, low variance.|
|**TD($\lambda$)**|$G_t^\lambda$|Unifies MC and TD. Tunable decay $\lambda$.|

### Next Step

Would you like me to construct a similar lecture for the next video in the series on **Model-Free Control**, where we introduce Q-Learning and SARSA?


Here is a comprehensive, deep-dive lecture on **Model-Free Control**, based on Lecture 5 of David Silver’s Reinforcement Learning course.

This lecture builds directly on the previous topic (Model-Free Prediction). We are moving from _evaluating_ a fixed policy to _optimizing_ behavior to find the best possible strategy.

---

# Lecture 5: Model-Free Control

## 1. Introduction: From Prediction to Control

In the previous lecture, we asked: "Given a policy $\pi$, how much reward will I get?" (Policy Evaluation).

Now, we ask the ultimate question of RL: "What is the best possible policy $\pi$?"* (Control).

In **Model-Free Control**, we face two massive challenges that Dynamic Programming (DP) did not have to deal with:

1. **Unknown Dynamics:** We do not know the transition probability $P(s'|s,a)$ or the reward function $R(s,a)$. We cannot look ahead.
    
2. **Exploration vs. Exploitation:** Since we learn by trying things, we must balance _doing what we think is best_ (exploiting) vs. _trying new things to find better options_ (exploring).
    

---

## 2. Generalized Policy Iteration (GPI) with Action Values

### Why $V(s)$ is not enough

In DP, we used the State-Value Function $V(s)$. To improve a policy using $V(s)$, we essentially used this greedy update:

$$\pi'(s) = \underset{a}{\text{argmax}} \left( R(s,a) + \gamma \sum_{s'} P(s'|s,a) V(s') \right)$$

Crucial Problem: This equation requires the model $P(s'|s,a)$. We need to know where action $a$ leads to calculate the expectation.

### The Solution: Action-Value Function $Q(s,a)$

In a model-free setting, we must use the Action-Value Function $Q(s,a)$.

$$Q_\pi(s,a) = \text{Expected return starting from } s, \text{ taking action } a, \text{ and then following } \pi$$

If we know $Q(s,a)$ for all actions, optimization becomes trivial. We don't need the model. We just pick the action with the highest number:

$$\pi'(s) = \underset{a}{\text{argmax}} \ Q(s,a)$$

### The GPI Loop

We follow the standard **Generalized Policy Iteration** pattern:

1. **Evaluation:** Estimate $Q_\pi$ (using MC or TD).
    
2. **Improvement:** Update $\pi$ to be greedy with respect to $Q_\pi$.
    

---

## 3. Monte Carlo Control & Exploration

### The Problem with Greedy Policies

If we simply act greedily ($\pi(s) = \text{argmax}_a Q(s,a)$) during learning, we face a catastrophe.

- _Example:_ You open Door A and get reward 0. You open Door B and get reward +1.
    
- _Greedy Update:_ $Q(A)=0, Q(B)=1$. You will now _forever_ choose Door B. You will never learn that Door A might actually give +100 reward 50% of the time.
    

We need **Exploration**.

### $\epsilon$-Greedy Exploration

The simplest way to ensure exploration is the $\epsilon$-greedy policy.

With probability $1-\epsilon$, choose the greedy action.

With probability $\epsilon$, choose a random action.

$$\pi(a|s) = \begin{cases} \epsilon/m + 1 - \epsilon & \text{if } a^* = \text{argmax } Q(s,a) \\ \epsilon/m & \text{otherwise} \end{cases}$$

_(where $m$ is the number of actions)_

**Theorem (Policy Improvement):** For any $\epsilon$-greedy policy $\pi$, the $\epsilon$-greedy policy $\pi'$ with respect to $q_\pi$ is an improvement, i.e., $v_{\pi'}(s) \ge v_\pi(s)$.

### GLIE (Greedy in the Limit with Infinite Exploration)

To converge to the _optimal_ policy (which is deterministic, not random), we must anneal $\epsilon$ over time.

- **GLIE condition:** $\epsilon_k \to 0$ as $k \to \infty$.
    
- If we satisfy GLIE, Monte Carlo Control converges to the optimal action-value function $q^*(s,a)$.
    

---

## 4. On-Policy TD Control: SARSA

Monte Carlo is slow (high variance, must wait for episode end). We want to use Temporal Difference (TD) learning for control.

### The SARSA Algorithm

This is the quintessential On-Policy control algorithm. The name comes from the sequence of experience in a transition:

$$(S, A, R, S', A')$$

1. **S**: Start in state $S$.
    
2. **A**: Take action $A$ (chosen by current policy).
    
3. **R**: Get reward $R$.
    
4. **S'**: End up in state $S'$.
    
5. **A'**: Choose next action $A'$ (from the _same_ current policy).
    

The Update Rule:

$$Q(S, A) \leftarrow Q(S, A) + \alpha [ \underbrace{R + \gamma Q(S', A')}_{\text{TD Target}} - Q(S, A) ]$$

### Intuition

We are updating the value of the current step $(S,A)$ based on the value of the _actual next step_ $(S', A')$ we plan to take. Because $A'$ is chosen by our current policy (which might be random/exploratory), we learn the value of _that specific exploratory behavior_.

### Convergence

SARSA converges to $Q^*(s,a)$ if:

1. Policy sequences satisfy GLIE (e.g., decaying $\epsilon$).
    
2. Step-sizes $\alpha_t$ satisfy Robbins-Monro conditions ($\sum \alpha_t = \infty, \sum \alpha_t^2 < \infty$).
    

---

## 5. n-Step SARSA and Eligibility Traces

Just like in prediction, we can look $n$ steps ahead for better efficiency.

### n-Step SARSA

- **n=1 (SARSA):** $q_t^{(1)} = R_{t+1} + \gamma Q(S_{t+1}, A_{t+1})$
    
- **n=2:** $q_t^{(2)} = R_{t+1} + \gamma R_{t+2} + \gamma^2 Q(S_{t+2}, A_{t+2})$
    
- **n=$\infty$ (MC):** Full return $G_t$.
    

### SARSA($\lambda$)

We average all $n$-step returns using $\lambda$.

Forward View:

$$q_t^\lambda = (1-\lambda) \sum_{n=1}^\infty \lambda^{n-1} q_t^{(n)}$$

Backward View (Eligibility Traces):

We maintain an eligibility trace $E_t(s,a)$ for every state-action pair.

- Did we visit $(s,a)$? Increase eligibility.
    
- Every step: Decay eligibility by $\gamma\lambda$.
    

$$E_t(s,a) = \gamma \lambda E_{t-1}(s,a) + \mathbf{1}(S_t=s, A_t=a)$$

$$Q(s,a) \leftarrow Q(s,a) + \alpha \delta_t E_t(s,a)$$

(Where $\delta_t$ is the standard SARSA TD error)

**Intuition:** The error "echoes" backwards to all recent state-action pairs that led to the reward.

---

## 6. Off-Policy Learning: Q-Learning

So far, we looked at **On-Policy** (learning about the job while doing the job). Now we look at **Off-Policy** (learning about the optimal job while behaving randomly).

### Motivation

Why Off-Policy?

1. Learn from observing humans or other agents.
    
2. Re-use old experience (experience replay).
    
3. **Learn the optimal policy while following an exploratory policy.**
    

### The Q-Learning Algorithm (SARS-Max)

This is one of the most important algorithms in RL history.

We decouple the behavior policy from the target policy.

- **Behavior Policy:** Choose action $A$ using $\epsilon$-greedy (to explore).
    
- **Target Policy:** Assume next action is Greedy (to optimize).
    

The Update Rule:

$$Q(S, A) \leftarrow Q(S, A) + \alpha [ R + \gamma \max_{a'} Q(S', a') - Q(S, A) ]$$

Notice the difference from SARSA:

- **SARSA:** uses $Q(S', A')$ (the action we _actually_ took).
    
- **Q-Learning:** uses $\max_{a'} Q(S', a')$ (the action we _would_ take if we were purely optimal).
    

### Intuition: The Bellman Optimality Equation

Q-Learning directly approximates the Bellman Optimality Equation. It ignores the fact that we might act randomly next step. It aggressively assumes, "If I played optimally from the next state onwards, what would the value be?"

### Cliff Walking Example: SARSA vs. Q-Learning

Imagine a path along a cliff.

- **Optimal Path:** Walk right on the edge. (Fastest, but risky).
    
- **Safe Path:** Walk a few steps away from the edge. (Slower, but safe).
    

If trained with $\epsilon$-greedy (random exploration):

- **Q-Learning:** Learns the **Optimal Path** (on the edge). It assumes "I will act optimally next," ignoring the random $\epsilon$ risk. However, during training, it will fall off the cliff often because it _is_ actually acting randomly.
    
- **SARSA:** Learns the **Safe Path**. It sees that "When I am on the edge, sometimes I randomly jump off." It incorporates this risk into the value calculation and prefers the safer route.
    

---

## 7. Summary: DP vs. TD vs. MC

We can visualize the relationship between these methods via their backup diagrams.

|**Method**|**Bellman Equation**|**Target**|**Type**|
|---|---|---|---|
|**Iterative Policy Evaluation**|Expectation|$\mathbb{E}[R + \gamma V(S')]$|DP / Prediction|
|**Q-Policy Iteration**|Expectation|$\mathbb{E}[R + \gamma Q(S', A')]$|DP / Control|
|**Q-Value Iteration**|Optimality|$\mathbb{E}[R + \gamma \max Q(S', a')]$|DP / Control|
|**SARSA**|Expectation (Sample)|$R + \gamma Q(S', A')$|TD / On-Policy|
|**Q-Learning**|Optimality (Sample)|$R + \gamma \max Q(S', a')$|TD / Off-Policy|

### Practical Takeaways

1. **SARSA** is a safer, more conservative learner during training. It cares about the policy you are _actually_ following.
    
2. **Q-Learning** is an aggressive learner. It learns the optimal value function regardless of how you are behaving right now (assuming you visit states enough).
    
3. **Off-Policy** is generally harder and can be unstable, but Q-Learning is a special stable case in tabular settings.
    

**Associated URL:** [http://www.youtube.com/watch?v=0g4j2k_Ggc4](http://www.youtube.com/watch?v=0g4j2k_Ggc4)


Here is a comprehensive, deep-dive lecture on **Value Function Approximation**, based on Lecture 6 of David Silver’s Reinforcement Learning course.

This lecture marks a pivotal transition. In previous lectures, we used **tables** (arrays) to store value functions. Now, we scale up to solve problems where tables are impossible (e.g., Go, Helicopter control, real life) by using **Function Approximation**.

---

# Lecture 6: Value Function Approximation

## 1. The Motivation: Scaling Up

In small "Toy Problems" (like GridWorld), we can maintain a lookup table:

- $V(s)$ for every state $s$.
    
- $Q(s,a)$ for every state-action pair.
    

**The Problem:** Real-world state spaces are too big.

- **Backgammon:** $10^{20}$ states.
    
- **Go:** $10^{170}$ states.
    
- **Helicopter:** Continuous state space (infinite states).
    

We cannot visit every state. We cannot store every value.

Solution: We need to Generalize. If we learn that one specific configuration in Go is "bad," we should infer that a similar configuration is also likely "bad."

We use a Function Approximator $\hat{v}(s, \mathbf{w})$ parameterized by weights $\mathbf{w}$.

$$\hat{v}(s, \mathbf{w}) \approx v_\pi(s)$$

---

## 2. Gradient Descent and The Objective

We treat Reinforcement Learning as a **Supervised Learning** problem where the "labels" are the returns (or estimated returns) we observe.

### The Objective Function $J(\mathbf{w})$

We want to minimize the error between our approximate value $\hat{v}(s, \mathbf{w})$ and the true value $v_\pi(s)$. We define the Mean Squared Error (MSE):

$$J(\mathbf{w}) = \mathbb{E}_\pi \left[ (v_\pi(S) - \hat{v}(S, \mathbf{w}))^2 \right]$$

### Gradient Descent

To minimize $J(\mathbf{w})$, we move the weights $\mathbf{w}$ down the gradient of the error.

$$\Delta \mathbf{w} = -\frac{1}{2} \alpha \nabla_\mathbf{w} J(\mathbf{w})$$

Using the Chain Rule:

$$\nabla_\mathbf{w} J(\mathbf{w}) = 2 (v_\pi(S) - \hat{v}(S, \mathbf{w})) \nabla \hat{v}(S, \mathbf{w})$$

Thus, the update rule becomes:

$$\Delta \mathbf{w} = \alpha \underbrace{(v_\pi(S) - \hat{v}(S, \mathbf{w}))}_{\text{Error}} \underbrace{\nabla_\mathbf{w} \hat{v}(S, \mathbf{w})}_{\text{Direction}}$$

---

## 3. Incremental Prediction Methods

In RL, we don't have the "true" value $v_\pi(S)$. We only have estimates (targets). We substitute the target into the gradient descent update.

### 1. Monte Carlo with VFA

The target is the actual return $G_t$. This is unbiased but high variance.

$$\Delta \mathbf{w} = \alpha (G_t - \hat{v}(S_t, \mathbf{w})) \nabla \hat{v}(S_t, \mathbf{w})$$

### 2. Temporal Difference (TD(0)) with VFA

The target is the bootstrapped estimate $R_{t+1} + \gamma \hat{v}(S_{t+1}, \mathbf{w})$.

$$\Delta \mathbf{w} = \alpha (R_{t+1} + \gamma \hat{v}(S_{t+1}, \mathbf{w}) - \hat{v}(S_t, \mathbf{w})) \nabla \hat{v}(S_t, \mathbf{w})$$

- **Note (Semi-Gradient):** Technically, the target $R + \gamma \hat{v}(S', \mathbf{w})$ _also_ depends on $\mathbf{w}$. A true gradient method would take the derivative of the target too. However, in practice, we ignore this and treat the target as fixed. This is called **Semi-Gradient Descent**.
    

### 3. TD($\lambda$) with VFA

We can use eligibility traces to generalize the backward view to function approximation.

The eligibility trace accumulates the gradients:

$$E_t = \gamma \lambda E_{t-1} + \nabla \hat{v}(S_t, \mathbf{w})$$

The update is simply the TD-error times the trace:

$$\delta_t = R_{t+1} + \gamma \hat{v}(S_{t+1}, \mathbf{w}) - \hat{v}(S_t, \mathbf{w})$$

$$\Delta \mathbf{w} = \alpha \delta_t E_t$$

---

## 4. Linear Function Approximation

While Neural Networks are popular, **Linear** function approximation is easier to understand and analyze.

We represent a state $S$ by a Feature Vector $\mathbf{x}(S)$:

$$\mathbf{x}(S) = \begin{pmatrix} x_1(S) \\ \vdots \\ x_n(S) \end{pmatrix}$$

- _Example features:_ Distance to wall, "Is piece at C4?", Speed of robot.
    

The value function is a linear combination of features and weights:

$$\hat{v}(S, \mathbf{w}) = \mathbf{x}(S)^T \mathbf{w} = \sum_{j=1}^n x_j(S) w_j$$

The Beauty of Linear VFA:

The gradient is simply the feature vector itself!

$$\nabla_\mathbf{w} \hat{v}(S, \mathbf{w}) = \mathbf{x}(S)$$

So the update rule simplifies to:

$$\Delta \mathbf{w} = \alpha (\text{Target} - \hat{v}(S, \mathbf{w})) \mathbf{x}(S)$$

- _Intuition:_ If the error is positive (we under-predicted), we increase the weights of the active features $\mathbf{x}(S)$.
    

---

## 5. Control with Function Approximation

Now we move from predicting $V(s)$ to controlling $Q(s,a)$.

### Action-Value Approximation

We approximate $Q(s, a, \mathbf{w}) \approx q_\pi(s,a)$.

Usually, we input the state $S$ and output a separate value for each action $a$.

$$\hat{q}(S, a, \mathbf{w}) = \mathbf{x}(S, a)^T \mathbf{w}$$

### Algorithms

We simply plug the approximate $Q$ into our standard control loops.

1. **Linear SARSA (On-Policy):**
    
    - Target: $R + \gamma \hat{q}(S', A', \mathbf{w})$
        
    - $\Delta \mathbf{w} = \alpha (R + \gamma \hat{q}(S', A', \mathbf{w}) - \hat{q}(S, A, \mathbf{w})) \nabla \hat{q}(S, A, \mathbf{w})$
        
2. **Linear Q-Learning (Off-Policy):**
    
    - Target: $R + \gamma \max_{a'} \hat{q}(S', a', \mathbf{w})$
        
    - $\Delta \mathbf{w} = \alpha (R + \gamma \max_{a'} \hat{q}(S', a', \mathbf{w}) - \hat{q}(S, A, \mathbf{w})) \nabla \hat{q}(S, A, \mathbf{w})$
        

---

## 6. Batch Methods and Experience Replay

Standard Gradient Descent (SGD) is inefficient because it throws away data after one use. It is also unstable because adjacent samples ($S_t, S_{t+1}$) are highly correlated.

### Least Squares Prediction

If we have a whole "batch" of data (experience), we can solve for the **best** weights $\mathbf{w}$ directly (finding the global minimum of the sum of squared errors) without iterating. For linear models, this is just a matrix inversion ($O(N^3)$).

### Experience Replay

Instead of calculating the perfect solution (expensive), we store transitions $(s_t, a_t, r_{t+1}, s_{t+1})$ in a large buffer (memory).

1. Sample a mini-batch of random transitions from memory.
    
2. Perform SGD updates on this batch.
    

**Why is this vital?**

- It breaks the temporal correlation (samples are i.i.d.).
    
- It re-uses data (data efficiency).
    
- **Deep Q-Networks (DQN)** use this technique to stabilize training.
    

---

## 7. The Deadly Triad & Convergence

This is the most theoretically important part of the lecture. When does RL diverge?

We are safe if we use Linear VFA + On-Policy (Simulated by table).

Danger arises when we combine three specific elements, known as the Deadly Triad:

1. **Function Approximation:** Generalizing across states (not a table).
    
2. **Bootstrapping:** Using estimates to update estimates (TD).
    
3. **Off-Policy Learning:** Training on a distribution different from the target policy (e.g., Q-Learning).
    

### Baird's Counterexample

This is a famous simple 7-state MDP that proves Q-Learning with Linear VFA can diverge to infinity.

- Because we bootstrap off-policy, the "Target" can systematically move further away from the true value faster than the weights can catch up.
    
- The error essentially spirals out of control.
    

### Summary of Convergence

|**Algorithm**|**Table Lookup**|**Linear VFA**|**Non-Linear VFA (Neural Net)**|
|---|---|---|---|
|**Monte Carlo**|Converges|Converges|Converges (mostly)|
|**TD (On-Policy)**|Converges|Converges|Chattery / Diverges|
|**Q-Learning (Off-Policy)**|Converges|**DIVERGES**|**DIVERGES**|

How do we fix this?

Modern Deep RL (like DQN) "breaks" the triad artificially by using Target Networks (freezing the bootstrap target for a while) and Experience Replay (making data look more on-policy/iid).

**Associated URL:** [http://www.youtube.com/watch?v=UoPei5o4fps](https://www.google.com/search?q=http://www.youtube.com/watch%3Fv%3DUoPei5o4fps&authuser=2)

The selected video is the direct source for this lecture (Lecture 6: Value Function Approximation), covering the move from tabular methods to large-scale prediction and control using features and weights.


Here is a complete, deep-dive lecture covering **Value Function Approximation** and **Policy Gradients**.

I have structured this to start with a "Level 1" intuitive explanation (for the 5-year-old in us all) before graduating to "Level 50" mathematical rigor suitable for a researcher.

---

# Part 1: Value Function Approximation (VFA)

## 1. The Intuition (ELI5): Memorizing vs. Understanding

Imagine you are trying to learn how to play a video game.

The "Table Lookup" Way (Previous Lectures):

You take a giant notebook. Every time you see a specific screen (the enemy is exactly 5 pixels to the left, the sky is blue, your health is 90), you write down a number: "This situation is worth 10 points."

- **Problem:** If the enemy moves **one pixel** to the right, your notebook is useless. You've never seen _exactly_ this screen before. You have to learn it from scratch.
    
- **Real World:** Games like Go have more states than atoms in the universe. You cannot buy a notebook big enough.
    

The "Function Approximation" Way (This Lecture):

Instead of writing down every single screen, you learn rules (a function).

You look at the screen and say: "Okay, I don't care about the exact pixels. I see an enemy is close and my health is high. That is generally good."

- **Benefit:** You can now handle situations you have never seen before. You have learned to **generalize**.
    

---

## 2. The Formalism: From Tables to Weights

In previous lectures, our value function $V(s)$ was a big array. Now, we replace that array with a function $\hat{v}(s, \mathbf{w})$ that depends on a set of adjustable parameters (weights) $\mathbf{w}$.

$$\hat{v}(s, \mathbf{w}) \approx v_\pi(s)$$

- **Input:** State $s$.
    
- **Parameters:** Weights $\mathbf{w}$ (e.g., the strength of connections in a neural network).
    
- **Output:** The estimated value.
    

### The Objective Function

We treat this as a **Supervised Learning** problem. We want our approximation $\hat{v}$ to be as close as possible to the "true" value $v_\pi$. We measure "closeness" using Mean Squared Error (MSE):

$$J(\mathbf{w}) = \mathbb{E}_\pi \left[ (v_\pi(S) - \hat{v}(S, \mathbf{w}))^2 \right]$$

We want to find the best weights $\mathbf{w}$ that minimize this error $J(\mathbf{w})$.

### Gradient Descent

Imagine you are standing on a foggy mountain (the error surface). You want to get to the bottom (zero error). You look at your feet, find the steepest slope downwards, and take a step.

Mathematically, the "steepest slope" is the Gradient ($\nabla$).

To update our weights, we move in the opposite direction of the gradient:

$$\mathbf{w} \leftarrow \mathbf{w} - \alpha \nabla_\mathbf{w} J(\mathbf{w})$$

- $\alpha$: The step size (learning rate).
    

Using the Chain Rule of calculus, the gradient of the squared error is:

$$\nabla_\mathbf{w} J(\mathbf{w}) = -2 (v_\pi(S) - \hat{v}(S, \mathbf{w})) \nabla_\mathbf{w} \hat{v}(S, \mathbf{w})$$

So our update rule becomes:

$$\Delta \mathbf{w} = \alpha \underbrace{(v_\pi(S) - \hat{v}(S, \mathbf{w}))}_{\text{The Error}} \underbrace{\nabla_\mathbf{w} \hat{v}(S, \mathbf{w})}_{\text{Sensitivity}}$$

### The "Target" Problem

Wait! In the equation above, we used $v_\pi(S)$—the _true_ value of the state. But we don't _know_ the true value! If we did, we wouldn't need to learn anything.

So, we cheat. We substitute a **Target** in place of $v_\pi(S)$.

1. **Monte Carlo Target:** Wait until the end of the episode and use the actual return $G_t$.
    
    - $\text{Target} = G_t$
        
    - _Pros:_ Unbiased (true data). _Cons:_ High variance (noisy).
        
2. **TD(0) Target:** Use your own guess from the next step (Bootstrapping).
    
    - $\text{Target} = R_{t+1} + \gamma \hat{v}(S_{t+1}, \mathbf{w})$
        
    - _Pros:_ Low variance. _Cons:_ Biased (your guess might be wrong).
        

---

## 3. Linear Function Approximation

The simplest way to approximate a function is with a line (or a hyperplane).

We describe a state $s$ using a Feature Vector $\mathbf{x}(s)$.

- $x_1$: "Distance to goal"
    
- $x_2$: "Number of enemies"
    
- $x_3$: "Is having fun?" (1 or 0)
    

The value is just the sum of features multiplied by weights:

$$\hat{v}(s, \mathbf{w}) = \mathbf{x}(s)^T \mathbf{w} = \sum_{j=1}^n x_j(s) w_j$$

Why do we love Linear approximation?

The gradient is incredibly simple. If you ask, "How much does the value change if I change weight $w_j$?", the answer is just "How active feature $x_j$ is."

$$\nabla_\mathbf{w} \hat{v}(s, \mathbf{w}) = \mathbf{x}(s)$$

The Linear Update Rule:

$$\Delta \mathbf{w} = \alpha (\text{Target} - \hat{v}(s, \mathbf{w})) \mathbf{x}(s)$$

Intuition: If we underestimated the value (Error is positive), we look at all the features that were active ($\mathbf{x}(s)$) and increase their weights.

---

# Part 2: Policy Gradients

## 1. The Intuition (ELI5): Score vs. Instinct

Up until now, we have been **Value-Based**.

- **Value-Based:** "If I go North, I get 10 points. If I go South, I get 5 points. 10 is bigger than 5, so I will go North."
    
    - You calculate the score of every option, then pick the best.
        

Now, we introduce **Policy-Based** (Policy Gradients).

- **Policy-Based:** "When I see a scary monster, I run. I don't know _exactly_ how many points running saves me, but my gut says RUN!"
    
    - You learn the **probabilities** of actions directly. You tune your "instincts" (parameters) to maximize rewards.
        

**Why switch?**

1. **Continuous Actions:** In a robot, you can't calculate the "score" for every possible angle of the arm ($30.1^\circ, 30.01^\circ, \dots$). There are infinite actions. A policy can just output "30.5 degrees" directly.
    
2. **Stochastic Policies:** Sometimes the best strategy is to be random (like Rock-Paper-Scissors). Value methods are deterministic (always pick the max). Policy gradients can learn to be random.
    

---

## 2. The Formalism: Parameterizing the Policy

We define a policy $\pi_\theta(s,a)$ with parameters $\theta$.

$$\pi_\theta(s,a) = \mathbb{P}[a | s, \theta]$$

- _Example:_ A Neural Network takes the state image $s$ and outputs a probability for each action (Up, Down, Left, Right). The weights of this network are $\theta$.
    

### The Goal

We want to maximize the total expected reward $J(\theta)$.

$$J(\theta) = \mathbb{E}_{\pi_\theta} [r_1 + r_2 + r_3 + \dots]$$

To maximize $J(\theta)$, we use Gradient Ascent. We want to change $\theta$ to make the good actions more likely.

$$\theta \leftarrow \theta + \alpha \nabla_\theta J(\theta)$$

---

## 3. The Policy Gradient Theorem (The "Magic")

Finding the gradient $\nabla_\theta J(\theta)$ looks impossible. The reward depends on the actions, which depend on the policy, which depends on the states... and the states depend on the environment dynamics! We don't know the environment dynamics!

The Theorem:

Ideally, we want to know "how does changing the policy change the total reward?"

Ideally, we don't want to calculate "how does the policy change the state distribution?" (because that's hard).

The **Policy Gradient Theorem** proves that we don't need to know the environment dynamics. The gradient is simply:

$$\nabla_\theta J(\theta) = \mathbb{E}_{\pi_\theta} \left[ \nabla_\theta \log \pi_\theta(s,a) \cdot Q^{\pi_\theta}(s,a) \right]$$

Let's break this down step-by-step.

### The "Score Function" Trick (Log-Derivative)

How do we get that "log"?

Calculus identity: $\nabla x = x \cdot \nabla \log x$.

Therefore:

$$\nabla_\theta \pi_\theta(s,a) = \pi_\theta(s,a) \cdot \nabla_\theta \log \pi_\theta(s,a)$$

This $\nabla_\theta \log \pi_\theta(s,a)$ is called the Score Function. It tells us how to adjust the weights $\theta$ to make action $a$ more likely.

### Interpretation of the Theorem

$$\Delta \theta \propto \underbrace{\nabla_\theta \log \pi_\theta(s,a)}_{\text{Direction to increase prob of } a} \cdot \underbrace{Q(s,a)}_{\text{How good action } a \text{ actually is}}$$

- If $Q(s,a)$ is high (good action): Move $\theta$ in the direction that makes $a$ **more** likely.
    
- If $Q(s,a)$ is low (bad action): Move $\theta$ in the direction that makes $a$ **less** likely (negative sign).
    

---

## 4. The REINFORCE Algorithm (Monte Carlo PG)

This is the most basic Policy Gradient algorithm.

1. Play: Run an entire episode using your current policy $\pi_\theta$.
    
    $$s_1, a_1, r_2, s_2, a_2, \dots, s_T$$
    
2. **Evaluate:** For every step $t$, calculate the total return $G_t$ (the actual reward sum you got). We use $G_t$ as an estimate for $Q(s_t, a_t)$.
    
3. Update:
    
    $$\theta \leftarrow \theta + \alpha \gamma^t G_t \nabla_\theta \log \pi_\theta(s_t, a_t)$$
    

**Intuition:**

- You played a game. You won (High $G_t$).
    
- The algorithm looks at _every_ move you made and says "Whatever you did, do it more!"
    
- You played a game. You lost (Negative $G_t$).
    
- The algorithm says "Whatever you did, do it less!"
    

The Problem:

If you play a game that lasts 1000 steps and you lose, REINFORCE blames every single action equally. Even if 999 actions were brilliant and only the last one was bad. This creates High Variance. It takes a long time to learn.

---

## 5. Reducing Variance: Baselines

To fix the variance, we subtract a Baseline $B(s)$ from the return.

$$\nabla_\theta J(\theta) = \mathbb{E} \left[ \nabla_\theta \log \pi_\theta(s,a) (Q(s,a) - B(s)) \right]$$

A common baseline is the Value Function $V(s)$ (the average goodness of the state).

The Advantage Function $A(s,a)$:

$$A(s,a) = Q(s,a) - V(s)$$

- $Q(s,a)$: How good was _this specific action_?
    
- $V(s)$: How good is the situation usually?
    
- $A(s,a)$: How much **better** was this action than the average action?
    

If $A(s,a) > 0$, the action was surprisingly good $\to$ Increase probability.

If $A(s,a) < 0$, the action was worse than average $\to$ Decrease probability.

This leads to **Actor-Critic** methods (where we learn both the Policy/Actor and the Value/Critic), which is the foundation of modern AI like ChatGPT (RLHF) and AlphaGo.

---

## Summary Cheat Sheet

|**Concept**|**Value-Based (VFA)**|**Policy-Based (PG)**|
|---|---|---|
|**What is learned?**|Values $Q(s,a)$ or $V(s)$|Probabilities $\pi(a|
|**How to act?**|$\text{argmax}_a Q(s,a)$|Sample $a \sim \pi(a|
|**Objective**|Minimize Error (MSE)|Maximize Reward $J(\theta)$|
|**Update Rule**|Gradient Descent|Gradient Ascent|
|**Best for...**|Discrete actions, deterministic|Continuous actions, stochastic|
|**Main Algorithm**|DQN (Deep Q-Network)|REINFORCE / PPO|

Next Step:

Would you like me to create an implementation guide for the REINFORCE algorithm in Python/PyTorch so you can see this math come to life?


Here is a comprehensive lecture on the **Actor-Critic** method.

This is the "Endgame" of standard Reinforcement Learning. It combines everything we have learned so far: the stability of **Value functions** (Lecture 4-6) and the flexibility of **Policy Gradients** (Lecture 7).

---

# Lecture 8: Actor-Critic Methods

## 1. The Intuition (ELI5): The Athlete and The Coach

Imagine a child learning to play tennis. We can view this learning process in two ways:

1. **The Actor (The Athlete):** This is the child holding the racket. They control the muscles. They decide whether to swing left or right. They are the ones actually **doing** the policy.
    
2. **The Critic (The Coach):** This is the expert standing on the sidelines. The coach doesn't hold a racket. They can't hit the ball. Their only job is to watch the athlete and say, "That swing was bad," or "That footwork was excellent."
    

**Why do we need both?**

- **If you only have the Athlete (REINFORCE/Policy Gradient):** The athlete plays a whole match. If they lose, they feel bad about _every single move_ they made. They don't know _which_ specific move caused the loss. Learning is slow and messy (High Variance).
    
- **If you only have the Coach (Q-Learning/Value-Based):** The coach knows the game perfectly (the Value Function), but they can't physically move the muscles. They can calculate the score, but they can't generate the continuous, complex actions needed to win.
    

Actor-Critic combines them:

The Athlete (Actor) tries a move. The Coach (Critic) immediately critiques that specific move. The Athlete adjusts their behavior instantly based on the Coach's feedback, rather than waiting for the match to end.

---

## 2. The Motivation: Variance Reduction

To understand the math, we must recall the Policy Gradient equation from the previous lecture.

The update for our policy parameters $\theta$ is:

$$\nabla_\theta J(\theta) = \mathbb{E} \left[ \nabla_\theta \log \pi_\theta(s,a) \cdot G_t \right]$$

- $\nabla_\theta \log \pi_\theta(s,a)$: "The direction to make action $a$ more likely."
    
- $G_t$: "The total return (score) of the episode."
    

**The Problem:** $G_t$ is the return from the _start_ to the _end_ of the episode. It is effectively a random number with huge noise.

- Sometimes you get lucky, sometimes unlucky.
    
- One bad action in a sequence of 100 good ones ruins the $G_t$.
    
- This **High Variance** means we need millions of samples to learn.
    

The Solution: Replace the noisy return $G_t$ with an estimate learned by a neural network.

We introduce a second set of weights, $w$, to approximate the Value Function:

$$Q_w(s, a) \approx \text{Expected Return}$$

Now, instead of waiting for the real return $G_t$, we ask the Critic: "Hey, how good was that move?"

---

## 3. The Architecture: Two Brains

In Actor-Critic, we maintain **two** distinct function approximators (usually two Neural Networks):

1. **The Critic (Value-Based):**
    
    - **Goal:** Learn to predict the return (Value Function $V_w(s)$ or $Q_w(s,a)$).
        
    - **Updates:** Uses TD Learning (minimizing MSE error).
        
    - **Input:** State $s$.
        
    - **Output:** A scalar value (e.g., "7.5 points").
        
2. **The Actor (Policy-Based):**
    
    - **Goal:** Learn how to act ($\pi_\theta(a|s)$).
        
    - **Updates:** Uses Policy Gradients guided by the Critic.
        
    - **Input:** State $s$.
        
    - **Output:** Probabilities over actions (e.g., "Left: 80%, Right: 20%").
        

---

## 4. The Math: Step-by-Step Derivation

We want to perform gradient ascent on the policy $\theta$.

### Step 1: The Bootstrapped Critic

We don't use the full return $G_t$. We use the TD Target.

Recall the TD target from Lecture 4:

$$\text{Target} = R_{t+1} + \gamma V_w(S_{t+1})$$

The Critic updates its own weights $w$ to get closer to this target (just like standard TD learning):

$$\text{Loss}_w = \left( (R_{t+1} + \gamma V_w(S_{t+1})) - V_w(S_t) \right)^2$$

### Step 2: The Advantage Function

We don't just want to know if a state is good ($Q$). We want to know if an action was **better than expected**. This is the **Advantage** $A(s,a)$.

$$A(s,a) = Q(s,a) - V(s)$$

Since we don't know $Q$, we approximate it using the reward + next state value:

$$A(s_t, a_t) \approx \underbrace{R_{t+1} + \gamma V_w(S_{t+1})}_{\text{Estimated Q}} - \underbrace{V_w(S_t)}_{\text{Baseline}}$$

Wait! Look closely at that equation.

$$R_{t+1} + \gamma V_w(S_{t+1}) - V_w(S_t)$$

This is exactly the TD Error ($\delta_t$)!

**Crucial Insight:** The TD Error $\delta_t$ is an unbiased estimate of the Advantage Function.

- If $\delta_t > 0$: The transition was better than the Critic expected. (Good surprise).
    
- If $\delta_t < 0$: The transition was worse than the Critic expected. (Bad surprise).
    

### Step 3: The Actor Update

Now the Actor updates its policy $\theta$. It wants to increase the probability of actions that had a **positive TD error** (positive advantage).

$$\theta \leftarrow \theta + \alpha \nabla_\theta \log \pi_\theta(S_t, A_t) \cdot \delta_t$$

- $\nabla \log \pi$: The "steering wheel" to change probability.
    
- $\delta_t$: The "gas pedal" (if positive) or "brake" (if negative).
    

---

## 5. The Algorithm: Online Actor-Critic

Here is the full loop for an Online Actor-Critic agent (one step at a time):

1. **Observe** current state $S_t$.
    
2. **Act:** Sample action $A_t \sim \pi_\theta(S_t)$.
    
3. **Step:** Execute $A_t$, get reward $R_{t+1}$ and next state $S_{t+1}$.
    
4. Critique (Calculate TD Error):
    
    Using the Critic network $V_w$:
    
    $$\delta_t = R_{t+1} + \gamma V_w(S_{t+1}) - V_w(S_t)$$
    
5. Update Critic (Coach learns):
    
    Minimize the square of the TD error.
    
    $$w \leftarrow w + \beta \delta_t \nabla_w V_w(S_t)$$
    
6. Update Actor (Athlete learns):
    
    Move policy in direction of advantage.
    
    $$\theta \leftarrow \theta + \alpha \delta_t \nabla_\theta \log \pi_\theta(S_t, A_t)$$
    
7. **Repeat.**
    

---

## 6. Bias-Variance Trade-off: The Spectrum

It is helpful to see where Actor-Critic sits on the spectrum of RL algorithms.

|**Algorithm**|**Target Used**|**Bias**|**Variance**|
|---|---|---|---|
|**REINFORCE**|Return $G_t$|Zero|High|
|**Actor-Critic**|Estimate $R + \gamma V(S')$|**Some**|**Medium**|
|**Pure Critic (TD)**|Estimate $R + \gamma V(S')$|High|Low|

- **REINFORCE** is unbiased but noisy (slow).
    
- **Pure Critic** is stable but biased (if the value function approximation is wrong, the policy will be permanently wrong).
    
- **Actor-Critic** is the "Goldilocks" zone. We introduce a little bias (by using the Critic) to drastically reduce variance (speeding up learning).
    

---

## 7. Modern Variants (Deep Learning Era)

If you read papers from DeepMind or OpenAI today, they use advanced versions of this:

1. A2C (Advantage Actor Critic):
    
    Instead of updating after every single step (which is shaky), we run $N$ parallel workers (e.g., 16 robots playing at once). We average their gradients. This stabilizes the learning.
    
2. A3C (Asynchronous Advantage Actor Critic):
    
    The workers run on different threads and update the global brain asynchronously. (This was the algorithm that famously mastered Doom and Super Mario).
    
3. PPO (Proximal Policy Optimization):
    
    Standard Actor-Critic can be unstable. If the Actor takes a "too big" step update, the policy might collapse. PPO adds a mathematical constraint (clipping) to ensure the Actor only makes small, safe changes to its behavior. This is currently the industry standard (used to train ChatGPT).
    

---

## 8. Summary

- **Actor (Policy):** Decides _what to do_. It's probabilistic.
    
- **Critic (Value):** Decides _how good_ that was. It's deterministic.
    
- **Advantage ($A$):** The Critic tells the Actor not just the raw value, but how much _better_ that action was compared to the average.
    
- **TD Error ($\delta$):** We use the one-step prediction error as our signal for Advantage. This allows us to learn online, step-by-step, without waiting for the episode to finish.
    

Next Step:

You have now covered the core pillars of classical RL. The natural next step is to look at Exploration strategies or Model-Based RL (where we try to learn the physics of the world itself). Which interests you more?

Here is a comprehensive, deep-dive lecture on **Proximal Policy Optimization (PPO)**.

This is the algorithm that currently runs the world. It is the default algorithm at OpenAI, DeepMind, and Anthropic. It was used to train the Dota 2 bot (OpenAI Five) and is the engine behind the Reinforcement Learning from Human Feedback (RLHF) that powers ChatGPT.

---

# Lecture 9: Proximal Policy Optimization (PPO)

## 1. The Motivation: The "Step Size" Problem

In the previous lecture (Actor-Critic), we learned that we update our policy by taking a step in the direction of the gradient:

$$\theta \leftarrow \theta + \alpha \nabla J(\theta)$$

**The Problem:** How big should the step size $\alpha$ be?

- **If $\alpha$ is too small:** The agent learns agonizingly slowly. It takes millions of years to solve a simple maze.
    
- **If $\alpha$ is too big:** The agent makes a massive update to its weights. The policy changes drastically.
    
    - _Disaster Scenario:_ The agent tries a new move, gets a bad reward, and the "big step" destroys its previous good knowledge. Because the policy is now broken, it gathers _bad data_ in the next episode, which leads to _worse updates_. The agent effectively "falls off a cliff" and never recovers.
        

The PPO Intuition (ELI5): The Conservative Learner

Imagine you are learning to play golf.

- **Standard Policy Gradient:** "I missed the hole? Okay, for the next swing, I will hold the club completely differently, stand on one leg, and close my eyes." (Too risky).
    
- **PPO:** "I missed the hole? Okay, I will adjust my grip by _1 millimeter_ to the right. If that works, I'll do another millimeter. If it doesn't, I haven't ruined my swing."
    

PPO enforces a **Trust Region**: "You are allowed to change your policy, but only a little bit at a time."

---

## 2. From "On-Policy" to "Surrogate" Objectives

To understand PPO, we must introduce the concept of the **Probability Ratio**.

In standard Policy Gradients, we use the log-probability. But if we want to update the policy multiple times using the _same_ batch of data (to be more efficient), we need to measure how much the _new_ policy $\pi_\theta$ differs from the _old_ policy $\pi_{\theta_{old}}$ that collected the data.

We define the ratio $r_t(\theta)$:

$$r_t(\theta) = \frac{\pi_\theta(a_t | s_t)}{\pi_{\theta_{old}}(a_t | s_t)}$$

- If $r_t(\theta) = 1$: The new policy is exactly the same as the old one.
    
- If $r_t(\theta) > 1$: The new policy thinks this action is **more likely** than the old policy did.
    
- If $r_t(\theta) < 1$: The new policy thinks this action is **less likely**.
    

We can rewrite the standard objective function using this ratio:

$$L^{CPI}(\theta) = \hat{\mathbb{E}}_t \left[ r_t(\theta) \hat{A}_t \right]$$

(CPI stands for Conservative Policy Iteration)

If we maximize this $L^{CPI}$ without constraints, $r_t$ will explode to infinity (the step size problem). We need to constrain it.

---

## 3. The Core Mechanism: The Clipped Objective

This is the heart of PPO. It is arguably the most valuable single equation in modern Deep RL.

We want to maximize the objective, but we want to punish the agent if $r_t(\theta)$ moves too far away from 1.

$$L^{CLIP}(\theta) = \hat{\mathbb{E}}_t \left[ \min \left( r_t(\theta) \hat{A}_t, \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon) \hat{A}_t \right) \right]$$

Let's break this down piece by piece.

- **$\epsilon$ (Epsilon):** A hyperparameter, usually $0.2$. This defines our "safe zone." We allow the ratio to vary between $0.8$ and $1.2$.
    
- **$\hat{A}_t$:** The Advantage (how good the action was).
    

### Case Study 1: The Action was Good ($A > 0$)

Suppose we took an action and it resulted in a huge reward ($A = +1.5$). We want to **increase** the probability of this action (make $r_t > 1$).

- **Scenario A:** The ratio $r_t$ increases from $1.0 \to 1.1$.
    
    - This is inside the safe zone ($1 + 0.2$).
        
    - We use the unclipped term: $1.1 \times 1.5$. We keep pushing.
        
- **Scenario B:** The ratio $r_t$ increases to $1.3$.
    
    - This is **outside** the safe zone.
        
    - The clipping kicks in: $\text{clip}(1.3, 0.8, 1.2) = 1.2$.
        
    - The gradient becomes zero! We stop pushing. We say, "Okay, you've increased the probability enough. Don't get greedy."
        

### Case Study 2: The Action was Bad ($A < 0$)

Suppose the action caused a crash ($A = -0.5$). We want to **decrease** the probability (make $r_t < 1$).

- **Scenario A:** The ratio $r_t$ decreases from $1.0 \to 0.9$.
    
    - Inside safe zone. We keep pushing it down.
        
- **Scenario B:** The ratio $r_t$ drops to $0.01$ (probability nearly zero).
    
    - This is a massive change. It might destroy exploration.
        
    - The clipping kicks in at $0.8$. We stop the update.
        

**Intuition:** The $\min$ operator effectively builds a "flatline" in the objective function. Once the policy changes _too much_, the reward for changing it further becomes zero. The agent loses the incentive to make massive jumps.

---

## 4. The Algorithm: PPO Step-by-Step

PPO is surprisingly simple to implement compared to its predecessor (TRPO).

1. Collect Data: Run the current policy $\pi_{\theta_{old}}$ in the environment for $T$ timesteps. Collect trajectories:
    
    $$(s_1, a_1, r_1, \dots, s_T, a_T, r_T)$$
    
2. **Calculate Advantage:** Using the Critic $V(s)$, calculate the advantage $\hat{A}_t$ for every step (usually using GAE - Generalized Advantage Estimation).
    
3. Optimize (The PPO Loop):
    
    For $K$ epochs (e.g., 3-10 times):
    
    - Shuffle the data and break it into mini-batches.
        
    - For each mini-batch, calculate the ratio $r_t(\theta) = \pi_\theta / \pi_{old}$.
        
    - Calculate the clipped loss $L^{CLIP}$.
        
    - Add a Value Function loss (squared error) to train the Critic: $L^{VF} = (V_\theta(s) - V_{target})^2$.
        
    - Add an Entropy Bonus $S[\pi_\theta]$ (to encourage exploration).
        
    - **Total Loss:** $L = L^{CLIP} - c_1 L^{VF} + c_2 S$.
        
    - Update $\theta$ using Adam Optimizer.
        
4. **Update Old Policy:** Set $\theta_{old} \leftarrow \theta$.
    
5. **Repeat.**
    

---

## 5. PPO vs. TRPO (Trust Region Policy Optimization)

PPO didn't appear out of nowhere. It replaced TRPO.

- **TRPO (The "Math Heavy" Older Brother):** TRPO mathematically enforced the trust region using a constraint: $KL(\pi_{old} || \pi_{new}) < \delta$. This required calculating **second-order derivatives** (Hessian matrices) and using Conjugate Gradient descent. It was slow, complicated, and hard to code.
    
- **PPO (The "Practical" Younger Brother):** PPO approximates the TRPO constraint simply by using `clip()`. It only requires first-order derivatives (standard backpropagation). It is much faster and easier to implement.
    

---

## 6. Critical Implementation Details

If you implement PPO in Obsidian or Python, keep these "pro-tips" in mind:

1. Generalized Advantage Estimation (GAE):
    
    PPO rarely uses raw TD error. It uses GAE($\lambda$), which is an exponentially weighted average of $n$-step advantages (similar to TD($\lambda$)). This balances bias and variance perfectly for policy gradients.
    
    $$\hat{A}_t^{GAE} = \sum_{l=0}^\infty (\gamma \lambda)^l \delta_{t+l}$$
    
2. Normalization:
    
    Always normalize your Advantages (subtract mean, divide by std dev) within each batch. This ensures the gradients are always in a stable range.
    
3. Entropy Bonus:
    
    Because PPO is "conservative," it can sometimes get stuck in a suboptimal strategy and refuse to leave (premature convergence). Adding an entropy term forces the probability distribution to stay somewhat "wide" (random) to keep exploring.
    

---

## 7. Summary

- **The Problem:** Standard RL falls off cliffs if the learning rate is too high.
    
- **The Fix:** Limit the update size.
    
- **The Mechanism:** Calculate the ratio between the new policy and the old policy. If the ratio goes beyond $1 \pm \epsilon$, clip the gradient to zero.
    
- **The Result:** An incredibly stable, robust algorithm that allows us to train massive agents (like LLMs) without them collapsing into nonsense.
    

**Associated URL:** [https://openai.com/research/proximal-policy-optimization](https://www.google.com/search?q=https://openai.com/research/proximal-policy-optimization&authuser=2)

Next Step:

Now that you understand the "Brain" (PPO), would you like to explore the "Body"? We can do a deep dive into Environment Design (how to create the Gym/Simulation) or Reward Engineering (how to craft the signal that PPO optimizes). Which is more relevant to your work?



Here is a deep-dive lecture on **PPO for Large Language Models (LLMs)**.

This is the specific machinery behind **RLHF (Reinforcement Learning from Human Feedback)**. It is the bridge between a raw text-predictor (like Base GPT-4) and a helpful assistant (like ChatGPT).

---

# Lecture 10: PPO for Large Language Models (RLHF)

## 1. The Context: Why PPO for Text?

When we pre-train an LLM on the internet, we get a **Base Model**.

- **Base Model's Goal:** Minimize log-loss. Ideally, predict the next word in a document.
    
- **The Problem:** If you ask a Base Model, _"How do I bake a cake?"_, it might reply, _"and then I ate it. The End."_ because it thinks it is completing a story.
    

We want the model to follow instructions and be safe. We used to try to do this with just supervised fine-tuning (SFT), but that hits a ceiling. It is hard to write a dataset for every possible conversation.

**Enter PPO:** Instead of showing the model _what_ to write (Supervised Learning), we let the model write whatever it wants, and then we give it a **Score** (Reward). PPO optimizes the model to maximize this score.

---

## 2. Mapping RL Concepts to LLMs

To understand PPO in this context, we must translate standard RL terms into LLM terms. This is often the biggest hurdle for newcomers.

|**Standard RL Concept**|**LLM Equivalent**|
|---|---|
|**Agent**|The Language Model (Transformer weights).|
|**Environment**|The Prompt + The text generated so far.|
|**State ($s_t$)**|The current Context Window (e.g., "User: Hello \n AI: Hi").|
|**Action ($a_t$)**|The **Next Token** chosen from the vocabulary (e.g., token 2451: " there").|
|**Reward ($r$)**|A scalar score given by a separate **Reward Model** after the full response is generated.|
|**Trajectory**|The sequence of tokens that make up one full response.|

**Key Insight:** In standard RL (like robotics), the environment dynamics are external (physics). In LLM RL, the "environment transition" is simply appending the chosen token to the context.

---

## 3. The Architecture: The "Four Model" Setup

Implementing PPO for LLMs is memory-intensive because we typically keep **four** distinct models (or heads) in VRAM during training:

1. **The Actor (Policy $\pi_\theta$):** The LLM we are training (e.g., Llama-3-8B). It generates the text.
    
2. **The Reference Model ($\pi_{ref}$):** A frozen copy of the LLM before PPO started (usually the SFT model). We need this to calculate the KL divergence (explained below).
    
3. **The Reward Model ($R_\phi$):** A frozen LLM trained to predict human preference scores. It looks at the text and outputs a scalar (e.g., +1.5).
    
4. **The Critic ($V_w$):** A Value Function being trained. It looks at the current token sequence and predicts _how much total reward_ we expect by the end of the sentence.
    

---

## 4. The Critical Addition: KL Penalty

This is the most important mathematical difference between "Robot PPO" and "LLM PPO."

The Problem: Reward Hacking

If you train an LLM purely to maximize the Reward Model's score, it will find "exploits."

- _Example:_ If the Reward Model slightly prefers positive words, the LLM might learn to scream _"LOVE LOVE LOVE LOVE"_ infinitely. The Reward Model gives it a high score, but the text is useless to humans.
    
- _The "Mode Collapse":_ The model forgets how to speak English and focuses only on maximizing numbers.
    

The Fix: The KL Divergence Penalty

We force the PPO model to stay "close" to the Reference Model (the one that knew good English). We modify the reward function $R(x, y)$ at every token step:

$$R_{total}(x, y) = R_{model}(x, y) - \beta \log \left( \frac{\pi_\theta(y|x)}{\pi_{ref}(y|x)} \right)$$

- $R_{model}$: The raw score from the reward model (usually only given at the End of Sequence).
    
- $\pi_\theta / \pi_{ref}$: The ratio of probabilities.
    
- $\beta$ (Beta): The penalty strength (hyperparameter).
    

**Intuition:**

- If the Actor produces a token that the Reference model thinks is highly unlikely (gibberish), the ratio $\pi_\theta / \pi_{ref}$ becomes huge.
    
- The term $-\beta \log(\dots)$ becomes a large **negative penalty**.
    
- **Effect:** "You can be creative to get a high score, but do not drift too far from standard English grammar and facts."
    

---

## 5. The Training Loop (Step-by-Step)

Here is what happens during a single PPO update step for an LLM:

### Phase 1: Rollout (Experience Collection)

1. **Sample Prompt:** Grab a prompt from the dataset (e.g., "Write a poem about rust").
    
2. **Generate:** The Actor ($\pi_\theta$) generates a response token-by-token.
    
3. **Calculate Log-Probs:** We record the log-probability of every token chosen by the Actor. We also calculate the log-probability of those _same_ tokens using the Reference Model.
    

### Phase 2: Evaluation

4. **Get Reward:** Pass the full text (Prompt + Response) to the **Reward Model**. It gives a single scalar score (e.g., 2.0).
    
5. **Distribute Reward:** Usually, the RM only rewards the _last_ token (End of Sentence). The KL penalty is applied to _every_ intermediate token as a small instantaneous reward/penalty.
    

### Phase 3: Advantage Estimation

6. **Critic Pass:** The Critic ($V_w$) estimates the value of the sequence at every token step.
    
7. **GAE:** We calculate the Generalized Advantage Estimation (GAE) for every token.
    
    - _Intuition:_ "This specific adjective (token #5) was a pivotal moment that improved the sentence's value."
        

### Phase 4: Optimization (PPO Update)

8. Update Actor: We maximize the PPO Objective:
    
    $$L_{CLIP} = \hat{\mathbb{E}} \left[ \min(r_t \hat{A}_t, \text{clip}(r_t, 1-\epsilon, 1+\epsilon)\hat{A}_t) \right]$$
    
    - We nudge the token probabilities to make high-advantage tokens more likely.
        
9. Update Critic: We minimize the error between the Critic's prediction and the actual returns.
    
    $$L_{VF} = (V(s) - (R + \gamma V(s')))^2$$
    

---

## 6. Intuitive Analogy: The Writer, The Editor, and The Critic

To explain this to a 5-year-old (or a curious Executive):

- **The Actor (The Writer):** Writes the story. Initially, he writes okay, but sometimes is rude.
    
- **The Reference Model (The Grammar Book):** A book of standard, correct English.
    
- **The Reward Model (The Audience):** Reads the finished story and claps (gives a score).
    
- **The Critic (The Editor):** Stands over the Writer's shoulder. As the Writer puts down a word, the Editor whispers, _"Good word choice, that will make the Audience happy"_ (High Advantage) or _"Bad word, the Audience will hate that"_ (Low Advantage).
    

**The Process:**

1. The Writer writes a draft.
    
2. The Audience rates it.
    
3. The Editor analyzes _why_ the Audience liked it ("It was the third sentence that did it").
    
4. The Writer updates his brain: "Okay, I'll use sentences like that third one more often."
    
5. **The KL Constraint:** If the Writer gets too crazy and starts inventing words to please the Audience, the Grammar Book hits him on the head.
    

---

## 7. Technical Nuances & Challenges

### A. Sparse Rewards

In robotics, you might get a reward every second. In LLMs, you generate 500 tokens and get **one** reward at the end.

- This makes credit assignment ("Which of the 500 tokens was the good one?") very hard.
    
- The Critic is essential here to diffuse that final reward backward through time to the earlier tokens.
    

### B. Token-Level vs. Sentence-Level

Standard PPO treats actions as discrete steps. However, an LLM prompt is a sequence. We effectively treat an LLM generation of length $T$ as an episode of length $T$.

- $\gamma$ (Gamma) is usually set close to 1 (e.g., 0.99 or 1.0) because the "action" at the start of the sentence deeply affects the coherence at the end.
    

### C. PPO-pt (PPO-pytorch) vs TRXL

Standard PPO implementations often fail with Transformers because of how batches are handled.

- In LLM PPO, we cannot just shuffle random tokens from different sentences into a mini-batch (like we shuffle frames in Atari).
    
- We must keep the **sequence integrity**. If you break the sequence, the Attention mechanism breaks.
    
- Therefore, batching is done by **Episodes** (Full Prompt+Response pairs), not individual timesteps.
    

---

## 8. Summary: The RLHF Recipe

1. **SFT:** Teach the model to speak English and follow instructions (Imitation Learning).
    
2. **RM:** Train a judge to recognize "good" outputs.
    
3. **PPO:**
    
    - **Generate** text.
        
    - **Penalize** deviation from SFT (KL).
        
    - **Reward** high RM scores.
        
    - **Clip** the updates so the model doesn't go insane.
        

Next Step:

Would you like to explore Direct Preference Optimization (DPO)? It is the newer (2023) rival to PPO that claims to remove the need for the Reward Model and PPO entirely, solving the same problem mathematically in a simpler way.

Here is a comprehensive, deep-dive lecture on **Direct Preference Optimization (DPO)**.

This is the algorithm that shook the RLHF world in 2023. It mathematically proved that we can achieve the same goal as PPO (aligning models to human preferences) _without_ needing a Reward Model and _without_ complex Reinforcement Learning loops.

---

# Lecture 11: Direct Preference Optimization (DPO)

## 1. The Motivation: PPO is Painful

In the previous lecture (Lecture 10), we saw the standard RLHF pipeline ("PPO-based"). It is effective, but it is an engineering nightmare.

**The PPO Pipeline Complexity:**

1. **Train SFT Model** (Supervised Fine-Tuning).
    
2. **Train Reward Model** (Requires a separate dataset of comparisons).
    
3. **Run PPO:**
    
    - Load **4 Models** into memory (Actor, Critic, Ref, Reward).
        
    - Sample from Actor (slow).
        
    - Calculate Rewards and Values.
        
    - Update Actor and Critic.
        
    - Balance the "Deadly Triad" of hyperparameters (clipping, KL beta, learning rates, GAE lambda).
        

The DPO Promise:

What if we could skip steps 2 and 3? What if we could take the dataset of "Response A is better than Response B" and just use a simple classification loss to train the model directly?

DPO transforms the **RL problem** into a **Supervised Learning problem**.

---

## 2. The Intuition (ELI5): Skipping the Middleman

The PPO Approach (The "Critic" Method):

Imagine you are training a comedian.

1. You hire a Focus Group (Reward Model).
    
2. The Comedian tells a joke.
    
3. The Focus Group rates it "7/10."
    
4. The Comedian tries to figure out _why_ it was a 7 and not a 10, and adjusts their brain.
    

The DPO Approach (The "Comparison" Method):

You don't hire a Focus Group. You just show the Comedian two videos:

- **Video A:** The audience is laughing hysterically (Winning Joke).
    
- **Video B:** The audience is silent (Losing Joke).
    
- **Instruction:** "Adjust your brain so that you are _more_ likely to tell Joke A and _less_ likely to tell Joke B."
    

We effectively bypass the need for an explicit score. We optimize the relative probability directly.

---

## 3. The Derivation: The "Magic" Math

DPO works because of a beautiful mathematical derivation. It shows that the optimal policy for the RLHF objective can be solved in closed form.

### Step 1: The RLHF Objective

Recall from the PPO lecture that we want to maximize a reward $r(x,y)$ while staying close to a reference model $\pi_{ref}$.

The objective function is:

$$\max_{\pi} \mathbb{E} \left[ r(x, y) - \beta \log \frac{\pi(y|x)}{\pi_{ref}(y|x)} \right]$$

### Step 2: The Optimal Solution Form

It is a known result in mathematics (from convex duality) that the optimal solution $\pi^*(y|x)$ for this specific equation has a precise form:

$$\pi^*(y|x) = \frac{1}{Z(x)} \pi_{ref}(y|x) e^{\frac{1}{\beta} r(x,y)}$$

- $Z(x)$ is just a partition function (normalizing constant) to make probabilities sum to 1.
    

This equation tells us: **If we knew the perfect reward $r(x,y)$, the perfect policy $\pi^*$ is just the reference policy scaled by that reward.**

### Step 3: The Inversion (The "Trick")

The authors of DPO did something clever. They took the equation above and rearranged it to solve for the reward $r(x,y)$ instead!

Take the log of both sides:

$$\log \pi^*(y|x) = \log \pi_{ref}(y|x) + \frac{1}{\beta} r(x,y) - \log Z(x)$$

Rearrange to isolate $r(x,y)$:

$$r(x,y) = \beta \log \frac{\pi^*(y|x)}{\pi_{ref}(y|x)} + \beta \log Z(x)$$

**Implication:** The reward is implicitly defined by the ratio of the optimal policy to the reference policy. We don't need a separate neural network to estimate rewards; the policy _itself_ estimates the reward.

### Step 4: The Bradley-Terry Model

We assume human preferences follow the Bradley-Terry model. The probability that human prefers answer $y_w$ (winner) over $y_l$ (loser) depends on the difference in their rewards:

$$p(y_w > y_l | x) = \sigma(r(x, y_w) - r(x, y_l))$$

- $\sigma$: The sigmoid function.
    

### Step 5: Substitution

Now, plug our "Inverted Reward" (Step 3) into the Bradley-Terry model (Step 4).

$$r(x, y_w) - r(x, y_l) = \beta \log \frac{\pi^*(y_w|x)}{\pi_{ref}(y_w|x)} - \beta \log \frac{\pi^*(y_l|x)}{\pi_{ref}(y_l|x)}$$

_Notice something amazing:_ The annoying $Z(x)$ term (partition function) appears in both reward terms and **cancels out**!

We are left with a probability that depends **only** on the policy $\pi$ and the reference $\pi_{ref}$. No reward model $r(x,y)$ needed.

---

## 4. The DPO Loss Function

We replace the theoretical optimal $\pi^*$ with our parameterized model $\pi_\theta$. We minimize the negative log-likelihood of the preference data.

$$L_{DPO}(\pi_\theta; \pi_{ref}) = - \mathbb{E}_{(x, y_w, y_l) \sim \mathcal{D}} \left[ \log \sigma \left( \beta \log \frac{\pi_\theta(y_w|x)}{\pi_{ref}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_{ref}(y_l|x)} \right) \right]$$

This looks scary, but let's break it down intuitively.

We define an Implicit Reward for a completion $y$ as:

$$\text{ImplicitReward}(y) = \beta \log \frac{\pi_\theta(y|x)}{\pi_{ref}(y|x)}$$

The loss simply says:

"Make the Implicit Reward of the Winner higher than the Implicit Reward of the Loser."

---

## 5. Gradient Analysis: How it Learns

How does the model actually update its weights during backpropagation?

The gradient of the DPO loss reveals exactly what is happening:

$$\nabla_\theta L_{DPO} = - \underbrace{\beta \sigma(\hat{r}_l - \hat{r}_w)}_{\text{Weight}} \left[ \underbrace{\nabla_\theta \log \pi(y_w|x)}_{\text{Increase Winner}} - \underbrace{\nabla_\theta \log \pi(y_l|x)}_{\text{Decrease Loser}} \right]$$

1. **Increase Winner:** The gradient pushes up the probability of the winning response $y_w$.
    
2. **Decrease Loser:** The gradient pushes down the probability of the losing response $y_l$.
    
3. **Dynamic Weighting:** The term $\sigma(\hat{r}_l - \hat{r}_w)$ acts as a dynamic weight.
    
    - If the model _already_ thinks the winner is much better than the loser ($\hat{r}_w \gg \hat{r}_l$), the sigmoid term is close to 0. **Gradient is small.** (Don't fix what isn't broken).
        
    - If the model incorrectly thinks the loser is better ($\hat{r}_l > \hat{r}_w$), the sigmoid is large. **Gradient is huge.** (Major correction needed).
        

This naturally prevents overfitting to easy examples and focuses the model on the hard comparisons where it is currently wrong.

---

## 6. DPO vs. PPO: The Practical Comparison

|**Feature**|**PPO (Classic RLHF)**|**DPO (Direct Optimization)**|
|---|---|---|
|**Complexity**|Extremely High (4 models, GAE, value estimation)|Low (2 models, standard Classification loss)|
|**Stability**|Unstable (sensitive to hyperparameters)|Stable (Standard gradient descent)|
|**Data Usage**|Online (Needs to generate new samples constantly)|Offline (Uses a static dataset of pref pairs)|
|**Distribution**|Can explore new trajectories (Exploration)|Limited to the preference dataset (No Exploration)|
|**Memory**|High (Actor + Critic + Ref + Reward)|Medium (Policy + Ref)|
|**Objective**|Maximize Reward Model score|Maximize likelihood of preferred data|

---

## 7. Implementation Nuances

If you implement DPO, keep these in mind:

### 1. Memory Efficiency

You only need the Reference Model for inference (to compute the denominator probabilities). You do not need gradients for it. You can:

- Run the Reference Model in `float16` or `int8`.
    
- Pre-compute the reference log-probs for your dataset once and delete the Reference Model entirely from memory (if your dataset is static).
    

### 2. The Beta ($\beta$) Hyperparameter

Beta controls the strength of the KL divergence (just like in PPO).

- **High Beta (e.g., 0.5):** Stick very close to the reference model. Ignore the preferences if they require drifting too far.
    
- **Low Beta (e.g., 0.1):** Preference satisfaction is prioritized. Drift away from the reference model.
    
- _Typical Range:_ 0.1 to 0.5.
    

### 3. Dataset Quality

PPO is somewhat robust to noise because the Reward Model acts as a smoother.

DPO is sensitive to noisy labels. If your dataset says "A > B" but A is actually terrible, DPO will aggressively force the model to output the terrible A. Data cleaning is critical.

---

## 8. Summary: The Paradigm Shift

DPO represents a shift from **explicit** reward modeling to **implicit** reward modeling.

- **Old View (PPO):** We need a separate judge to tell us what is good.
    
- **New View (DPO):** The preference data _contains_ the reward signal. By simply increasing the likelihood of preferred answers relative to a baseline, we essentially "solve" the RL equation without doing RL.
    

Next Step:

Now that you have mastered the "Big Three" (Value Approximation, Policy Gradients, and DPO), would you like to build a practical Roadmap to implement a custom LLM from scratch (Pre-training $\to$ SFT $\to$ DPO)?