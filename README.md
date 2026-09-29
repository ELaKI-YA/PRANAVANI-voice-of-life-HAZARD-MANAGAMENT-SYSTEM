<h1 align="center">Kerala Hazard Review</h1>

<p align="center">
  <strong>Flood and Landslide Scenario Dashboard with Local-Centre Reports</strong>
</p>

<p align="center">
  Rescue Simulation • Admin Dashboard • Experimental ML • Voice-First Warning Drafts
</p>

<p align="center"><strong>PROTOTYPE — NO PUBLIC WARNINGS ARE SENT</strong></p>

---

# Local Demo

| Application | URL |
| --- | --- |
| **Rescue Simulation** | http://127.0.0.1:5173/simulation |
| **Admin Dashboard** | http://127.0.0.1:5173/admin |

Both addresses run on your own computer. Start the frontend and backend using the instructions below.

---

# Table of Contents

| No. | Section | Contents |
| ---: | --- | --- |
| 01 | [Overview](#overview) | Project summary and coverage |
| 02 | [Project Objectives](#project-objectives) | Goals of the prototype |
| 03 | [Key Features](#key-features) | Rescue, Admin and report functions |
| 04 | [Technology Stack](#technology-stack) | Languages, libraries and storage |
| 05 | [Why Kerala Hazard Review?](#why-kerala-hazard-review) | Role alongside official systems |
| 06 | [System Architecture](#system-architecture) | How the components connect |
| 07 | [Project Modules](#project-modules) | Each workspace and backend module |
| 08 | [Machine Learning](#machine-learning) | Model inputs and limitations |
| 09 | [System Workflow](#system-workflow) | Current demo and planned flow |
| 10 | [Project Structure](#project-structure) | Repository directories |
| 11 | [Running Locally](#running-locally) | Windows setup and commands |
| 12 | [Demonstration](#demonstration) | Tested Munnar scenario |
| 13 | [Future Enhancements](#future-enhancements) | Remaining development |
| 14 | [Research and References](#research-and-references) | Official data and warning sources |
| 15 | [License](#license) | Software and dataset terms |

---

# Overview

Kerala Hazard Review is a web-based **research prototype** for reviewing flood and landslide scenarios. A Rescue user selects a place, enters rainfall and soil conditions, and runs a dual-hazard simulation. The backend saves a draft report, which an Admin user can find on the dashboard.

The project emphasizes a **local-centre report**: the selected area, inputs, results, evidence limitations, review status and unsent warning drafts are kept together. Voice-first resident warnings with SMS follow-up are part of the proposed workflow; no call or message is transmitted by the current prototype.

The map shows all **14 Kerala districts**. Candidate ward boundaries have been prepared for **Idukki, Wayanad, Kozhikode, Kottayam and Pathanamthitta**. These boundaries need operational verification.

---

# Project Objectives

- Show flood and landslide scenario results for a selected local area.
- Give local centres a clear draft report for reviewing the situation.
- Keep rule simulation separate from experimental machine-learning scores.
- Prepare voice, SMS and responder messages for future human review.
- Record what remains unverified before any real warning decision.

---

# Key Features

| Rescue Workspace | Admin Dashboard | Reports and Review |
| --- | --- | --- |
| Select district and candidate ward | India-to-Kerala district map | Automatically save a scenario draft |
| Set rainfall and soil moisture | Highlight selected candidate ward | List and retrieve saved reports |
| Run flood and landslide rule scenarios | View latest scenario and demo sensor | Draft local-centre and responder text |
| View experimental ML scores separately | View report and alert previews | Prepare voice-first and SMS drafts |

---

# Technology Stack

| Category | Technology |
| --- | --- |
| Frontend | React, Vite, JavaScript, CSS |
| Mapping | React Leaflet, Leaflet, GeoJSON; Mapshaper for preparation |
| Backend | Python standard-library HTTP server and JSON API |
| Data processing | pandas, NumPy, CSV |
| Experimental ML | scikit-learn Random Forest, joblib |
| Local storage | JSON files for draft reports and simulated sensor readings |

The backend is **not FastAPI**, and the project does **not** currently use a production database or a live messaging provider.

---

# Why Kerala Hazard Review?

Official systems already issue warnings. The NDMA's **SACHET** distributes geo-targeted alerts, and Kerala's **KaWaCHaM** supports state-level decision-making and dissemination. This project explores a complementary *report and local verification workflow*; it is not a replacement for official alerts.

A colored map or model score cannot tell a field team which road is passable, which people need assistance, or whether residents received a warning. The proposed workflow places those checks and delivery records after the initial hazard assessment.

---

# System Architecture

~~~mermaid
flowchart TD
    A["Rescue simulation"] --> B["Python API"]
    B --> C["Flood and landslide rule results"]
    B --> D["Experimental ML display"]
    C --> E["Saved draft report"]
    E --> F["Admin dashboard"]
    F --> G["Local-centre review and message drafts"]
~~~

**Current boundary:** The rule scenario creates the saved report. Experimental ML is displayed separately. Local-centre assignment is a manual demo record, while public voice/SMS delivery is not connected.

---

# Project Modules

<details>
<summary><strong>Rescue Simulation</strong></summary>

Choose a district and available candidate ward. Enter rainfall and modeled/scenario soil moisture. The rule engine returns separate flood and landslide levels, and the frontend saves a draft report.

</details>

<details>
<summary><strong>Admin Dashboard</strong></summary>

Explore the India and Kerala map, select a district, and inspect the saved draft list. Scenario, alert and simulated-sensor panels are also displayed. The Admin interface is still being simplified around the saved report.

</details>

<details>
<summary><strong>Draft Reports</strong></summary>

Each report stores its ID, creation time, selected place, candidate ward identity when available, scenario inputs, hazard levels, evidence status and message drafts. The backend can list and retrieve reports. Demo API routes can record a centre assignment or manual acknowledgment; they do not authenticate a real officer.

</details>

<details>
<summary><strong>Sensor Demonstration</strong></summary>

A user can submit a *simulated* rainfall, soil-moisture or river-level reading. The Admin page displays the latest reading. No physical IoT sensor is connected, and these readings do not currently change scenario severity.

</details>

<details>
<summary><strong>Voice-First Message Drafts</strong></summary>

The backend prepares local-centre, resident voice, SMS follow-up and responder wording. Recipient lists, safe destinations and safe routes are not verified. Delivery remains marked as not attempted.

</details>

---

# Machine Learning

The project contains **two experimental Random Forest classifiers**: one for flood labels and one for landslide labels. They use five numerical features:

| Feature | Meaning |
| --- | --- |
| <code>rainfall_12h_mm</code> | Rain accumulated over 12 hours |
| <code>rainfall_24h_mm</code> | Rain accumulated over 24 hours |
| <code>rainfall_7d_mm</code> | Rain accumulated over 7 days |
| <code>soil_0_7cm_m3_m3</code> | Modeled moisture in the top soil layer |
| <code>soil_7_28cm_m3_m3</code> | Modeled moisture in a deeper layer |

The training event labels were **synthetically generated from invented rules**. Strong test scores therefore show that the models learned those invented labels; they do **not** establish real disaster accuracy or real event probability. The displayed rule-simulation severity is a separate result.

Before selecting an operational model, the team needs verified event and non-event records, independent testing across time and locations, and assessment of missed events, false alarms and usable warning lead time. Logistic Regression and gradient-boosted trees are candidates to compare with Random Forest when those records are available.

---

# System Workflow

### Working demonstration

1. Rescue selects **Idukki → Munnar → Ward 18 (MUNNAR TOWN)**.
2. Rescue sets scenario rainfall and soil moisture.
3. The rule engine returns separate flood and landslide levels.
4. The backend saves a **draft report** with the candidate ward identity.
5. Admin lists the saved report. Draft voice and SMS messages remain unsent.

### Planned operational workflow

1. Collect quality-checked, time-stamped weather, sensor, terrain and event data.
2. Generate a validated assessment with a documented uncertainty and lead time.
3. Assign a report to an authorized local centre for field verification.
4. Verify impacts, recipients, safe destinations and routes.
5. Obtain an authorized human decision on warning text and scope.
6. Send voice/IVR first, follow with SMS, and record delivery and acknowledgment.

Steps involving real verification, authorization and delivery are **not implemented operationally**.

---

# Project Structure

~~~text
kerala-warning/
├── backend/
│   ├── server.py
│   ├── simulation_rules.py
│   ├── experimental_ml.py
│   ├── alert_templates.py
│   ├── report_review.py
│   └── models/
├── frontend/
│   ├── src/
│   └── public/
│       ├── maps/
│       └── wards/
├── data/
│   ├── raw/
│   └── processed/
└── README.md
~~~

Large source files, model binaries, local reports and simulated readings may not belong in a public repository. Review data licences and remove personal information before committing.

---

# Running Locally

This project has been run on **Windows with Python 3.13 and Node.js/npm**. Install the packages required by the frontend and Python ML modules. The experimental ML endpoint requires its trained files under <code>backend/models/</code>. If the repository has a <code>requirements.txt</code>, install from it.

### Start the backend

~~~powershell
Set-Location 'D:\kai\kerala-warning'
py backend\server.py
~~~

The local backend listens on **http://127.0.0.1:8765**. Leave this PowerShell window open.

### Start the frontend in another PowerShell window

~~~powershell
Set-Location 'D:\kai\kerala-warning\frontend'
npm install
npm run dev -- --host 127.0.0.1
~~~

Open the two workspace links in [Local Demo](#local-demo). The frontend uses prototype-only login gates. Its Vite configuration must forward <code>/api</code> requests to the backend.

### Check the API

~~~powershell
curl.exe --noproxy "*" http://127.0.0.1:8765/api/health
~~~

Expected response: <code>{"status":"ok","mode":"SIMULATION"}</code>. If an older backend process owns port 8765, stop it before starting the saved <code>server.py</code>.

---

# Demonstration

For a high-rainfall example, the team tested **75 mm over 12 hours**, **130 mm over 24 hours** and **0.48 m³/m³ soil moisture** at the selected Munnar candidate ward. The rule simulation returned **High** for both hazards and created a draft. The saved report identifies **Munnar Ward 18** and marks its boundary <code>CANDIDATE_UNVERIFIED</code>.

A demo API action has recorded an assignment to a demo local centre. This is a manual test entry, not proof that a real centre was contacted. No resident voice call or SMS was sent.

---

# Future Enhancements

- Make the saved report the primary Admin view and reduce repeated previews.
- Verify candidate ward boundaries and align weather, terrain and hazard layers.
- Connect physical sensors with timestamp, location and quality checks.
- Collect independently verified flood and landslide event labels.
- Compare and calibrate models using held-out dates and locations.
- Add real local-centre authentication, assignment, field verification and approval.
- Integrate authorized multilingual voice/IVR and SMS with delivery logs and retries.
- Test workflows and potential warning lead time with local authorities.

---

# Research and References

- [NDMA SACHET](https://sachet.ndma.gov.in/) — existing national geo-targeted warning platform.
- [KSDMA KaWaCHaM](https://sdma.kerala.gov.in/kawacham/) — existing Kerala decision-support and warning infrastructure.
- [KSDMA Hazard Maps](https://sdma.kerala.gov.in/hazard-maps/) — flood probability and GSI landslide susceptibility references.
- [IMD Met Centre Thiruvananthapuram](https://mausam.imd.gov.in/thiruvananthapuram/) — official weather warnings and forecasts.
- [Open-Meteo Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api) — historical precipitation and modeled soil-moisture documentation.

---

# License

No software licence has been chosen yet. Add a <code>LICENSE</code> file once the team decides how to share the code. Third-party datasets and map files may have separate conditions.

---

<p align="center">
  <strong>KERALA HAZARD REVIEW</strong><br>
  <em>From a scenario to a traceable local-centre draft</em>
</p>
