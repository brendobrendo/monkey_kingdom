# Monkey Social Simulation Game

## Overview
This project is a **high-level simulation game** that models how **monkeys develop social capital, gain access to resources, and navigate intra- and inter-tribal dynamics**. Instead of direct interactivity, the game provides **a structured dashboard with exportable data** for analysis and storytelling.

## Core Features

### 1. **AI & Behavior Modeling**
- **Agent-Based Simulation**: Monkeys act based on **social rank, resource availability, and environmental factors**.
- **Finite State Machines (FSMs)**: Handles basic actions (foraging, resting, moving).
- **Behavior Trees/Decision Trees**: Governs **complex social interactions** (alliances, betrayals, dominance struggles).
- **Simulation Logic**: Focused on **lightweight CPU performance**, avoiding GPU-intensive operations.

### 2. **Game UI & Visualization**
- **Dashboard-Based Interface** (No real-time rendering of individual agents).
- **Risk-Style 2D Map**:
  - Territories represented as **colored regions**.
  - Icons overlay to indicate key states (**dominance, conflict, resource levels**).
  - **Color Coding**: Green (resource-rich), Yellow (balanced), Red (conflict), Grey (unoccupied).
  ## **Map Design & Representation**

The game map follows a **Risk-style** design, divided into **major territories** with distinct **sub-territories**, each labeled for reference (e.g., "Western Plains," "Tallgrass Valley"). **Only one tribe can occupy a territory at a time**, with control visually represented by the tribe's **color** (e.g., red for the Firefangs, blue for the Shadow Claws). Within each territory, a **numeric piece/token** displays the **number of monkeys present**, similar to army stacks in Risk, dynamically updating as monkeys migrate, battle, or reproduce. 

**Icons provide quick insights** into each region’s status, such as ⚔️ for active conflict, 🍌 for resource abundance, or ⏳ for pending migrations. Monkeys can **move into adjacent unoccupied territories** or **attack enemy-held regions**, with **combat resolving control**—the winner **claims the land**, and the losing tribe is displaced. The **map updates instantly** to reflect new ownership. Movement is constrained by **natural barriers** (e.g., rivers, mountains), shaping strategic decisions. 

This **simple, data-driven board** allows players to quickly **interpret tribal dominance, population shifts, and strategic positioning**, making it highly **compatible with AI-driven simulations and exportable game state data for storytelling**.


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

### 4. **Development Considerations**
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


