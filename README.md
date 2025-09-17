# Data Analysis and Aggregation

## General Information and Purpose
- **Service Name/Title:** `data_analysis_and_aggregation`
- **Description and purpose:** Real-time analysis and aggregation to support immediate actions and decision-making.
- **Owner/Contact Information:** ICCS

---

## Functional Requirements
1. Calculate averages, maximums, minimums, and other statistics over time intervals (e.g., 1 min, 15 min, hourly).  
2. Group data (e.g., multiple houses) for spatial insights.  
3. Capture specific events like peak energy usage for triggering alerts.  
4. Perform trend analysis to assess changes over time.  

---

## Non-Functional Requirements
- Aggregated data available in a few seconds.  
- Access restricted to registered users.  
- Periodic backups with automated recovery.  
- 99.9% uptime for the service.  

---

## Service Interfaces

### API Endpoints

#### Endpoint 1 — Retrieve Aggregated Consumption Data for Last Day
- **URL:** `/aggregator/consumption/lastday`  
- **Method:** `GET`  
- **Description:** Retrieves aggregated consumption data by hour for the past 24 hours.  
- **Headers:** `Authorization: Bearer <token>`  

**Response Example**
```json
{
  "labels": ["18:00","19:00","20:00"],
  "values": [40.57, 38.64, 41.46]
}
```

**Error Handling**
```json
401 Unauthorized: { "error": "Unauthorized", "message": "Invalid token" }
```

---

#### Endpoint 2 — Compare Consumption Data for Last Month
- **URL:** `/aggregator/consumption/compare/lastmonth`  
- **Method:** `GET`  
- **Description:** Compares the previous month’s daily consumption data with the current month’s.  
- **Headers:** `Authorization: Bearer <token>`  

**Response Example**
```json
{
  "current_consumption": {
    "labels": ["01-07-2025", "02-07-2025"],
    "values": [634.34, 607.61]
  },
  "last_month_consumption": {
    "labels": ["01-06-2025", "02-06-2025"],
    "values": [319.87, 274.93]
  }
}
```

**Error Handling**
```json
401 Unauthorized: { "error": "Unauthorized", "message": "Invalid token" }
```

---

### UI Mock-ups
*(Not applicable)*  

---

## Data Model

### Entities and Relationships
- Users manage one or more Houses.  
- Houses contain Devices.  
- Devices may be appliances or Energy Meters.  
- Users may join Energy Communities managed by Aggregators.  
- PV Parks and private PV Systems linked to communities or houses.  

### Database Schema
- **Real-Time Energy Measurements:** High-frequency device data (current, voltage, power, energy).  
- **Aggregated Energy Usage:** Summarized hourly/daily/quarterly data.  
- **Metadata Schema:** Models users, houses, devices, energy communities, PV systems.  

---

## Integration and Dependencies

### Roles and Responsibilities
- **Aggregator / Resource Aggregator:** Collects and integrates metering, disaggregated consumption, and forecasted energy data; identifies flexibility potential; formulates optimized bids.  
- **Market Operator (HENEX):** Depends indirectly on forecast-informed bids for efficiency.  

### Domains
- **Forecasting Zone:** Operational region for forecasts.  
- **Metering Points:** Provide detailed real-time and historical data.  
- **Disaggregated Output:** Provides appliance-level usage via NILM/disaggregation.  

---

## Security and Privacy
- **TLS Encryption:** Secures all communication with HTTPS, protecting tokens, personal data, and payloads.  
- **Keycloak Authentication:** Token-based access control. Tokens expire quickly to minimize risk.  
- **Access Control:** Restricted to authorized users.  

---

### Grafana-Based Dashboard
A monitoring and analysis dashboard was implemented with panels for:  
1. **Energy Monitoring (Top Left):** Visualizes median power consumption per device (`hedge-iot-01` to `hedge-iot-25`), grouped into EM and 3EM categories. Color-coded bars highlight high/low consumption.  
2. **Time-Series Graphs (Top Right):** Shows energy consumption trends at different granularities (hourly, daily, 5-min, quarterly). Devices like *HedgeIoT-01*, *HedgeIoT-04*, *HedgeIoT-07* highlighted.  
3. **Pod and Resource Metrics (Bottom):** Tracks CPU, memory, resource saturation, network traffic, and container status in Kubernetes environments.  

This validated the system’s ability to provide operational visibility for both **energy efficiency** and **IT infrastructure health**.  
