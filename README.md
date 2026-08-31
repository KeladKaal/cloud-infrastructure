# Cloud Infrastructure

> 🌐 **Language:** English (branch `main`) · [Русская версия → branch `main-rus`](https://github.com/KeladKaal/cloud-infrastructure/tree/main-rus)

A fourth-year course about what cloud infrastructure is made of and how you run it afterwards.

We look at the cloud from two sides at once. On one side — **how the cloud itself is built**: virtual networks and load balancers, storage types, access control and secrets, money, and resilience. On the other — **which systems live in that cloud and who keeps them running**: databases, message brokers, search, monitoring, CI/CD. One instructor teaches the first half, another the second, and the lectures alternate so that cloud theory and operational practice run side by side.

No deep prior knowledge needed — just basic command line, Docker, and a general idea of what a network is.

## Lectures

Fourteen lectures. Click a lecture to see what's inside.

| # | Lecture | What's in it |
| --- | --- | --- |
| 1 | [Introduction](lecture-1-introduction/) | course organization and plan |
| 2 | [Monitoring and observability](lecture-2-monitoring-observability/) | Prometheus, Grafana, metrics/logs/traces |
| 3 | [Cloud network architecture](lecture-3-cloud-networking/) 👥 | VLAN, VPC, VxLAN, load balancers, DNS/CDN/Anycast, gateways and NAT |
| 4 | [Message brokers and cache](lecture-4-brokers-cache/) | Kafka, RabbitMQ, Redis |
| 5 | [Cloud data storage](lecture-5-cloud-storage/) 👥 | Block, File, Object; EBS, EFS, S3 and their equivalents |
| 6 | [Databases](lecture-6-databases/) | PostgreSQL/MySQL: access, replication, backups, migrations |
| 7 | [Cloud infrastructure security](lecture-7-cloud-security/) 👥 | Shared Responsibility, IAM, secrets, encryption, SG/NACL/WAF |
| 8 | [Search and logging](lecture-8-search-logging/) | Elasticsearch / ELK |
| 9 | [Cost management (FinOps)](lecture-9-finops/) 👥 | pricing models, tags and budgets, optimization |
| 10 | [GitLab, nginx and access](lecture-10-gitlab-nginx-access/) | self-hosted GitLab and CI/CD, nginx, Keycloak (SSO) |
| 11 | [Disaster recovery and resilience](lecture-11-disaster-recovery/) 👥 | RPO/RTO, DR models, Multi-AZ and Multi-Region, chaos engineering |
| 12 | [AI infrastructure](lecture-12-ai-infrastructure/) | agents, tools, MCP, n8n, hosting and cost |
| 13 | [Serverless and event-driven architectures](lecture-13-serverless-event-driven/) 👥 | FaaS, event buses, state machines ⏳ |
| 14 | [Protection from DDoS and network attacks](lecture-14-ddos-protection/) 👥 | AWS Shield, WAF, Anycast/CDN, monitoring and response ⏳ |

👥 — lectures and labs by the course's co-instructor. ⏳ — materials still in preparation; only the topic outline is published so far. The labs are translated into English; **the lecture texts are still in the author's original Russian** and are kept unchanged, diagrams included.

## Labs

Every lecture has a lab except the introductory one. For lectures 13 and 14 the materials are still in preparation.

The two halves of the course work differently, and that is deliberate:

- **Labs 2, 4, 6, 8, 10, 12** — hands-on: you stand the system up yourself, **locally and for free** (Docker / `docker compose` or Kubernetes, whichever you prefer), configure it and show that it works. You submit a repository with configs, a README and screenshots. **Mandatory in every one of these labs — monitoring:** decide yourself which metrics go on the dashboard, and pick **3 metrics you'd build alerts on**, with a rationale.
- **Labs 3, 5, 7, 9, 11** (and, most likely, 13 and 14) — design: you design and justify an architecture, fill in calculation tables and submit a report with a schematic.

**Everything is submitted the same way — in Markdown:** the report as an `.md` file, schematics and diagrams as images or Mermaid blocks. Grading, the defense and whether monitoring is mandatory do still differ between the two halves — see [Course rules](COURSE-RULES.md). Read it before your first submission.

## Running examples

The design labs all revolve around one customer — an online bookstore called **"Книжный мир" (Book World)**: each lab builds on the architecture designed in the previous one. The hands-on labs use their own example — a small grocery delivery service, which is convenient for watching metrics, queues and logs.

## Using AI for the labs

Using an AI assistant is allowed and encouraged — but this repo is set up so it **helps you learn instead of handing you finished answers**: it works step by step, explains the reasoning, and checks your understanding before moving on.

The rules live in [`AGENTS.md`](AGENTS.md) (and `CLAUDE.md` for Claude Code). Most AI tools pick these up automatically. **If yours doesn't, just point it at [`AGENTS.md`](AGENTS.md)** and ask it to follow the file.

## How this repo is organized

- One folder per lecture, containing the notes (`README.md`) and the lab (`lab.md`); the co-instructor's lectures also have an `images/` folder with the diagrams from the original text.
- **`main`** — everything in **English**. **`main-rus`** — the same in **Russian**.
- Anything pushed here is mirrored in both languages: English → `main`, Russian → `main-rus`.

## License

© 2026 KeladKaal. Licensed under [CC BY-NC 4.0](LICENSE). Free to reuse and adapt for **non-commercial** purposes with **attribution**. The co-instructor's lectures and labs remain theirs and are included here with the author's text preserved.
