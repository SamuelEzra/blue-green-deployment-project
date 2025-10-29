# DECISION.md

## 🧠 Architecture Overview

This project implements a **Blue-Green deployment model** to achieve **zero-downtime releases**. Two Node.js app containers—`app_blue` and `app_green`—run concurrently, with **NGINX** acting as a reverse proxy to route traffic to the active pool. Failover is handled automatically using NGINX’s `proxy_next_upstream` directive.

---

## 🔧 Tools Used

- **Docker Compose**: Manages multi-container orchestration.
- **NGINX**: Serves as a reverse proxy with built-in failover logic.
- **envsubst**: Injects environment variables into the NGINX configuration file.
- **Healthchecks**: Monitors app container responsiveness to trigger failover.

---

## 🔁 Failover Logic

- The environment variable `ACTIVE_POOL` defines the primary container (`blue` or `green`).
- NGINX routes traffic to `app_${ACTIVE_POOL}`.
- If the primary container fails (via `/chaos/start`), NGINX retries the request using the backup container (`app_${BACKUP_POOL}`).
- Failover is seamless and ensures no failed client requests.

---

## ⚠️ Challenges Faced

- **Image Pull Errors**: Initial registry images were unavailable.
  - ✅ Resolved by using fallback image: `yimikaade/wonderful:devops-stage-two`
- **Port Conflicts**: Conflicts between internal and host ports.
  - ✅ Solved by mapping each app to a unique host port (`8081` for Blue, `8082` for Green).

---

## ✅ Why This Design Works

- Fully parameterized using a `.env` file for flexibility and CI integration.
- Dynamic NGINX configuration generated via `entrypoint.sh` script.
- Clear separation of concerns between proxy and app containers.
- Easy to test, extend, and swap images without changing architecture.

---

## 📌 Notes

This setup is **production-ready** for testing and CI grading. Once the official HNGx images are available, they can be swapped into the `.env` file without modifying the overall architecture or deployment logic.
