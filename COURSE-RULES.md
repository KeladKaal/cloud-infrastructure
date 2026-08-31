# Course rules

> ⚠️ **Draft — being agreed between the instructors.** This course is assembled from two halves that historically had different rules for accepting work. Below is an honest account of what matches, what differs, and what still needs to be settled. **Until it is settled, go by what the instructor of your lab says.**

Labs come in two kinds:

- **hands-on** — labs 2, 4, 6, 8, 10, 12 (you stand the system up yourself, locally);
- **design** — labs 3, 5, 7, 9, 11 (you design and justify an architecture). Lectures 13 and 14 belong to this half too, but their materials are still in preparation.

## What is the same in both halves

- A lab is an **assignment**, not a step-by-step recipe: you make the engineering decisions yourself and **justify** them.
- The report always has to explain **why** something was done, not just **what** was done.
- **Submission is Markdown only.** The report is an `.md` file, schematics and diagrams go in as images or Mermaid blocks, screenshots as images alongside. No `.docx`, no `.pdf`.
- **The assignments no longer fix the time and place of work** — those are set by the timetable.

## Where we currently differ

| Rule | Hands-on labs (2, 4, 6, 8, 10, 12) | Design labs (3, 5, 7, 9, 11) |
| --- | --- | --- |
| **Cloud account needed** | No | Yes — except lab 9, where the numbers come from a pricing calculator and only one demo VM is created |
| **Working mode** | Individually; teaming up is allowed if one machine's resources aren't enough | Individually or in a pair (the lab texts say "in a pair (2–3 people)") |
| **Cleaning up resources** | Not applicable — everything runs locally | "Resources have been deleted after the work" is on the checklist |
| **Monitoring** | Mandatory in every lab: your own dashboard + 3 metrics for alerts, with a rationale | Not required |
| **Grading** | Criteria not formalized in the repository | A criteria table, maximum 6 points per lab |
| **Defense** | None | Review questions — orally at the defense or in writing at the end of the report |
| **Self-check list** | None | Yes, to go through before submitting |
| **What you attach to the report** | Code, configs, screenshots | Schematics, calculation tables, policies |
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

- [ ] **A single grading scale**, and how all the labs add up to a final course grade.
- [ ] **Deadlines and late work.** Due dates and what happens if you're late — currently written down nowhere.
- [ ] **Resubmission.** Whether work can be fixed up after feedback, and how many times.
- [ ] **Cloud accounts.** Which provider is the default, who pays for resources, what students without AWS access should do, and whether resources really have to be created — or a design on paper is enough.
- [ ] **Pair work.** Whether it is allowed in the hands-on labs, and how each person's contribution is assessed.
- [ ] **Where and when labs are done.** This has been taken out of the assignments. It needs deciding from the timetable which labs happen in class and which at home, so the workload stays even.
- [ ] **What demonstrates the work is the student's own in the design labs.** The assistant is allowed to write answers into the report once the student has shown they understand them (see [`AGENTS.md`](AGENTS.md)), but the report itself doesn't show that. Decide whether a defense or oral questions are needed.
- [ ] **The topic of lecture 14.** In the repository it is named after what the outline covers — "Protection from DDoS and network attacks". The programme says "Cloud databases". Confirm with the author which of the two was meant.
- [ ] **The name of the course.** The repository calls it "Cloud Infrastructure"; the original lecture texts call it "Organization and management of cloud infrastructure".
- [ ] **The running example.** Design labs follow the "Book World" online store, hands-on labs follow a grocery delivery service. Decide whether to unify them.

All of this is covered in **lecture 1**, which is the organizational one. If anything is still unclear afterwards, ask before your first submission, not after.
