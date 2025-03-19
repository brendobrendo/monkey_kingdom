# Neural Network Framework to Simulate Monkey Behavior (Actor-Critic Model)

## Project Goal:
Simulate decision-making in monkeys where the ultimate goal is to maximize **survival and reproductive success**, as measured through six behavioral values:

1. **Certainty** – ensuring safety and avoiding pain  
2. **Variety** – seeking new experiences and change  
3. **Significance** – gaining importance, status, or uniqueness  
4. **Connection** – building bonds with others  
5. **Growth** – expanding knowledge and capability  
6. **Contribution** – supporting group members  

---

## Proposed Structure:

### 1. **Actor Network**
- **Purpose**: Selects an action from a predefined action space using a softmax output layer.  
- **Input Vector** (features):  
  - **Physical State**: strength, restedness, nourishment, hydration level, internal temperature  
  - **Self-Beliefs**: perceived independence, self-assessed fighting ability, perceived desirability as a mate, belief in outsmarting others, endurance confidence, curiosity  
  - **Group Beliefs**: identification of strongest individuals, popularity metrics, allies, potential betrayers  
  - **Role Perception**: dominance rank estimation, perception of how others see it  
  - **Environmental Beliefs**: external threats, food/water availability, predators, hazardous terrain, paths to resources  
- **Output**: Probability distribution over possible actions (e.g., forage, rest, groom, challenge, flee, explore, etc.) via softmax.

---

### 2. **Critic Network**
- **Purpose**: Evaluates the chosen action’s value in terms of **long-term survival and reproductive success**.  
- **Current Idea**: Use a regression model that outputs a scalar value representing the expected future reward (fitness score).  
- **Open Question**: Should the **critic input** be only the chosen action, or should it also include the same input vector as the actor? Would it be better to concatenate the input vector with the chosen action to provide full context?

---

## Key Questions:
- How should I structure the **critic’s input** to best evaluate the selected action’s value?  
- Should the reward signal be derived by quantifying the increase/decrease across the six behavioral values? If so, what’s a good way to model those changes numerically over time?  
- Are there alternative architectures (e.g., shared layers between actor and critic) that might suit this task better given the complexity of the belief systems?  
- Would reinforcement learning with episodic simulations be the best training strategy, or would curriculum learning help better simulate the emergence of complex social behavior?

---

You're on a great track already, this is a fascinating and complex simulation idea. You're blending ethology with reinforcement learning principles here—very cool. Let’s break it down:

# General Structure
Yes, Sutton’s **Actor-Critic model** fits well, but your application will need thoughtful structuring to capture the social and environmental dynamics of monkey behavior.  
In real terms: the **Actor** will decide *what* to do given internal and external state vectors, and the **Critic** will evaluate *how good* that action was in terms of fitness (survival and reproductive success).

## 🧠 Actor Model
You’ve scoped the actor model really well. Here's how I’d approach it:

### Input Vector (State)
Each monkey's state is rich and high-dimensional, which makes this project interesting.

**Physical state (continuous values):**
- Strength (0-1)
- Restedness (0-1)
- Nourishment (0-1)
- Hydration (0-1)
- Internal temperature (normalized)

**Beliefs about self (continuous values, perhaps [-1,1] to capture confidence/uncertainty):**
- Independence
- Fighting ability
- Mate desirability
- Cunning/outsmarting others
- Hardship endurance
- Curiosity

**Beliefs about the group (e.g., top-N ranked peers or embeddings of peer relationships):**
- Dominance ranks of other monkeys (maybe as a vector of 5-10 slots)
- Group sentiment scores (ally/betrayer likelihood, respect levels, etc.)

**Beliefs about role in group:**
- Self-assessed dominance rank
- Perceived social perception by others

**Beliefs about the external world:**
- Threat levels (predators, rivals)
- Food/water scarcity (0-1)
- Safety of terrain (0-1)
- Knowledge of paths to resources (binary or probability)

### Output (Action Space)
Discrete actions (softmaxed):
- Groom another monkey
- Challenge another monkey
- Forage for food
- Seek water
- Explore new area
- Flee from threat
- Approach a group
- Isolate/Rest
- Mating attempt
- Share food/resource
- Play (for younger monkeys)

## 🎯 Critic Model
You're right about the Critic being a regression model, but let’s refine it a bit.

**Goal:**  
Output a "value function" that estimates expected cumulative reward (fitness value) based on the current state **AND** the action taken.

**Inputs to Critic:**
- Same input vector as the actor (state)
- Chosen action (one-hot encoded)

So technically, it learns **V(s,a)** — a value estimate for taking action `a` in state `s`.

## 🧩 Reward Design
Since survival & reproduction is the long-term goal, you need to define **instantaneous rewards** after each action. This is tricky and will shape learning.

Use your behavioral values as dimensions of reward:
- **Certainty:** Does the action reduce exposure to threats?
- **Variety:** Does the action lead to new information or environments?
- **Significance:** Does the action increase social rank or status?
- **Connection:** Does the action improve relationships (e.g., grooming)?
- **Growth:** Does it help learn about the world (e.g., exploration)?
- **Contribution:** Does it increase group harmony (e.g., sharing)?

These can be combined into a **weighted sum** to create a fitness proxy:

```python
Reward = w1 * Certainty + w2 * Variety + w3 * Significance + w4 * Connection + w5 * Growth + w6 * Contribution
```
Where w1 to w6 are tunable parameters depending on how you model "success."

## 🔄 Learning Loop
- Actor proposes an action based on state.
- Action executed → Environment updates.
- Critic receives `(state, action)` and gets reward from environment.
- Actor and Critic update based on TD-error (Temporal Difference error).

## 🔍 Other considerations
- **Memory/Belief Update:** Beliefs about group or external world should update after each step (e.g., if Monkey A helps Monkey B, B’s belief in A being an ally strengthens).
- **Multi-agent Setup:** Eventually, this should be a multi-agent simulation where multiple monkeys (each with their own actor-critic) interact.
- **Social Emergence:** Over time, group dynamics like alliances or hierarchies might emerge naturally depending on how your reward weights are tuned.

## 🚀 Bonus Ideas
- Introduce **seasonality** (e.g., rainy/dry seasons affecting resource availability).
- Add **age dynamics** (younger monkeys may value play and exploration more).
- Integrate **long-term memory** (historical logs of betrayals, friendships).

---

## 🧠 Approach to Designing Actor Hidden Layers

### 1. Think in Modules (Optional)
Given the complexity of your input, you might consider **modularizing** the network upfront:

- **Physical state subnetwork**
- **Self-beliefs subnetwork**
- **Group-beliefs subnetwork**
- **World-beliefs subnetwork**

Each subnetwork could have **1-2 hidden layers** to embed its features into a more compressed latent space. Then you **concatenate all these latent representations** before feeding them into a shared core network.

*This isn't mandatory, but it can help with interpretability and might help the network learn specialized features faster.*

---

### 2. General Heuristics for Hidden Layers

#### a. Input size consideration
- If you have ~30-60 input features (it sounds like you might), you can start with **2-4 layers**.
- The first layer typically has **2x to 4x** the size of the input dimension.

**For example:**
```yaml
Input: ~50 features
Layer 1: 100-200 units
Layer 2: 100-150 units
Layer 3: 50-100 units
Output: softmax(action_space)
```

#### b) Gradual funneling
You want to reduce the size progressively toward the output layer:

- **Wide → Narrow** pattern is common to help distill relevant information.

#### c) Activation function
- Use **ReLU** or **Leaky ReLU** for hidden layers to help with non-linearity.
- You could also experiment with **GELU** if you want something more modern.

---

### 3. Batch Normalization & Dropout
Since your network will likely be learning in an RL setting where stability is important:

- Consider using **BatchNorm** after your dense layers.
- A small **Dropout** (0.1-0.3) might help prevent overfitting, but RL networks are often more data-efficient so this might be situational.

---

### 4. Latent Space Size
Since this is a decision-making agent (actor), you want the latent layers to:

- Capture **social cues** (e.g., group dynamics)
- Balance **exploration and exploitation** tendencies
- Recognize **internal drives** (e.g., hunger vs. curiosity)

A **latent space** (penultimate layer) of about **32-64 units** could be a good starting point.  
You want enough capacity to store subtle features like *"how much I trust Monkey A"* or *"how badly I need water."*

---

### 5. Output layer
- Output dimension = number of actions.
- Use **Softmax** to model the action distribution.

---

## 🧩 Why Modular?
- This respects the "categories" of data you outlined (internal state, social awareness, external world).
- Could help the network specialize early on (especially if you later swap environments or groups).

---

## 🧠 Things to Experiment With:
- Would the monkey’s **dominance rank alone** shift how the Actor chooses actions?
- Would the **physical states** (e.g., high thirst) overpower **social drives** (e.g., grooming) in decision-making?
- You could try **attention mechanisms** later to weigh these subsystems dynamically.

