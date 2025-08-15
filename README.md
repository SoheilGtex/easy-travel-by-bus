# 🚌 Easy Travel by Bus — Asansafar (Case Study)

[![status](https://img.shields.io/badge/Status-Case%20Study-blueviolet)]()
[![platform](https://img.shields.io/badge/Platform-Mobile%20(React%20Native)%20%7C%20Web%20(React)-0a7bbb)]()
[![backend](https://img.shields.io/badge/Backend-Django-0C4B33)]()
[![maps](https://img.shields.io/badge/Maps-Neshan%20API-1f6feb)]()
[![routing](https://img.shields.io/badge/Routing-Multi--criteria%20A*-2ea043)]()
[![license](https://img.shields.io/badge/License-MIT-2c974b)](LICENSE)

> **Easy Travel by Bus** is an urban mobility assistant that helps citizens navigate city buses effortlessly.  
> Built as a cross-platform solution (React Native mobile + React web, Django backend), it integrates **Neshan API** for mapping/traffic and uses a **multi-criteria routing engine** to recommend optimal bus paths with **minimum transfers, shortest travel time, and realistic ETAs**.

> **Note:** This repository is a **public case study**. The production source code is proprietary and cannot be published.  
> This README documents the architecture, algorithms, features, and my role in the project.

---

## 🔎 Quick Overview

- **Goal:** Make bus usage simple, fast, and reliable for everyday commuters.
- **My Role:** **Technical Manager** — system architecture, roadmap planning, integration strategy, quality oversight, and delivery.
- **Stack:**  
  - **Mobile:** React Native  
  - **Web:** React  
  - **Backend:** Django (REST)  
  - **Maps/Traffic/Geocoding:** **Neshan API**  
  - **Storage/Caching:** PostgreSQL + Redis (for geodata caching & rate-limit protection)
- **Collaboration:** Exploratory collaboration with **Tehran Municipality** (MVP reviews & alignment).
- **Status:** MVP completed and validated; production details remain private.

---

## ✨ Key Features

- ✅ **Smart route planning** with **minimum transfers** & **time-aware suggestions**  
- ✅ **ETA estimation** leveraging Neshan traffic/road conditions  
- ✅ **Nearest stops** detection (from current location or a chosen point)  
- ✅ **Favorites / recent routes** persistence  
- ✅ **Multi-language** UI & **Dark/Light** themes  
- ✅ **Map visualization** (stops, lines, walking segments)  
- ✅ **Web + Mobile parity** in core UX  
- ✅ **Privacy & rate-limit aware** API design

> Optional enterprise features (in prod): offline fallback tiles, quality telemetry, and admin dashboards.

---

## 🧠 Routing Approach (Multi-Criteria A\*)

The routing engine models the transport network as a weighted directed graph:

- **Nodes:** bus stops (+ virtual nodes for origin/destination anchors)  
- **Edges:** 
  - **In-vehicle edges** (between consecutive stops on a line)  
  - **Transfer edges** (between stops within a walkable radius; includes **transfer penalty**)  
  - **Walking edges** (origin ↔ nearest stop, stop ↔ destination)

**Edge cost** combines:
- `in_vehicle_time` (from schedules/line speed)
- `waiting_time` (line frequency or inferred headway)
- `transfer_penalty` (discourages excessive transfers)
- `walk_time` for walking segments

A **multi-criteria A\*** variant is used:
- **Heuristic `h(n)`** ≈ straight-line time to destination (Haversine + avg. speed)  
- **Cost `g(n)`** aggregates the components above  
- **Optimization objectives:** *minimize total time* with *low transfer count* as a secondary constraint

**ETA** is adjusted using **Neshan** traffic layers where available (fallback to historical averages otherwise).

```mermaid
flowchart LR
    A[Origin (lat,lng)] --> B{Nearest Stop(s)}
    B -->|walk| C[Graph Node]
    C --> D((A* Search))
    D --> E[Path w/ edges]
    E --> F[ETA Adjustment (Neshan traffic)]
    F --> G[Final Route + Steps]
    G --> H((Mobile/Web UI))
