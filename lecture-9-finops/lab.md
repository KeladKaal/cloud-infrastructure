# Lab 9 — Cloud cost management (FinOps): pricing, cost allocation, optimization

> 👥 **Lab by the co-instructor.** Below is the author's original assignment, translated into English. The content is unchanged: only the time and place of work and the per-stage durations were removed — those are set by the timetable, not by the assignment.
>
> **Numbering.** For the author this is lab No. 4, and the text refers to their own numbers. Mapping: No. 1 → Lab 3, No. 2 → Lab 5, No. 3 → Lab 7, No. 4 → Lab 9, No. 5 → Lab 11.
>
> **Submission is in Markdown.** Everything on this course is submitted the same way: the report as an `.md` file, schematics as an image or a Mermaid diagram. Details in [Course rules](../COURSE-RULES.md).

---

## 1. Goal

Put into practice what lecture No. 4 "Cost management (FinOps)" covered:

- choose and justify a pricing model for different kinds of workload (On-Demand, Reserved, Spot);
- organize cost allocation using tags and projects;
- set up budgets and spending alerts;
- apply optimization techniques (rightsizing, auto-scaling, intelligent tiering);
- document the resulting cost model.

The output is a working cost management model for a notional online store, plus a project report.

> ⚠️ Important. The work is done in "calculator simulation" mode (Option A). All monetary figures are computed on an official pricing calculator (Yandex Cloud Pricing / AWS Pricing Calculator). No real money is spent. In the cloud console, on a free (trial) account, only one demonstration VM is created — to practise tagging, budgeting and monitoring.

## 2. Format

| Parameter | Value |
| --- | --- |
| Working mode | Individually or in a pair (2–3 people) |
| Submission | A report in `.md` + a calculation table |
| Note | Simulation: no real money is spent, all prices come from the calculator |

## 3. Inputs (the problem statement)

You are a DevOps engineer at the online bookstore "Книжный мир" (Book World). The customer asks you to design a cost management model for new cloud infrastructure services.

Customer requirements:

| Service | Purpose | Load profile |
| --- | --- | --- |
| Web frontend | The store's public site | Steady baseline load + peaks during sales hours |
| API backend | Order logic, catalogue | Load proportional to the frontend, peak periods |
| Database | Stores the catalogue and orders | Runs 24/7 without stopping |
| Test environment | Development and QA | Working hours only, available irregularly |
| Object storage | Static content, backups, old files | Rare access to old data, frequent access to new |

Budget and cost management requirements:

- Every VM/resource must carry the tags: env, project, team, cost_center;
- Costs must be allocated across projects and teams;
- You need to see a forecast and receive timely alerts about budget overruns;
- The goal is to cut monthly spend by 20–40% by combining pricing models and optimizing.

## 4. Assignment and steps

### Stage 1. Choosing pricing models

- Build a table of the services from the statement and pick a pricing model for each: On-Demand, Reserved or Spot/Preemptible.
- Justify each choice through the load profile (per the lecture): steady baseline → Reserved; peaks → On-Demand; irregular/test → Spot.
- Check that a steadily running service isn't left on the most expensive mode for no reason.
- Fill in the model selection table (see the template in the "Report" section).

**Checkpoint 1.** Make sure the database and the web frontend aren't left on the most expensive On-Demand mode without justification, and that the test environment isn't "locked" into Reserved.

### Stage 2. Calculating cost on the calculator

- Open the pricing calculator (Yandex Cloud Pricing / AWS Pricing Calculator).
- For each VM in the assignment, calculate the monthly cost in On-Demand mode.
- Then recalculate the same VM in the model you chose (Reserved/Spot, using the discount from the calculator).
- Fill in the comparison table: On-Demand vs the chosen model, and calculate the savings in ₽ and as a percentage.
- For object storage, work out the cost in three classes: Standard, Standard-IA (cold), Glacier (archive).

**Checkpoint 2.** Check that the savings for each model are computed from the calculator, not "pulled out of thin air".

> 💡 Hint from the lecture: "baseline — Reserved, peaks — On-Demand and Spot". Mixing models lowers the average hourly rate while keeping flexibility.

### Stage 3. Cost allocation through tags

- In the free console, create one demonstration VM of the smallest size (t2.micro / g2.1xsmall) — this is the only real resource.
- Assign it the mandatory tags in key = value form: env, project, team, cost_center.
- Using it as the example, fill in the tag table for all the services in your architecture (as if they had been created).
- Explain what happens to the spend of a resource with no tag (the "unknown" / "black hole" category in the report).
- Take a screenshot of the demo VM with its tags and insert it into the report.

**Checkpoint 3.** Check that no service has an empty mandatory tag.

### Stage 4. Budgets and alerts

- In the Billing / Cost Management section, create a project budget with a small real limit (for example, 100 ₽) so you stay inside the free quota.
- Configure alerts on spending thresholds — for example, 50% and 90% of the limit.
- State the notification channel (email / Telegram) and explain why several thresholds are needed.
- Take a screenshot of the configured budget and insert it into the report.

**Checkpoint 4.** Check that the alerts fire before the limit is exhausted, not after.

### Stage 5. Cost optimization

- **Rightsizing.** For each VM, assume a realistic CPU/RAM utilization, use the calculator to pick the optimal size, and calculate the savings.
- **Auto-scaling.** Describe how the web server group should scale (metric, thresholds, min/max instances) and at which hours the instance count changes.
- **Intelligent tiering.** For object storage, decide which data stays in Standard, which goes to Standard-IA and which to Glacier, and explain the logic.

**Checkpoint 5.** Check that every optimization decision comes with a calculation from the calculator.

### Stage 6. Writing up the report
- Fill in the report using the template (section 5), attach the calculation tables, the screenshots of the demo VM and the budget, and the self-check list (section 6).
- Be sure to delete the demonstration VM at the end of the work and record that in the report.

## 5. Report

Write the report in the following structure (as a text document):

### 5.1. Justification of the pricing model choices

Give the table of services and the pricing model for each, justified through the load profile.

| Service | Load profile | Pricing model | Justification |
| --- | --- | --- | --- |
| Web frontend | Steady baseline + peaks | Reserved + On-Demand | Baseline on Reserved, peaks on On-Demand |
| API backend | Proportional to the frontend |  |  |
| Database | Runs 24/7 |  |  |
| Test environment | Irregular, working hours |  |  |
| Object storage | Rare access to old data |  |  |

### 5.2. Cost calculation table (simulation)

Give the On-Demand vs chosen model comparison for each VM.

| VM | On-Demand (₽/month) | Chosen model (₽/month) | Savings (₽) | Savings (%) |
| --- | --- | --- | --- | --- |
| VM-1 (web) |  |  |  |  |
| VM-2 (db) |  |  |  |  |
| VM-3 (test) |  |  |  |  |
| Total |  |  |  |  |

### 5.3. Cost allocation by tags

Give the tag table for all services.

| Resource | env | project | team | cost_center |
| --- | --- | --- | --- | --- |
| VM-1 (web) | prod | shop | web | eCommerce |
| VM-2 (db) | prod | shop | db | eCommerce |
| VM-3 (test) | test | shop | qa | R&D |

### 5.4. Budgets and alerts

Describe the budget setup: limit, thresholds, notification channel, and insert the screenshot.

### 5.5. Cost optimization

Give the rightsizing calculations, the auto-scaling description and the distribution of data across storage classes.

### 5.6. Conclusions

What you took away, which mistakes you avoided, what was new to you about FinOps.

## 6. Self-check list (before submitting)

Go through the list and tick every item:

- ☐ Every service has a chosen and justified pricing model (On-Demand / Reserved / Spot).
- ☐ The cost calculation was done on the calculator and the amounts match the table.
- ☐ The savings are calculated correctly (₽ and %), not "pulled out of thin air".
- ☐ All resources carry the tags env, project, team, cost_center.
- ☐ It is explained what happens to a resource with no tag (the "black hole").
- ☐ The demo VM was created and tagged, and the screenshot is in the report.
- ☐ A budget with threshold alerts was created, and the screenshot is in the report.
- ☐ Rightsizing was calculated on the calculator for every VM.
- ☐ Auto-scaling is described (metric, thresholds, min/max).
- ☐ Storage classes are assigned with a cost-difference calculation.
- ☐ The demonstration VM was deleted after the work (recorded in the report).
- ☐ The total project savings are calculated and conclusions are drawn.

## 7. Review questions (after the work)

Answered orally at the defense or in writing at the end of the report.

- What is the difference between On-Demand, Reserved and Spot? Which model suits a 24/7 database, and why?
- How does the "baseline — Reserved, peaks — Spot" principle work? Give an example workload.
- What are tags and projects for in cost allocation? What does the report look like without the mandatory tags?
- Which monitoring tool suits AWS, which suits Azure, which suits Yandex Cloud? What are their key features?
- What is rightsizing and what is the main rule when picking a VM size?
- How does auto-scaling help save money on an unstable load?
- What does S3 Intelligent-Tiering do, and why doesn't it need manual management?
- Name the three phases of the FinOps cycle and what happens in each.
- What is the danger of a "forgotten" test VM left running overnight on On-Demand?
- Why is cost allocation a prerequisite for any optimization?
- Where in this work was the calculator simulation used, and where was it real practice on a free account? Why is this approach safe for the budget?

## 8. Grading criteria

| Points | Criterion |
| --- | --- |
| 1 | A pricing model is chosen and justified correctly for every service |
| 1 | The cost calculation was done on the calculator, the amounts match, savings are computed |
| 1 | All resources are tagged and the role of tags in cost allocation is explained |
| 1 | A budget with threshold alerts is configured and the screenshot is attached |
| 1 | Optimization (rightsizing, auto-scaling, tiering) is justified and calculated |
| 1 | The demo VM is deleted, the report and checklist are filled in, conclusions are drawn |
| 6 | Maximum total |
