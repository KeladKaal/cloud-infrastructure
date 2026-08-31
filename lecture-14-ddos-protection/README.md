# Lecture 14 — Protection from DDoS and network attacks

How a cloud service survives an attack: what absorbs it, where it gets filtered, and how you notice in time.

## Lecture outline

- Protection from DDoS and network attacks in the cloud
- Cloud protection services: AWS Shield (Standard / Advanced), Azure DDoS Protection, Yandex DDoS Protection
- Architectural strategies: rate limiting, geo-distribution via CDN/Anycast, isolation via VPC, automatic scaling
- WAF and application-level filtering: OWASP Top 10, correct filtering rules, bot management
- Monitoring and response: CloudWatch, GuardDuty, SIEM integration, automated playbooks (autoscaling + trigger-based blocking)
