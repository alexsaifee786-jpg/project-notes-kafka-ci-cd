# 08 · Monitoring, Metrics & Alerts

_Read the signal correctly • Explain the outage exercise • Know current coverage_

## 🗣️ Say it in the interview

> “I exposed health and Prometheus metrics through Actuator. Prometheus scrapes Inventory Service every five seconds, and Grafana uses those metrics for monitoring and service-down alerts. I exercised the alert behavior locally. The current Compose configuration keeps SMTP disabled unless I explicitly configure it.”

## WHO DOES WHAT?

- **Actuator** exposes operational endpoints. **Micrometer** provides application/JVM meters. **Prometheus** collects time-series samples. **Grafana** queries and visualizes them, and evaluates configured alerts.
- Both services expose **/actuator/health** and **/actuator/prometheus**. The committed scrape configuration targets only **Inventory :8082** every **5s**.

## THE AVAILABILITY SIGNAL

- **up=1:** the latest target scrape succeeded. **up=0:** it failed. A failed scrape can mean the app, network or metrics endpoint is unavailable.
- Example query: **up{job="inventory-service"}**. Use the same job label when tracing the alert condition.
- A service-stop exercise should show failed scrapes, a firing alert after its configured evaluation/pending period, then recovery after successful scrapes resume. Five-second scraping is not a guaranteed five-second notification.

## WHAT I CAN SHOW FROM THIS PROJECT

- Committed captures include Prometheus target-up, a Grafana dashboard folder and separate Firing/Normal alert states. They support the local monitoring walkthrough.
- The dashboard image shows the folder listing, not panel graphs. Separate alert-rule identifiers are not proof of one correlated firing-to-recovery history.
- Grafana UI configuration persists in **grafana-data**. Dashboard/alert provisioning files are not committed; a fresh machine needs that setup.

## HOW I ANSWER AN OPERATIONS FOLLOW-UP

- “I check health, scrape status and logs first, then trace the affected event through the databases and Kafka flow. A healthy process can still have a stuck business workflow.”
- **Future additions:** Order-service scraping, outbox backlog/age, DLT rate and consumer-lag monitoring. These are useful next signals, not claimed current dashboards.
- Alert evaluation and email delivery are separate. **GF_SMTP_ENABLED=false** is the default; an email contact-point label does not prove message delivery.

> **Recall:** YAAD RAKHO: process health, scrape health and business completion are different.

**Verified source:** [monitoring/prometheus.yml + Compose + README monitoring/evidence chapters](https://github.com/alexsaifee786-jpg/kafka-order-platform/blob/main/monitoring/prometheus.yml)

Personal hands-on project. Source inspection on 29 Sep 2026; no new runtime test claimed.
