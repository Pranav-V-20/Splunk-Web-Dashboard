# 🌐 Splunk Web Activity Monitoring Dashboard

A step-by-step guide to building a complete Splunk dashboard for analyzing web server logs, including request counts, error summaries, URI statistics, and geo-based traffic visualization.

---
## 📌 Overview

This project walks through creating a Splunk dashboard using apache_mixed_access_full.json logs.
You will configure time range inputs, add visualizations, and build insights on web traffic, errors, and geographic distribution.

---

## 🛠️ Lab Setup

### Task 0 — Setting Up Time Range

* Click Add Input

* Select Time

* Click the pencil icon

* Set:

  * Label: Time Range

  * Token: time_range

* Add another input → Submit

For every panel in the project, use the shared time picker time_range for consistency.

### 📊 Task 1 — Web Activities

Goal: Provide a summary of overall web activity.

#### 1. Total Web Requests

* Panel Type: Single Value
* Content Title: Total Web Requests
* Search Query:
```
source="apache_mixed_access_full (1).json" host="webserver" sourcetype="_json" 
| stats count AS "Total Web Requests"
```

#### 2. Successful Response

* Panel Type: Single Value
* Content Title: Successful Response
* Search Query:
```
source="apache_mixed_access_full (1).json" host="webserver" sourcetype="_json" method=GET status=200 
| stats count AS "Successful Responses"
```

#### 3. Client Errors (4xx)

* Panel Type: Single Value
* Content Title: Client Errors
* Search Query:
```
source="apache_mixed_access_full (1).json" host="webserver" sourcetype="_json" 
| where status>=400 and status<500 
| stats count AS "Client Errors"
```

#### 4. Server Errors (5xx)

* Panel Type: Single Value
* Content Title: Server Errors (5xx)
* Search Query:
```
source="apache_mixed_access_full (1).json" host="webserver" sourcetype="_json" 
| where status>=500 and status<600
| stats count AS "Server Errors"
```
---

### 📈 Task 2 — Web Statistics

Goal: Summaries of URI and user access patterns.

#### 1. Top Requested URIs

* Panel Type: Bar Chart
* Content Title: Top Requested URIs
* Search Query:
```
source="apache_mixed_access_full (1).json" host="webserver" sourcetype="_json" 
| stats count AS "Hits" by uri
```

#### 2. Top Users by IP Address

* Panel Type: Bar Chart
* Content Title: Top Users by IP Address
* Search Query:
```
source="apache_mixed_access_full (1).json" host="webserver" sourcetype="_json" 
| stats count AS IP by ip
```
---

### 🗺️ Task 3 — Web Traffic by Client IP Addresses

* Visualization: Choropleth Map
* Content Title: Web Traffic by Client IP Addresses

Search Query:
```
source="apache_mixed_access_full (1).json" host="webserver" sourcetype="_json" method=GET
| table ip
| iplocation ip
| stats count by Country
| geom geo_countries featureIdField="Country"
```
---
## ✅ Final Result

By completing these tasks, you will have an interactive Splunk dashboard that displays:

* Total web activity

* Successful vs unsuccessful requests

* Client and server errors

* Top URIs and user IPs

* Global traffic distribution via geographic mapping

Perfect for learning Splunk dashboards, monitoring web server logs, and detecting anomalies.
