# Course rules

> ⚠️ **Draft — being agreed between the instructors.** This course is assembled from two halves that historically had different rules for accepting work. Below is an honest account of what matches, what differs, and what still needs to be settled. **Until it is settled, go by what the instructor of your lab says.**

Labs come in two kinds:

- **hands-on** — labs 2, 4, 6, 8, 10, 12 (you stand the system up yourself, locally);
- **design** — labs 3, 5, 7, 9, 11 (you design and justify an architecture).

## What is the same in both halves

- A lab is an **assignment**, not a step-by-step recipe: you make the engineering decisions yourself and **justify** them.
- The report always has to explain **why** something was done, not just **what** was done.

## Where we currently differ

| Rule | Hands-on labs (2, 4, 6, 8, 10, 12) | Design labs (3, 5, 7, 9, 11) |
| --- | --- | --- |
| **What you submit** | A repository: code, `README.md`, configs, screenshots | A report file in `.docx` or `.pdf` + a schematic file |
| **Where you work** | Locally and for free: Docker / `docker compose` or Kubernetes | A cloud console (AWS / Yandex Cloud / VK Cloud) or a diagramming tool |
| **Cloud account needed** | No | Yes — except lab 9, where the numbers come from a pricing calculator and only one demo VM is created |
| **Working mode** | Individually; teaming up is allowed if one machine's resources aren't enough | Individually or in a pair (the lab texts say "in a pair (2–3 people)") |
| **When you do it** | Not specified in the materials | In class, 2 academic hours (80 minutes in labs 3, 5, 9, 11; 90 in lab 7) |
| **Cleaning up resources** | Not applicable — everything runs locally | "Resources have been deleted after the work" is on the checklist |
| **Monitoring** | Mandatory in every lab: your own dashboard + 3 metrics for alerts, with a rationale | Not required |
| **Grading** | Criteria not formalized in the repository | A criteria table, maximum 6 points per lab |
| **Defense** | None | Review questions — orally at the defense or in writing at the end of the report |
| **Self-check list** | None | Yes, to go through before submitting |
| **Using AI** | Allowed and encouraged; the assistant tutors step by step per [`AGENTS.md`](AGENTS.md) | Not addressed in the assignment text |
| **What counts as the result** | A working system you can demonstrate | A justified design and its documentation |

## The design labs form a chain

The design labs are linked: labs 5, 7 and 11 build on the architecture designed in **lab 3** (the "Book World" store). If lab 3 isn't done, there is nothing for the later ones to build on — so don't put it off.

Numbering inside the original texts is the **author's own**. The mapping:

| In the assignment text | In this course |
| --- | --- |
| Lab work No. 1 | Lab 3 |
| Lab work No. 2 | Lab 5 |
| Lab work No. 3 | Lab 7 |
| Lab work No. 4 | Lab 9 |
| Lab work No. 5 | Lab 11 |

The same holds for lectures: "lecture No. 1" in the author's text is lecture 3 of this course, No. 2 → 5, No. 3 → 7, No. 4 → 9, No. 5 → 11.

## Still to be agreed

The points below are not settled yet. They don't stop you from starting, but they affect how work will be accepted.

- [ ] **A single submission format.** Either bring both halves to one medium (e.g. everyone submits a folder or a repository, with the report inside as `README.md` or an attached `.pdf`), or deliberately keep the two.
- [ ] **A single grading scale**, and how 11 labs add up to a final course grade.
- [ ] **Deadlines and late work.** Due dates and what happens if you're late — currently written down nowhere.
- [ ] **Resubmission.** Whether work can be fixed up after feedback, and how many times.
- [ ] **Cloud accounts.** Which provider is the default, who pays for resources, what students without AWS access should do, and whether resources really have to be created — or a design on paper is enough.
- [ ] **Pair work.** Whether it is allowed in the hands-on labs, and how each person's contribution is assessed.
- [ ] **Where labs are done.** Design labs assume class time, hands-on labs assume homework; this needs to be reconciled with the timetable so the workload stays even.
- [ ] **AI policy for the design labs.** They largely consist of filling in tables and answering questions — exactly what an assistant does in a minute. We need to decide what demonstrates the work is the student's own: a defense, oral questions, or something else.
- [ ] **Report format with diagrams.** Lab 7 asks for Mermaid diagrams, but submission is `.docx`, where Mermaid doesn't render. Either the report moves to Markdown, or the diagrams are submitted as images.
- [ ] **The name of the course.** The repository calls it "Cloud Infrastructure"; the original lecture texts call it "Organization and management of cloud infrastructure".
- [ ] **The running example.** Design labs follow the "Book World" online store, hands-on labs follow a grocery delivery service. Decide whether to unify them.

All of this is covered in **lecture 1**, which is the organizational one. If anything is still unclear afterwards, ask before your first submission, not after.
