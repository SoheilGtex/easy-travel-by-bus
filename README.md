<h1 align="center">🚌 Easy Travel by Bus</h1>
<p align="center">An intelligent public transport assistant built with modern web and mobile technologies to help citizens navigate city buses more easily.</p>

<div align="center">
  <img src="https://img.shields.io/badge/Platform-Web%20%26%20Mobile-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Frontend-React%20%7C%20ReactNative-orange?style=flat-square"/>
  <img src="https://img.shields.io/badge/Backend-Django-brightgreen?style=flat-square"/>
  <img src="https://img.shields.io/badge/API-Neshan%20%7C%20OpenStreetMap-informational?style=flat-square"/>
  <img src="https://img.shields.io/badge/Role-CTO%20%7C%20Tech%20Lead-purple?style=flat-square"/>
</div>

---

## 📌 About the Project

This system helps citizens access the city bus network more easily by:
- Visualizing real-time nearby bus routes
- Recommending optimal paths based on time and traffic
- Reducing transfer complexity and trip duration
- Providing a mobile-first user experience

The data comes from the Neshan API and is enhanced through a custom optimization algorithm developed for this project.

---

## 🚀 Tech Stack

| Category       | Technologies                     |
|----------------|----------------------------------|
| 📱 Mobile App   | React Native                    |
| 🌐 Web App      | React + Tailwind CSS             |
| ⚙️ Backend      | Django + Django REST Framework   |
| 🌍 Data Source  | Neshan API + City Transit Data   |
| 🧠 Algorithm    | Custom Path Optimization Engine  |
| 📈 Hosting      | Docker, Railway / Heroku (Optional) |

---

## 🧠 Features

- 🔍 Bus stop & route finder
- ⏰ Live arrival and delay estimator
- 📍 Smart nearby station detection
- 🗺️ Interactive map visualization
- 💡 Minimal transfer optimization
- 📲 Cross-platform (PWA + Android/iOS)

---

## 🧪 Architecture Overview

```mermaid
graph TD;
  User[🧑 User App] -->|Request Route| ReactApp[🌐 React / React Native]
  ReactApp -->|API Call| Backend[⚙️ Django REST API]
  Backend -->|Geo Data| Neshan[Neshan API]
  Backend -->|Compute| Optimizer[🚀 Route Optimizer]
............. 