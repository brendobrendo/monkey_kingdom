## **Simulation Architecture**  

### **Social & Political Dynamics**  
- **Coordinated Defections** – Groups of monkeys within a tribe may **choose to defect together**, forming a new faction.  
- **Power Struggles & Coups** – Monkeys can **attempt to overthrow the alpha** or **eliminate rival factions** within their tribe.    

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

### **Tribal Instincts**
- **Peer Instinct** - Monkeys will adapt their behavior based on the actions of their peers, following group norms to maintain social cohesion.
- **Hero Instinct** - Certain monkeys may strive to emulate high-status individuals, adopting their strategies and behaviors to climb the social hierarchy.
- **Ancestor Instinct** - Traditional behaviors and past strategies may be passed down, with younger monkeys mimicking and inheriting the habits of previous generations.

## Reward Function

The monkey receives **rewards or penalties** based on the outcome of its actions:

\[
R(s, a, s') =
\begin{cases} 
+10, & \text{if the action increases rank or reproductive success} \\
+5, & \text{if the action secures food, water, or protection} \\
-5, & \text{if the action results in temporary social rejection} \\
-10, & \text{if the action causes injury or exile} \\
-20, & \text{if the action leads to death}
\end{cases}
\]

This reward function ensures that behaviors aligned with **survival and reproduction** are reinforced over time. 

