# Monkey Tribe Simulator  

## Overview  
This project is a **high-level simulation** that models how **monkeys develop social capital, gain access to resources, and navigate intra- and inter-tribal dynamics**. The game generates structured simulation data, which users can copy and paste into ChatGPT to generate compelling storylines based on in-game events.

## How It Works  
Monkey Tribe Simulator runs autonomously, meaning you observe the world evolve rather than controlling tribes directly. The game progresses as follows:  

1. **Turn Progression**: Each turn represents a new day in the monkey world.  
2. **Monkey Actions**: Each monkey makes one decision per turn (moving, eating, fighting, etc.).  
3. **Social & Tribal Dynamics**: Tribes form alliances, battle for territory, and rise or fall in power.  
4. **Resource Consumption**: Monkeys eat available fruit, and food regenerates based on tree growth.  
5. **Export Data for Storytelling**: The dashboard allows easy data export in YAML format for use with ChatGPT to generate narratives.  

## **Core Mechanics & Features**  

### 🔄 **Turn-Based Simulation**  
- The game is **turn-based**, with each iteration representing **a single day** in the simulation.  
- Every turn, **each monkey and game object processes one move** before the simulation advances.  
- A monkey’s **natural lifespan is 150 days**.  

### 🧠 **AI-Driven Individual Behavior**  
- **Each monkey has its own AI model**, determining decisions based on its **social rank, past experiences, and environmental factors**.  
- Monkeys **form alliances, betray rivals, and compete for dominance**, leading to **emergent, unscripted gameplay**.  

### 🌍 **Territory Control & Battles**  
- Tribes **compete for land** using a **Risk-style** combat system.  
- Battles are **probabilistic**, where **larger armies have a higher chance of success**, but smaller groups can still win.  

### 🎭 **Social Struggles & Leadership Succession**  
- Monkeys engage in **power struggles** within their tribes, and leadership succession is determined **probabilistically** based on:
  - **Influence & reputation** within the tribe  
  - **Past victories** and combat history  
  - **Alliances and support** from key tribe members  

### 🏆 **Population Growth**  
- To simplify the simulation, monkeys are a **single-sex species (male)** and reproduce only when achieving **significant milestones**, such as:
  - Gaining **Alpha rank**  
  - **Holding territory** for X turns  
  - Winning **a certain number of battles**  
- This ensures that **high-status or successful individuals** drive population expansion, mirroring real-world monkey tribe growth dynamics.  

### 🍌 **Resource Management**  
- The only tracked resource is **food (fruit)**, which serves as both **sustenance and a decaying asset**.  
- **Monkeys consume food** to maintain energy levels.  
- **Food grows on trees**, which have unique properties based on their type, including:
  - **Current size & growth potential**  
  - **Growth rate**  
  - **Fruit replenishment cycles**  

### 🧠 **Memory & Learning**  
- Monkeys **remember past betrayals and alliances**, influencing their future decisions.  
- Over time, this can lead to emerging social behaviors, such as:
  - **Long-standing feuds** with rivals  
  - **Deep-rooted loyalties** to allies  

### 🗺️ **Interactive Visualization**  
- A **2D map & structured dashboard** display real-time tribal dynamics, showing:
  - **Territory control** (which tribes own what land)  
  - **Population distribution** (size and movement of tribes)  
  - **Key game events** (battles, leadership changes, resource depletion)  

### 📋 **Data Export for Storytelling**  
- The game exports structured simulation data in **YAML format**, making it easy to copy and paste into ChatGPT.  
- Players can extract:
  - **Game State Summary** (Full snapshot of simulation state)  
  - **Territory Breakdown** (Who controls what, resource availability)  
  - **Monkey Profiles** (Rank, actions, allies, and enemies)  
  - **Recent Events Log** (Battles, power shifts, major changes)  
- This allows users to **generate compelling stories** explaining what happened in the monkey world.  

### ⚠️ **Error Handling & Data Integrity**  
- **Error management and data integrity** will be addressed in a **future development phase** to ensure a stable simulation.

## **Game UI & Visualization**  

The **primary visual component** is a **Risk-style 2D map**, which serves as the central tool for tracking **territory control, population distribution, and key game events**. This is complemented by a **structured dashboard with side panels**, which provide detailed insights into simulation metrics.  

The dashboard is designed for **seamless data export**, allowing users to **copy and paste** game state data into ChatGPT. This enables **further analysis and storytelling**, helping to generate narratives that explain the simulation outputs, enrich the game world, and provide deeper context for tribal dynamics.

---

### **Map Design & Representation**
The map is divided into **major territories**, each containing **distinct sub-territories** labeled for reference (e.g., *Western Plains*, *Tallgrass Valley*). **Only one tribe can occupy a territory at a time**, with the controlling tribe’s **color** visually marking ownership (e.g., 🔴 Firefangs, 🔵 Shadow Claws). Each territory displays a **numeric token** representing the **number of monkeys present**, similar to army stacks in *Risk*, dynamically updating as the board state changes after each turn/iteration.

---

### **Main Dashboard Overview**
The **dashboard** provides a **structured, data-driven** view of the game state, divided into **multiple panels** displaying high-level statistics, trends, and interactive data export options.

#### **A. Game State Summary (Snapshot View)**
A **top-level overview** that condenses **key statistics** into a readable, glanceable format.
- 🏞 **Number of Territories** – Total, occupied vs. unoccupied.  
- 🐵 **Monkey Population** – Total count, tribe distributions.  
- 🏆 **Dominant Tribes/Factions** – Top tribes and leaders.  
- 🍌 **Resource Availability** – Overall food abundance.  
- ⚔️ **Conflict Level** – Heatmap indicator (low to high).  
- 🔄 **Recent Major Events** – Summary of significant changes (e.g., "A power struggle occurred in the Eastern Jungle").  
- ⏳ **Simulation Time** – Number of days in-game.  
- 📋 **Copy Full Game State** – Copies **all data** (entire map data, events, monkeys, territories).  
- 📋 **Copy High-Level Summary** – Copies **only the top-level dashboard stats**.

#### **B. Interactive Panels & Territory Data**
Players can drill down into **specific territories** and **monkey profiles** using detailed panels:

##### **1. Territory Breakdown Panel**
Clicking on a **territory** opens a structured breakdown of:
- **Current Owner** – Which tribe controls the territory.  
- **Population Count** – Total number of monkeys present.  
- **Resource Levels** – Food and other resource availability (so far food is the only resource).  
- **Recent Events** – Leadership changes, battles, or major population changes.  
- **Territory Relationships** – Allies, rivalries, and power struggles.  
📋 **Copy Territory Data**: **Copy and paste a specific territory’s** state to ChatGPT for analysis. 

##### **2. Individual Monkey Panel**
Selecting a **monkey profile** provides granular details:
- **Tribe Affiliation** – Which faction they belong to.  
- **Social Rank** – Alpha, Beta, subordinate, or outcast.  
- **Allies & Rivals** – List of connected relationships.  
- **Recent Actions** – Battles fought, leadership changes, or betrayals.  
📋 **Copy Monkey Data**: Extract a **single monkey’s** details to copy and paste into ChatGPT for analysis. 

---

This dashboard-centric design ensures **clear game-state tracking**, making it **easy to analyze, export, and generate stories from** the evolving world of monkey tribes.

## Data Export & Storytelling  
- The game exports structured simulation data in **YAML format**, making it easy to copy and paste into ChatGPT.  
- Players can extract:
  - **Game State Summary** (Full snapshot of simulation state)
  - **Territory Breakdown** (Who controls what, resource availability)
  - **Monkey Profiles** (Rank, actions, allies, and enemies)
  - **Recent Events Log** (Battles, power shifts, major changes)  
- This allows users to **generate compelling stories** explaining what happened in the monkey world.  

## **Development Considerations**
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
✅ **Hands-off simulation**: The world evolves naturally without direct player control.  
✅ **AI-driven storytelling**: Every run produces **unique** monkey history that can be turned into a narrative.  
✅ **Lightweight & scalable**: The simulation is optimized for performance, avoiding GPU-heavy computations (No GPU required).  

---

## **Next Steps**  

1. **Develop UI Components**  
   - Implement a **Risk-style 2D map** displaying **territory control, population distribution, and game events**.  
   - Build a **structured dashboard with side panels** for detailed simulation insights.  

2. **Implement Core Simulation Logic**  
   - Develop **Finite State Machines (FSMs)** to govern basic monkey actions (foraging, resting, fighting).  
   - Implement **Behavior Trees/Decision Trees** for complex interactions (alliances, betrayals, leadership struggles).  
   - Introduce **turn-based processing** where each monkey makes one move per day.  

3. **Create Data Export & ChatGPT Integration**  
   - Implement **YAML export functionality** for easy game-state extraction.  
   - Design a **copy-and-paste feature** that allows users to quickly transfer data into ChatGPT for storytelling.  

4. **Refine AI-Driven Monkey Behavior**  
   - Playtest monkey interactions to ensure **realistic alliances, betrayals, and power struggles**.  
   - Fine-tune **memory retention** so monkeys recall past betrayals and alliances appropriately.  
   - Adjust **leadership succession mechanics** to ensure natural leadership shifts within tribes.  

---