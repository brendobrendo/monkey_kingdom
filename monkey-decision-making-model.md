# 🐵 Monkey Decision-Making Model

## **1️⃣ Core Motivations: Survival & Reproduction**
At the most fundamental level, a monkey's **decisions are driven by its need to survive and reproduce**. Every action it takes is based on an **internal model** that estimates which behaviors will **maximize** its likelihood of passing down its genes.

## **2️⃣ Monkey’s Internal State**
Each monkey maintains an **internal physical state** that influences its capabilities and decision-making:

### **Physical State Variables**
- **Strength/Weight** – Represents both the monkey’s physical **size and muscular capability**. A heavier monkey is also a stronger monkey, simplifying the world while still maintaining a useful generalization. This directly influences **combat effectiveness**, dominance in social hierarchies, and the ability to carry objects or traverse difficult terrain.
- **Restedness** – The monkey’s **energy level**. A well-rested monkey can fight more effectively, socialize with ease, and assert itself in the dominance hierarchy. Low restedness leads to sluggish reactions and reduced ability to maintain or improve status.
- **Nourishment** – The monkey’s **hunger level**. A well-fed monkey is more capable of engaging in fights, social interactions, and competing for dominance. A hungry monkey will prioritize food-seeking over social climbing or combat.
- **Hydration Level** – Represents the monkey’s **need for water**. A well-hydrated monkey is better able to engage in fights and social activities, while a dehydrated monkey may struggle to compete and focus on securing water.
- **Internal Temperature** – Regulates behavior in extreme heat or cold conditions.

## **3️⃣ Belief Systems & Perception**
Monkeys form beliefs that shape their understanding of themselves, their group, and the external world.

### **Beliefs About the Self**
- **Self-Reliance** – Perceived ability to act independently without relying on the group.
- **Combat Prowess** – Self-assessment of fighting ability based on past encounters.
- **Attractiveness** – Perception of desirability as a mate, influencing reproductive opportunities.
- **Cunning** – Belief in one's ability to outsmart others, whether for food, status, or safety.
- **Resilience** – Confidence in enduring hardships, including injuries, hunger, or social rejection.
- **Curiosity** – Willingness to explore new areas and engage with unfamiliar objects or situations.

### **Beliefs About the Group**
- **Strength Hierarchy** – Identifies the strongest individuals.
- **Social Status** – Determines who is the most well-liked.
- **Trustworthiness** – Identifies allies and potential betrayers.

### **Beliefs About Its Role in the Group**
- **Social Rank** – Estimates its own position in the dominance hierarchy.
- **Personal Likeability** – Determines how others perceive it within the group.

### **Beliefs About the External World**
- **Safety Assessment** – Evaluates threats from other groups.
- **Resource Availability** – Determines if there are sufficient food and water sources.
- **Environmental Dangers** – Identifies predators or hazardous terrain.
- **Geographical Awareness** – Recognizes paths to critical resources like food and water.

## **4️⃣ Core Behavioral Drivers**
A monkey's decisions are guided by its **physical state, beliefs, and fundamental needs**, which can be categorized into six core **behavioral values**:

| **Behavioral Value**   | **Description** |
|------------------------|----------------|
| **Certainty**          | Ensuring safety and avoiding pain. |
| **Variety**            | Seeking new experiences and change. |
| **Significance**       | Gaining importance, status, or uniqueness. |
| **Connection**         | Building close bonds with other monkeys. |
| **Growth**             | Expanding understanding and capabilities. |
| **Contribution**       | Supporting and helping others in the group. |

Each action a monkey takes is based on an **internal calculation** of how well the expected outcome of that action aligns with these **behavioral values**. Ultimately, these values serve the monkey’s core goals of **survival and reproductive success** by shaping its priorities and decision-making.

## **5️⃣ Available Actions**
Monkeys can take various actions based on their beliefs, status, and immediate needs:

### **Social & Hierarchical Actions**
- 🥊 **Fight** – Challenge another monkey to increase rank or defend resources.
- 🏳 **Submit** – Avoid conflict by showing deference.
- 🤝 **Request Allyship** – Attempt to form an alliance.
    - 🍼 **Nurture** – Take care of offspring or close allies.
    - 🧼 **Groom** – Strengthen bonds by grooming another monkey.
    - 🍽 **Share Food** – Offer food to build trust and gain allies.
- ✅ **Accept Allyship** – Strengthen social bonds with a trusted monkey.
- ❌ **Betray** – Backstab an ally for personal gain.
- 🔁 **Remember Betrayal** – Store memory of past betrayals.

### **Survival Actions**
- 💧 **Get Water** – Seek hydration from the nearest source.
- 🍌 **Eat** – Consume food to restore nourishment.

---

## **Example Implementation**
```
python
import torch
import torch.nn as nn
import torch.optim as optim
import numpy as np

# Define the Neural Network Model
class MonkeyDecisionNet(nn.Module):
    def __init__(self, input_size, hidden_size, output_size):
        super(MonkeyDecisionNet, self).__init__()
        self.fc1 = nn.Linear(input_size, hidden_size)  # First hidden layer
        self.fc2 = nn.Linear(hidden_size, hidden_size) # Second hidden layer
        self.fc3 = nn.Linear(hidden_size, output_size) # Output layer
        self.activation = nn.ReLU()  # Activation function for hidden layers

    def forward(self, x):
        x = self.activation(self.fc1(x))
        x = self.activation(self.fc2(x))
        x = self.fc3(x)  # No activation on output layer (raw utility scores)
        return x

# Define Monkey Inputs
physical_state = [
    "Strength/Weight", "Restedness", "Nourishment", "Hydration Level", "Internal Temperature"
]

beliefs = [
    "Self-Reliance", "Combat Prowess", "Attractiveness", "Cunning", "Resilience", "Curiosity",
    "Strength Hierarchy", "Social Status", "Trustworthiness",
    "Social Rank", "Personal Likeability",
    "Safety Assessment", "Resource Availability", "Environmental Dangers", "Geographical Awareness"
]

behavioral_values = ["Certainty", "Variety", "Significance", "Connection", "Growth", "Contribution"]

actions = [
    "Fight", "Submit", "Request Allyship", "Nurture", "Groom", "Share Food", 
    "Accept Allyship", "Betray", "Remember Betrayal", "Get Water", "Eat"
]

# Set input size, hidden size, and output size
input_size = len(physical_state) + len(beliefs) + len(behavioral_values)
hidden_size = 32  # Can be tuned
output_size = len(actions)

# Instantiate model
model = MonkeyDecisionNet(input_size, hidden_size, output_size)

# Define loss function and optimizer
criterion = nn.MSELoss()  # Mean Squared Error for utility prediction
optimizer = optim.Adam(model.parameters(), lr=0.01)  # Adam optimizer

# Generate Sample Data (Simulated)
def generate_sample():
    # Random values between 0 and 1 for input features
    input_tensor = torch.tensor(np.random.rand(input_size), dtype=torch.float32)
    
    # Simulate a target utility score for each action (random for now)
    target_tensor = torch.tensor(np.random.rand(output_size), dtype=torch.float32)

    return input_tensor, target_tensor

# Training Loop
num_epochs = 500
for epoch in range(num_epochs):
    optimizer.zero_grad()

    # Generate training data
    input_tensor, target_tensor = generate_sample()
    
    # Forward pass
    output = model(input_tensor)
    
    # Compute loss
    loss = criterion(output, target_tensor)
    
    # Backpropagation
    loss.backward()
    optimizer.step()
    
    if epoch % 50 == 0:
        print(f"Epoch [{epoch}/{num_epochs}], Loss: {loss.item():.4f}")

# Decision Function
def choose_action(state):
    """
    Given a state (physical state, beliefs, behavioral values),
    predict the best action based on learned utility scores.
    """
    state_tensor = torch.tensor(state, dtype=torch.float32)
    with torch.no_grad():
        action_utilities = model(state_tensor)
    best_action_idx = torch.argmax(action_utilities).item()
    return actions[best_action_idx], action_utilities.numpy()

# Example Decision
test_state = np.random.rand(input_size)  # Random monkey state
best_action, utilities = choose_action(test_state)
print(f"Best Action: {best_action}")
```

## **📌 Summary**
- Monkeys make **decisions** based on their **physical state, beliefs, and core needs**.
- **Survival & reproduction** drive all actions.
- Their beliefs about **self, group, and environment** shape their strategy.
- They can take **various actions** like fighting, submitting, forming alliances, and resource gathering.

---

### **🔜 Next Steps**
Would you like:
1. **A mathematical model** for decision-making probabilities?  
2. **A reinforcement learning framework** to evolve these behaviors over time?  
3. **A state machine diagram** to visualize decision pathways?  

Let me know how you'd like to proceed! 🚀🐵
