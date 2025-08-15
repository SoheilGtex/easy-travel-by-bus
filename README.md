# 🚌 Easy Travel by Bus — Asansafar (Case Study)

[![status](https://img.shields.io/badge/Status-Case%20Study-blueviolet)]()
[![platform](https://img.shields.io/badge/Platform-Mobile%20(React%20Native)%20%7C%20Web%20(React)-0a7bbb)]()
[![backend](https://img.shields.io/badge/Backend-Django-0C4B33)]()
[![maps](https://img.shields.io/badge/Maps-Neshan%20API-1f6feb)]()
[![routing](https://img.shields.io/badge/Routing-Multi--criteria%20A*-2ea043)]()
[![license](https://img.shields.io/badge/License-MIT-2c974b)](LICENSE)

> **Easy Travel by Bus** is an urban mobility assistant that helps citizens navigate city buses effortlessly.  
> Cross-platform (React Native mobile + React web) with a Django REST backend, powered by **Neshan API** for maps/traffic and a **multi-criteria routing engine** that minimizes transfers and travel time while providing realistic ETAs.

> **Note:** This repository is a **public case study**. Production code is proprietary and cannot be published.  
> Architecture, algorithms, and my role are documented below.

---

## 🔎 Quick Overview

- **Goal:** Make bus usage simple, fast, and reliable for commuters.
- **My Role:** **Technical Manager** — system architecture, roadmap, integrations, quality oversight, and delivery.
- **Stack:**  
  - **Mobile:** React Native  
  - **Web:** React  
  - **Backend:** Django (REST)  
  - **Maps/Traffic/Geocoding:** **Neshan API**  
  - **Storage/Caching:** PostgreSQL + Redis
- **Collaboration:** Exploratory collaboration with **Tehran Municipality** (MVP reviews & alignment).
- **Status:** MVP completed and validated; details kept private.

---

## ✨ Features

- **Smart route planning** (minimum transfers, time-aware suggestions)  
- **ETA estimation** from Neshan traffic/road conditions  
- **Nearest stops** (from current location / chosen point)  
- **Favorites / recents**, **Dark/Light** themes, **multi-language** UI  
- **Map visualization** (stops, lines, walking segments)  
- **Web/Mobile parity**, **privacy-aware**, **rate-limit-aware** API

> Optional (enterprise): offline tiles, telemetry dashboards, service health.

---

## 🧠 Routing (Multi-Criteria A\*)

Model: weighted directed graph

- **Nodes:** bus stops (+ virtual anchors for origin/destination)  
- **Edges:** in-vehicle, transfer (with penalty), walking  
- **Cost `g(n)`** combines: `in_vehicle_time` + `waiting_time` + `transfer_penalty` + `walk_time`  
- **Heuristic `h(n)`**: time-to-goal from Haversine + avg. speed  
- **Objective:** minimize **total time**; **transfers** as a secondary constraint  
- **ETA:** adjusted using **Neshan** traffic layers (fallback: historical averages)

```mermaid
flowchart LR
    A[Origin (lat,lng)] --> B{Nearest Stop(s)}
    B -->|walk| C[Graph Node]
    C --> D((A* Search))
    D --> E[Path w/ edges]
    E --> F[ETA Adjustment (Neshan traffic)]
    F --> G[Final Route + Steps]
    G --> H((Mobile/Web UI))
