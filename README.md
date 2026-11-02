<img src="./Pixel%20art%20developer%20studio%2C%20city%20skyline.png" width="100%" alt="Varun Reddy’s workspace featuring LiveKaart, CreAIve Labs, and CUDA C++ experiments.">

<h1 align="center">Varun Kumar Reddy Nemilidinne</h1>

<p align="center">
  Software & AI Engineer · Virginia, USA
</p>

<p align="center">
  <a href="https://livekaart.com">LiveKaart</a> ·
  <a href="https://creaivelabs.com">CreAIve Labs</a> ·
  <a href="https://humanqa.ai">HumanQA</a> ·
  <a href="https://www.linkedin.com/in/vkrnemilidinne/">LinkedIn</a>
</p>

I built two live products: LiveKaart, a grocery search app, and CreAIve Labs, an AI platform for local service businesses. I’m now building my third product and learning CUDA C++ to understand how data moves between a CPU and a GPU.

I like engineering questions I can test. Can a shopper get an accurate stock answer without waiting for a model? If a Kafka broker goes down during a payment run, does every transaction still reach the ledger? Here’s what I found.

## Products I’ve shipped

### [LiveKaart](https://livekaart.com) · Grocery search and price comparison

A shopper asking “Is toor dal in stock?” needs a current catalog answer. I built LiveKaart’s assistant to look up straightforward questions in PostgreSQL, reuse answers while the catalog version matches, and call a model when a question needs one.

- **Speed:** Median assistant latency fell from **3.7 s to 1.5 s**. One repeated query took **0.8 s** from cache versus **5.5 s** through the model, with no model charge on the cache hit.
- **Quality:** I tested releases against **60 reference queries**, measuring whether the right products appeared and how high they ranked.
- **Scope:** I shipped the first version solo in **7 weeks**, including the shopper experience, store and admin tools, and a React Native mobile app.

### [CreAIve Labs](https://creaivelabs.com) · AI for local service businesses

A groomer handling a customer call and a landscaper preparing an estimate need different tools. I built a platform with **15 trade dashboards**, per-business AI phone agents, and a shared runtime that keeps each business’s data separate.

- **Setup:** An **8-agent workflow** prepares each business configuration. Application code validates the output, runs simulations, and checks the experience in Playwright before publishing.
- **Reliability:** The website assistant has to pass **120 test cases across 15 metrics** before release. When a check failed intermittently, I traced it to search ranking and raised recall on that evaluation from **0.8 to 1.0**.
- **Cost:** Straightforward business questions use SQL directly. For research tasks that need a model, prompt caching reduced the measured cost from **$2.41 to $1.16**.

## Engineering projects

| Project | What I tested |
| --- | --- |
| [Kafka payments benchmark](https://github.com/creAIveLabs-Test/kafka-payments-pipeline-benchmark) | A Java pipeline processed **2 million transactions at 56,638 tx/s** on my Windows i7. In a separate three-broker run, I killed one broker midstream. Throughput recovered in **8 seconds**, and all 2 million transactions reached PostgreSQL with zero duplicate ledger rows. |
| [AdRank](https://github.com/creAIveLabs-Test/adrank) | I built a simulator to test which ad a user should see. Across **13,500 simulated impressions**, ranking by bid × predicted click probability earned **43% more revenue per 1,000 impressions** than ranking by click probability alone. Click-through rate fell from **6.73% to 5.21%**. |
| [HumanQA](https://humanqa.ai) | I built a TypeScript SDK that tests what an AI agent actually did. In the refund demo, deleting the customer record fails the test, returns exit code `1`, and links the failure to the recorded action. |
| [Weather SDK](https://github.com/creAIveLabs-Test/Weather-SDK) | A public Python SDK project. |

## What I’m learning · GPU memory and CUDA C++

I’m writing small kernels on my **NVIDIA RTX 2070 Max-Q** to understand where execution time goes. I started with vector addition on **1,048,576 floats**. My first timing was inflated by **17×**. After warming up the GPU and measuring 100 launches with CUDA events, I got **0.043 ms per launch** and **294 GB/s effective memory bandwidth**.

Next I’m testing pinned host memory, overlapping transfers with CUDA streams, and comparing a tiled matrix multiplication kernel against cuBLAS.

## Professional experience

**Index Systems Inc. · Software Development Engineer, AI · Jun 2026–present**  
I build backend services for a platform spanning **8 business modules**. My work includes **20+ REST endpoints** and an AI-assisted migration that converted **2,000+ legacy files into 7,500+ PostgreSQL records**. Validation checks, retries, and explicit write decisions helped achieve **95% field-level accuracy** and **97% automation**. I also mentor an engineering intern.

**Tekrubix · AI Engineer, Supply Chain Systems · Feb 2025–May 2026**  
I built Kafka and PySpark pipelines processing **about 1 million records per day** for inventory, shipment, and delivery visibility across **60+ stores**. I also built a FastAPI assistant that retrieved supply-chain data with SQL, validated model responses, and handled failures. I evaluated it on **300+ queries**, with **p95 response time under 4 seconds**.

## Tools I work with

- **Backend and applications:** Python, Java, TypeScript, SQL, FastAPI, React, Next.js, React Native
- **Data and streaming:** Kafka, Kafka Streams, PostgreSQL, Redis, PySpark, Databricks, Delta Lake
- **AI systems:** Claude, OpenAI APIs, LangGraph, pgvector, tool calling, retrieval evaluation
- **Delivery and testing:** Docker, Kubernetes, Helm, GitHub Actions, Temporal, OpenTelemetry, pytest, Playwright

## Education

**M.S. in Computer Science** · George Mason University, Fairfax, Virginia · 2024  
**B.Tech. in Computer Science** · Sri Venkateswara University, Tirupati, India · 2022

## Contact

I’m interested in software engineering and applied AI roles where I can build a product, measure how it behaves, and improve the systems behind it.

[LinkedIn](https://www.linkedin.com/in/vkrnemilidinne/) · [Email](mailto:hello@creaivelabs.com)
