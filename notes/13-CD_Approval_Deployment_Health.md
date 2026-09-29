# 13 · CD, Approval & Health Checks

_Deploy deliberately • Verify readiness • Explain failure after container replacement_

## 🗣️ Say it in the interview

> “After publishing both images, Jenkins waits for manual deployment approval. It replaces Order and Inventory containers, then checks both Actuator health endpoints. This is a continuous-delivery workflow with an approval gate. The current local setup does not provide automatic rollback or zero-downtime deployment.”

## WHAT HAPPENS AFTER APPROVAL?

- **Before approval:** images are prepared; the deployment stages have not replaced running apps. The Jenkins input step offers the Deploy decision.
- **After approval:** remove old Order container → start new Order → remove old Inventory → start new Inventory → check Order health → check Inventory health.
- Runtime environment variables supply the Docker/host connection addresses described in Chapter 10. The same application code runs with different endpoint configuration.

## THE READINESS PREDICATE

- For each service: **up to 30 attempts**, with **3-second waits** between failures. The loop reruns curl against **/actuator/health** and checks for **"status":"UP"**.
- Jenkins reaches Order at host.docker.internal:8080 and Inventory at host.docker.internal:8082. curl uses --fail; the response must also satisfy the text check.
- This is up to **29 sleeps** plus HTTP request time, not a strict 90-second deadline. The current curl command has no explicit per-request timeout.

## WHAT FAILURE LEAVES BEHIND

- If the final check fails, Jenkins exits with an error. It does not automatically restore the old image or container.
- Old containers are removed before the new versions are proven healthy. There can be downtime; a failure between deployments can leave mixed service versions.
- Both replacements happen before health verification. A successful Order start is not a gate that verifies Order health before Inventory replacement.

## HOW I WOULD IMPROVE IT

- **Next:** explicit request deadlines, structured top-level health parsing, retained previous versions, rollback validation and an end-to-end smoke order.
- For stronger availability, evaluate staged/blue-green deployment. Explain the database and event-contract compatibility required during mixed-version operation.
- Actuator UP is a readiness signal in this pipeline; it does not prove an order completed correctly. Use the correlated demo in Chapter 17 for that.

> **Recall:** YAAD RAKHO: approval → replace → health check; failure currently needs recovery.

**Verified source:** [Jenkinsfile input/deployment/health stages + README deployment boundaries](https://github.com/alexsaifee786-jpg/kafka-order-platform/blob/main/Jenkinsfile)

Personal hands-on project. Source inspection on 29 Sep 2026; no new runtime test claimed.
