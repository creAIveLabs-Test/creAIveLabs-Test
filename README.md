\<img src="./Pixel%20art%20developer%20studio%2C%20city%20skyline.png" width="100%" alt="Pixel-art developer studio overlooking a city skyline, with screens showing LiveKaart, CreAIve Labs, and CUDA experiments.">

<h1 align="center">Varun Kumar Reddy Nemilidinne</h1>

<p align="center">
  <b>Software & AI Engineer</b> · Backend · Streaming Data · Production AI · Virginia, USA
</p>

<p align="center">
  <a href="https://livekaart.com"><img src="https://img.shields.io/badge/LiveKaart-live-8B1E1E?style=flat-square" alt="LiveKaart"></a>
  <a href="https://creaivelabs.com"><img src="https://img.shields.io/badge/CreAIve%20Labs-live-8B1E1E?style=flat-square" alt="CreAIve Labs"></a>
  <a href="https://humanqa.ai"><img src="https://img.shields.io/badge/HumanQA-building-555555?style=flat-square" alt="HumanQA"></a>
  <a href="https://www.linkedin.com/in/vkrnemilidinne/"><img src="https://img.shields.io/badge/LinkedIn-connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
  <a href="mailto:hello@creaivelabs.com"><img src="https://img.shields.io/badge/Email-say%20hello-222222?style=flat-square&logo=gmail&logoColor=white" alt="Email"></a>
</p>

<p align="center">
  <i>I build systems, then try to break them, then write down the numbers.</i>
</p>

---

I've shipped two live products on my own: **LiveKaart**, a grocery search and price-comparison app, and **CreAIve Labs**, an AI platform for local service businesses. I'm building a third, **HumanQA**, and learning CUDA C++ to see how data really moves between a CPU and a GPU.

Most portfolios list what someone built. This one lists **questions I asked and what the measurements said.**

## 🧪 Questions I answered with measurements

| The question | How I tested it | The answer |
| --- | --- | --- |
| Can a shopper get an accurate stock answer without waiting on a model? | Routed plain lookups to PostgreSQL and reused validated answers keyed to the catalog version | Median latency **3.7 s → 1.5 s**. Repeats in **0.8 s** at **$0** model cost |
| If a Kafka broker dies mid-payment run, does every transaction still reach the ledger? | Killed 1 of 3 brokers during a 2M-transaction run | Recovered in **8 s**. **0 lost, 0 duplicate** ledger rows |
| How fast can one laptop push a correct payments pipeline? | Java + Kafka Streams + Redis + PostgreSQL, idempotent end to end | **56,638 tx/s** sustained over 2M transactions |
| Why did my AI release gate fail only *sometimes*? | Traced a flaky eval to retrieval ranking | Recall on that eval **0.8 → 1.0** |
| Does prompt caching actually matter for research tasks? | Measured cost per research run before and after | **$2.41 → $1.16** per run |
| Can tenant isolation be *proven*, not just promised? | CI checks every table on every release | **0 of 303** tables publicly readable, **0 of 256** leaking across tenants |
| Which ad should a user see? | Simulated 13,500 impressions across ranking strategies | Bid × pCTR earned **+43% revenue per 1K** impressions, even as CTR fell 6.73% → 5.21% |
| Why was my first CUDA timing wrong? | Warmed up the GPU, averaged 100 launches with CUDA events | First number was inflated **17×**. Real: **0.043 ms**, **294 GB/s** |

## 🚀 Products I've shipped

### [LiveKaart](https://livekaart.com) · Grocery search and price comparison

A shopper asking *"Is toor dal in stock?"* needs the current catalog, not a model's guess. So the assistant asks the database first and only calls a model when the question actually needs one.

```mermaid
flowchart LR
    Q["Shopper question"] --> L{"Plain product lookup?"}
    L -- yes --> DB[("PostgreSQL")]
    L -- no --> C{"Validated answer for this catalog version?"}
    C -- yes --> R["Reuse answer · 0.8 s · $0"]
    C -- no --> M["Model call"] --> V["Validate"] --> S["Store, keyed to catalog version"]
```

- **Speed:** median assistant latency **3.7 s → 1.5 s**; a repeated query took **0.8 s** from cache vs **5.5 s** through the model.
- **Quality:** every deploy gated on **60 golden queries** scored with Precision, Recall, NDCG, and MRR.
- **Uptime:** **99.91%** across **2,177** probes, watched from a 21-tab observability console I built.
- **Scope:** shipped solo in **7 weeks / 268 commits**: web app, store and admin tools, and an Expo React Native app for iOS and Android.

### [CreAIve Labs](https://creaivelabs.com) · AI for local service businesses

A groomer answering a call and a landscaper writing an estimate need different tools. CreAIve Labs gives each trade its own dashboard and AI phone agent, on one shared runtime that keeps every business's data separate.

- **Setup without blind trust:** an **8-agent pipeline** drafts each business's configuration, but agents never write to the database. Code runs **~175 checks**, simulations, and Playwright verification before anything publishes.
- **Release gate:** the website assistant must pass **120 cases across 15 metrics** before it ships.
- **Cost:** plain business questions are answered with SQL at **$0**; hybrid full-text + pgvector retrieval handles the rest.
- **Security:** default-deny JWT/JWKS gateway, PostgreSQL row-level security, signed idempotent webhooks, and **121 backend tests** with automatic rollback.
- **Scale:** **15 trade dashboards** and **211 API routes**, built and run as the sole engineer.

#### 🏗️ System design · 20 steps from sign-up to safe release

One rule runs through the whole design: **AI proposes, code decides.** Models draft, rank, and answer. Deterministic code validates, isolates, and publishes.

```mermaid
flowchart LR
    subgraph PV["① – ⑩ Provision a business"]
        direction TB
        A["①–③ Tenant + trade template + durable workflow"] --> B["④–⑦ 8 agents propose · Pydantic outputs · tiered models · no DB writes"]
        B --> K["⑧–⑩ Code decides · ~175 checks · simulation · Playwright"]
    end

    subgraph CL["Clients"]
        direction TB
        D["15 trade dashboards"]
        W["Website assistant"]
        PH["AI phone agents"]
    end

    D & W & PH --> G["⑪ Default-deny JWT/JWKS gateway"]
    G --> R["⑬ AI runtime · rate + token/cost budgets"]
    R --> Q{"⑭ Answerable in SQL?"}
    Q -- "yes · $0" --> DB[("⑫ Supabase Postgres · row-level security per tenant")]
    Q -- no --> H["⑮ Hybrid retrieval · full-text + pgvector · RRF"]
    H --> M["⑯ Model call · prompt caching"]
    H --> DB
    WH["⑰ Signed, idempotent webhooks"] --> DB
    K -- publish --> DB
    R -.-> O["⑱ OpenTelemetry traces"]

    subgraph SH["⑲ – ⑳ Ship"]
        direction TB
        CI["⑲ CI gates · 121 tests · isolation proof · 120-case eval"] --> K3["⑳ k3s on Oracle Cloud · auto-rollback"]
    end
```

<details>
<summary><b>Read all 20 steps</b></summary>
<br>

**Provision: agents propose, code decides**

1. **Tenant created.** A business signs up and gets its own tenant ID; every row it will ever own carries that ID.
2. **Trade template chosen.** The business maps to one of **15 trade dashboards**, so a groomer and a landscaper start from different tools.
3. **Durable workflow starts.** Provisioning runs on a Postgres-backed workflow engine, so a crash resumes from the last finished step instead of starting over.
4. **Eight agents draft.** An **8-agent pipeline** drafts the business's configuration.
5. **Structured outputs only.** Every agent returns a **Pydantic** schema; anything that doesn't fit the shape is rejected, not repaired by guesswork.
6. **Tiered model escalation.** Work starts on a cheaper model and escalates to a stronger one only when it fails.
7. **No database writes from AI.** Agents return proposals. They have no path to write to the database.
8. **Deterministic checks.** Code runs **~175 checks** against every proposal.
9. **Simulation.** The configured assistant is exercised before any customer sees it.
10. **Browser verification, then publish.** **Playwright** checks the rendered experience; only a passing configuration is published.

**Serve: every request is isolated, budgeted, and cheap by default**

11. **Default-deny gateway.** Every call from dashboards, the website assistant, or phone agents passes a **JWT/JWKS** gateway that rejects anything not explicitly allowed.
12. **Isolation in the database itself.** **PostgreSQL row-level security** scopes every query to its tenant, so a bug in app code still can't read another business's data.
13. **Budgets before models.** The **Python AI runtime** enforces per-tenant rate limits and token/cost budgets before any model is called.
14. **SQL first.** If a question can be answered from data, SQL answers it, at **$0** model cost.
15. **Hybrid retrieval.** Otherwise, full-text search and **pgvector** results are fused with **Reciprocal Rank Fusion (RRF)**.
16. **Model call with caching.** The model gets only what it needs; **prompt caching** cut research cost from **$2.41 to $1.16**. The website assistant runs on **FastAPI, LangGraph, Temporal, Redis, and MCP**.
17. **Safe inbound events.** External events arrive through **signed, idempotent webhooks**, so a forged or repeated event changes nothing.
18. **Everything traced.** **OpenTelemetry** follows each request through gateway, runtime, retrieval, and model.

**Ship: releases have to prove they're safe**

19. **CI gates.** A release must pass **121 backend tests**, prove **0 of 303 tables publicly readable** and **0 of 256 leaking across tenants**, and clear the assistant's **120-case / 15-metric eval gate**.
20. **Deploy with a way back.** Passing builds deploy to **k3s Kubernetes on Oracle Cloud** with **automatic rollback** if the release goes bad.

</details>

## 🛠️ Engineering projects

| Project | What it proves |
| --- | --- |
| [**Kafka payments benchmark**](https://github.com/creAIveLabs-Test/kafka-payments-pipeline-benchmark) | Java pipeline with idempotent producers, Kafka Streams (RocksDB state), cache-aside enrichment, and a ledger committed *before* offsets. **56,638 tx/s**, p50 **92 ms** at a fixed 20K tx/s, all **24.4M** IBM TabFormer rows replayed, and a broker kill survived with zero lost or duplicate rows. |
| [**AdRank**](https://github.com/creAIveLabs-Test/adrank) | Ad-ranking simulator showing why the "best click rate" ad isn't the best ad: bid × predicted CTR earned **43% more revenue per 1K impressions**. |
| [**HumanQA**](https://humanqa.ai) | TypeScript SDK that tests what an AI agent *actually did*, not what it said. In the refund demo, deleting the customer record fails the test with exit code `1` and links the failure to the recorded action. |
| [**Weather SDK**](https://github.com/creAIveLabs-Test/Weather-SDK) | A public Python SDK. |

## 🔬 Lab notebook · GPU memory and CUDA C++

Running small kernels on an **NVIDIA RTX 2070 Max-Q** to learn where execution time really goes.

- [x] Vector addition on **1,048,576 floats**: fixed a **17×** timing error, measured **0.043 ms** per launch and **294 GB/s** effective bandwidth
- [ ] Pinned host memory vs pageable transfers
- [ ] Overlapping transfers and compute with CUDA streams
- [ ] Tiled matrix multiplication vs cuBLAS

## 💼 Experience

**Index Systems Inc.** · Software Development Engineer, AI · *Jun 2026 – present*  
Backend for a platform spanning **8 business modules** and **20+ REST endpoints**. Built an AI-assisted migration that turned **2,000+ legacy files into 7,500+ PostgreSQL records** at **95% field-level accuracy** and **97% automation**, with PASS / FAIL / UNKNOWN write gates so nothing uncertain lands in the database. Mentor an engineering intern.

**Tekrubix** · AI Engineer, Supply Chain Systems · *Feb 2025 – May 2026*  
Kafka and PySpark pipelines on Databricks and Delta Lake processing **~1M records/day** for inventory, shipment, and delivery visibility across **60+ stores**. Built a FastAPI supply-chain assistant with SQL retrieval and validated structured outputs, evaluated on **300+ queries** at **p95 under 4 s**.

## 🧰 Toolbox

| | |
| --- | --- |
| **Backend & apps** | Python · Java · TypeScript · SQL · FastAPI · Pydantic · Next.js · React · React Native |
| **Data & streaming** | Kafka · Kafka Streams · PySpark · Databricks · Delta Lake · PostgreSQL · Redis · Temporal |
| **AI systems** | Claude · OpenAI APIs · LangGraph · RAG · pgvector · tool calling · MCP · golden-set evals · MLflow |
| **Delivery & quality** | Docker · Kubernetes · Helm · GitHub Actions · OpenTelemetry · pytest · Playwright · PostgreSQL RLS |

## 🎓 Education

**M.S., Computer Science and Engineering** · George Mason University, Fairfax, VA · 2024  
**B.Tech.** · Sri Venkateswara University, Tirupati, India · 2022

---

<p align="center">
  I'm looking for <b>software engineering and applied AI roles</b> where I can build the product, measure how it behaves, and improve the systems behind it.<br><br>
  <a href="https://www.linkedin.com/in/vkrnemilidinne/">LinkedIn</a> · <a href="mailto:hello@creaivelabs.com">hello@creaivelabs.com</a>
</p>
