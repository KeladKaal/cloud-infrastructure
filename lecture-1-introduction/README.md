# Lecture 1 — Introduction

The first lecture is organizational and an overview. How the course is structured, the schedule, requirements and how labs are submitted — we go over that in class. Here is a short course plan so you can see where we're heading, plus what to read before your first submission.

## What the course is about

This course looks at cloud infrastructure from two sides at once.

**Side one — the cloud itself.** What it is built from and by what rules it works: virtual networks and load balancers, the three storage types, access control and secrets, money, and plans for recovering from disasters. Here you play architect: you choose, you justify, you calculate.

**Side two — the systems that run in that cloud.** Databases, message brokers, search, monitoring, CI/CD — the things devops sets up and keeps running so that developers can build on top of them. Here you play operations engineer: you stand things up, configure them, break them and fix them.

The lectures alternate so the two sides don't drift apart: we cover storage in the cloud, and the next session we stand up a database; we talk about cloud security, and next we set up single sign-on for GitLab.

## Course plan

| # | Lecture | What's in it |
| --- | --- | --- |
| 1 | Introduction | course organization and plan (this lecture) |
| 2 | Monitoring and observability | Prometheus, Grafana, metrics/logs/traces. It comes second because monitoring is mandatory in every later hands-on lab |
| 3 | Cloud network architecture 👥 | VLAN, VPC, VxLAN, load balancers, DNS/CDN/Anycast, gateways and NAT |
| 4 | Message brokers and cache | Kafka, RabbitMQ, Redis |
| 5 | Cloud data storage 👥 | Block, File, Object; EBS, EFS, S3 and their equivalents |
| 6 | Databases | PostgreSQL/MySQL: access, replication, backups, migrations |
| 7 | Cloud infrastructure security 👥 | Shared Responsibility, IAM, secrets, encryption, SG/NACL/WAF |
| 8 | Search and logging | Elasticsearch / ELK |
| 9 | Cost management (FinOps) 👥 | pricing models, tags and budgets, optimization |
| 10 | GitLab, nginx and access | self-hosted GitLab and CI/CD, nginx, Keycloak (SSO) |
| 11 | Disaster recovery and resilience 👥 | RPO/RTO, DR models, Multi-AZ and Multi-Region, chaos engineering |
| 12 | AI infrastructure | agents, tools, MCP, n8n, hosting and cost |
| 13 | Serverless and event-driven architectures 👥 | FaaS, event buses, state machines ⏳ |
| 14 | Cloud databases 👥 | DDoS protection, WAF, monitoring and response ⏳ |

👥 — lectures by the course's co-instructor. ⏳ — materials still in preparation; only the topic outline is published so far.

## How the labs work

Every lecture has a lab except this one. There are two kinds, and they are submitted differently:

- **Hands-on** (2, 4, 6, 8, 10, 12) — you stand the system up yourself, locally and for free, and show that it works. Every one of these has a mandatory **monitoring** part: decide yourself which metrics go on the dashboard, and pick **3 metrics for alerts** with a rationale.
- **Design** (3, 5, 7, 9, 11; most likely 13 and 14 too) — you design an architecture for the "Book World" online store, justify your choices, calculate costs and submit a report with a schematic. These labs form a chain: each one builds on the previous.

⚠️ **Submission and report rules currently differ between the two halves of the course.** What exactly differs is in [Course rules](../COURSE-RULES.md). Read it before your first submission; everything marked there as unsettled we go over in this session.

## Read before the first lab

- [Course rules](../COURSE-RULES.md) — how work is submitted and where the two halves of the course diverge.
- [`AGENTS.md`](../AGENTS.md) — how AI assistants are meant to be used here. In short: using one is allowed and encouraged, but the repo is set up so the assistant guides you step by step and checks your understanding instead of handing over a finished solution.
