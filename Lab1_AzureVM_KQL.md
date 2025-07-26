
# ✅ Lab 1: Analyzing Azure VM Performance Logs Using KQL (Ubuntu 22.04)

## 🎯 Objective
Provision an Ubuntu 22.04 VM in Azure, configure diagnostics and log forwarding, simulate CPU load using `stress`, and analyze performance metrics in Log Analytics using KQL.

---

## 🧱 Part 1: Create Ubuntu 22.04 VM with Monitoring

### 🔹 Step 1: Launch the VM
1. Navigate to **Azure Portal** → **Virtual Machines** → **+ Create**
2. Fill these values:
   - **Name:** `ubuntu-vm-kql-lab`
   - **Region:** As preferred
   - **Image:** Ubuntu Server 22.04 LTS
   - **Size:** Standard B2s
   - **Authentication:** Password or SSH
   - **Username:** `azureuser`
   - **Password/Key:** As applicable

### 🔹 Step 2: Configure Monitoring (per screenshot)
1. Go to the **Monitoring** tab:
   - ✅ **Boot diagnostics** → *Enable with managed storage account*
   - ✅ **Enable OS guest diagnostics**
   - ❌ Skip "Enable application health monitoring"
2. Select or create a **Log Analytics Workspace**
   - If not shown here, you can connect the VM later (see below)

### 🔹 Step 3: Review and Create
- Click **Review + Create** → then **Create**

---

## ⚙️ Part 2: Install `stress` and Simulate Load

### 🔹 Step 1: SSH into the VM
- Use SSH from Cloud Shell or terminal:
  ```bash
  ssh azureuser@<your-vm-public-ip>
  ```

### 🔹 Step 2: Install `stress`
```bash
sudo apt update
sudo apt install -y stress
```

### 🔹 Step 3: Simulate CPU Load
```bash
stress --cpu 2 --timeout 180
```
➡️ This causes two CPU cores to run at full capacity for 3 minutes.

---

## 🧩 Part 3: Enable Insights (If Missed During Setup)

If you didn’t link the VM to Log Analytics earlier:

1. Go to **Virtual Machine** → **Monitoring** → **Insights**
2. Click **Enable**  
3. Choose the **Log Analytics Workspace**  
4. Click **Apply**

---

## 📊 Part 4: Analyze with KQL in Log Analytics

### 🔹 Step 1: Open Logs
- Go to **Log Analytics Workspace** → **Logs**

### 🔹 Step 2: Run KQL Query
```kql
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated > ago(30m)
| summarize avg(CounterValue) by bin(TimeGenerated, 5m), Computer
| render timechart
```

➡️ This displays average CPU usage over time.

---

## ✅ Outcome
You’ve successfully:
- Launched a VM with monitoring,
- Simulated performance load,
- Analyzed the logs using KQL.
