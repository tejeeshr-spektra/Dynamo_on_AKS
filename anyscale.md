
# Getting Started with Anyscale on Azure

## Overview

Welcome to the Anyscale on Azure lab environment. In this guide, you will:

- Access the Anyscale  Console
- Sign in using your lab account
- Launch your first workspace
- Connect to GPU compute (H100/G100)
- Validate your environment

## Getting Started with the lab

Welcome to your **Anyscale on Azure** workshop. Let's begin by making the most of this experience.

## Virtual Machine & Lab Guide

Your virtual machine is your workhorse throughout the workshop. The lab guide is your roadmap to success.

## Accessing Your Lab Environment

Once you're ready to dive in, your virtual machine and **Guide** will be right at your fingertips within your web browser.

![Access Your VM and Lab Guide](media/gs0.png)

## Lab Guide Zoom In/Zoom Out

To adjust the zoom level for the environment page, click the **A↕ : 100%** icon located next to the timer in the lab environment.

![](./media/gs1.png)

## Exploring Your Lab Resources

To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.

![Explore Lab Resources](./media/gs1.1.png)

## Utilizing the Split Window Feature

For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the Top right corner.

![Use the Split Window Feature](./media/gs1.2.png)

## Managing Your Virtual Machine

Feel free to **Start, Stop, or Restart (2)** your virtual machine as needed from the **Resources (1)** tab. Your experience is in your hands!

![Manage Your Virtual Machine](media/VMSS.png)

## Let's Get Started with Azure Portal

1. On your virtual machine, click on the Azure Portal icon.

2. You'll see the **Sign into Microsoft Azure** tab. Here, enter your **credentials (1)** and select **Next (2)**:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

     ![Enter Your Username](media/odlusr.png)

3. Next, provide your **Temporary Access pass (1)**, enter the password and select **Sign In (2)**:

    - Enter **Temporary Access Pass:** <inject key="AzureAdUserPassword"></inject> **(1)**

      ![](./media/image.png)

4. If **Action required** pop-up window appears, click on **Ask later**.
5. If prompted to **stay signed in**, you can click **No**.
6. If a **Welcome to Microsoft Azure** pop-up window appears, simply click **"Cancel"** to skip the tour.




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


## Support Contact

The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

- Email Support: [cloudlabs-support@spektrasystems.com](mailto:cloudlabs-support@spektrasystems.com)
- Live Chat Support: https://cloudlabs.ai/labs-support

Click **Next** from the bottom right corner to embark on your Lab journey!

![Start Your Azure Journey](./media/PageNo.png)

Now you're all set to explore the powerful world of technology. Feel free to reach out if you have any questions along the way. Enjoy your workshop!
