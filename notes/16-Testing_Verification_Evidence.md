# 16 · Testing & Verification Evidence

_Know the assertions • Separate unit and integration scope • Avoid coverage claims_

## 🗣️ Say it in the interview

> “The current repository has thirteen JUnit test methods. I use unit tests for business decisions and outbox failure handling, a MySQL integration test for persistence, and an Embedded Kafka test for the DLT route. I also demonstrated the end-to-end workflow manually; that is separate from automated coverage.”

## THE VERIFIED TEST INVENTORY — 13 METHODS

- **Order context:** 1 startup test. **Order OutboxPublisher:** 6 tests.
- **Inventory context:** 1 startup test. **InventoryProcessingService:** 3 unit tests.
- **InventoryDatabaseIntegrationTest:** 1 persistence test. **InventoryRetryToDltIntegrationTests:** 1 Kafka/DLT integration test.

## WHAT THE ASSERTIONS ACTUALLY PROTECT

- **Order outbox:** publish status only after successful send; timeout then retry; asynchronous failure; synchronous serialization failure; interruption preserved and batch stopped; empty poll.
- **Inventory unit:** duplicate skipped, new event reduces stock, insufficient stock creates a rejection outbox with expected payload.
- **MySQL test:** stock changes from **10 to 8** and the processed event exists. This checks actual persistence, not only mocked repository calls.
- **DLT test:** missing product triggers recovery; assertions check the key, selected decoded Avro fields, original-topic header and exception metadata. It does not explicitly count listener attempts.

## ENVIRONMENT & SAFETY BOUNDARIES

- The DLT test embeds Kafka, not Schema Registry or MySQL. Jenkins supplies their URLs; the exact test commands are in Chapter 11.
- Inventory DB integration uses **create-drop**. Keep it on **inventory_test_db**, never a database whose data you need.

## RUNTIME PROOF AND NEXT TESTS

- The repo has **16 screenshots**: monitoring, Jenkins, Docker, Docker Hub and MySQL. A missing additional capture does not mean you never performed the hands-on exercise.
- For a strong walkthrough, correlate one orderId across API, outboxes, stock and final status. Historical screenshots from different runs cannot establish that full sequence.
- Future automation: Order result-consumer tests, Inventory outbox failure tests, full end-to-end flow and deployment rollback verification. Existing tests remain useful without claiming those gaps are covered.

> **Recall:** YAAD RAKHO: test count ≠ coverage; screenshot ≠ automated assertion.

**Verified source:** [All six test classes + Jenkinsfile + README evidence inventory](https://github.com/alexsaifee786-jpg/kafka-order-platform#testing-strategy--verification-evidence)

Personal hands-on project. Source inspection on 29 Sep 2026; no new runtime test claimed.
