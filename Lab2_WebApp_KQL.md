
# ✅ Lab 2: Deploy Azure Web App and Analyze Application Logs Using KQL

## 🎯 Objective
Deploy a sample Node.js web app to Azure App Service, enable logging and diagnostics with Application Insights, generate HTTP requests (including errors), and analyze them using KQL.

---

## 🧱 Part 1: Create Azure Web App with Monitoring

### 🔹 Step 1: Create App Service
1. Go to **Azure Portal** → **App Services** → **+ Create**
2. Fill in the details:
   - **App name:** `kql-webapp-lab`
   - **Runtime stack:** Node.js (or .NET, Python)
   - **Region:** Same as your Log Analytics workspace
3. In **Monitoring** tab:
   - ✅ Enable **Application Insights**
   - ✅ Choose or create a **Log Analytics Workspace**
4. Click **Review + Create** → **Create**

---

## ⚙️ Part 2: Deploy Sample Web Application

### 🔹 Step 1: Use Azure Cloud Shell (or local CLI)
```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
cd nodejs-docs-hello-world
az webapp up --name <your-app-name> --resource-group <your-resource-group>
```

### 🔹 Step 2: Generate Web Traffic
- Visit your web app in a browser: `https://<your-app-name>.azurewebsites.net`
- Test a fake route to trigger errors:  
  `https://<your-app-name>.azurewebsites.net/fake`

---

## 📊 Part 3: Analyze Logs with KQL

### 🔹 Step 1: Open Logs
- Go to **Application Insights** → **Logs**

### 🔹 Step 2: Query Application Logs
```kql
requests
| where timestamp > ago(30m)
| summarize count(), avg(duration) by bin(timestamp, 5m), resultCode
| render timechart
```

```kql
exceptions
| where timestamp > ago(1h)
| summarize count() by type, outerMessage
```

---

## ✅ Outcome
You deployed a monitored web app, generated diagnostic data, and analyzed requests and errors using KQL.
