# Neural Network Framework to Simulate Monkey Behavior  
### (Actor-Critic Model)

## 🎯 Project Goal  
Simulate decision-making in monkeys where the ultimate goal is to maximize survival and reproductive success, measured via six core behavioral values:

1. **Certainty** – Ensuring safety and avoiding pain  
2. **Variety** – Seeking new experiences and change  
3. **Significance** – Gaining importance, status, or uniqueness  
4. **Connection** – Building bonds with others  
5. **Growth** – Expanding knowledge and capability  
6. **Contribution** – Supporting group members  

---

## 🧩 Architecture Overview  
Based on Sutton’s Actor-Critic architecture, the system features:  
- **Actor**: Chooses actions based on internal and external states.  
- **Critic**: Evaluates how beneficial that action was for fitness (survival and reproduction).  

### 🔄 Shared Backbone (Feature Extractor)  
- **Purpose**: Extracts high-level features from input state vectors for use by both Actor and Critic.  
- **Structure**: Combines modular subnetworks into a unified shared representation.  

---

## 🧠 Actor Model  

### 🔹 Input (State Vector)  
A high-dimensional vector representing:

**1. Physical State (continuous values):**  
- Strength (0-1)  
- Restedness (0-1)  
- Nourishment (0-1)  
- Hydration (0-1)  
- Internal temperature (normalized)  

**2. Self-Beliefs (continuous, e.g., [-1, 1] scale):**  
- Independence  
- Fighting ability  
- Mate desirability  
- Cunning/outsmarting others  
- Hardship endurance  
- Curiosity  

**3. Group Beliefs:**  
- Dominance ranks of other monkeys (vector of 5-10 slots)  
- Group sentiment scores (ally/betrayer likelihood, respect, etc.)  

**4. Role Perception:**  
- Self-assessed dominance rank  
- Perceived social perception by others  

**5. Environmental Beliefs:**  
- Threat levels (predators, rivals)  
- Food/water scarcity (0-1)  
- Terrain safety (0-1)  
- Knowledge of paths to resources (binary or probabilistic)  

### 🔹 Output (Action Space)  
Categorical distribution over discrete actions (via softmax):  
- Groom another monkey  
- Challenge another monkey  
- Forage for food  
- Seek water  
- Explore new area  
- Flee from threat  
- Approach group  
- Isolate/Rest  
- Mating attempt  
- Share food/resource  
- Play (juvenile monkeys)  

---

## 🎯 Critic Model  

### Purpose  
Outputs a value function estimating cumulative reward (fitness proxy) based on state and action.  
- **Learns V(s, a):** Value of taking action _a_ in state _s_.  

### Inputs  
- Same state vector as Actor  
- Chosen action (one-hot encoded)  

### Output  
- Scalar value estimating expected long-term reward  

---

## 🧩 Modular Subnetworks  

### 1️⃣ Physical State Subnetwork  
- Inputs: strength, restedness, nourishment, hydration, temperature  
- Output: Physical readiness representation  

### 2️⃣ Self-Belief Subnetwork  
- Inputs: independence, fighting ability, desirability, cunning, endurance, curiosity  
- Output: Self-assessment representation  

### 3️⃣ Group Belief Subnetwork  
- Inputs: knowledge of ranks, allies/betrayers, social dynamics  
- Output: Social knowledge embedding  

### 4️⃣ Role Perception Subnetwork  
- Inputs: self-dominance rank, perceived external perception  
- Output: Hierarchical role understanding  

### 5️⃣ Environmental Belief Subnetwork  
- Inputs: threat levels, scarcity, terrain safety, resource knowledge  
- Output: Environmental modeling  

### 🧩 Subnetwork Fusion  
- All subnetwork outputs are concatenated into a shared feature vector  
- Processed through shared layers (MLP, LSTM, or Transformer block depending on complexity)  

---

## 🧠 Actor-Critic Head Split  

- **Shared Features → Split into:**  

### 🔹 1. Actor Head  
- Input: Shared feature representation  
- Output: Action probabilities (softmax)  

### 🔹 2. Critic Head  
- Input: Shared feature representation + one-hot encoded action  
- Output: Scalar value for V(s, a)  

---

## 🔄 Learning Framework  
- **Critic**: Trained via regression on TD-targets (value estimation)  
- **Actor**: Trained via policy gradients (maximize expected rewards)  
- **Loss**: Actor-Critic loss using TD-error (Temporal Difference error)  

---

## 🏆 Reward Design  

Instantaneous rewards are multi-dimensional, aligned with the six behavioral values:  
- **Certainty**: Avoid threats  
- **Variety**: Seek new experiences  
- **Significance**: Improve rank/status  
- **Connection**: Build/maintain relationships  
- **Growth**: Gain knowledge through exploration  
- **Contribution**: Enhance group harmony  

### Composite Reward Function  

![Reward Formula](https://latex.codecogs.com/svg.image?Reward%20=%20w_1%20\cdot%20Certainty%20+%20w_2%20\cdot%20Variety%20+%20w_3%20\cdot%20Significance%20+%20w_4%20\cdot%20Connection%20+%20w_5%20\cdot%20Growth%20+%20w_6%20\cdot%20Contribution)
*Where _w1 - w6_ are tunable weights for balancing the importance of each factor.*

---

## 🔄 Learning Loop  
1. Actor selects an action based on the current state.  
2. Action is executed; environment updates accordingly.  
3. Critic evaluates the (state, action) pair and receives the reward.  
4. Actor and Critic update parameters based on TD-error. 

---

## ⚙ Benefits of this Setup
- ✅ Shared layers reduce redundancy and improve learning stability.
- ✅ Subnetworks allow specialization within each "belief system" or domain of monkey cognition.
- ✅ Flexible enough to plug in attention mechanisms later (e.g., for focusing on most relevant group members or environmental factors).

## 🧠 What is TD-error?

At its core, **TD-error** is the difference between:

1. What your **Critic predicted** the value of taking action `a` in state `s` would be (**V(s, a)**).
2. What you **actually observed** after taking that action, plus what you now expect the next state to bring in terms of future value.

### Mathematically:
![TD Error](https://latex.codecogs.com/svg.image?\delta%20=%20r%20+%20\gamma%20V(s',%20a')%20-%20V(s,%20a))

Where:
- `r` = reward received after taking action `a` in state `s`
- `γ` = discount factor (how much future rewards are "worth" compared to immediate rewards, typically between 0.9 and 0.99)
- `V(s', a')` = critic’s estimate of the next state-action pair’s value
- `V(s, a)` = critic’s estimate of the current state-action value before taking the action

---

## 🎯 Goal: Minimize TD-error
Your Critic is trained to **minimize** this TD-error over time. In other words, the Critic wants its predictions to match reality as closely as possible.

- When **δ = 0**, your value estimates are perfect.
- If **δ ≠ 0**, that’s a "mistake," and the Critic updates its weights to bring its value estimate closer to the new target:  
  
  ![Target](https://latex.codecogs.com/svg.image?r%20+%20\gamma%20V(s',%20a'))


---

## 🧠 But what about the Actor?

While the Critic **minimizes TD-error**, the Actor uses **δ as a signal**:

- If **δ > 0**, it means the action taken performed **better than expected** — the Actor is encouraged to increase the probability of taking that action again in similar states.
- If **δ < 0**, it means the action performed **worse than expected** — the Actor is discouraged from repeating it.

### In short:
- The **Critic** is a value estimator → **minimize TD-error**.
- The **Actor** is a policy optimizer → **maximize expected returns**, guided by **δ** as feedback.

---  

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
Given the complexity of the input, consider **modularizing** the network upfront:

- **Physical state subnetwork**
- **Self-beliefs subnetwork**
- **Group-beliefs subnetwork**
- **World-beliefs subnetwork**

Each subnetwork could have **1-2 hidden layers** to embed its features into a more compressed latent space. Then you **concatenate all these latent representations** before feeding them into a shared core network.

*This isn't mandatory, but it can help with interpretability and might help the network learn specialized features faster.*

---

### 2. General Heuristics for Hidden Layers

#### a. Input size consideration
- If you have ~30-60 input features (it may apply here), you can start with **2-4 layers**.
- The first layer typically has **2x to 4x** the size of the input dimension.

**For example:**
```yaml
Input: ~50 features
Layer 1: 100-200 units
Layer 2: 100-150 units
Layer 3: 50-100 units
Output: softmax(action_space)
```

#### b. Gradual funneling
You want to reduce the size progressively toward the output layer:

- **Wide → Narrow** pattern is common to help distill relevant information.

#### c. Activation function
- Use **ReLU** or **Leaky ReLU** for hidden layers to help with non-linearity.
- You could also experiment with **GELU** if you want something more modern.

---

### 3. Batch Normalization & Dropout
Since my network will likely be learning in an RL setting where stability is important:

- Consider using **BatchNorm** after your dense layers.
    - Batch Normalization stabilizes training by normalizing the outputs of each layer to have a consistent mean and variance across mini-batches, reducing internal covariate shift. In your actor-critic model, it helps keep activations steady despite noisy or highly variable reinforcement signals, leading to smoother learning and faster convergence, especially in environments with complex social dynamics like yours.
- A small **Dropout** (0.1-0.3) might help prevent overfitting, but RL networks are often more data-efficient so this might be situational.
    - Dropout randomly deactivates a fraction of neurons during training to prevent the network from overfitting to early patterns or noise. In your RL setting, applying light dropout (e.g., 0.1–0.3) after dense layers can improve generalization by encouraging more robust representations, although it’s typically used sparingly in RL due to the naturally high variability in agent experiences.

---

### 4. Latent Space Size
Since this is a decision-making agent (actor), the latent layers should:

- Capture **social cues** (e.g., group dynamics)
- Balance **exploration and exploitation** tendencies
- Recognize **internal drives** (e.g., hunger vs. curiosity)

A **latent space** (penultimate layer) of about **32-64 units** could be a good starting point.  
You want enough capacity to store subtle features like *"how much I trust Monkey A"* or *"how badly I need water."*

A **compressed latent space** refers to creating a more **compact, lower-dimensional representation** of the input data after it's processed by each subnetwork. When I modularize my **Actor model** into subnetworks, each one processes a chunk of your input (e.g., physical state, social beliefs, environment). Each subnetwork transforms that input into a latent space—basically, an intermediate representation (vector) that captures the essence of that input block.

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

## 1️⃣ How to generate initial state & belief data
Since this is a simulation, you have some creative freedom, but you’ll want the initial states and beliefs to be:

- **Biologically plausible**
- **Sufficiently varied** (for exploration)
- **Internally consistent** (a monkey with high strength might also be perceived as higher ranking)

---

## 🧪 Ideas to generate initial monkey states/beliefs

### A. Randomized, but weighted distributions
For physical stats (e.g., strength, hydration):
- Sample from **normal** or **beta distributions** centered around species norms.

**Example:**
- Strength ~ `N(0.5, 0.1)`
- Hydration ~ `U(0.7, 1.0)` (if they’re starting fresh in a new environment).

---

### B. Hierarchical priors
**Dominance rank** can be seeded using **rank-based priors**, e.g.:

- Monkeys ranked higher at the start get slightly higher:
  - Strength
  - Self-confidence (fighting ability, endurance)
  - Group perception of them

- Lower-rank monkeys might get higher:
  - Curiosity
  - Social bonding orientation

---

### C. Personality Archetypes (optional flavor)
Assign **"personality archetypes"** to diversify:

- **"Bold Alpha"** = High strength + low curiosity + high dominance.
- **"Wily Outsider"** = Medium strength + high cunning + low connection.
- **"Social Butterfly"** = Medium-low strength + high connection + low self-confidence.
- **"Hermit Explorer"** = High curiosity + low connection + high independence.

*These archetypes can bias how initial states & beliefs are sampled for each monkey.*

---

### D. Environmental context dependency
If starting the simulation in a **"hostile environment"** (e.g., scarce food or presence of predators), then:
- Start **hydration/nourishment** lower on average.
- Increase **group cohesion beliefs** (banding together to survive).

---

### E. Belief noise
- Add **small Gaussian noise** to self and group beliefs to make each monkey unique (even within archetypes).

### Concrete fields to randomize

| **Category**            | **Example fields**                                 | **Sampling approach**                                     |
|-------------------------|----------------------------------------------------|-----------------------------------------------------------|
| **Physical State**       | Strength, Hydration, Nourishment                   | Normal/Uniform distributions                              |
| **Self-Beliefs**         | Independence, Mate value, Cunning                  | Based on rank/archetype + noise                           |
| **Group Beliefs**        | Trust in others, threat perception                 | Derived from randomized monkey relationships              |
| **Group Role Beliefs**   | Dominance perception, peer perceptions             | Can loosely match actual social hierarchy + noise         |
| **World Beliefs**        | Perceived food abundance, threat level             | Related to global environment config                      |

---

## 2️⃣ Should you create a framework for labeling what outputs should be?

**Short answer:**  
Not exactly — but you DO need a **reward-shaping framework**.

### 🧠 Why?
In reinforcement learning, you typically don’t supervise with "correct action labels."  
Instead, you **reward or penalize actions based on their consequences**.

---

### What you do need:

### A. A "reward function" framework
This will determine **how to reward or punish actions** based on your six core values (certainty, variety, etc.).

**For example:**
- If a monkey **shares food**, increase its **Connection** and **Contribution** rewards.
- If a monkey **explores alone**, increase **Variety** but reduce **Certainty** (risk).
- If a monkey **challenges a rival**, boost **Significance** but with a risk of injury.

---

### B. A mechanism to record actual outcomes:
Track **episodic summaries** like:
- Was the monkey well-fed by the end?
- Did it strengthen alliances?
- Was its social status improved?

*This is how you’ll retroactively know if the action was good or bad (which feeds into the Critic’s learning process).*

---

### ⚠️ Warning:
If you labeled what action the monkey "should take" directly, you'd be turning this into a **supervised learning problem** (which wouldn't generalize as well to emergent behavior).

---

### 🔄 Alternative idea (Hybrid approach):
You could create a rough set of **heuristic policies** (e.g., "if thirsty, prioritize finding water") to **bootstrap training early on**, and then let the Actor gradually replace heuristics with learned policies.  
This is sometimes called **"Imitation Learning"** or **"Behavioral Cloning" + RL**.

---

# 🌍 Environment Dynamics Framework

## 🔄 1. Physical State Updates
Physical states should degrade or improve with time and actions.

| **Physical State**       | **Action/Event Impact**                                                                     |
|--------------------------|---------------------------------------------------------------------------------------------|
| **Strength**             | Gradually decays if malnourished or dehydrated; increases with rest, decreases after fights |
| **Restedness**           | Decreases over time or after high-effort actions; increases with sleep/rest actions         |
| **Nourishment**          | Slowly declines over time; increases after successful foraging or sharing food              |
| **Hydration**            | Declines every time step; replenished by finding water                                      |
| **Internal Temperature** | Fluctuates with environment (hot/cold weather, exertion); extreme values reduce Certainty reward |

💡 *Tip: Create baseline environmental cycles like day/night or dry/wet seasons that make survival harder or easier.*

---

## 🧠 2. Belief Updates
Beliefs about self, group, and world should evolve based on experience, creating feedback loops.

### A. Self-beliefs

| **Self-Belief**               | **Action/Event Impact**                                                   |
|-------------------------------|---------------------------------------------------------------------------|
| **Independence**              | Increases if monkey succeeds when acting alone; decreases if rescued by allies |
| **Fighting ability confidence**| Increases after winning fights; decreases after losses or injuries         |
| **Mate desirability**         | Increases after positive mating/social interactions; decreases after rejection or humiliation |
| **Cunning/outsmarting others**| Improves if successfully steals food or manipulates rivals                  |
| **Hardship endurance**        | Improves if monkey survives harsh conditions                               |
| **Curiosity**                 | Increases if rewarded after exploration; may decrease if exploration leads to danger |

---

### B. Group Beliefs

| **Group Belief**                                  | **Action/Event Impact**                                                      |
|---------------------------------------------------|------------------------------------------------------------------------------|
| **Trust in Ally X**                               | Increases after grooming, sharing food, mutual defense                       |
| **Suspicion of Rival Y (betrayal risk)**          | Increases after witnessing betrayal, or hearing alarm calls against Y        |
| **Perceived group’s strongest member**            | Updates after observing fights, displays of dominance                        |
| **Well-liked member perception**                  | Increases after observing social bonding events (grooming others, alliances) |

💡 *Tip: Introduce slight observation errors ("partial observability") — monkeys might occasionally misjudge others.*

---

### C. Beliefs about Role in Group

| **Role Belief**                                    | **Action/Event Impact**                                                        |
|----------------------------------------------------|--------------------------------------------------------------------------------|
| **Perceived dominance rank**                       | Increases after wins or displays of power; decreases after defeat              |
| **Perceived how others see me (peer respect)**     | Improves after helping others or receiving attention from group leaders        |

---

### D. World Beliefs

| **World Belief**                                   | **Action/Event Impact**                                                        |
|----------------------------------------------------|--------------------------------------------------------------------------------|
| **Food/Water availability perception**             | Adjusted after foraging success/failure or observing scarcity in others        |
| **External threat level (predators)**              | Spikes after encountering predators or hearing alarm calls                     |
| **Rival group threat level**                       | Increases after skirmishes or signs of other tribes                           |
| **Knowledge of paths to resources**                | Expands after successful explorations (could add a "map" or spatial memory)    |

---

## ⚙️ 3. Natural Decay/Forgetting
- Some beliefs should **decay over time** if not reinforced.
  - E.g., if Monkey A hasn’t helped Monkey B in a while, trust gradually erodes.
  - **Confidence** might also fall after long periods without success.

---

## 💡 4. Emergent Group Effects
- **Ripple effects:** If a high-ranking monkey is defeated, all monkeys should reassess their group beliefs.
- **Group tension meter:** Could track overall tension (e.g., scarcity or frequent fights make everyone warier).

---

## 🔄 5. External Environment Dynamics

| **World Element**     | **Dynamic changes**                                              |
|-----------------------|------------------------------------------------------------------|
| **Food supply**       | Cycles seasonally (wet/dry season), decays after over-foraging   |
| **Water sources**     | Rivers might dry up, forcing new exploration                     |
| **Weather cycles**    | Affect temperature, foraging success, and exertion costs         |
| **Predator frequency**| Increases at night or during certain seasons                     |
| **Group migrations**  | Could force relocation or encounters with rival groups           |

---

## 🎯 Goal: Feedback Loop
Monkey actions update internal state and beliefs → beliefs influence next decisions via the Actor network → decisions alter environment & social dynamics → loop continues.

---

## ⚠️ Design Challenge
Balance **stability** (so monkeys don’t all die immediately) with **pressure** (to encourage learning meaningful survival & social strategies).
- **Stability** refers to how well your actor-critic system avoids catastrophic or chaotic behavior early in training—basically, preventing the system from going off the rails before it has a chance to learn useful policies.
- Stability = Avoiding situations where all your agents (monkeys) engage in actions that lead to mass die-offs, social collapse, or extreme maladaptive behavior patterns before learning can meaningfully occur.
- For example, if early training causes most monkeys to constantly fight and die of injuries or starve because they fail to forage, the learning signal (from the Critic) could be so noisy or uniformly negative that your Actor never gets useful gradients to improve.

---

## Key Questions: 
- Should the reward signal be derived by quantifying the increase/decrease across the six behavioral values? If so, what’s a good way to model those changes numerically over time?  
- Would reinforcement learning with episodic simulations be the best training strategy, or would curriculum learning help better simulate the emergence of complex social behavior?
- How should I use attention mechanisms to weigh the subsystems dynamically?

---

👉 **Next Step Options:**
- We could outline **reward functions** for each of your six behavioral values.
- Or, sketch out **sample simulation runs** (e.g., "Day in the Life of Monkey A") to visualize how all of this plays out.







