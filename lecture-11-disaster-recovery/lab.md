# Lab 11 — Building a disaster recovery strategy and ensuring resilience

> 👥 **Lab by the co-instructor.** Below is the author's original assignment, translated into English. The content is unchanged: only the time and place of work and the per-stage durations were removed — those are set by the timetable, not by the assignment.
>
> **Numbering.** For the author this is lab No. 5, and the text refers to their own numbers. Mapping: No. 1 → Lab 3, No. 2 → Lab 5, No. 3 → Lab 7, No. 4 → Lab 9, No. 5 → Lab 11.
>
> **Submission is in Markdown.** Everything on this course is submitted the same way: the report as an `.md` file, schematics as an image or a Mermaid diagram. Details in [Course rules](../COURSE-RULES.md).

---

## 1. Goal

Put into practice what lecture No. 5 "Disaster recovery and resilience" covered:

- calculate and assign target resilience metrics (RPO, RTO, SLO);
- choose and justify a disaster recovery model for a specific service;
- design a Multi-AZ / Multi-Region architecture with replication and automatic recovery;
- set up automation (backups, failover, restore) and run a chaos experiment;
- document the resulting DR strategy.

The output is a working recovery strategy for a notional online store, plus a project report.

## 2. Format

| Parameter | Value |
| --- | --- |
| Working mode | Individually or in a pair (2–3 people) |
| Submission | A report in `.md` + a schematic (as an image or a Mermaid diagram) |

## 3. Inputs (the problem statement)

You are a DevOps engineer at the online bookstore "Книжный мир" (Book World). The customer asks you to design a disaster recovery strategy and make the web application resilient (the services from lab work No. 1).

Acceptable per-service figures (RPO/RTO):

| Service | Purpose | RPO | RTO |
| --- | --- | --- | --- |
| Web frontend | The store's public site | Up to 5 min | Up to 15 min |
| API backend | Order logic, catalogue | Up to 5 min | Up to 15 min |
| Database | Stores the catalogue and orders | Up to 15 min | Up to 30 min |

Additional customer requirements:

- critical services (frontend and API) must run in two regions (Multi-Region) with automatic failover;
- the database must be resilient within the primary region (Multi-AZ with a replica);
- backup and a restore procedure must be provided for;
- to validate the plan, run a "chaos experiment": deliberately take out one zone / one instance and confirm the service recovers;
- every function must have redundancy; there must be no Single Point of Failure (SPOF).

## 4. Assignment and steps

### Stage 1. Defining the resilience metrics

- Determine the target RPO and RTO for each service and break the target RTO into its parts (detection / failover / verification), estimating each.
- Choose and justify a target availability SLO for each critical service (for example 99.9%, 99.95%, 99.99%).
- Record the results in the table in the "Report" section.

> 💡 Hint from the lecture: "RPO — how old the data may be after recovery (how much we lose), RTO — how quickly the service must be back", while the SLO shows how many "nines" of availability we guarantee.

**Checkpoint 1:** the database's RTO (30 min) is longer than the frontend's (15 min) — a heavy component is allowed to recover more slowly than fast ones.

### Stage 2. Choosing a disaster recovery model

- For each service, choose a DR model from the options covered in the lecture: backup & restore, pilot light, warm standby, active-active / multi-site — and justify the choice based on RPO/RTO and budget.
- Fill in the model selection table (see the "Report" section).
- Justify why cheap backup & restore is not enough for orders (compare the recovery times).

**Checkpoint 2:** for each service, the chosen model numerically covers the target RPO/RTO.

### Stage 3. Designing Multi-AZ / Multi-Region

- Design the target architecture based on the diagram from lab work No. 1, placing the critical services in two regions.
- Configure database replication: Multi-AZ (synchronous) in the primary region and cross-region replication (asynchronous) for the standby.
- Provide for a failover policy to the standby region.
- Draw a block diagram (mermaid / diagramming tool): primary region + standby, availability zones, database replicas, gateways and load balancers, failover points.

> 💡 Hint from the lecture: "synchronous replication within a zone (Multi-AZ) protects against the failure of a single instance/zone, asynchronous cross-region replication protects against the failure of an entire region, but with a small data lag".

**Checkpoint 3:** there is no SPOF in the design, and the database has both a synchronous (within the AZ) and an asynchronous (cross-region) replica.

### Stage 4. Automating backups and recovery

- Design the backup automation: schedule and type (snapshots, logical dumps) consistent with the RPO; the retention policy and restore testing.
- Configure automatic DNS / load balancer failover to the standby region (health checks, failover rules).
- Describe the recovery playbook/runbook: the steps, who performs them and in what order, and how long each takes.

**Checkpoint 4:** the total time across the runbook steps does not exceed each service's target RTO.

### Stage 5. Chaos experiment

- Run resilience testing against the scenarios: "one frontend instance is forcibly stopped" and "one availability zone is taken out".
- Describe the expected and the actual behaviour of the system, the recovery time, and what happened from the user's point of view.
- Draw a conclusion: was the target RTO met, and which weak spots were revealed.

**Checkpoint 5:** after the "failure", the service was back in operation within the target RTO.

### Stage 6. Writing up the report

Fill in the report using the template (section 5), attach the architecture diagram and the self-check list (section 6).

## 5. Report

### 5.1. Resilience metrics

| Service | RPO | RTO | SLO | RTO breakdown (detection/failover/verification) |
| --- | --- | --- | --- | --- |
| Web frontend | 5 min | 15 min | 99.95% | 2 / 10 / 3 min |
| API backend | 5 min | 15 min | 99.95% | 2 / 10 / 3 min |
| Database | 15 min | 30 min | 99.9% | 3 / 20 / 7 min |

### 5.2. DR model selection table

| Service | DR model | Covers RPO/RTO? | Justification |
| --- | --- | --- | --- |
| Web frontend | Active-active (multi-site) | Yes | ... |
| API backend | Warm standby | Yes | ... |
| Database | Active-passive (Multi-AZ + cross-region replica) | Yes | ... |

### 5.3. Architecture diagram

Insert the block diagram: primary region + standby, availability zones, database replicas, load balancers, failover and recovery points.

### 5.4. Automation (backups and recovery)

Describe the backup schedule, the retention policy, automatic failover and the restore verification.

### 5.5. Recovery runbook

List the recovery steps, the order they are performed in, who is responsible, and the expected duration of each step.

### 5.6. Chaos experiment results

Describe the scenario, the expected and actual behaviour, the recovery time, the conclusions drawn and the weak spots.

### 5.7. Conclusions

What you took away, which mistakes you avoided, what was new to you.

## 6. Self-check list (before submitting)

Go through the list and tick every item:

- ☐ RPO and RTO are calculated for every service, and the target RTO is broken into its parts.
- ☐ A target SLO is chosen and justified for every critical service.
- ☐ The DR model is chosen and justified, and numerically covers the target RPO/RTO.
- ☐ A Multi-AZ and Multi-Region architecture is designed, and the database has both a synchronous and an asynchronous replica.
- ☐ Automatic DNS/load balancer failover to the standby region is configured.
- ☐ Backup automation is thought through: schedule, retention, restore test.
- ☐ A recovery runbook is written, and the total step time ≤ the target RTO.
- ☐ The chaos experiment was run, the result and weak spots are described, conclusions are drawn.
- ☐ The architecture diagram and documentation are attached.
- ☐ Resources (if a real cloud environment was used) have been deleted after the work.

## 7. Review questions (after the work)

Answered orally at the defense or in writing at the end of the report.

- How does RPO differ from RTO? Give an example where RPO is small but RTO is large, and vice versa.
- Which DR model suits financial transactions, and why is backup & restore not enough?
- What is the difference between synchronous (Multi-AZ) and asynchronous (Multi-Region) database replication, and when is each needed?
- What is a recovery point, and how does it relate to RPO?
- Which components must be made redundant so there is no Single Point of Failure (SPOF)?
- How does automatic failover affect meeting the target RTO?
- Which chaos experiment scenario would you run first, and why?
- What happens to the service if both regions are unavailable at once, and how should the DR plan account for that?

## 8. Grading criteria

| Points | Criterion |
| --- | --- |
| 1 | RPO/RTO are calculated correctly and the target RTO is broken into its parts |
| 1 | A DR model is chosen and justified for every service |
| 1 | A Multi-AZ/Multi-Region architecture and database replication are designed |
| 1 | Automation is configured (backups, failover, recovery runbook) |
| 1 | A chaos experiment was run and the result and weak spots are analysed |
| 1 | A diagram is produced and the report is written up |
| 6 | Maximum total |
