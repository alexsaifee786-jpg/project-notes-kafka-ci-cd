# 14 · GitHub Webhook & ngrok

_A push can trigger Jenkins • A public route reaches a local endpoint_

## 🗣️ Say it in the interview

> “My Jenkins server runs locally on port 8085. I used ngrok to expose a public HTTPS route, then configured GitHub to send push events to Jenkins through that route. I verified automatic pipeline triggering using empty commits, without changing the application code.”

## THE THREE ROLES

- **Webhook:** GitHub sends an event notification after a push. It does not build or deploy the application itself.
- **ngrok:** forwards the public HTTPS request to local Jenkins. GitHub cannot directly call my machine's localhost.
- **Jenkins:** receives **/github-webhook/**, checks the configured repository and runs the job. The job checks out main and executes its Jenkinsfile.

## WHAT I CONFIGURED OUTSIDE GIT

- Jenkins UI: **GitHub hook trigger for SCM polling**. GitHub repository settings: the public webhook URL ending with **/github-webhook/**.
- The active tunnel, hostname, GitHub webhook settings and Jenkins trigger checkbox are runtime/UI configuration, not recreated by cloning the repository.
- A tunnel URL may change between sessions. A stale GitHub webhook URL can break the trigger while the application and Jenkinsfile remain correct.

## HOW I TESTED AND HOW I WOULD DEBUG

- Recorded test commits: **13e0a345** (“Test Jenkins webhook”) and **de9f5e97** (“Verify Jenkins webhook”). An empty commit generates a real push without source changes.
- The project history reports a successful callback and automatic job start. A commit alone proves a push attempt, not successful webhook delivery.
- Troubleshoot in order: GitHub delivery response → active tunnel/forwarding port → Jenkins reachability → trigger setting/repository match → job/build cause.
- Redelivery sends the existing webhook again; an empty commit creates a new Git event. Use these to isolate trigger issues from application failures.

## KEEP THESE CLAIMS PRECISE

- The webhook triggers the pipeline; it does not bypass the manual deployment approval described in Chapter 13.
- The public tunnel exposes a route to local Jenkins. Keep tokens private; do not claim webhook-signature validation is configured without checking the actual settings.

> **Recall:** YAAD RAKHO: webhook notifies; ngrok forwards; Jenkins builds; approval gates deploy.

**Verified source:** [README webhook chapter + empty trigger-test commits + Jenkinsfile](https://github.com/alexsaifee786-jpg/kafka-order-platform#github-webhook-ngrok--automatic-jenkins-trigger)

Personal hands-on project. Source inspection on 29 Sep 2026; no new runtime test claimed.
