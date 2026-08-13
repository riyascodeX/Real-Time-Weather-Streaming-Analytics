<div align="center">

# ☁️ SkyFlow — Real-Time Weather Streaming Analytics

### An end-to-end real-time data engineering pipeline on Microsoft Azure

Live weather data → Event-driven ingestion → Real-Time Intelligence → Instant dashboards & automated alerts.

[![Azure Functions](https://img.shields.io/badge/Azure-Functions-0062AD?logo=azurefunctions&logoColor=white)](https://azure.microsoft.com/en-us/products/functions)
[![Azure Databricks](https://img.shields.io/badge/Azure-Databricks-FF3621?logo=databricks&logoColor=white)](https://azure.microsoft.com/en-us/products/databricks)
[![Azure Event Hubs](https://img.shields.io/badge/Azure-Event_Hubs-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/event-hubs)
[![Microsoft Fabric](https://img.shields.io/badge/Microsoft-Fabric-1a6ce7?logo=microsoft&logoColor=white)](https://www.microsoft.com/en-us/microsoft-fabric)
[![Power BI](https://img.shields.io/badge/Power_BI-Dashboard-F2C811?logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://www.python.org/)

</div>

---

## 📖 Overview

**SkyFlow** is a real-time data pipeline that ingests live weather and air-quality data from a public API and turns it into instant dashboards and automated alerts. An Azure Function polls the [WeatherAPI.com](https://www.weatherapi.com/) service on a timer, flattens and enriches the response, and streams it through **Azure Event Hub** into **Microsoft Fabric's Real-Time Intelligence** stack. There, **Eventstream** and an **Eventhouse (KQL database)** handle continuous ingestion and querying, powering a live **Power BI** dashboard for monitoring — while **Data Activator** triggers automated alerts sent directly to Outlook when defined weather conditions are met.

This project demonstrates a complete, production-style event-driven streaming architecture: secure secret management, scheduled ingestion, real-time processing, and automated business alerting — all built natively on Azure.

## 🏗️ Architecture

```
WeatherAPI.com
      │  (current, forecast, alerts)
      ▼
Azure Function (Timer Trigger, every 30s)
      │  fetch → flatten → merge
      │  secrets pulled from Azure Key Vault
      ▼
Azure Event Hub  (weather-streaming-eventhub)
      ▼
Microsoft Fabric — Eventstream
      ▼
Real-Time Intelligence Eventhouse (KQL Database)
      │
      ├──► Power BI  (live monitoring dashboard)
      │
      └──► Data Activator  (rule-based alerting)
                  │
                  ▼
              Outlook (automated email alerts)
```

## ✨ Features

- ⏱️ **Scheduled Ingestion** — Azure Function on a timer trigger polls multiple locations every 30 seconds
- 🌦️ **Rich Weather Data** — Pulls current conditions, multi-day forecasts, air quality, and severe weather alerts in a single run
- 🔐 **Secure Secret Management** — API keys retrieved at runtime from Azure Key Vault via managed identity (`DefaultAzureCredential`) — no hardcoded secrets
- 📡 **Event-Driven Streaming** — Flattened weather payloads published to Azure Event Hub as structured JSON events
- ⚡ **Real-Time Processing** — Microsoft Fabric Eventstream and a KQL Eventhouse handle continuous ingestion and low-latency querying
- 📊 **Live Dashboard** — Power BI visualizes real-time conditions, trends, and air quality across all monitored locations
- 🚨 **Automated Alerting** — Data Activator watches the Eventhouse for defined thresholds and fires real-time Outlook notifications

## 🛠️ Tech Stack

| Layer                  | Technology                                                    |
|--------------------------|-----------------------------------------------------------------|
| Data Source               | [WeatherAPI.com](https://www.weatherapi.com/) (current, forecast & alerts endpoints) |
| Ingestion                 | Azure Functions (Python, Timer Trigger)                        |
| Secret Management         | Azure Key Vault + Managed Identity                              |
| Event Streaming           | Azure Event Hub                                                  |
| Real-Time Processing      | Microsoft Fabric — Eventstream & Eventhouse (KQL Database)       |
| Visualization             | Power BI                                                          |
| Alerting                  | Microsoft Fabric Data Activator → Outlook                        |
| Language                  | Python 3, `azure-functions`, `azure-eventhub`, `azure-identity`, `azure-keyvault-secrets` |

## 📂 Project Structure

```
SkyFlow-Weather-Analytics-Alerting-System/
├── function_app.py       # Azure Function: fetch, flatten & stream weather data
├── host.json               # Azure Functions host configuration
├── requirements.txt        # Python dependencies
└── .vscode/                 # VS Code Azure Functions debug/deploy config
```

## ⚙️ How It Works

1. **Trigger** — An Azure Function runs on a timer (every 30 seconds) for a configured list of locations.
2. **Fetch** — For each location, it calls the WeatherAPI current, forecast, and alerts endpoints.
3. **Secure Auth** — The API key is retrieved securely from Azure Key Vault using `DefaultAzureCredential`, avoiding hardcoded secrets.
4. **Flatten & Merge** — Current conditions, air quality, forecast, and alert data are flattened into a single structured JSON payload per location.
5. **Stream** — Each payload is published as an event to Azure Event Hub.
6. **Ingest & Query** — Microsoft Fabric's Eventstream picks up the events and lands them in a KQL Eventhouse for real-time querying.
7. **Visualize** — Power BI connects to the Eventhouse for a live-updating monitoring dashboard.
8. **Alert** — Data Activator continuously evaluates the incoming data against defined rules and sends real-time email alerts via Outlook when conditions are met.

## 🚀 Getting Started

### Prerequisites

- An Azure subscription with:
  - Azure Functions (Python runtime)
  - Azure Key Vault
  - Azure Event Hub
  - Microsoft Fabric workspace (Eventstream + Eventhouse + Data Activator)
  - Power BI
- A [WeatherAPI.com](https://www.weatherapi.com/) API key
- Azure Functions Core Tools + Python 3.11 (for local development)

### Setup

1. Clone the repository
   ```bash
   git clone https://github.com/riyascodeX/SkyFlow-Weather-Analytics-Alerting-System.git
   cd SkyFlow-Weather-Analytics-Alerting-System
   ```
2. Install dependencies
   ```bash
   pip install -r requirements.txt
   ```
3. Store your WeatherAPI key as a secret named `weatherapikey` in your Azure Key Vault, and update `VAULT_URL` in `function_app.py` to point to your vault.
4. Update the `EVENT_HUB_NAME` and `EVENT_HUB_NAMESPACE` values in `function_app.py` to match your Event Hub.
5. Deploy the function to Azure (via VS Code Azure Functions extension or Azure Functions Core Tools).
6. In Microsoft Fabric, connect an Eventstream to the Event Hub, route it into an Eventhouse (KQL DB), and build your Power BI report and Data Activator alert rules on top of it.

## 📊 Monitored Locations

Currently configured to track: `Kodaikanal`, `Madurai`, `Chennai`, `Ooty`, `Coimbatore` — easily extendable by updating the `locations` list in `function_app.py`.

## 🔗 Links

- 👤 Author: [Riyas Khan](https://github.com/riyascodeX)
- 🌐 Portfolio: [riyas-portfolio-phi.vercel.app](https://riyas-portfolio-phi.vercel.app/)

</div>
