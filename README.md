# Azure Threat Detection Web App (CST8919 - Lab 2)

## Overview

This lab demonstrates how to build and secure a simple Python Flask web application by:

- Deploying it to Azure App Service
- Enabling diagnostic logging with Azure Monitor
- Detecting brute-force login attempts using KQL
- Triggering email alerts on suspicious activity

## Lab Breakdown

### Part 1: Flask App

A simple login endpoint was created to simulate both successful and failed login attempts. The application logs each attempt.

### Part 2: Azure App Service Deployment

- Runtime: **Python 3.13**
- Deployment: From GitHub repository
- Region: Same as Log Analytics Workspace

### Part 3: Diagnostic Logging Enabled

- **AppServiceConsoleLogs** enabled
- Logs routed to **Log Analytics Workspace**
- Tested using `.http` file and REST Client in VS Code

### Part 4: KQL Query for Threat Detection

We used the following Kusto Query Language (KQL) query to detect potential brute-force attacks:

```kusto
AppServiceConsoleLogs
| where TimeGenerated > ago(15m)
| where Level has "Error"
```

```kusto
AppServiceConsoleLogs
| where TimeGenerated > ago(30m)
| where ResultDescription has "FAILED LOGIN"
```

This query filters logs generated in the past 15 minutes and only includes entries marked as "Error", which correspond to failed login attempts.

### Part 5: Alert Rule Creation

- Scope: Log Analytics Workspace
- Condition: Number of matching rows > **5**
- Aggregation: Every 5 minutes
- Frequency: 1-minute checks
- Action: Email notification to the administrator

## Key Learnings

- **Azure Monitor** makes it easy to gain visibility into app behavior.
- **KQL** provides powerful filtering for diagnostics and security alerts.
- **Alert Rules** can proactively notify about threats in near real-time.

## Reflections & Improvements

In a real-world scenario, I would improve detection by:
- Including user agent/device fingerprint in logs
- Setting up throttling or IP banning for excessive failed attempts
- Expanding queries to detect suspicious access patterns over time

## Included Files

- `app.py`: Main Flask application
- `requirements.txt`: Dependencies
- `test-app.http`: VS Code REST Client test script
- `.github/workflows`: (if GitHub Actions used for CI/CD)

## Demo Video

Watch a 5-minute walkthrough showing the app, log generation, KQL, and alert setup:  
[YouTube Demo Video](https://youtu.be/aCGStTRxtHc)