# 🌾 AgroDrone — Autonomous Crop Disease Monitoring System

> **AI-powered drone & satellite precision agriculture platform**  
> Integrating Fuzzy Logic · A\* Coverage Path Planning · Bayesian Inference · IoT Sensor Networks

[![Live Demo](https://img.shields.io/badge/Live%20Demo-View%20MVP-blue?style=flat-square)](https://agro-drone-mvp.vercel.app/)
[![Assignment](https://img.shields.io/badge/Course-Artificial%20Intelligence-green?style=flat-square)]()
[![Institute](https://img.shields.io/badge/Institute-NIET%2C%20Greater%20Noida-orange?style=flat-square)]()
[![AI Modules](https://img.shields.io/badge/AI%20Modules-1%2C%203%20%26%204-purple?style=flat-square)]()

---

## 📌 Overview

This project proposes an **Autonomous Agentic Drone-Based and Satellite Crop Disease Monitoring System** — a convergence of Artificial Intelligence, IoT, Low Earth Orbit (LEO) satellite imagery, and multi-rotor drone swarms to deliver real-time, precision crop health surveillance across vast farmlands with zero need for human intervention.

The system is a closed-loop autonomous pipeline:
- 🛰️ **LEO satellites** provide macro-level spectral data (NDVI, EVI) for trend prediction
- 🚁 **Drone swarms** navigate fields in boustrophedon grid patterns, capturing visual data and spraying targeted pesticide doses
- 🌡️ **IoT sensors** measure wind, humidity, temperature, and soil moisture in real time
- 🧠 **AI modules** govern every decision — from path planning to dosing to disease prediction

---

## 🎮 MVP Prototype

An interactive browser-based simulation of the full system pipeline.

**[➜ Open `AgroDrone_MVP.html` in any browser — no installation needed]**

### What the MVP demonstrates

| Feature | Description |
|---|---|
| 🗺️ Farm Grid (10×7) | Drone traverses 70 cells in boustrophedon (lawnmower) path |
| 🧠 Fuzzy Logic Engine | Real COA defuzzification — drag sliders to see live dose computation |
| 📡 IoT Telemetry | Wind, humidity, temperature, NDVI update per cell with sensor noise |
| 📊 Bayesian Predictor | Outbreak probability updates live as cells are surveyed |
| 📋 Mission Log | Expert system decisions logged in real time |
| ▶️ Mission Control | Start, Pause, Resume, Reset — full simulation control |

### Color legend

| Color | Meaning |
|---|---|
| ⬜ Light gray | Unvisited cell |
| 🟩 Green | Healthy crop — dose ≤ 2 ml/m² |
| 🟨 Amber | Low infection — dose 2–8 ml/m² |
| 🟧 Orange | Moderate infection — dose 8–16 ml/m² |
| 🟥 Red | High / Severe infection — dose > 16 ml/m² |
| 🟦 Steel blue | Wind abort — spray suspended (wind > 5 m/s) |
| 💙 Blue pulse | Current drone position |

---

## 🤖 AI Modules Implemented

### Module 1 — Coverage Path Planning (A\* Search)

The drone traverses every cell of the M×N grid exactly once using the **Boustrophedon (lawnmower) pattern**, modelled as a graph traversal problem. A\* search is overlaid to handle dynamic obstacles detected mid-flight by LIDAR.

```
Heuristic: h(n) = Manhattan Distance to nearest unvisited cell (weighted by battery)
Cost:       f(n) = g(n) + h(n)
            g(n) = energy consumed so far
            h(n) = estimated energy to cover remaining cells
Complexity: O(M × N × log(M×N)) time | O(M×N) space
```

### Module 3 — Knowledge Representation & Expert System

A rule-based expert system stores agronomic knowledge as IF-THEN production rules. The inference engine uses **forward chaining** — starting from sensor facts and firing rules until an action is derived.

```
IF  NDVI < 0.3  AND  leaf_color = 'yellow-brown'
THEN  disease = 'Leaf_Blight'  (CF = 0.87)

IF  humidity > 85%  AND  temperature > 28°C
THEN  risk = 'Fungal_Outbreak'  (CF = 0.92)

IF  wind_speed > 5 m/s
THEN  SUSPEND spraying  AND  log_event('wind_abort')
```

### Module 4 — Fuzzy Logic Inference Engine (FLIE)

A Mamdani-style fuzzy system converts imprecise, continuous sensor inputs into calibrated pesticide doses via **5-step inference**:

```
Step 1 — Fuzzification:   Crisp input → membership degrees across fuzzy sets
Step 2 — Rule Evaluation: IF Infection=Moderate AND Wind=Calm → Dose=Medium
Step 3 — Aggregation:     Maximum aggregation of all activated rule outputs
Step 4 — Defuzzification: Centroid of Area (COA) → crisp dose value (ml/m²)
Step 5 — Actuation:       Dose sent to sprayer pump controller
```

**Membership Functions:**

```
Leaf Discolouration (%)    →  None | Low | Moderate | High | Severe
Wind Speed (m/s)           →  Calm | Breezy | Windy | Stormy
NDVI Value                 →  Healthy | Stressed | Diseased
Humidity (%)               →  Dry | Optimal | Wet

Output: Pesticide Dose (ml/m²)  →  0 | 4 | 10 | 18 | 25
```

### Module 4 — Bayesian Network (Disease Prediction)

A Bayesian Network (DAG) uses historical LEO satellite data to predict disease outbreak probability **7–14 days before visible symptoms appear**.

```
Nodes:  NDVI_Drop → Rainfall → Temperature → Humidity → Disease_Outbreak

P(Outbreak | NDVI_Drop=T, Rainfall=High, Humidity=High) = 0.91
→ Schedule preventive drone mission
```

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────┐
│         LAYER 1: SATELLITE              │
│  Sentinel-2 / Planet Labs               │
│  NDVI/EVI Maps → Bayesian Network       │
│  → Disease Outbreak Probability Map     │
└──────────────────┬──────────────────────┘
                   │ Mission Priority Map
┌──────────────────▼──────────────────────┐
│      LAYER 2: MISSION PLANNING          │
│  A* Coverage Path Planner               │
│  CSP Swarm Zone Allocator               │
│  → Ordered cell sequence per drone      │
└──────────────────┬──────────────────────┘
                   │ Flight Path + IoT Triggers
┌──────────────────▼──────────────────────┐
│      LAYER 3: DRONE EDGE AI             │
│  Camera → Expert System KB              │
│  Sensors → Fuzzy Inference Engine       │
│  LIDAR → SLAM Navigation                │
└──────────────────┬──────────────────────┘
                   │ Spray Commands + Telemetry
┌──────────────────▼──────────────────────┐
│      LAYER 4: ACTUATION & REPORTING     │
│  Sprayer Pump → Precision Application   │
│  Log: {GPS, Cell_ID, Disease, Dose}     │
│  Dashboard: Real-time farm health map   │
└─────────────────────────────────────────┘
```

---

## 🛠️ Technologies

| Layer | Technology |
|---|---|
| Satellite Data | Sentinel-2 (ESA), Planet Labs — NDVI, EVI at 10m resolution |
| Drone Hardware | DJI Agras T40-class multi-rotor agricultural drones |
| IoT Sensors | RGB + multispectral camera, LIDAR, anemometer, humidity sensor |
| Ground Nodes | Soil moisture, weather stations, pH sensors via LoRaWAN |
| Edge Computing | On-drone inference (Fuzzy Engine + Expert System) |
| Cloud Platform | MQTT telemetry · REST APIs · Farm management dashboard |
| AI Frameworks | Fuzzy Logic (Mamdani) · Bayesian Networks · A\* Search · CSP |
| MVP Stack | Vanilla HTML/CSS/JS — zero dependencies, runs offline |

---

## 📂 Repository Structure

```
agrodrone/
├── AgroDrone_MVP.html          # ← Interactive simulation (open in browser)
├── AI_Assignment_Report.docx   # Full assignment report
├── README.md                   # This file
```

---

## 🚀 Running the MVP

**No installation. No server. No dependencies.**

```bash
# Clone the repo
git clone https://github.com/your-username/agrodrone.git
cd agrodrone

# Open the MVP
open AgroDrone_MVP.html        # macOS
start AgroDrone_MVP.html       # Windows
xdg-open AgroDrone_MVP.html   # Linux
```

Or just double-click `AgroDrone_MVP.html` in your file explorer.

---

## 📊 Complexity Analysis

| Component | Time Complexity | Space Complexity |
|---|---|---|
| A\* Coverage Path Planning | O(MN · log MN) | O(MN) |
| Fuzzy Inference Engine | O(R × F) | O(R + F) |
| Expert System (Forward Chaining) | O(R × F) | O(KB size) |
| Bayesian Network Inference | O(n²) approx. | O(n²) |
| CSP Swarm Partitioning (AC-3) | O(n log n) | O(n + c) |

*R = rules, F = fuzzy sets, n = nodes/drones, c = constraints*

---

## ✅ Key Advantages

- **Precision Dosing** — Fuzzy Logic delivers exact pesticide doses, reducing chemical usage by an estimated 35–60%
- **Zero Human Presence** — Fully autonomous from satellite ingestion to spray actuation
- **Proactive Detection** — Bayesian pre-symptomatic detection up to 14 days before visible symptoms
- **100% Field Coverage** — Boustrophedon + A\* guarantees complete coverage with dynamic obstacle avoidance
- **Fully Explainable AI** — Fuzzy linguistic rules are interpretable by agronomists without technical AI knowledge
- **Scalable** — Grid-based state space scales linearly from 1-hectare pilot to 1000-hectare commercial farms

---

## 🔭 Future Scope

- [ ] CNN-based leaf image disease classification (replaces rule-based expert system)
- [ ] Reinforcement Learning for optimal spray policy via field trials
- [ ] Multi-drone swarm optimisation using Nash equilibrium game theory
- [ ] Digital Twin farm using drone LIDAR + satellite data
- [ ] Carbon & water footprint monitoring module
- [ ] FSSAI / EU Regulation 2009/1107 compliance rule engine

---

## 📚 References

1. Russell, S. & Norvig, P. — *Artificial Intelligence: A Modern Approach, 4th Ed.* Pearson, 2020
2. Zadeh, L.A. — *Fuzzy Sets*, Information and Control, Vol. 8(3), 1965
3. Hart, P., Nilsson, N., Raphael, B. — *A Formal Basis for Heuristic Determination of Minimum Cost Paths*, IEEE, 1968
4. Pearl, J. — *Probabilistic Reasoning in Intelligent Systems*, Morgan Kaufmann, 1988
5. Choset, H. — *Coverage for Robotics — A Survey*, Annals of Mathematics and AI, 2001
6. Mamdani, E.H. & Assilian, S. — *Linguistic Synthesis with a Fuzzy Logic Controller*, 1975
7. Mohanty et al. — *Deep Learning for Image-Based Plant Disease Detection*, Frontiers in Plant Science, 2016

---

<div align="center">
  <sub>Built with 🌱 for smarter, sustainable agriculture</sub>
</div>
