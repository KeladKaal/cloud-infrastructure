# Lecture 14 — Cloud databases

> 👥 **Lecture by the co-instructor. Materials in preparation.** For now this is only the topic outline from the course programme. Once the author sends the lecture text and the lab, they will go here the same way as their other lectures — the author's text unchanged.
>
> ⚠️ **The title needs confirming with the author.** The programme calls the topic "Cloud databases", but every outline item is about protection from DDoS and network attacks. The title is reproduced as it appears in the programme; what the lecture will actually cover needs to be clarified.

## Topic outline

- Protection from DDoS and network attacks in the cloud
- Cloud protection services: AWS Shield (Standard / Advanced), Azure DDoS Protection, Yandex DDoS Protection
- Architectural strategies: rate limiting, geo-distribution via CDN/Anycast, isolation via VPC, automatic scaling
- WAF and application-level filtering: OWASP Top 10, correct filtering rules, bot management
- Monitoring and response: CloudWatch, GuardDuty, SIEM integration, automated playbooks (autoscaling + trigger-based blocking)

## To settle while preparing this

- If the topic really is about **attack protection**, it overlaps with [lecture 7](../lecture-7-cloud-security/): WAF, OWASP Top 10 and application-level filtering are already covered there. The split needs to be agreed.
- If the topic is in fact about **cloud databases** (managed services, RDS and equivalents), it overlaps with [lecture 6](../lecture-6-databases/) — PostgreSQL/MySQL, replication, backups and migrations.
