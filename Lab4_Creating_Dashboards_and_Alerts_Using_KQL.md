````markdown
# ✅ Lab 4: Creating Dashboards and Alerts Using KQL

## 🎯 Objective
Learn how to visualize KQL query results using Azure Dashboards, create alerts based on KQL queries, and configure Action Groups for automated responses.

---

## 🧱 Part 1: Pin KQL Results to Azure Dashboard

### 🔹 Step 1: Open Log Analytics Workspace
- Go to **Log Analytics Workspace** → **Logs**
- Choose a relevant table (e.g., `Perf`, `requests`)

### 🔹 Step 2: Run Example KQL Query
```kql
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by bin(TimeGenerated, 5m), Computer
| render timechart
````

### 🔹 Step 3: Pin to Dashboard

1. Click the **Pin to dashboard** icon (📌) above the chart
2. Choose:

   * **Pin to existing dashboard** or **Create new**
   * Select shared or private as per need
3. View the chart from **Azure Dashboard** pane

---

## 🚨 Part 2: Create Alerts from KQL Queries

### 🔹 Step 1: Open Alerts

* Go to **Azure Monitor** → **Alerts** → **+ New alert rule**

### 🔹 Step 2: Define Alert Condition

* **Scope:** Select a target resource (e.g., a VM or Log Analytics workspace)
* **Condition:** Custom log search with query such as:

```kql
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize avg(CounterValue) by Computer
| where avg_CounterValue > 80
```

* **Frequency:** Every 5 minutes
* **Threshold condition:** Number of results > 0

---

## 🔗 Part 3: Link with Action Group

### 🔹 Step 1: Create Action Group

1. In the alert rule wizard → **Actions** → **+ Create action group**
2. Define:

   * **Action group name:** `HighCPUAlertActions`
   * **Action type:** Email/SMS/Logic App/Webhook
   * **Email recipient:** `your-email@example.com`
3. Save and select the action group

### 🔹 Step 2: Review and Enable

* Complete alert rule creation
* Test by triggering high CPU usage (e.g., using `stress` in a VM)

---

## ✅ Outcome

You have created:

* Visual dashboards for real-time insights
* Alert rules to monitor key metrics
* Action groups for automated response like email or logic apps

```

---

```
