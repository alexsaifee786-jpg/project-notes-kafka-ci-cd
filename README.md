# 📘 Kafka Order Platform · Interview Notes

**17 chapters · Simple English answers · Hinglish recall cues · Verified project details**

Production-style interview preparation based on the existing personal Kafka Order Platform. The implementation repository remains separate.

Start with [the complete PDF](Kafka_Order_Platform_Interview_Notes.pdf), or open one chapter below. Each chapter owns its detailed concepts; later references connect the workflow without redefining the topic.

## Chapter index

| # | Topic | Read | Download |
|---|---|---|---|
| 01 | Project Overview & Architecture | [Markdown](notes/01-Project_Overview_Architecture.md) | [PDF](chapters/Chapter_01_Project_Overview_Architecture.pdf) |
| 02 | Kafka Cluster, Topics & Partitions | [Markdown](notes/02-Kafka_Cluster_Topics_Partitions.md) | [PDF](chapters/Chapter_02_Kafka_Cluster_Topics_Partitions.pdf) |
| 03 | Order Processing & Outbox | [Markdown](notes/03-Order_Transactional_Outbox.md) | [PDF](chapters/Chapter_03_Order_Transactional_Outbox.pdf) |
| 04 | Inventory, Idempotency & ACK | [Markdown](notes/04-Inventory_Idempotency_Manual_ACK.md) | [PDF](chapters/Chapter_04_Inventory_Idempotency_Manual_ACK.pdf) |
| 05 | Retry, DLT & Controlled Replay | [Markdown](notes/05-Retry_DLT_Controlled_Replay.md) | [PDF](chapters/Chapter_05_Retry_DLT_Controlled_Replay.pdf) |
| 06 | Avro & Schema Evolution | [Markdown](notes/06-Avro_Schema_Registry_Evolution.md) | [PDF](chapters/Chapter_06_Avro_Schema_Registry_Evolution.pdf) |
| 07 | Results & Order Lifecycle | [Markdown](notes/07-Inventory_Results_Order_Lifecycle.md) | [PDF](chapters/Chapter_07_Inventory_Results_Order_Lifecycle.pdf) |
| 08 | Monitoring, Metrics & Alerts | [Markdown](notes/08-Monitoring_Metrics_Alerts.md) | [PDF](chapters/Chapter_08_Monitoring_Metrics_Alerts.pdf) |
| 09 | Real Failures & Debugging | [Markdown](notes/09-Real_Failures_Debugging_Stories.md) | [PDF](chapters/Chapter_09_Real_Failures_Debugging_Stories.pdf) |
| 10 | Docker & Runtime Networking | [Markdown](notes/10-Docker_Containers_Networking.md) | [PDF](chapters/Chapter_10_Docker_Containers_Networking.pdf) |
| 11 | Jenkins CI: Test to Image | [Markdown](notes/11-Jenkins_CI_Test_Package_Build.md) | [PDF](chapters/Chapter_11_Jenkins_CI_Test_Package_Build.pdf) |
| 12 | Docker Hub & Image Versions | [Markdown](notes/12-Docker_Hub_Image_Versioning.md) | [PDF](chapters/Chapter_12_Docker_Hub_Image_Versioning.pdf) |
| 13 | CD, Approval & Health Checks | [Markdown](notes/13-CD_Approval_Deployment_Health.md) | [PDF](chapters/Chapter_13_CD_Approval_Deployment_Health.pdf) |
| 14 | GitHub Webhook & ngrok | [Markdown](notes/14-GitHub_Webhook_Ngrok_Trigger.md) | [PDF](chapters/Chapter_14_GitHub_Webhook_Ngrok_Trigger.pdf) |
| 15 | Secrets & Configuration | [Markdown](notes/15-Secrets_Configuration_Boundaries.md) | [PDF](chapters/Chapter_15_Secrets_Configuration_Boundaries.pdf) |
| 16 | Testing & Verification Evidence | [Markdown](notes/16-Testing_Verification_Evidence.md) | [PDF](chapters/Chapter_16_Testing_Verification_Evidence.pdf) |
| 17 | Demo & Interview Walkthrough | [Markdown](notes/17-Local_Demo_Interview_Walkthrough.md) | [PDF](chapters/Chapter_17_Local_Demo_Interview_Walkthrough.pdf) |

## How to practise

1. Read Chapter 01 aloud once; explain the architecture without listing every tool.
2. Work through Chapters 02–07 for the core event flow and transaction boundaries.
3. Use Chapters 08–15 for monitoring, real debugging stories and delivery follow-ups.
4. Finish with Chapters 16–17: test evidence and one correlated demo.
5. For each chapter, practise its short interview answer, then answer its follow-up points from memory.

## Accuracy and scope

- This is a personal hands-on project, not a substitute for the candidate's company experience.
- Delivery is at-least-once with idempotent business processing; no distributed MySQL/Kafka exactly-once claim.
- Current implementation, recorded local observations and proposed improvements are distinguished.
- Source inspection is not a new test run or a fresh runtime-health check.
- No passwords or token values are included. The known DB-credential migration is documented, not performed.
- GitHub Markdown uses headings, tables and Mermaid. The PDFs carry the consistent colour design.

## Review and sources

Each chapter received three review passes: source accuracy, interview wording/concept ownership, and rendered visual readability. See [review record](REVIEW.md).

Source: [alexsaifee786-jpg/kafka-order-platform](https://github.com/alexsaifee786-jpg/kafka-order-platform), branch `main`.

Verified source revision: [`2bad80e88cf7`](https://github.com/alexsaifee786-jpg/kafka-order-platform/commit/2bad80e88cf7b0f7b4dd77359287b04ef6403537). Reviewed 29 September 2026.
