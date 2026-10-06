# Azure Diet Analytics Platform

A cloud-native nutrition analytics application built on Microsoft Azure. The platform processes the **All_Diets** dataset with Python Azure Functions, serves an authenticated dashboard through Azure Static Web Apps, and lets users explore recipes by diet, cuisine, and nutrition data.

**[Open the live Azure demo](https://happy-moss-08fc03c10.7.azurestaticapps.net/)**

> Team project for SAIT's Cloud Computing for Software Development course. My role focused on frontend-to-backend integration, cloud deployment, CORS and authentication flows, end-to-end testing, repository assembly, and technical documentation.

![Azure Diet Analytics dashboard](docs/dashboard-demo.png)

## Why this project matters

This project evolved from a local cloud-native prototype into a deployed Azure application:

- **Phase 1:** local Blob Storage simulation with Azurite, Python data processing, Docker, and GitHub Actions.
- **Phase 2:** Azure Blob Storage, three HTTP-triggered Azure Functions, a public analytics dashboard, and integration testing.
- **Phase 3:** login and registration flows, GitHub OAuth support through Azure Static Web Apps, recipe search, diet filtering, pagination, and automated deployment.

## Architecture

![Azure architecture](docs/architecture-diagram.png)

```text
Browser
   |
   v
Azure Static Web Apps
   |
   v
Azure Functions (Python)
   |
   v
Azure Blob Storage
   |
   v
All_Diets.csv
```

The frontend is hosted over HTTPS on Azure Static Web Apps. Python Azure Functions read the dataset from Blob Storage, process it with Pandas, and return JSON to the browser. GitHub Actions deploys the frontend from the `main` branch.

## Features

- Email registration and login against deployed Azure Function endpoints
- GitHub OAuth entry point using Azure Static Web Apps authentication
- Protected dashboard view with session-aware logout
- Recipe search across names, cuisines, diets, and other fields
- Dynamic diet-type filtering
- Responsive recipe cards and six-item pagination
- Refreshable data loaded from a deployed Azure Function
- Phase 2 nutrition, recipe, and cuisine-cluster API implementations
- Exact-origin CORS configuration guidance
- Integration smoke test for the deployed data endpoints
- Continuous deployment to Azure Static Web Apps through GitHub Actions

## Azure services

| Service | Purpose |
|---|---|
| Azure Static Web Apps | Hosts the HTTPS frontend and provides the GitHub authentication route |
| Azure Functions | Runs the Python data and application endpoints |
| Azure Blob Storage | Stores the `All_Diets.csv` dataset |
| GitHub Actions | Deploys the frontend after changes to `main` |
| Application settings | Keeps storage configuration outside source control |

## API endpoints

| Endpoint | Purpose |
|---|---|
| `GET /api/GetNutritionalInsights` | Calculates average protein, carbohydrate, and fat values by diet type |
| `GET /api/GetRecipes` | Returns recipe records and accepts an optional `diet_type` filter |
| `GET /api/GetClusters` | Aggregates recipe counts and average macronutrients by cuisine |
| `POST /api/Register` | Supports the deployed Phase 3 account-registration flow |
| `POST /api/Login` | Supports the deployed Phase 3 email-login flow |

## My contribution — Manya Sethi

I worked as **Member C: Integration and Documentation**. My contribution included:

- Connected the frontend to the deployed Azure Function APIs.
- Implemented refresh, diet filtering, keyword search, recipe rendering, and pagination.
- Added loading, error, empty, and session states.
- Added client-side timing metadata and integration checks.
- Prepared exact-origin CORS and authentication guidance.
- Built the Phase 3 authentication-facing interface and GitHub OAuth flow.
- Integrated team branches and assembled the final repository.
- Deployed the frontend to Azure Static Web Apps.
- Produced the architecture diagram, deployment documentation, test plan, and final evidence package.

## Team

| Member | Primary responsibility |
|---|---|
| Anagha Roy | Azure backend, Blob Storage, Function App, and deployed data endpoints |
| Jasmeen Garcha | Initial dashboard UI and chart components |
| Manya Sethi | Integration, Phase 3 UI, deployment, testing, architecture, and documentation |

## Technology stack

- **Cloud:** Azure Functions, Blob Storage, Static Web Apps
- **Languages:** Python, JavaScript, HTML, CSS
- **Data:** Pandas, CSV, JSON
- **Authentication:** Azure Static Web Apps authentication and deployed login/register APIs
- **DevOps:** GitHub Actions, Git, deployment smoke testing
- **Security:** environment-based configuration, secret exclusion, HTTPS, and exact-origin CORS

## Repository structure

```text
.
├── .github/workflows/       # Azure Static Web Apps deployment
├── deployment/              # CORS, deployment, and merge guidance
├── docs/                    # Architecture and project evidence
├── frontend/                # Phase 3 static web application
├── tests/                   # Deployed-endpoint smoke test
├── function_app.py          # Python data-analysis Azure Functions
├── host.json
└── requirements.txt
```

## Run the frontend locally

```bash
python3 -m http.server 8080 --directory frontend
```

Then open [http://localhost:8080](http://localhost:8080).

GitHub OAuth is provided by Azure Static Web Apps and is therefore available in the deployed environment, not from a basic local HTTP server.

## Run the integration check

```bash
python3 -m pip install requests
python3 tests/integration_check.py
```

The smoke test requests the deployed data endpoints, validates the JSON responses, reports record counts, and prints request duration.

## Security notes

- Azure storage configuration is read from application settings rather than committed secrets.
- Connection strings and `local.settings.json` are excluded from source control.
- The browser calls Azure Functions and never receives the storage-account connection string.
- Production CORS should allow only the exact Static Web Apps origin.
- This repository contains no production passwords or API keys.

## Project status

The frontend is deployed through Azure Static Web Apps and the latest checked GitHub Actions deployment completed successfully. This is an academic team project maintained as a portfolio case study; availability of the student Azure resources may change after the course subscription ends.
