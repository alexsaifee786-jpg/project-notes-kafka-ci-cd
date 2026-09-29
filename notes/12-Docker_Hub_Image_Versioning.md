# 12 · Docker Hub & Image Versions

_Publish a specific build • Track the source • Separate publication from deployment_

## 🗣️ Say it in the interview

> “Jenkins tags both service images with BUILD_NUMBER and pushes them to Docker Hub. The Docker Hub token is stored in Jenkins Credentials and passed through standard input during login. I can trace a numbered image back to its Jenkins build and the source commit recorded by that build.”

## THE NAMES I ACTUALLY USE

- **Local:** order-service:${BUILD_NUMBER}
**Registry:** saifee162007/order-service:${BUILD_NUMBER}
- **Local:** inventory-service:${BUILD_NUMBER}
**Registry:** saifee162007/inventory-service:${BUILD_NUMBER}
- Example: build **28** produces both **:28** tags. The current pipeline does not also push latest.

## BUILD, TAG, PUSH — THREE DIFFERENT ACTIONS

- **docker build** creates an image. **docker tag** adds another name to an existing image; it does not rebuild it. **docker push** publishes the registry-qualified image.
- Both services use the same build number for correlation, but remain separate images. One successful push does not make the other push atomic.
- A Docker Hub tag can exist even if later approval/deployment fails. The committed captures show tags 28 and 30; they are publication evidence, not complete pipeline-success evidence.

## AUTHENTICATION WITHOUT EMBEDDING A TOKEN

- Credential ID: **dockerhub-credentials**. Jenkins withCredentials binds the username and token for the login step.
- **docker login --password-stdin** receives the token through stdin rather than a password argument. The Jenkinsfile contains the credential reference, not the actual token.
- Jenkins masking is useful, but not permission to print secrets. Docker login can retain authentication in the agent's Docker configuration; agent access still matters.

## VERSIONING TRADE-OFFS

- A numbered tag makes a build easier to identify than latest. It is not automatically immutable: a registry tag can be overwritten unless policy prevents it.
- Current tags do not embed a Git SHA or semantic version. Retain the Jenkins build record to connect **Git commit → build → image tag**.
- Current deployment runs the already-built **local image** after push; it does not pull it back from Docker Hub.

> **Recall:** YAAD RAKHO: tag identifies a release candidate; successful push ≠ successful deploy.

**Verified source:** [Jenkinsfile login/tag/push stages + Docker Hub evidence in project README](https://github.com/alexsaifee786-jpg/kafka-order-platform/blob/main/Jenkinsfile)

Personal hands-on project. Source inspection on 29 Sep 2026; no new runtime test claimed.
