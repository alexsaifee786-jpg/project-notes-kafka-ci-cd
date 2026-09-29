# 17 · Demo & Interview Walkthrough

_Start with your contribution • Show one correlated order • Finish with one real lesson_

## 🗣️ Say it in the interview

> “I will first explain the order flow, then show one successful reservation and how I handle duplicates or failures. After that, I can walk through the Jenkins deployment and monitoring. This project helped me practise reliable event processing, debugging and production-readiness patterns in a local environment.”

## START THE EXISTING PROJECT IN THE RIGHT ORDER

- Use **D:\kafka-order-platform**; do not recreate it. Start host MySQL, then **docker compose up -d**; verify brokers and Registry readiness.
- Use one app mode: host processes **or** Jenkins-deployed containers. For host mode, run **.\mvnw.cmd spring-boot:run** in each service folder with local DB credentials supplied securely.
- Start Inventory, ensure the demo product exists with enough stock, then start Order. Check health on **:8082** and **:8080**. Keep replay disabled for normal startup.

## A THREE-MINUTE BUSINESS DEMO

- **Before:** note source commit, timestamp and product stock. Submit **POST /api/orders** with productId, quantity and amount; retain the returned orderId.
- **Response:** HTTP **202** with a text orderId. Show the Order outbox, then Inventory processed event, stock and result outbox, then final Order status.
- With sufficient stock, expect **INVENTORY_RESERVED** and the requested deduction. Correlate orderId across services; source-event and result-event IDs are different.
- Repeat HTTP submission creates another order. A duplicate-event demonstration must redeliver the same eventId and show no second stock deduction.

## AN INTERVIEW ANSWER YOU CAN CONTROL

- **First minute:** use Chapter 01's introduction and architecture. **Next:** explain the transaction boundaries in Chapters 03–04. Then let the interviewer choose a deeper topic.
- For “a challenge you solved,” use one Chapter 09 incident: symptom, evidence, fix and verification. Avoid reciting every tool before explaining the business problem.
- Useful English: **“Let me explain with one example.”** / **“In my implementation…”** / **“I have not implemented that yet; my next step would be…”**

## END WITH EVIDENCE AND A CLEAR BOUNDARY

- Do not guess throughput, user count or production ownership. These notes cover the personal project; introduce real company work separately using verified resume facts.
- For full commands, topic setup and safe shutdown, use the project README's **How to Run / Local Setup** chapter. Stop normally without deleting volumes.

> **Recall:** YAAD RAKHO: explain one flow → show one order → discuss one failure → own the limits.

**Verified source:** [OrderController + README local setup + verified project workflow](https://github.com/alexsaifee786-jpg/kafka-order-platform#how-to-run--local-setup)

Personal hands-on project. Source inspection on 29 Sep 2026; no new runtime test claimed.
