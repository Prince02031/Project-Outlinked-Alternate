# 🎮 Game Design Document  
### *Outlinked: Disaster Coordination Command*

---

## Table of Contents
1. [Game Overview](#1-game-overview)  
2. [Game Concept](#2-game-concept)  
3. [Gameplay Mechanics](#3-gameplay-mechanics)  
   - [3.1 Camps and Connectivity](#31-camps-and-connectivity)  
   - [3.2 Volunteer and Unit System](#32-volunteer-and-unit-system)  
   - [3.3 Disaster System](#33-disaster-system)  
   - [3.4 Success Determination Formula](#34-success-determination-formula)  
   - [3.5 Unit Risk System *(Secondary Mechanic)*](#35-unit-risk-system-secondary-mechanic)  
   - [3.6 Scouting and Discovery](#36-scouting-and-discovery)  
   - [3.7 Turn-Based System](#37-turn-based-system)  
4. [Progression and Replayability](#4-progression-and-replayability)  
5. [Visual and UI Design (Conceptual)](#5-visual-and-ui-design-conceptual)  
6. [Key Features & Differentiators](#6-key-features--differentiators)  
7. [Class Design Examples](#7-class-design-examples)  
   - [7.1 Camp (Abstract Base Class)](#71-camp-abstract-base-class)  
   - [7.2 Disaster (Class)](#72-disaster-class)  
   - [7.3 Unit (Abstract Base Class)](#73-unit-abstract-base-class)  
   - [7.4 MissionManager (Class)](#74-missionmanager-class)  
   - [7.5 GameStateManager (Class)](#75-gamestatemanager-class)  

---

## 1. Game Overview

**Title:** Outlinked: Disaster Coordination Command  
**Genre:** Turn-based Strategy / Disaster Management Simulator  
**Target Platform:** PC  
**Target Audience:** Strategy and simulation enthusiasts 

**Elevator Pitch:**  
A post-disaster world where communication has collapsed. As the central crisis coordinator, you must reconnect isolated survivor camps, dispatch volunteers to resolve crises, and restore stability — all under tight resource constraints. Every decision can mean life or loss in this humanitarian strategy simulation.

---

## 2. Game Concept

Set in a near-future world disrupted by large-scale environmental disasters, *Outlinked* places the player in control of a central coordination hub. The world is fractured, and communication lines are down. Camps are scattered across a devastated region, suffering from disease, infrastructure failures, and shortages.

Your mission: Scout, reconnect, and stabilize. You won’t do this with weapons — but with volunteers, planning, and resolve.

---

## 3. Gameplay Mechanics

### 3.1 Camps and Connectivity
- The map contains two types of camps: **General Camps** (large regional hubs) and **Small Camps** (isolated or outpost camps).
- Camps are initially disconnected from the central hub and must be **reconnected** by dispatching **Courier Units**.
- Each camp has a **level (1–5)** indicating how far it is from the center. Level 1 is immediately accessible, while others must be discovered.

### 3.2 Volunteer and Unit System
-  The **Central Command** fields three categories of specialized units:
	- **Medical Teams:** handles disease outbreaks
	- **Repair Crews:** fix infrastructure and communication
	- **Courier Squad:** scout and reconnect camps, perform delivery
- These units are formed and managed directly by the **central hub**.
- Camps generate **volunteers** of same three types — Medical, Repair, and Courier — based on their operational capacity.
- Volunteers are **stored at the central hub**, and once enough are gathered, they are converted into **specialized units** that can be dispatched on missions.

### 3.3 Disaster System
- Camps can be struck by disasters:
  - **Disease Outbreaks**
  - **Supply Shortages**
  - **Structural Damage**
  - **Communication Breakdown**
  - **Environmental Hazards** (divided into subtypes)
- Each disaster has:
  - A **type**
  - A **severity level** (1–5)
  - A **resolution timer** (turns left before escalation)
- Unresolved disasters **escalate**, causing further health damage and increased difficulty.
- Only matching units can attempt resolution (e.g., Medical unit for disease).

### 3.4 Success Determination Formula

- Success of a mission depends on multiple factors:

	- **Disaster severity** — Higher severity lowers the success rate.
	- **Camp level** — Distant camps (higher levels) are harder to assist effectively.
	- **Unit match** — Mismatched units cannot resolve certain disaster types.
	- **Unit sufficiency** — Sending fewer units than required reduces the success chance.
	- **Camp health condition** — Poor health reduces overall success odds.

### 3.5 Unit Risk System *(Secondary Mechanic)*

- If a mission **fails**, the dispatched unit may suffer consequences:

	- **Injury** — The unit becomes temporarily unavailable for future missions.
	- **Disbanded/Lost** — The unit is permanently removed from the available pool.

- These risks increase when facing high-severity disasters, under-resourced missions, or poor camp conditions.

### 3.6 Scouting and Discovery
- Camps beyond level 1 start **undiscovered**.
- Courier units are used to **scout** for new camps.
- Upon successful discovery, camps can start sending volunteers and receiving aid.

### 3.7 Turn-Based System
- The game is divided into **turns**, each representing a day.
- **Day Phase**: Player assigns actions — dispatch units, form new teams, scout.
- **Night Phase**: AI resolves missions, updates disasters, generates volunteers, applies consequences.
- Player must adapt to new threats and keep camps alive under rising pressure.

---

## 4. Progression and Replayability

- Early gameplay focuses on **reconnection** and **basic disaster response**.
- As time progresses:
  - **Disasters grow more severe**
  - **More camps become vulnerable**
  - **Volunteers may become scarce**
- Game difficulty increases based on player's recon pace and disaster handling.
- Optional features: Random map layouts, multiple region modules.

---

## 5. Visual and UI Design (Conceptual)

- The UI is split into:
  - **Map View**: Shows camps, links, disasters, unit paths
  - **Command Panel**: Dispatch units, form teams, view status
  - **Camp Info View**: Hover or click camps for disaster, health, and volunteer data
- Visual style is minimalist, using clear icons and overlays to reflect strategic clarity over aesthetic detail.
- **Miniature graphics and terrain details** are included to provide environmental context and improve immersion, without compromising clarity.

---

## 6. Key Features & Differentiators

- **No combat, only coordination** — pure strategy through aid and logistics  
- **Connectivity-focused gameplay** – reconnect and support camps, not dominate or conquer
- **Volunteer-driven system** — your only resource is human will  
- **Escalating disasters** and branching failure paths  
- **Turn-based tempo** that rewards planning and adapts to emergencies  
- **Educational potential** around real-world crisis management themes (aligned with SDG 11 and 13)

------

## 7. Class Design Examples

These simplified Java class outlines show the object-oriented structure behind the simulation logic.

### 7.1 Camp (Abstract Base Class)
```java
// Abstract base class for all camps in the game
public abstract class Camp {
    protected String id;
    protected boolean isConnectedToCenter;
    protected boolean scouted;
    protected int distanceLevel;
    protected int health;
    protected List<Disaster> activeDisasters;

    // Disaster management
    public void addDisaster(Disaster d);
    public void resolveDisaster(Disaster d);
    public boolean isCollapsed();

    // Scouting
    public boolean isScouted();
    public void markScouted();

    // Volunteer generation
    public abstract Optional<Volunteer> produceVolunteer();
    public abstract List<Volunteer> gatherVolunteers();
}
```

### 7.2 Disaster (Class)
```java
// Represents a disaster affecting a camp
public class Disaster {
    private DisasterType type;
    private int severity; // Ranges from 1 (mild) to 5 (severe)
    private int turnsRemaining;
    private boolean resolved;
    private boolean escalatedThisTurn;

    // Disaster behavior
    public void escalate();
    public boolean isCritical();
    public boolean canBeResolvedBy(UnitType unitType);
    public int getRequiredUnitCount();
    public void markResolved();

    // Escalation tracking
    public boolean wasEscalatedThisTurn();
    public void resetEscalationFlag();

    // Accessor
    public int getSeverity();
}

```

### 7.3 Unit (Abstract Base Class)
```java
// Abstract base class for all unit types dispatched from the command center
public abstract class Unit {
    protected String id;
    protected UnitType type;
    protected boolean isAvailable;
    protected int turnsUntilReturn;

    // Dispatch the unit to a target camp
    public void dispatchTo(Camp target);

    // Progress unit state each turn (e.g., return countdown)
    public void processTurn();
}
```

### 7.4 MissionManager (Class)
```java
// Represents a unit’s mission to a camp
public class MissionManager {
    private Camp targetCamp;
    private Unit assignedUnit;
    private boolean success;
    private int delayTurns; // Turns delayed before mission completion

    // Resolves the mission outcome (success or failure)
    public void resolveOutcome();
}
```

### 7.5 GameStateManager (Class)
```java
// Manages the overall game flow and turn progression
public class GameStateManager {
    private int dayCount;
    private GamePhase currentPhase; // e.g., DAY, NIGHT

    // Starts a new day phase
    public void startNewDay();

    // Transitions to the night phase where events resolve
    public void enterNightPhase();

    // Processes end-of-turn updates and state changes
    public void processEndOfTurn();
}

```
