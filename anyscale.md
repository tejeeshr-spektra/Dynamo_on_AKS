# Getting Started with Anyscale on Azure

## Overview

Welcome to the Anyscale on Azure lab environment. In this guide, you will:

- Access the Anyscale  Console
- Sign in using your lab account
- Launch your first workspace
- Connect to GPU compute (H100/G100)
- Validate your environment

---

# Prerequisites

Before starting, ensure you have received:

- Lab username
- Temporary password or SSO access
- Anyscale workspace URL
- Azure lab environment access



Step 1 — Open the Anyscale Azure Console

Open the following URL in your browser:

https://console.azure.anyscale.com

You should see the Anyscale login screen.

Step 2 — Sign In

Use your provided lab credentials.

Username
odl_user_<labid>@cloudlabsai.com

Example:

odl_user_2234890@cloudlabsai.com
Authentication Method

Depending on your lab setup:

Microsoft SSO
Email authentication
Temporary lab credentials

Select:

Continue with Microsoft

if prompted.

Step 3 — Complete First Login

After successful authentication:

Accept any onboarding prompts
Select the assigned organization
Wait for the dashboard to load

You should land on the:

Workspaces

page.

Step 4 — Verify Workspace Access

From the left navigation menu:

Workspaces → My Workspaces

---

# Step 5 — Monitor Service Metrics and Health

Once your service is deployed and running, use the built-in observability tools to
monitor request traffic, latency, resource utilization, and overall health. Anyscale
exposes two complementary views:

- The **Metrics** tab — curated dashboards for the service, cluster, and LLM workers.
- The **Ray Dashboard** — low-level Ray Serve deployment status, metrics, and logs.

> Tip: Use the time-range selector (e.g. **Last 30 mins**) in the top-right corner to
> widen or narrow the window, and **View tab in Grafana** to open the underlying
> dashboards for deeper analysis or custom alerting.

## 5.1 — Open the Metrics Tab (Service view)

From your service's top navigation bar, select **Metrics**, then the **Service** sub-tab.

Review the **Service version metrics** and **Service metrics** panels:

- **QPS per version** — request throughput for each deployed version.
- **Error QPS per version** — rate of failed requests.
- **P90 latency per version** — 90th-percentile response latency.
- **Num replicas per version** — replicas currently serving each version.
- **Cluster Utilization** — GRAM and Memory (RAM) usage across the cluster.
- **QPS per application** — throughput broken down by application.

![](anyscaleimage/mainmetrics.png)

## 5.2 — Inspect Core Cluster Metrics

Select the **Core** sub-tab to view the **Overview and Health** of the underlying Ray
cluster:

- **Node Count** — active nodes by type (e.g. head-node and `1xA10GB:34CPU-288GB`).
- **Workload Instances** — instance IDs, node IDs, and IP addresses.
- **Ray OOM Kills (Tasks and Actors)** — out-of-memory failures (none expected in a
  healthy run).
- **Cluster Utilization** and **Hardware Utilization by Node** — GRAM / RAM trends.

![](anyscaleimage/core.png)

## 5.3 — Review Serve LLM (vLLM) Metrics

Select the **Serve LLM** sub-tab for vLLM-specific performance metrics:

- **QPS per vLLM worker** — per-worker request rate.
- **vLLM: Time Per Output Token Latency** and **Time To First Token Latency**.
- **vLLM: Token Throughput** — tokens generated per second.
- **vLLM: Cache Utilization** and **KV Cache Hit Rate** — KV-cache efficiency.

![](anyscaleimage/metricllm.png)

## 5.4 — Open the Ray Dashboard (Serve)

From the top navigation bar, select **Ray Dashboard**, then the **Serve** tab to confirm
your deployments are healthy:

- **Controller status**, **Proxy status**, and **Application status** should all be
  HEALTHY / RUNNING.
- The **Applications / Deployments** table lists each deployment (e.g. `default`,
  `ContentFilter`, `SentimentClassifier`, `Summarizer`) with its status, replica count,
  and quick links to **View config**, **Logs**, and **Metrics**.

![](anyscaleimage/raudashboard.png)

## 5.5 — View Per-Application Metrics

Scroll to the **Metrics** section of the Ray Dashboard (or click **Metrics** on a
deployment) to see per-application charts:

- **QPS per application**
- **Error QPS per application**
- **P90 latency per application**

Use **VIEW IN GRAFANA** and the refresh interval / time-range controls to drill in.

![](anyscaleimage/raymetrics.png)

## 5.6 — Inspect Serve Controller Logs

For troubleshooting, open the **Logs** view and select the **Controller** component.
Here you can confirm the controller started, the proxy is listening, applications were
imported and built successfully, and deployments registered their autoscaling state.
Use the keyword filter, time range, or **Download log file** for deeper investigation.

![](anyscaleimage/raymelogs.png)

---

## Validation Checklist

Your environment is healthy when:

- [ ] The **Metrics → Service** view shows the expected number of replicas.
- [ ] **Cluster Utilization** (GRAM / RAM) is within normal limits and **Ray OOM Kills**
      shows no data.
- [ ] The **Ray Dashboard → Serve** view shows Controller, Proxy, and Application as
      HEALTHY / RUNNING.
- [ ] All deployments report HEALTHY with their target replica counts.
- [ ] **Controller logs** show no recurring errors.


