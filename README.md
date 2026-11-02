<img src="./Pixel%20art%20developer%20studio%2C%20city%20skyline.png" width="100%" alt="Varun Reddy’s workspace featuring LiveKaart, CreAIve Labs, and CUDA C++ experiments.">

<h1 align="center">Varun Kumar Reddy Nemilidinne</h1>

<p align="center">
  Software & AI Engineer · Founder of LiveKaart and CreAIve Labs
</p>

<p align="center">
  <a href="https://livekaart.com">LiveKaart</a> ·
  <a href="https://www.creaivelabs.com">CreAIve Labs</a> ·
  <a href="https://github.com/creAIveLabs-Test/kafka-payments-pipeline-benchmark">Kafka Benchmark</a> ·
  <a href="https://github.com/creAIveLabs-Test/adrank">AdRank</a> ·
  <a href="mailto:hello@creaivelabs.com">Email</a>
</p>

I’m a software engineer based in Herndon, Virginia. I’ve built two live products, LiveKaart and CreAIve Labs, and I’m building my third. My work spans AI applications, backend services, and real-time data pipelines.

I like questions I can answer by running an experiment. Can a grocery assistant answer from the catalog without calling a model? If a Kafka broker goes down halfway through a run, does every payment still reach the ledger? That curiosity is also taking me closer to the hardware, where I’m learning how data moves through memory and writing CUDA kernels in C++.

## Products I’ve built

### [LiveKaart](https://livekaart.com) · Grocery product search

LiveKaart helps shoppers find products and ask questions about what’s in stock. A question like “Is toor dal available?” can go straight to PostgreSQL. A repeated question can use Redis if the catalog version still matches. Requests that need a generated answer go to Claude.

That routing brought median gateway latency from **3.7 seconds to 1.5 seconds**. In one measured example, a repeated query took **0.8 seconds** from cache versus **5.5 seconds** through the model. A catalog change invalidates the old answer, and personal actions such as saving an item bypass the shared cache.

### [CreAIve Labs](https://www.creaivelabs.com) · AI tools for local businesses

At CreAIve Labs, I build assistants around the work a business owner actually does. A grooming business needs to handle customer inquiries. A landscaper needs measurements for an estimate. A restaurant needs its assistant to work with its point-of-sale system.

I work on the Business Brain and the chatbot behind these workflows, from the React widget and FastAPI service to retrieval with PostgreSQL and pgvector. I use Temporal for workflows, OpenTelemetry to trace requests, and Playwright to test the experience a customer sees.

## Engineering projects

| Project | What I built and measured |
| --- | --- |
| [Kafka payments benchmark](https://github.com/creAIveLabs-Test/kafka-payments-pipeline-benchmark) | A Kafka, Redis, and PostgreSQL pipeline that processed 2 million transactions at **56,638 tx/s** on a Windows i7 and **33,651 tx/s** on an Apple M2. In a separate failure test, I killed one of three brokers mid-run. All 2 million transactions reached the ledger with zero duplicate rows. |
| [AdRank](https://github.com/creAIveLabs-Test/adrank) | An ad-ranking simulator that retrieves candidates, predicts clicks, runs an auction, and compares policies through A/B tests. On simulated traffic, ranking by bid × predicted click probability earned **43% more revenue per 1,000 impressions**, while click-through rate fell from **6.73% to 5.21%**. |
| humanqa | A TypeScript testing SDK that checks an AI agent’s tool calls and database changes. In the refund demo, deleting a customer record fails the test, returns exit code `1`, and links the failure to the action that caused it. |
| [Weather SDK](https://github.com/creAIveLabs-Test/Weather-SDK) | A public Python SDK project. |

## What I’m learning · GPU programming and memory

I’m using an **NVIDIA RTX 2070 Max-Q** to study what happens to data between a CPU and a GPU. I started with a CUDA C++ kernel that adds two vectors of **1,048,576 floats**. After warming up the GPU and timing 100 launches with CUDA events, I measured **0.043 ms per launch** and **294 GB/s of effective memory bandwidth**.

My next experiments are pinned host memory, overlapping transfers with CUDA streams, and comparing tiled matrix multiplication against cuBLAS.

## Professional experience

- **Index Systems Inc. · Software Development Engineer, AI** — I build APIs and PostgreSQL data models for a platform spanning eight business modules. My work includes migrating 2,000+ legacy files into 7,500+ validated records and mentoring an engineering intern.
- **Tekrubix · AI Engineer** — I built Kafka and PySpark pipelines processing about one million supply-chain records per day across 60+ stores. I also built a FastAPI retrieval service and evaluated it on 300+ queries, with p95 response time under four seconds.

## Tools I work with

- **Applications and APIs:** Python, FastAPI, TypeScript, React, Java
- **Data and streaming:** PostgreSQL, Redis, Kafka, Kafka Streams, PySpark, Databricks
- **AI and retrieval:** Claude, retrieval-augmented generation, pgvector, model evaluation
- **Infrastructure and testing:** Docker, Kubernetes, Helm, GitHub Actions, OpenTelemetry, Pytest, Playwright
- **Currently learning:** CUDA C++, GPU memory access, kernel profiling

## Education

**M.S. in Computer Science** · George Mason University, Fairfax, Virginia · 2024  
**B.Tech. in Computer Science** · Sri Venkateswara University, Tirupati, India · 2022

## Get in touch

I’m interested in software engineering and applied AI roles where I can build products, measure how they behave, and improve the systems underneath them. Reach me at [hello@creaivelabs.com](mailto:hello@creaivelabs.com).
