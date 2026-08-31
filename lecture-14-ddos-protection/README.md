# Lecture 14 — Protection from DDoS and network attacks

> 👥 **Lecture by the co-instructor. Materials in preparation.** For now this is only the topic outline from the course programme. Once the author sends the lecture text and the lab, they will go here like their other lectures.
>
> ⚠️ **The title is being confirmed with the author.** The programme calls the topic "Cloud databases", but every outline item is about attack protection. The heading here follows what the outline actually covers; if the author did mean databases, the topic needs rewriting.

## Topic outline

- Protection from DDoS and network attacks in the cloud
- Cloud protection services: AWS Shield (Standard / Advanced), Azure DDoS Protection, Yandex DDoS Protection
- Architectural strategies: rate limiting, geo-distribution via CDN/Anycast, isolation via VPC, automatic scaling
- WAF and application-level filtering: OWASP Top 10, correct filtering rules, bot management
- Monitoring and response: CloudWatch, GuardDuty, SIEM integration, automated playbooks (autoscaling + trigger-based blocking)

## To settle while preparing this

The topic overlaps with [lecture 7](../lecture-7-cloud-security/): WAF, OWASP Top 10 and application-level filtering are already covered there. The split needs agreeing — for example, lecture 7 keeps the filtering layers and rules, while volumetric attacks, Anycast/CDN and incident response move here.
