
# ✅ Lab 3: Alert Automation with Logic App and KQL

## 🎯 Objective
Create a KQL-based alert in Log Analytics for high CPU usage on a VM, and trigger an automated email notification via Logic App.

---

## 🧱 Part 1: Create Alert Rule with KQL

### 🔹 Step 1: Go to Log Analytics Workspace
- Navigate to your **Log Analytics Workspace** → **Alerts** → **+ New alert rule**

### 🔹 Step 2: Define the Alert
- **Scope:** Select the target VM
- **Condition:** Custom log search  
  Use the following query:
```kql
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize avg(CounterValue) by Computer
| where avg_CounterValue > 80
```
- **Frequency:** Every 5 minutes
- **Threshold:** When result count > 0

---

## ⚙️ Part 2: Create Logic App to Send Email

### 🔹 Step 1: Create Logic App
- Go to **Logic Apps** → **+ Create**
- Choose **Consumption** plan
- Use the template: *When an HTTP request is received*

### 🔹 Step 2: Add Email Action
- Add step: **Send Email (Outlook or Gmail)**
- Compose message:
  ```
  Alert! High CPU usage detected on VM: @{triggerBody()?['Computer']}
  Average CPU: @{triggerBody()?['avg_CounterValue']}%
  ```

### 🔹 Step 3: Save & Copy URL
- Save the Logic App and copy the HTTP trigger URL

---

## 🧩 Part 3: Connect Alert to Logic App

- In the Alert Rule → **Action Group** → **Add Action**
  - Action type: **Logic App**
  - Paste the Logic App URL

---

## ✅ Outcome
You created a real-time alert with KQL, triggered a Logic App, and sent an automated email for high CPU usage.
