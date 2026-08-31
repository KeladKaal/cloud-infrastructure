# Lab 3 — Designing a cloud network (VPC, load balancers, connectivity)

> 👥 **Lab by the co-instructor.** Below is the author's original assignment, translated into English; the content is unchanged.
>
> **Numbering.** For the author this is lab No. 1, and the text refers to their own numbers. Mapping: No. 1 → Lab 3, No. 2 → Lab 5, No. 3 → Lab 7, No. 4 → Lab 9, No. 5 → Lab 11.
>
> **Note: the submission rules here are different.** The assignment asks for a .docx/.pdf report, pair work and a 6-point score — that is the co-instructor's format for the second half of the course. Labs in the first half are submitted differently. What applies on this course is in [Course rules](../COURSE-RULES.md); until that is agreed, check the submission format with your instructor.

---

## 1. Goal

Put into practice what lecture No. 1 "Cloud network architecture" covered:

- design a virtual private cloud (VPC) spanning several availability zones;
- choose and configure a load balancer for a specific service;
- provide connectivity between subnets and the outside world (IGW, NAT);
- apply the design checklist and avoid the common mistakes;
- document the resulting network.

The output is a working cloud network design for a notional online store, plus a project report.

## 2. Format

| Parameter | Value |
| --- | --- |
| Working mode | Individually or in a pair (2–3 people) |
| Time | 2 academic hours (80 minutes) |
| Environment | A cloud console (AWS / Yandex Cloud / VK Cloud) or a diagramming tool |
| Submission | A report file in .docx or .pdf + a schematic file |

## 3. Inputs (the problem statement)

You are a DevOps engineer at the online bookstore "Книжный мир" (Book World). The customer asks you to design and deploy the network architecture for a new web application.

Customer requirements:

| Service | Purpose | Specifics |
| --- | --- | --- |
| Web frontend | The store's public site | HTTP/HTTPS, users across the whole region, static content must be cached |
| API backend | Order logic, catalogue | REST API, paths /api/*, /orders/* |
| Database | Stores the catalogue and orders | Must not be reachable from the internet |
| Internal services | Updates, telemetry | Must reach the internet outbound, but accept no inbound connections |

Resilience requirements:

- Operation across three availability zones (AZ);
- Load balancer: the web frontend and the API must be served through a load balancer;
- Internet access for all subnets; for private ones — outbound only;
- Every element must have redundancy; there must be no Single Point of Failure.

## 4. Assignment and steps

### Stage 1. Designing the VPC

- Choose a CIDR block for the VPC. Justify the prefix length (per the lecture: /16 is the standard, /20 is the minimum). Make sure the range does not overlap any existing company network.
- Carve out the subnets: one public, one private and one data subnet in each of the three availability zones.
- Fill in the subnet table (see the template in the "Report" section).
- Build the route tables:
- public subnets → via the Internet Gateway (IGW);
- private subnets → via the NAT Gateway;
- data subnets → via a NAT Gateway or VPC Endpoints (justify your choice).
- Draw a diagram of the VPC (a block diagram): three zones, subnets, gateways.

**Checkpoint 1:** don't put the database in a public subnet; make sure every function has redundancy.

### Stage 2. Load balancers

Work out which load balancers the services in the table need, and configure them.

- Web frontend + API backend — choose the load balancer type and configure the routing rules:
- path /api/* → the API server group;
- path /orders/* → the API server group;
- all other traffic → the web frontend (static) group.
- Justify your choice of load balancer through the layer it operates at (L4 or L7).
- For each load balancer configure a health check and write down the distribution strategy (Round Robin, Least Connections, etc.) you picked and why.
- If raw TCP is needed for database replication — think through which load balancer would be required, though you don't have to configure it (note it in the report).

> 💡 Hint from the lecture: "need to see the HTTP content — that's an ALB (L7); need speed and ports — that's an NLB (L4)".

**Checkpoint 2:** check that path-based routing matches the logic: API traffic doesn't land on static content and vice versa.

### Stage 3. Connectivity with the outside world

- Configure the Internet Gateway for the public subnets.
- Deploy a NAT Gateway for the private and data subnets. Mandatory — one in every availability zone. Justify why this is needed (Single Point of Failure).
- Describe (in the report text) how you would set up the primary and backup links to the company's on-premise data centre, if it existed (Direct Connect + Site-to-Site VPN).
- State how to make the connection redundant so that the network keeps working when one link goes down.

### Stage 4. Security and logging

- Describe which Security Groups (stateful) you would create for:
- the web frontend;
- the API backend;
- the database;
- the NAT Gateway.
- State which inbound/outbound rules you would set (ports, sources/destinations).
- Enable VPC Flow Logs and explain what they are for and what they let you detect.

**Checkpoint 3:** check that the database is open only to the API backend, not to everyone.

### Stage 5. Writing up the report (10 minutes)

Fill in the report using the template (section 5), attach the network diagram and the self-check list (section 6).

## 5. Report

Write the report in the following structure (as a text document):

### 5.1. CIDR justification

Give the chosen range and prefix length, and explain why it does not overlap other networks.

### 5.2. Subnet table

| Zone | Subnet | CIDR | Type | Default route |
| --- | --- | --- | --- | --- |
| AZ1 | public | 10.0.1.0/24 | public | IGW |
| AZ1 | private | 10.0.10.0/24 | private | NAT GW |
| AZ1 | data | 10.0.20.0/24 | data | NAT GW / Endpoint |
| AZ2 | ... | ... | ... | ... |
| AZ3 | ... | ... | ... | ... |

### 5.3. Network diagram

Insert the VPC block diagram with three zones, subnets, gateways and load balancers.

### 5.4. Load balancers

| Service | Balancer type | Layer | Rule / strategy |
| --- | --- | --- | --- |
| Web frontend | ALB | L7 | /api/*, /orders/* → API; everything else → frontend |

### 5.5. Connectivity and redundancy

Describe how the IGW and NAT are configured and how link redundancy is achieved.

### 5.6. Conclusions

What you took away, which mistakes you avoided, what was new to you.

## 6. Self-check list (before submitting)

Go through the list and tick every item:

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

## 8. Grading criteria

| Points | Criterion |
| --- | --- |
| 1 | A correct, justified CIDR block is provided |
| 1 | Subnets are designed across three availability zones |
| 1 | Load balancers are chosen and configured correctly (L4/L7) with health checks |
| 1 | Connectivity is in place (IGW, NAT in every zone, redundancy) |
| 1 | Security is thought through (Security Groups, Flow Logs, database closed off) |
| 1 | A network diagram is produced and the report is written up |
| 6 | Maximum total |
