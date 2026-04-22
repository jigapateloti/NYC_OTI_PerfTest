# LRE Smoke Test – Automated GitHub Actions Workflow

This repository contains a fully automated GitHub Actions workflow that executes a **LoadRunner Enterprise (LRE) Smoke Test**, generates a **Trend Report**, downloads the **PDF report**, uploads it as a **GitHub Artifact**, and sends a **Microsoft Teams notification** with run details.

The workflow is designed for **repeatable, stable, production‑grade automation** and runs on a **self‑hosted runner** inside the LRE network.

---

## 📌 Overview

This workflow performs the following end‑to‑end steps:

1. Authenticates to LoadRunner Enterprise (LRE)
2. Starts a new LRE test run
3. Polls the run status until completion
4. Waits for analysis data to be generated
5. Creates a **unique Trend Report** (RunID + timestamp)
6. Adds the run (and optional baseline run) to the Trend Report
7. Downloads the Trend Report PDF
8. Applies a **2‑minute cooling period** to ensure report stability
9. Uploads the final report as a GitHub Artifact
10. Sends a **Microsoft Teams notification** with:
   - Run ID  
   - Trend Report ID  
   - Artifact path  
   - Link to the GitHub Actions run  

---

## 🚀 Triggering the Workflow

This workflow is manually triggered using:


Run it from:

**GitHub → Actions → LRE Smoke Test → Run workflow**

---

## 🔐 Required Secrets

| Secret Name     | Purpose |
|-----------------|---------|
| `LRE_USERNAME`  | LRE API username |
| `LRE_PASSWORD`  | LRE API password |
| `TEAMS_WEBHOOK` | Incoming Teams webhook URL |

---

## 🌐 Required Repository Variables

| Variable | Description |
|---------|-------------|
| `LRE_BASE_URL` | Base URL of LRE server |
| `LRE_DOMAIN` | LRE domain name |
| `LRE_PROJECT` | LRE project name |
| `LRE_TEST_ID` | Test ID to execute |
| `LRE_TEST_INSTANCE_ID` | Test instance ID |
| `TENANT_ID` | Tenant ID for authentication |
| `LRE_TEST_SET_ID` | (Optional) Test Set ID for Trend Report |
| `LRE_BASELINE_RUN_ID` | (Optional) Baseline run to compare |
| `CI_PROJECT_DIR` | Directory where artifacts will be stored |

---

## 🧠 How the Workflow Works

### 1. **Authentication**
The workflow logs into LRE using the REST API and establishes a persistent session.

### 2. **Start Test Run**
A new test run is created and the returned **Run ID** is stored.

### 3. **Poll Run Status**
The workflow checks the run state every 20 seconds until it becomes:

- `Finished`
- `Failed`

### 4. **Cooling Period**
A 60‑second wait ensures LRE has generated analysis data.

### 5. **Create Trend Report**
A unique Trend Report name is generated:


This prevents duplicate name collisions.

### 6. **Add Run(s) to Trend Report**
The workflow adds:

- The current run  
- Optional baseline run  

### 7. **Download Trend Report PDF**
The PDF is downloaded from the TrendReports API.

### 8. **2‑Minute Cooling Period**
Ensures the PDF is fully generated and stable.

### 9. **Artifact Handling**
The workflow checks:

- If the file is already a ZIP → upload directly  
- If it is a PDF → zip it once  

This prevents nested ZIP files.

### 10. **Upload Artifact**
The final ZIP or PDF is uploaded as a GitHub Artifact.

### 11. **Teams Notification**
A Teams message is sent containing:

- Run ID  
- Trend Report ID  
- Artifact path  
- Link to the GitHub Actions run  

---

## 📊 Workflow Outputs

| Output | Description |
|--------|-------------|
| `runId` | LRE Run ID |
| `trendId` | Trend Report ID |
| `artifactPath` | Path to uploaded artifact |

These are used in the Teams notification step.

---

## 🧪 Self‑Hosted Runner Requirements

The runner must:

- Have network access to the LRE server  
- Support PowerShell  
- Allow outbound HTTPS to Teams webhook  
- Have permissions to write to `CI_PROJECT_DIR`  

---

## 📌 Summary

This workflow provides a **fully automated**, **repeatable**, and **production‑grade** pipeline for:

- Running LRE smoke tests  
- Generating Trend Reports  
- Downloading and storing artifacts  
- Sending real‑time notifications  

It eliminates manual steps and ensures consistent reporting for every test execution.

---

## 📄 Support

For enhancements, issues, or feature requests, open a GitHub Issue in this repository.
