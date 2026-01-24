# Defense Visibility & Line of Sight System

A geospatial web application for defense and infrastructure planning. It enables users to simulate visibility over terrain using Digital Elevation Models (DEMs).

## 🚀 Key Capabilities

### 1. Viewshed Simulation (The "Watchtower" Tool)
*   **What it does:** Calculates all areas visible from a specific observer point (e.g., a tower, a soldier on a hill).
*   **How it works:** It casts rays in 360 degrees, checking the terrain elevation at every step. If a mountain blocks the view, the area behind it is marked as "hidden".
*   **Output:** A map overlay showing exactly which areas are visible (Green) and which are blind spots.

### 2. Line of Sight (LOS) Checker (The "Point-to-Point" Tool)
*   **What it does:** Determines if two specific points (Observer and Target) have a direct visual connection.
*   **How it works:** It "draws" a straight line between the two points and samples the ground height along that line. If the ground ever rises above the straight line, the view is blocked.
*   **Output:** A clear **Visible / Blocked** status, with a map showing the obstruction point.

---

## 💡 Practical Use Cases
This tool answers specific "What if?" questions:
*   **🛡️ Defense:** "If we place a sensor here, can it see the enemy approach route?"
*   **📡 Telecom:** "Will a 5G tower at this location cover the entire village?"
*   **🚧 Construction:** "Does this new building block the view from the observation deck?"

---

## 📂 Project Structure & File Guide

### `defense_los/` (Source Code)
This directory contains the core logic of the application.

*   **`app.py` (Frontend - Streamlit)**
    *   **Role:** The User Interface.
    *   **Function:** Renders the map, collects your inputs (Lat, Lon, Height), and displays the results. It communicates with the backend to get the data.
    
*   **`main.py` (Backend - FastAPI)**
    *   **Role:** The Brain.
    *   **Function:** An API server that receives requests from the frontend. It runs the heavy logical scripts (`compute_*.py`) in the background so the website doesn't freeze.

*   **`compute_viewshed.py`**
    *   **Role:** The Viewshed Calculator.
    *   **Function:** Loads the DEM file and runs the 360-degree visibility algorithm. Generates the green polygons for the map.

*   **`compute_los.py`**
    *   **Role:** The Line-of-Sight Calculator.
    *   **Function:** Mathematically checks the terrain profile between two points to find any obstructions.

### `data/` (Files)
*   **`demo_aoi.tif`**: **The Map Data (DEM)**. A special image where pixel colors represent actual ground height. This is the "ground truth" for all simulations.
*   **`*.geojson / *.json`**: Temporary output files storing the results of your latest simulation.

---

## 🛠️ Setup & Running

### 1. Install Dependencies
Make sure you have `uv` installed, then:
```bash
uv sync
```

### 2. Run the Application
You need two terminal windows:

**Terminal 1: Backend**
```bash
cd defense_los
uv run uvicorn main:app --reload
```

**Terminal 2: Frontend**
```bash
cd defense_los
uv run streamlit run app.py
```

### 3. Usage
Open [http://localhost:8501](http://localhost:8501) in your browser. 