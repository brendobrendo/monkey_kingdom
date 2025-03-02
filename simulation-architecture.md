## **Simulation Architecture**  

### **Social & Political Dynamics**  
- **Coordinated Defections** – Groups of monkeys within a tribe may **choose to defect together**, forming a new faction.  
- **Power Struggles & Coups** – Monkeys can **attempt to overthrow the alpha** or **eliminate rival factions** within their tribe.  

### **Resource Management & Environmental Constraints**  
- **Diverse Resource Types** – The world will contain multiple resource types:  
  - 🍌 **Food (Bananas)** – The **only food source for now**. Grows on trees and is essential for survival.  
  - 🌳 **Trees** – Provide both **bananas** and **wood**, enabling monkeys to gather food and construct tools or shelters.  
  - 🪵 **Wood** – Usable for **nest-building, crafting tools, or making defensive structures**.  
  - 🪨 **Stone** – Can be used to create **tools, weapons, and machines**.  
  - 💧 **Water** – Essential for survival; monkeys must remain close to a source to drink.  

### **Individual Monkey AI & Decision-Making Model**  
- Each monkey has an **internal AI model** that determines its actions based on:  
  - **Perceived Tribe Resources** – Estimates the tribe’s **food, water, and shelter** availability.  
  - **Resource Entitlement & Rank** – Assesses whether it is receiving **a fair share** of resources based on its **social status**.  
  - **Strategic Social Behavior** – Can **form alliances, betray allies, or attempt to rise in social rank**.  

### **Advanced Learning & Cognitive Features**  
To create **realistic and evolving monkey behavior**, the AI model will incorporate:  
- **Actor-Critic Model** – Enables reinforcement learning by evaluating past actions and outcomes.  
- **Temporal Credit Assignment** – Helps monkeys attribute long-term rewards to their decisions.  
- **Exploration-Exploitation Tradeoff** – Balances between trying new strategies vs. relying on known successful behaviors.  
- **Associative Learning** – Monkeys can develop reflexes by linking specific stimuli to past experiences.  
- **Reinforcement Learning** – They can learn new behaviors through trial and error.  
- **Vicarious Learning** – Monkeys can imagine alternative outcomes to inform future decisions.  
- **Mentalization (Theory of Mind)** – They can predict **other monkeys' intentions** based on past behavior.  
