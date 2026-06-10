# Dynamo On AKS

End-to-end tutorials for running [NVIDIA Dynamo](https://github.com/ai-dynamo/dynamo) on **Azure Kubernetes Service (AKS)**, integrated with **Azure Managed Prometheus** and **Grafana**, showcasing different inference serving architectures and autoscaling strategies for LLM workloads.


![image-alt-text](autoscaling_agg_keda/img/vllm_dashboard.png)


---

## Overview

| # | tutorial | Description | Status |
|---|------|----------|--------|
| 1 | [Autoscaling Aggregated Serving with KEDA](#1-autoscaling-aggregated-serving-with-keda) | Metric-driven HPA via KEDA + Prometheus TTFT | ✅ Complete |
| 2 | [Autoscaling Disaggregated Serving with Dynamo Planner](#2-autoscaling-disaggregated-serving-with-dynamo-planner) | Disaggregated Prefill/Decode with Dynamo planner | 🚧 In Progress |
| 3 | [KV Cache Routing](#3-kv-cache-routing) | Intelligent KV cache-aware request routing | 🚧 In Progress |

---

## Architecture

All demos share a common base infrastructure on Azure:

```
┌─────────────────────────────────────────────────────────┐
│                    Azure AKS Cluster                    │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │                                                  │   │
│  │                 Dynamo operator                  │   │
│  └──────────────────────────────────────────────────┘   │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │      Azure GPU Node Pool (ND / NC / NV)          │   │
│  │   Frontend pods  →  Prefill / Decode Workers     │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
         │                          │
         ▼                          ▼
┌─────────────────┐      ┌──────────────────────┐
│  Azure Managed  │      │   Azure Managed      │
│  Prometheus     │────▶ │   Grafana            │
└─────────────────┘      └──────────────────────┘
```


## Demos

### 1. Autoscaling Aggregated Serving with KEDA

**Folder:** [`autoscaling_agg_keda/`](./autoscaling_agg_keda/)

**What it demonstrates:**

Deploys Dynamo in **aggregated serving** mode (combined Prefill + Decode per pod) and autoscales the number of Worker replicas using **KEDA** triggered by **Time-to-First-Token (TTFT)** latency sourced from Prometheus.

**Architecture:**

```
Client → LoadBalancer :8000
           └─ Frontend pods ×2
                   └─ VllmDecodeWorker pods ×2–4  (autoscaled)
```

**Autoscaling Signal:**


When **TTFT p95 > 300 ms**, KEDA scales up Decode Workers from **2 → 4** replicas. Scale-down cooldown is 120 s.

## Task 1: 

1. From the Lab VM, select **Visual Studio Code
        ![](images/w14.png)

1. Click the **More Actions (⋯) (1)**, select **Terminal** **(2)**, and then click **New Terminal** **(3)** to open a new integrated terminal.
        ![](images/w15.png)

1. Clone the repository by running the following command:

   ```bash
   git clone "https://github.com/maljazaery/Dynamo_on_AKS.git"
   ```

   ![](images/w16.png)

1. Click **OneDrive** **(1)**, select the **Dynamo_on_AKS** folder **(2)**, and then click **Select Folder** **(3)** to open the repository in Visual Studio Code.

    ![](images/w17.png)

1. When prompted, select **Yes, I trust the authors**.
        ![](images/w18.png)

1. In the Explorer pane, expand the **DYNAMO_ON_AKS**  **(1)**, open the **autoscaling_agg_keda** folder **(2)**, and then select the **dynamo-keda.ipynb** notebook file **(3)**.

   ![](images/w19.png)

1. In the **Environment Configuration** section, update the **CLUSTER_NAME** value by replacing **XXXXXX** **(1)** with a **Cloud DID**, and then click the **Run Cell** button **(2)** to execute the configuration cell.

   ![](images/w20.png)

1. Verify that the configuration cell has executed successfully by confirming that a green check mark and execution status are displayed at the bottom of the cell.

   ![](images/w21.png)

1. In the **Provision AKS Cluster & GPU Node Pool** section, click the **Run Cell** button to execute the AKS cluster provisioning command and create the system node pool.

   ![](images/w22.png)

1. After the command completes, verify that the AKS cluster was created successfully by reviewing the JSON output, which should resemble the output shown below.

   ![](images/w23.png)

1. Click the **Run Cell** button to execute the GPU node pool provisioning command, and verify that the operation completes successfully by confirming that the output displays the node pool configuration details in JSON format, similar to the output shown below.

   ![](images/w24.png)


> See [autoscaling_agg_keda/dynamo-keda.ipynb](./autoscaling_agg_keda/dynamo-keda.ipynb) for the full step-by-step guide.



---

### 2. Autoscaling Disaggregated Serving with Dynamo Planner

> **Status: 🚧 In Progress**

**What it demonstrates:**

Deploys Dynamo in **disaggregated serving** mode, separating **Prefill** and **Decode** workers onto dedicated pods. The **Dynamo Planner** continuously monitors queue depths and GPU utilization to dynamically rebalance the Prefill-to-Decode worker ratio without requiring an external autoscaler.

**Architecture:**

```
Client → LoadBalancer :8000
           └─ Frontend pods
                   ├─ Prefill Worker pods  (planner-managed)
                   └─ Decode Worker pods   (planner-managed)
```

**Key concepts:**
- Disaggregated Prefill/Decode for improved GPU memory efficiency
- Dynamo Planner controls replica ratios based on live load signals
- Integration with Azure Managed Prometheus for observability




---

### 3. KV Cache Routing

> **Status: 🚧 In Progress**

**What it demonstrates:**

Implements **KV cache-aware request routing** to maximize KV cache reuse across Decode Worker pods. Requests with overlapping prompt prefixes are steered to the worker that already holds the relevant KV cache blocks, reducing redundant computation and improving throughput.

**Key concepts:**
- Prefix-aware load balancing for LLM inference
- Reduced KV cache eviction under high-concurrency workloads
- Grafana dashboards for cache hit rate and routing efficiency

---




## Contributing

Contributions and issue reports are welcome. Please open a GitHub Issue or Pull Request.

