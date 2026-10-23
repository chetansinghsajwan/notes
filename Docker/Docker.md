Docker is a platform designed to make developing, shipping, and running applications easier by using **containerization**.
### ==Key Concepts==
- ==Container:== A lightweight, standalone, and executable package that runs in isolation.
- ==Image:== A read-only template (like a blueprint) used to create containers.
- ==Registry:== A storage location for images (e.g., Docker Hub).
- ==Daemon:== The background service that manages Docker objects and containers.
- ==Client:== The interface (CLI or GUI) used to send commands to the daemon.
# ==Timeline==
- **2008 – The Foundation:** Solomon Hykes, Sebastien Pahl, and Kamel Founadi launch **dotCloud** in Paris, a Platform-as-a-Service (PaaS) company designed to help developers deploy applications.
- **2010 – The Internal Struggle:** The dotCloud team struggles with "environment drift"—applications working on developers' laptops but failing on production servers. They build an internal, proprietary tool based on Linux kernel features (namespaces and cgroups) to wrap code and dependencies together.
- **2013, March – The Public Debut:** Solomon Hykes demonstrates Docker at PyCon in Santa Clara. The industry reaction is immediate and overwhelmingly positive.
- **2013, April – The Pivot:** Seeing the massive potential of the tool, the company abandons its original PaaS business, rebrands as **Docker, Inc.**, and open-sources the engine.
- **2014, June – First Production Version:** Docker 1.0 is released, signaling that the platform is stable enough for enterprise and professional use.
- **2014, December – Introducing Compose:** Docker introduces **Docker Compose**, allowing developers to define multi-container applications (like an app plus its database) in a single file, drastically simplifying local development.
- **2015, June – Building a Standard:** Docker co-founds the **Open Container Initiative (OCI)** to ensure that container technology is vendor-neutral and not locked into any single company, establishing a global industry standard.
- **2016, September – Orchestration War Begins:** Docker releases "Docker Swarm" to manage fleets of containers. This triggers intense competition with Google’s **Kubernetes**, which gains significant industry favor for large-scale management.
- **2017, October – Conceding the War:** Docker announces native support for Kubernetes within its platform, effectively ending the battle for orchestration dominance and positioning itself as a universal tool for any environment.
- **2019, November – The Big Reorganization:** To focus on developer experience, Docker, Inc. sells its "Docker Enterprise" platform to **Mirantis**, allowing the company to pivot back to its roots: high-quality developer tools.
- **2021, August – Moving to Paid Subscriptions:** Docker introduces paid tiers for **Docker Desktop**, creating a sustainable revenue stream to support the continued development of its ecosystem.
- **2023, January – Strengthening the Supply Chain:** Docker acquires **AtomicJar** (the makers of Testcontainers), doubling down on the developer experience and testing workflows.
- **2025, February – The AI Integration:** Docker launches **Docker AI**, integrating artificial intelligence to automate the generation of Dockerfiles and optimize container security.
- **2026, May – Current State:** Docker now operates as a developer-centric powerhouse, prioritizing secure supply chains (via Docker Scout) and AI-assisted workflows to stay relevant in the modern era of cloud-native development.
# ==FAQ==
==What is command `sleep infinity` in a service of docker compose file for a devcontainer?==
In the context of a `devcontainer` or a `docker-compose.yml` file, the command `sleep infinity` is a common trick used to keep a container running indefinitely without doing any actual work.
### Why is it used?
Normally, a Docker container exits as soon as its main process (the command defined in the `CMD` or `ENTRYPOINT`) finishes. If you want a container to stay "alive" in the background so you can interact with it—for example, to shell into it (`docker exec`) or to keep a development environment ready for an IDE like VS Code to attach to—you need a process that never terminates.
### How it works
- **The Command:** `sleep infinity` tells the container to start a process that simply waits forever.
    
- **The Result:** Because the `sleep` process never completes, the container’s main process never exits, and the container remains in a "running" state.
    
### Example in `docker-compose.yml`
You might see it used when you want a service to exist purely as a host for your development tools:
YAML
```
services:
  app:
    image: node:20
    command: sleep infinity
```
By using this command, the container starts up and stays active, allowing you to use VS Code's "Remote - Containers" extension to attach to the container and start running your development tasks manually.
Would you like to know how to use `docker-compose` to run your actual application code instead of keeping it in a sleep state?
# ==Sub Pages==
[[Build Context]]
[[Compose]]