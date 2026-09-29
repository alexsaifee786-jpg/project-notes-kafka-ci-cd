# 10 · Docker & Runtime Networking

_Know what Compose starts • Know what Jenkins deploys • Trace connection paths_

## 🗣️ Say it in the interview

> “I use Docker Compose for the Kafka cluster, Schema Registry and monitoring. Jenkins deploys the two application containers separately onto the Compose network. MySQL remains on the Windows host, so the application containers reach it through host.docker.internal.”

## IMAGE, CONTAINER & OUR BUILD

- **Image:** a packaged runtime filesystem and startup configuration. **Container:** a running instance created from that image.
- Both service Dockerfiles use **eclipse-temurin:17-jre**, copy target/*.jar to /app/app.jar and start **java -jar app.jar**. Maven builds the JAR before docker build.
- This is a simple runtime image, not a multi-stage Maven build. The custom Jenkins image is based on lts-jdk21 and installs Docker tooling.

## WHO OWNS WHICH RUNTIME?

- **Compose:** kop-kafka-1/2/3, kop-schema-registry, kop-prometheus and kop-grafana.
- **Jenkins deployment:** order-service-container on **8080** and inventory-service-container on **8082**. MySQL is not a Compose service.
- Application network: **kafka-order-platform_default**. Its existence depends on the Compose project name; running from another project name can change the default network.

## THREE CONNECTION RULES

- Container → Kafka: **kop-kafka-1:19092** (and peers). Container → Registry: **kop-schema-registry:8081**. See Chapter 02 for the full broker-port map.
- Container → host MySQL: **host.docker.internal:3306**. Inside a container, localhost refers to that container; it is not the Windows host.
- Host → application uses a published port, such as **8080:8080**. Shared-network container communication uses service/container names and internal ports.

## PERSISTENCE & STARTUP

- Each Kafka broker has its own named data volume. Grafana has **grafana-data**. Volumes can survive container replacement; deleting a volume deletes its persisted data.
- Prometheus configuration is a read-only bind mount; no explicit Prometheus data volume is declared in current Compose. Do not assume equivalent metrics-history durability.
- Registry depends on all three broker health checks. This controls startup ordering, not continuous dependency recovery or business readiness.

> **Recall:** YAAD RAKHO: Compose = infrastructure; Jenkins = apps; Windows host = MySQL.

**Verified source:** [docker-compose.yml + both service Dockerfiles + jenkins-docker/Dockerfile](https://github.com/alexsaifee786-jpg/kafka-order-platform/blob/main/docker-compose.yml)

Personal hands-on project. Source inspection on 29 Sep 2026; no new runtime test claimed.
