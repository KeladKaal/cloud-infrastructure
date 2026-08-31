# Lab 5 — Cloud storage (Block, File, Object: choosing and applying)

> 👥 **Lab by the co-instructor.** Below is the author's original assignment, translated into English; the content is unchanged.
>
> **Numbering.** For the author this is lab No. 2, and the text refers to their own numbers. Mapping: No. 1 → Lab 3, No. 2 → Lab 5, No. 3 → Lab 7, No. 4 → Lab 9, No. 5 → Lab 11.
>
> **Note: the submission rules here are different.** The assignment asks for a .docx/.pdf report, pair work and a 6-point score — that is the co-instructor's format for the second half of the course. Labs in the first half are submitted differently. What applies on this course is in [Course rules](../COURSE-RULES.md); until that is agreed, check the submission format with your instructor.

---

## 1. Goal

Put into practice what lecture No. 2 "Cloud data storage" covered:

- tell the three cloud storage types apart — Block, File and Object — and understand their levels;
- choose and apply the EBS, EFS and S3 services (and their equivalents at other providers);
- account for the trade-off between performance and cost (IOPS, Throughput, Latency);
- build a sound storage architecture as a mix of different types;
- apply the selection checklist and avoid the common mistakes.

The output is a justified data storage design for a notional online store, plus a project report.

## 2. Format

| Parameter | Value |
| --- | --- |
| Working mode | Individually or in a pair (2–3 people) |
| Time | 2 academic hours (80 minutes) |
| Environment | A cloud console (AWS / Yandex Cloud / VK Cloud) or a diagramming tool |
| Submission | A report file in .docx or .pdf + a schematic file |

## 3. Inputs (the problem statement)

You are a DevOps engineer at the online bookstore "Книжный мир" (Book World). The customer asks you to design the data storage architecture for the same web application whose network you already designed (lab work No. 1).

Customer requirements:

| Service | Purpose | Storage specifics |
| --- | --- | --- |
| Web frontend | Public site + static content | Static assets (images, styles, scripts) served through a CDN, traffic spikes possible |
| API backend | Order logic, catalogue | Runs on virtual machines, needs a fast system disk |
| Database | Stores the catalogue and orders | High load, predictable latency is critical (IOPS), snapshots required |
| Shared directory | Content for several servers | Several servers must see one directory at the same time |
| Backups and archive | Petabytes of backup copies | Multi-year retention at minimal cost, protection against accidental deletion |

Resilience and cost requirements:

- every kind of data lives in the storage class that suits it — no overpaying;
- there must be no Single Point of Failure: backups and snapshots must allow recovery in any zone;
- static content and archives must get cheaper as the data "cools down" (lifecycle).

## 4. Assignment and steps

### Stage 1. The three storage types and a comparison (15 minutes)

- Justify which of the three types (Block, File, Object) suits each service in the customer's table, and fill in the comparison table for the three AWS services — EBS, EFS, S3 (template in the "Report" section).
- State the pattern: how performance and cost change going from Block to Object.
- Fill in the table of EBS, EFS and S3 equivalents at Yandex Cloud, VK Cloud, Azure and Google Cloud.

**Checkpoint 1:** every service is matched to the right storage type, and there is no overpaying anywhere in the system.

### Stage 2. AWS services: EBS, EFS, S3

- For the database, choose an EBS volume type and justify it: gp3 (baseline), io2/io1 (provisioned IOPS), st1/sc1 (cheap HDD) — what to take and when.
- Plan volume snapshots for disaster recovery. State where an EBS snapshot is stored — in an availability zone or in the region — and why that matters for recovery in any zone.
- For the web frontend, set up object storage for static content plus a CDN.
- For the shared directory, choose file storage and describe how several servers mount it at once (protocol, shared mount).
- For backups and the archive, decide on storage classes and lifecycle rules (moving an object between classes by access frequency).

💡 Hint from the lecture: "Block — for speed, File — for shared access, Object — for scale and money".

**Checkpoint 2:** the database isn't on object storage, the shared directory isn't on a block disk, and archives get cheaper via the lifecycle.

### Stage 3. S3 storage classes and lifecycle (15 minutes)

- Fill in the table of S3 storage classes from "hot" to "cold" (Standard, Intelligent-Tiering, Standard-IA, Glacier, Glacier Deep Archive): purpose, access speed, cost.
- Write a lifecycle rule for backups: after how many days an object moves to Standard-IA, after how many to Glacier. Justify the savings.
- Describe how S3 Select and Glacier Select work (SQL queries inside an object) and what they are for.
- Enable versioning on the backup bucket and explain what it protects against.

**Checkpoint 3:** the lifecycle rule is described correctly and versioning is justified.

### Stage 4. Storage architecture and security

- Produce the final data storage diagram for "Book World" (a block diagram).
- Go through the 6-question storage selection checklist and apply it to each kind of data.
- Describe how redundancy is achieved: where EBS snapshots live, across how many zones EFS is spread, how S3 replicates objects between zones.

**Checkpoint 4:** every kind of data has a defined storage type and class, and redundancy is justified.

### Stage 5. Writing up the report

Fill in the report using the template (section 5), attach the storage diagram and the self-check list (section 6).

## 5. Report

Write the report in the following structure:

### 5.1. Comparison of the three services

| Parameter | EBS (Block) | EFS (File) | S3 (Object) |
| --- | --- | --- | --- |
| Level | Low (blocks) | Medium (files + folders) | High (objects/HTTP) |
| Unit of data | Block | File | Object (key + metadata) |
| Consumers | 1 EC2 | N EC2 | Unlimited (HTTP API) |
| Protocol | NVMe | NFSv4 | HTTPS / S3 API |
| Scaling | Manual (volume) | Automatic | Petabytes+ |
| Cost per GB | High | Medium | Low |
| Latency | Minimal | ~10 ms | Depends on class |
| Redundancy | Snapshot (in the region) | Many AZs | Replication within the region |

### 5.2. Storage type assignment table

| Customer's service | Storage type | Service (AWS) | Class / type | Justification |
| --- | --- | --- | --- | --- |
| Web frontend (static) | Object | S3 | Standard → CDN | Scale, low cost, CDN |
| Virtual machines (API) | Block | EBS | gp3 | Fast system disk |
| Database | Block | EBS | io2 | Guaranteed IOPS |
| Shared directory | File | EFS | General Purpose | Several servers, NFS |
| Backups and archive | Object | S3 | Standard-IA / Glacier | Lifecycle, savings |

### 5.3. Equivalents at other providers

| AWS | Yandex Cloud | VK Cloud | Azure | Google Cloud |
| --- | --- | --- | --- | --- |
| EBS (Block) | Compute Disks | Volumes | Managed Disks | Persistent Disk |
| EFS (File) | Filestorage | File storage | Azure Files | Filestore |
| S3 (Object) | Object Storage | Cloud Storage | Blob Storage | Cloud Storage (GCS) |

### 5.4. S3 storage classes

| Class | Purpose | Access speed | Cost |
| --- | --- | --- | --- |
| S3 Standard | Hot data, frequent access | Instant | Highest |
| S3 Intelligent-Tiering | Automatic movement between tiers | Instant | Medium (dynamic) |
| S3 Standard-IA | Infrequently read data | Instant (access fee) | Lower |
| S3 Glacier | Archive | A few minutes | Low |
| S3 Glacier Deep Archive | Cheapest archive | Up to 12 hours | Minimal |

### 5.5. Data storage diagram

Insert the architecture block diagram: static content in S3 + CDN, VMs on EBS gp3, the database on EBS io2, the shared directory on EFS, backups in S3 with lifecycles.

### 5.6. Lifecycle rule

Give the rule you wrote (for example: "not read for 30 days → Standard-IA; 90 days → Glacier") and justify the savings.

### 5.7. Conclusions

What you took away, which mistakes you avoided, what was new to you.

## 6. Self-check list (before submitting)

- ☐ Every kind of data has the right storage type (Block / File / Object).
- ☐ The database is on block storage with guaranteed IOPS (EBS io2), not on object storage.
- ☐ Static content is on object storage + CDN, and root disks are on EBS gp3.
- ☐ The shared directory for several servers is on file storage (EFS/NFS).
- ☐ Backups are on object storage with the Standard-IA/Glacier classes.
- ☐ The EBS snapshot is stored in the region (recovery in any AZ) — stated in the report.
- ☐ The lifecycle rule is written out and the savings are justified.
- ☐ Versioning is enabled on the backup bucket.
- ☐ Equivalents of EBS, EFS and S3 at other providers are listed (Yandex, VK, Azure, Google).
- ☐ The storage diagram and the tables are attached.
- ☐ Resources (if a real cloud environment was used) have been deleted after the work.

## 7. Review questions (after the work)

- Who builds the file system on top of a block disk — the storage or the OS? Give examples of file systems.
- Why can't one block disk be attached to several VMs at once, while file storage can?
- Where is an EBS snapshot stored — in an availability zone or in the region?
- List the S3 storage classes from hot to cold.
- How do S3 Select and Glacier Select make working with data easier?
- Name the equivalents of EBS, EFS and S3 in Yandex Cloud and VK Cloud.
- State the storage selection rule in one sentence.

## 8. Grading criteria

| Points | Criterion |
| --- | --- |
| 1 | A correct comparison table of storage types is provided, along with the "level → performance → price" pattern |
| 1 | Database and VMs: the EBS volume type (gp3 / io2) is chosen correctly and snapshots are planned (in the region) |
| 1 | Storage is chosen and configured correctly for static content (S3 + CDN), the shared directory (EFS) and root disks |
| 1 | A backup and archive scheme is in place: storage classes, lifecycle, versioning |
| 1 | A mixed architecture and redundancy are thought through (snapshots, many zones, S3 replication) |
| 1 | A data storage diagram is produced and the report is written up |
| 6 | Maximum total |
