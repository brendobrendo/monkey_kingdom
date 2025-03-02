# Monkey Social Simulation Game

## Overview
This project is a **high-level simulation game** that models how **monkeys develop social capital, gain access to resources, and navigate intra- and inter-tribal dynamics**.

## Core Features

**Game Interactivity: A Purely Observational Simulation** Unlike traditional strategy games where players make decisions, this simulation is entirely hands-off. Players do not directly control monkey tribes, influence battles, or allocate resources. Instead, the game autonomously runs its course, with monkeys acting based on pre-defined AI behaviors, environmental factors, and past experiences.

Players interact with the game through a **dashboard-based interface**, which provides insights into tribal dominance, social struggles, and resource distribution. The primary role of the player is to observe, analyze exported data, and use the structured YAML format for further storytelling or research purposes.

### 1. **AI & Behavior Modeling**
- **Agent-Based Simulation**: Monkeys act based on **social rank, resource availability, and environmental factors**.
- **Finite State Machines (FSMs)**: Handles basic actions (foraging, resting, fighting, etc).
- **Behavior Trees/Decision Trees**: Governs **complex social interactions** (alliances, betrayals, dominance struggles).
- **Simulation Logic**: Focused on **lightweight CPU performance**, avoiding GPU-intensive operations.

### 2. **Game UI & Visualization**
The **primary visual component** is a **Risk-style 2D map**, which serves as the central way to track **territory control, population distribution, and key game events**. This is supported by a **structured dashboard** that provides insights into simulation metrics and allows for exporting game state data for further analysis or storytelling.

---

### **Map Design & Representation**
The map is divided into **major territories**, each containing **distinct sub-territories** labeled for reference (e.g., *Western Plains*, *Tallgrass Valley*). **Only one tribe can occupy a territory at a time**, with the controlling tribe’s **color** visually marking ownership (e.g., 🔴 Firefangs, 🔵 Shadow Claws). Each territory displays a **numeric token** representing the **number of monkeys present**, similar to army stacks in *Risk*, dynamically updating as the board state changes after each turn/iteration.

#### **Territory Visualization**
- **Color-Coded Control System**:
  - 🟢 **Green** – Resource-rich region.  
  - 🟡 **Yellow** – Balanced conditions.  
  - 🔴 **Red** – Conflict zone.  
  - ⚫ **Grey** – Unoccupied territory.  
- **Overlay Icons for Key States**:
  - ⚔️ **Active Conflict** – A battle is ongoing in this region.  
  - 🍌 **Resource Abundance** – High food and water availability.  

---

### **Main Dashboard Overview**
The **dashboard** provides a **structured, data-driven** view of the game state, divided into **multiple panels** displaying high-level statistics, trends, and interactive data export options.

#### **A. Game State Summary (Snapshot View)**
A **top-level overview** that condenses **key statistics** into a readable, glanceable format.
- 🏞 **Number of Territories** – Total, occupied vs. unoccupied.  
- 🐵 **Monkey Population** – Total count, tribe distributions.  
- 🏆 **Dominant Tribes/Factions** – Top tribes and leaders.  
- 🍌 **Resource Availability** – Overall food & water abundance.  
- ⚔️ **Conflict Level** – Heatmap indicator (low to high).  
- 🔄 **Recent Major Events** – Summary of significant changes (e.g., "A power struggle occurred in the Eastern Jungle").  
- ⏳ **Simulation Time** – Number of days in-game.  

#### **B. Interactive Panels & Territory Data**
Players can drill down into **specific territories** and **monkey profiles** using detailed panels:

##### **1. Territory Breakdown Panel**
Clicking on a **territory** opens a structured breakdown of:
- **Current Owner** – Which tribe controls the territory.  
- **Population Count** – Total number of monkeys present.  
- **Resource Levels** – Fruit and other resource availability.  
- **Recent Events** – Leadership changes, battles, or major population changes.  
- **Territory Relationships** – Allies, rivalries, and power struggles.  
📋 **Copy Territory Data**: Allows for export of territory-specific details.

##### **2. Individual Monkey Panel**
Selecting a **monkey profile** provides granular details:
- **Tribe Affiliation** – Which faction they belong to.  
- **Social Rank** – Alpha, Beta, subordinate, or outcast.  
- **Allies & Rivals** – List of connected relationships.  
- **Recent Actions** – Battles fought, leadership changes, or betrayals.  
📋 **Copy Monkey Data**: Exports the full profile for deeper storytelling.

---

### **C. Data Export Buttons (Copy-Paste-Friendly)**
The dashboard integrates **export options** for easy data retrieval:
- 📋 **Copy Full Game State** – Copies **all data** (entire map, events, monkeys, territories).  
- 📋 **Copy High-Level Summary** – Copies **only the top-level dashboard stats**.  
- 📋 **Copy Territory-Specific Data** – Select and **export a specific territory’s** state.  
- 📋 **Copy Individual Monkey Data** – Extract a **single monkey’s** details for analysis.  

---

### **Why This Approach?**
✅ **Efficient, High-Level Overview** – Quick interpretation of **tribal dominance and population shifts**.  
✅ **Drill-Down Data Access** – Allows exploration of **territory-specific and individual monkey details**.  
✅ **ChatGPT-Compatible Exporting** – Facilitates **easy copy-paste** to generate storytelling and data analysis.  
✅ **Lightweight & AI-Driven** – Designed for **smooth simulation updates** without requiring heavy real-time rendering.  

---

This dashboard-centric design ensures **clear game-state tracking**, making it **easy to analyze, export, and generate stories from** the evolving world of monkey tribes.

### 3. **Data-Driven Gameplay (Copy-Paste Export for Storytelling)**
- **Game State Summary** (Snapshot of entire simulation).
- **Territory Breakdown Panel** (Stats on population, dominance, resources, and recent events).
- **Monkey Profiles** (Details on specific individuals, including rank, allies, needs, and actions).
- **Recent Events Log** (Chronological list of significant occurrences in the game world).

#### **Example Exported Data Format**
```yaml
Simulation Snapshot:
- Time: Day 34
- Total Monkeys: 120
- Dominant Tribe: Firefangs (35 members)
- Highest Conflict: Western Plains (Territory 4)
- Major Event: Alpha of the Firefangs, "Grizzle," challenged rival leader "Torn Ear" and won. The Firefangs expanded their territory.
```

### 4. Core Mechanics

#### 1. Data Export Format  
- All game state exports, including monkey profiles, tribal events, and simulation snapshots, will be in **YAML format** for readability and structured analysis by ChatGPT when it is copy and pasted.

#### 2. Territorial Battles  
- Battles will function similarly to **Risk**, using a probabilistic system where larger armies have a higher chance of success.  
- However, outcomes are **not purely deterministic**, meaning smaller groups still have a chance to win.

#### 3. Social Struggles & Leadership Succession  
- The mechanics for **social struggles** are still in development but will follow a structured approach similar to territorial battles.  
- **Leadership succession** will also be probabilistic, with factors like influence, past victories, and alliances playing a role.

#### 4. Population Growth  
- To simplify the game, monkeys will be a **single-sex species (male)** and will only reproduce when they achieve **significant success or milestones**.  
- This mechanic mirrors real-world monkey population growth, ensuring that high-status or successful individuals contribute more to the tribe's expansion.

#### 5. Resource System  
- The only tracked resource will be **food (fruit)**, which serves both as sustenance and a decaying asset.  
- Monkeys consume food for energy, but unconsumed food will **expire over time**, affecting long-term sustainability.
- The fruit grows on trees which have their own properties (ie current size, size potential, growth rate, replenishment of fruit and leaves) based on the type of tree

#### 6. Learning & Memory  
- Monkeys will **remember past betrayals and alliances**, influencing their future decisions.  
- Over time, this can lead to emerging social dynamics such as **long-standing feuds** or **deep-rooted loyalties**.

#### 7. Turn-Based Simulation  
- The game is **turn-based**, with each iteration representing **a new day**.  
- Every turn, **each monkey and game object will process one move** before the simulation advances.

#### 8. Error Handling & Data Integrity  
- The system will handle **error management and data integrity** in a future development phase.

### 5. **Development Considerations**
- **Tech Stack**:
  - **Web-Based UI**: React + D3.js for visualization.
  - **Backend Simulation**: Python (Flask).
  - **Data Storage**:
    - **MongoDB**: Stores **game state, territory details, and event logs** in a flexible, document-based format.
    - **Neo4j**: Manages **monkey relationships, social dynamics, and power structures**, enabling efficient graph-based queries.
    - **PostgreSQL**: Introduced later for **structured economic/resource systems**, ensuring transactional consistency as the game expands.
- **Performance Optimizations**:
  - No complex animations or rendering of individual agents.
  - Dashboard updates periodically based on simulation state.
- **Expandability**:
  - Future auto-generated **weekly summary reports**.

## Why This Approach?
✅ **Lightweight & CPU-Friendly** (No GPU required).  
✅ **Structured for Exporting Data to ChatGPT** (For AI-generated storytelling).  
✅ **Minimalist UI Focused on Simulation & Metrics**.  
✅ **Scalable & Modular for Future Enhancements**.  

---

### **Next Steps**
1. Develop **initial simulation logic** (FSMs, behavior trees, resource mechanics).  
2. Implement **basic UI with a working map and dashboard**.  
3. Design **export functionality for ChatGPT integration**.  
4. Playtest and refine monkey behaviors for emergent interactions.  

---