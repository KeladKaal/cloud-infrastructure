# Lab 1 — Designing a cloud network (VPC, load balancers, connectivity)

## 1. Goal

Put the fundamentals of cloud network architecture into practice:

- design a virtual private cloud (VPC) spanning several availability zones;
- choose and configure a load balancer for a specific service;
- provide connectivity between subnets and the outside world (IGW, NAT);
- apply the design checklist and avoid the common mistakes;
- document the resulting network.

The output is a **working cloud network design** for a service you invent, plus a **report** on the project.

## 2. Format

| Parameter | Value |
| --- | --- |
| Working mode | Individually or in a pair (1–2 people) |
| Submission | A report in Markdown (`.md`) committed to a git repository, + a schematic file |

## 3. Inputs (the problem statement)

You are a DevOps engineer. Invent the company and what it does yourself, or pick from the list:

- an online store;
- an online streaming service (video/music);
- a CRM system for a sales team;
- a banking / fintech application;
- an online courses platform;
- a SaaS product for teamwork (messenger / task tracker);
- your own option (agree it with the instructor).

The customer asks you to design and deploy the network architecture for a new web application.

In the report (section 3), describe your service using this template:

**Service name:** *give it a name*

**Service type:** *online store / streaming / CRM / …*

**Target audience:** *who uses it, from which region*

**Main user scenarios:** *briefly, 2–4 scenarios (e.g. browsing the catalogue, placing an order, signing in)*

**Composition of systems (services):** fill in the table below — which subsystems your application needs.

**Service composition (fill in for your own project):**

| Service | Purpose | Specifics (what matters for the design) |
| --- | --- | --- |
| *e.g. web frontend* | *the public part of the service* | *HTTP/HTTPS, static content caching* |
| *e.g. API backend* | *business logic* | *REST/summarize the API paths* |
| *e.g. database* | *data storage* | *must not be reachable from the internet* |
| *e.g. internal services* | *updates, telemetry* | *outbound internet, no inbound connections* |

> Put in at least 3–4 services typical of your application. Set the API paths (e.g. `/api/*`, `/orders/*`) and the routing logic to fit your own project.

**Resilience requirements:**

- Operation across **three availability zones** (AZ);
- Load balancer: the web frontend and the API must be served through a load balancer;
- Internet access for all subnets; for private ones — **outbound only**;
- Every element must have redundancy, **there must be no Single Point of Failure**.

## 4. Assignment and steps

### Stage 1. Designing the VPC

- Choose a **CIDR block** for the VPC. Justify the prefix length (/16 is the standard, /20 the minimum). Make sure the range does not overlap any existing company network.
- Carve out the **subnets**: one public, one private and one data subnet in each of the three availability zones.
- Fill in the subnet table (see the template in the "Report" section).
- Build the **route tables**:
- public subnets → via the Internet Gateway (IGW);
- private subnets → via the NAT Gateway;
- data subnets → via a NAT Gateway or VPC Endpoints (justify your choice).
- Draw the **VPC diagram** (a block diagram): three zones, subnets, gateways.

**Checkpoint 1:** don't put the database in a public subnet; make sure every function has redundancy.

### Stage 2. Load balancers

- Work out which load balancers your project's services need, and configure them.
- For the public services, choose the load balancer type and configure the routing rules: your application's paths (for example `/api/*`, `/orders/*` → the API server group; all other traffic → the frontend/static group).
- Justify your choice of load balancer through the layer it operates at (L4 or L7).
- For each load balancer configure a health check and write down the distribution strategy (Round Robin, Least Connections, etc.) you picked and why.
- If raw TCP is needed for database replication — think through which load balancer would be required, though you don't have to configure it (note it in the report).

> 💡 Hint: "need to see the HTTP content — that's an ALB (L7); need speed and ports — that's an NLB (L4)".

**Checkpoint 2:** check that path-based routing matches the logic: API traffic doesn't land on static content and vice versa.

### Stage 3. Connectivity with the outside world

- Configure the **Internet Gateway** for the public subnets.
- Deploy a **NAT Gateway** for the private and data subnets. Mandatory — **one in every availability zone**. Justify why this is needed (Single Point of Failure).
- Describe (in the report text) how you would set up the primary and backup links to the company's on-premise data centre, if it existed (Direct Connect + Site-to-Site VPN).
- State how to make the connection redundant so that the network keeps working when one link goes down.

### Stage 4. Security and logging

- Describe which **Security Groups** (stateful) you would create for:
- the web frontend;
- the API backend;
- the database;
- the NAT Gateway.
- State which **inbound/outbound rules** you would set (ports, sources/destinations).
- Enable **VPC Flow Logs** and explain what they are for and what they let you detect.

**Checkpoint 3:** check that the database is open only to the API backend, not to everyone.

### Stage 5. Writing up the report

Put the report together as an **`.md` file**, place it in a git repository and commit it (see section 5).

## 5. Report

The report is a **single Markdown file** (for example `report.md`) and is submitted through a **git repository**.

### 5.1. Repository structure

A recommended layout:

```
project-name/
├── README.md            # a short description of the project (1–2 paragraphs)
├── report.md            # the main report, sections as below
├── scheme/              # schematic files (PNG/SVG/JPEG)
│   └── vpc.png
└── (other files as needed)
```

> `README.md` is a short introduction; put the details in `report.md`.

You create this repository once and use it for every lab on the course — each later lab adds its own folder and its own section.

### 5.2. Contents of the `.md` report

`report.md` must include the following sections (as Markdown `##` headings):

- **## 1. CIDR justification** — the chosen range and prefix, and why it does not overlap other networks.
- **## 2. Subnet table** — as a Markdown table (template below).
- **## 3. Network diagram** — the schematic image inline (or a link to the file in `scheme/`).
- **## 4. Load balancers** — a table of types, layers and rules.
- **## 5. Connectivity and redundancy** — IGW, NAT, links and redundancy.
- **## 6. Security** — Security Groups and VPC Flow Logs.
- **## 7. Conclusions** — what you took away, which mistakes you avoided, what was new.

### 5.3. Required tables

**Subnet table:**

| Zone | Subnet | CIDR | Type | Default route |
| --- | --- | --- | --- | --- |
| AZ1 | public | 10.0.1.0/24 | public | IGW |
| AZ1 | private | 10.0.10.0/24 | private | NAT GW |
| AZ1 | data | 10.0.20.0/24 | data | NAT GW / Endpoint |
| AZ2 | ... | ... | ... | ... |
| AZ3 | ... | ... | ... | ... |

**Load balancer table:**

| Service | Balancer type | Layer | Rule / strategy |
| --- | --- | --- | --- |
| *your project's frontend* | ALB | L7 | `/api/*`, `/orders/*` → API; everything else → frontend |

*(substitute your own project's services and paths).*

### 5.4. Submitting via git (steps)

Initialize the repository and add the files:

```bash
git init
git add README.md report.md scheme/
```

Make the first commit:

```bash
git commit -m "Lab 1: cloud network architecture"
```

If the repository is remote (GitHub / GitLab / an internal server):

```bash
git remote add origin <repository-URL>
git push -u origin main
```

Send your instructor the repository link, or an archive with the commits.

> If you're submitting as a pair — both people must be co-authors of the commits (see 5.5).

### 5.5. Working as a pair

- Create a shared repository and work through branches/commits from both accounts.
- Name both authors in `README.md`.

## 6. Self-check list (before submitting)

Go through the list and tick every item:

- ☐ The service is described: name, type, audience, scenarios, composition of systems.
- ☐ CIDR block: prefix /16 or shorter, no overlap with existing networks.
- ☐ Subnets: at least one in each of the three availability zones (public + private + data).
- ☐ Routing: public → IGW, private → NAT/Endpoints.
- ☐ Load balancer chosen correctly by layer (L4/L7) — justified in the report.
- ☐ Health checks configured for all server groups.
- ☐ NAT Gateway in every availability zone (no SPOF).
- ☐ The database is not in a public subnet and is reachable only by the API backend.
- ☐ VPC Flow Logs are enabled.
- ☐ Link redundancy is described (Direct Connect + Site-to-Site VPN).
- ☐ The network diagram and route documentation are attached.
- ☐ Resources (if a real cloud environment was used) have been deleted after the work.

## 7. Review questions (after the work)

Answered orally at the defense or in writing at the end of the report.

- How does VxLAN differ from VLAN? Which VLAN limitations does VxLAN solve?
- ALB or NLB for gRPC and WebSocket? Argue via the layer the balancer operates at.
- What happens to a CDN if you don't set Cache-Control? Why does TTL matter for an API?
- How many NAT Gateways do you need in production across 3 availability zones, and why?
- How does a Security Group differ from a Network ACL (stateful vs stateless)?
- Why can't you keep a database in a public subnet?
- What are VPC Flow Logs for and what do they let you detect?
