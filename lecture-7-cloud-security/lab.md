# Lab 7 — Cloud infrastructure security

## 1. Goal

Put into practice what lecture 7 "Cloud infrastructure security" covered:

- apply the Shared Responsibility model (the split of responsibility between provider and customer);
- set up access management (IAM): roles, policies, service accounts, the principle of least privilege;
- organize secure secret storage (Vault / AWS Secrets Manager) with rotation;
- ensure data encryption in-transit (TLS) and at-rest (disks, S3, databases) via KMS;
- implement multi-layer network protection: Security Groups, Network ACL, WAF;
- avoid the common mistakes listed in the lecture and document the result.

The output is a hardened cloud environment for a notional service, plus a security report.

## 2. Format

| Parameter | Value |
| --- | --- |
| Working mode | Individually or in a pair (1–2 people) |
| Submission | A report in Markdown (`.md`) committed to a git repository, + a schematic file |

## 3. Inputs (the problem statement)

You are a DevOps engineer at the online bookstore "Книжный мир" (Book World) (you could invent the company and what it does yourself, or pick from the list in Lab 1). After the network architecture was deployed (Lab 1), the customer asks you to secure that cloud environment.

The following infrastructure components are available to design against:

| Component | Purpose | Security requirement |
| --- | --- | --- |
| Web frontend | The store's public site | HTTPS (TLS 1.2/1.3), protection against L7 attacks (SQLi, XSS) |
| API backend | Order logic, catalogue | Access only from the internal network, IAM roles with minimal rights |
| Database | Stores the catalogue and orders | Not reachable from the internet, encryption at rest, secret in a vault |
| Internal services | Backups, telemetry, CI/CD | A service account with roles, key rotation, secrets in a manager |
| S3 bucket | Stores static content and artifacts | Object encryption, a narrowly scoped access policy |

The customer's security requirements:

- Access to services goes through IAM roles and service accounts, with no permanent root account in production.
- All secrets (database passwords, keys, tokens) are stored only in a secrets manager, never in code.
- All data is encrypted: in-transit (TLS) and at-rest (disks, S3, databases) via KMS.
- Traffic is filtered at three levels: WAF → Network ACL → Security Group.
- Auditing is in place: logging of IAM actions, secret access, and key usage.

## 4. Assignment and steps

### Stage 1. The Shared Responsibility model

**Task 1.1.** Build a Mermaid diagram of the shared responsibility model. Lay it out as two independent sub-graph branches:

- **Provider** (Security OF the cloud): physical data centre security, hypervisor and virtualization, hardware/networks/base software, the physical perimeter.
- **Customer** (Security IN the cloud): data and its classification, access (IAM, passwords, keys), Security Group and NACL configuration, encryption, application configuration, OS patching on virtual machines.

Requirements: a node inside a subgraph is written as ["Text · Text"]; write every `---` edge **on its own line** (chains like A --- B --- C are not supported).

**Task 1.2 (written).** Determine where the responsibility boundary runs (at the hypervisor level). Describe the customer's and the provider's areas of responsibility for three services: IaaS (EC2), PaaS (RDS), SaaS (Office 365). Answer whether the customer ever hands off responsibility for their own data.

**Task 1.3 (case).** An attacker reached a database in a public subnet through a misconfigured Security Group. Who is responsible for the breach — the provider or the customer? Argue with reference to the Shared Responsibility model.

**Checkpoint 1:** the diagram renders without errors; the "hypervisor" boundary and responsibility for data are described correctly.

### Stage 2. Access management (IAM)

**Task 2.1.** Write a correct JSON policy for a service account. Requirements:

- allow only s3:PutObject and s3:GetObject;
- scope it strictly to the bucket my-backup-bucket;
- constrain it with a condition on the IP range 10.0.0.0/24;
- deny the right to delete objects.

The policy must follow the principle of least privilege.

**Task 2.2.** Fill in the table and describe the difference between the IAM entities:

| Entity | Who this is | How identity is proven |
| --- | --- | --- |
| User (IAM User) |  |  |
| Service account |  |  |
| Role (IAM Role) |  |  |

**Task 2.3.** State the least privilege principle in your own words. For the task "the CI server needs to publish files to an S3 bucket", give a "correct" and a "bad" way of configuring access.

**Task 2.4.** List 5 common IAM mistakes from the lecture. For each, state the consequence and the preventive measure.

**Checkpoint 2:** the JSON policy is valid, the service account has no admin rights, and there is no `*` in the resources.

### Stage 3. Secrets management

**Task 3.1.** Fill in the Vault vs AWS Secrets Manager comparison table:

| Criterion | HashiCorp Vault | AWS Secrets Manager |
| --- | --- | --- |
| Platform / cloud lock-in |  |  |
| Dynamic secrets (TTL) |  |  |
| Automatic rotation |  |  |
| Unseal / self-management |  |  |
| Cost and administration |  |  |
| IAM integration |  |  |

In the conclusion, state which tool to pick for a pure AWS stack and which for a multi-cloud environment.

**Task 3.2 (checklist).** Write 5 checklist items for handling secrets safely, based on the lecture. Underline the item that forbids storing secrets in code and git.

**Task 3.3 (case).** The production database password was committed to a repository and became public. Describe, step by step, what to do to limit the damage and prevent a repeat (rotation, moving it to a vault, restricting access, auditing).

**Checkpoint 3:** the secret has moved into a manager and is encrypted; access to it is restricted by an IAM policy.

### Stage 4. Encryption (in-transit / at-rest, KMS)

**Task 4.1.** Explain the difference between in-transit and at-rest encryption. Give two examples of each (TLS for traffic; EBS volumes, S3 objects, backups for data at rest).

**Task 4.2.** Describe the envelope encryption scheme in KMS (3–4 sentences): how the Data Key is generated, what encrypts it, where the master key (CMK) is stored, and why it never leaves KMS/HSM.

**Task 4.3.** Fill in the "what we encrypt and how" table:

| What we encrypt | With what / how | Service |
| --- | --- | --- |
| Traffic (network) | TLS/HTTPS | ALB, Route53 |
| Virtual machine disks |  |  |
| Objects in S3 | SSE-S3 / SSE-KMS |  |
| Databases |  |  |
| Backups |  |  |

**Task 4.4.** List 4 common encryption mistakes from the lecture and explain the danger of each.

**Checkpoint 4:** encryption is enabled both for traffic (TLS) and for data at rest (EBS, S3, database); the master key is in an HSM.

### Stage 5. Network protection: SG, NACL, WAF

**Task 5.1.** Fill in the comparison table:

| Property | Security Group | Network ACL |
| --- | --- | --- |
| Operating level |  |  |
| Stateful / Stateless |  |  |
| Rule type | Allow only | Allow + Deny |
| Rule evaluation order | doesn't matter | by number/priority |

**Task 5.2.** Build a Mermaid `flowchart LR` diagram of how traffic passes through the three levels (WAF → NACL → Security Group → Application). Label the operating level of each element (L7 / subnet / instance).

**Task 5.3 (case).** You need to open port 443 for the public web server and port 22 only for admins from the internal network 10.0.1.0/24. Write the Security Group rules (in words, in the format from the lecture).

**Task 5.4.** List the 3 main application-level threats a WAF protects against (SQL Injection, XSS and others) and explain why an ordinary Security Group can't block them.

**Checkpoint 5:** three filtering layers are created, the least-access principle is respected, and the database is not reachable from the internet.

### Stage 6. Writing up the report

Fill in the report using the template from Lab 1.

## 5. Report

The report must include:

### 5.1. The Shared Responsibility model

Insert the Mermaid diagram of the two responsibility branches and describe the "hypervisor" boundary.

### 5.2. Subnet and network filter table

| Zone / component | Subnet / level | Filter type | Stateful/Stateless | Key rule |
| --- | --- | --- | --- | --- |
| Web frontend | public | Security Group | stateful | allow TCP/443 |
| Admin access | — | Security Group | stateful | allow TCP/22 from 10.0.1.0/24 |
| Database | data | Security Group + NACL | stateful + stateless | only from API |
| The whole VPC | subnet | Network ACL | stateless | allow + deny by priority |
| Application | L7 | WAF | — | block SQLi / XSS |

### 5.3. IAM policies

Insert the final JSON policy from stage 2 and the IAM entity table.

### 5.4. Traffic protection diagram

Insert the Mermaid diagram WAF → NACL → Security Group → Application.

### 5.5. Secrets and encryption

Give the Vault vs Secrets Manager comparison table, the "what we encrypt and how" table, and describe the envelope encryption scheme.

### 5.6. Conclusions

What you took away, which mistakes you avoided, what was new to you.

## 6. Self-check list (before submitting)

Go through the list and tick every item:

- ☐ The Mermaid diagrams are correct (every edge on its own line, labels in ["..."]).
- ☐ The responsibility boundary in the Shared Responsibility model is identified correctly (the hypervisor).
- ☐ The IAM policy follows least privilege (no `*`, no Admin rights).
- ☐ The service account has no right to delete objects.
- ☐ Secrets are not stored in code / git and have been moved into Vault or Secrets Manager.
- ☐ Rotation is configured for the secrets and access is restricted via IAM.
- ☐ Encryption in-transit (TLS 1.2/1.3) and at-rest (EBS, S3, database) is enabled via KMS.
- ☐ The database is not reachable from the internet.
- ☐ All three protection layers are created: WAF → NACL → Security Group.
- ☐ Security Group and Network ACL are used for their proper purposes (stateful vs stateless).
- ☐ Logging / auditing (IAM, secret access, key usage) is described.
- ☐ Resources (if a real cloud environment was used) have been deleted after the work.

## 7. Review questions (after the work)

Answered orally at the defense or in writing at the end of the report.

- Who owns "security of the cloud" and who owns "security in the cloud"? Where does the boundary run in the Shared Responsibility model?
- What is an IAM policy, and what is the point of the "explicit Deny" rule (deny overrides allow)?
- What is the difference between an IAM user and a service account? When is each used?
- What does the principle of least privilege mean and how does it apply to service accounts?
- Why can't secrets be kept in code? What problems do Vault and Secrets Manager solve?
- What is the difference between in-transit and at-rest encryption? Give examples.
- How does envelope encryption work in KMS? Where is the master key (CMK) physically stored?
- How does a Security Group differ from a Network ACL (stateful vs stateless)?
- What is a WAF and which application-level attacks does it protect against?
- Lay out the order in which traffic passes through WAF → NACL → Security Group.
