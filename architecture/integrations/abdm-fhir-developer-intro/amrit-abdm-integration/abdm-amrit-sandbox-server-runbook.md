# ABDM - AMRIT Sandbox Server runbook

### Overview

This document describes how to deploy and operate the ABDM HIP / HIU / HIU-UI stack on a server using Docker and docker-compose.&#x20;

### Services & Responsibilities

**HIP-related (full infra)**

* Mongo (Mongo 5 supported) — primary HIP data store
* PostgreSQL (HIP-specific if used)
* RabbitMQ — async messaging
* Elasticsearch — search / analytics
* Logstash, Filebeat — logging pipeline

**HIU-related**

* PostgreSQL — HIU DB
* HIU-DB-Initializer — one-time DB seed job
* Orthnac — HIU supporting service (as provided)

**HIU-UI**

* HIU-UI (React) — frontend

**Other**

* Reverse proxy (Sandbox: Apache; Production: Nginx) — routing based on `X-HIP-ID` header
* LetsEncrypt / TLS cert management



### Service Overview

**Directories:**

```bash

Heading 2
￼Insert block
￼Block controls
￼Add comment
~/services/
 ├─ abdm-hip-service/
 ├─ abdm-hiu-service/
 ├─ abdm-hiu-ui/
 ├─ abdm-otp-service/
 └─ .git/
```

**Startup sequence:**

1. Start HIP Infra → HIP Service
2. Start HIU Infra-Lite → HIU Keyfile → HIU API
3. Start HIU-UI

**Published Ports**

| Component            | Host Port    | Container Port | Source File                                                 | Notes                              |
| -------------------- | ------------ | -------------- | ----------------------------------------------------------- | ---------------------------------- |
| **HIP Service**      | **9052**     | **80**         | `abdm-hip-service/docker-compose-hip.yml`                   | Main HIP API                       |
| **HIP Postgres**     | **5433**     | **5433**       | `abdm-hip-service/docker-compose-infra.yml`                 | Used by HIP backend                |
| **HIU API**          | **8003**     | **8080**       | `abdm-hiu-service/docker-compose-hiuapi.yml` + `Dockerfile` | Host:8003 mapped to container:8080 |
| **HIU Postgres**     | **5432**     | **5432**       | `abdm-hiu-service/docker-compose-infra-lite.yml`            | Used by HIU backend                |
| **Orthanc DICOM**    | **4242**     | **4242**       | `abdm-hiu-service/docker-compose-infra-lite.yml`            | DICOM interface                    |
| **Orthanc Web UI**   | **8042**     | **8042**       | `abdm-hiu-service/docker-compose-infra-lite.yml`            | Web dashboard                      |
| **Mongo**            | **27017**    | **27017**      | `abdm-hip-service/docker-compose-infra.yml`                 | MongoDB for HIP                    |
| **Proxy / External** | **80 / 443** | —              | Nginx / Apache                                              | Public HTTP(S) access              |

### HIP Setup

**Path:** `~/services/abdm-hip-service`

**Compose Files:**

* `docker-compose-infra.yml`
* `docker-compose-hip.yml`

**Infra Containers:**

* Mongo 5
* Postgres 12 (port 5433)
* RabbitMQ
* Elasticsearch 7.9.1
* Filebeat
* Logstash

**Start HIP:**

```bash
cd ~/services/abdm-hip-service
sudo docker-compose -f docker-compose-infra.yml up -d
sudo docker-compose -f docker-compose-hip.yml up -d
```

**Verify:**

```bash
docker ps -a | grep hip
```

### HIU Setup

**Path:** `~/services/abdm-hiu-service`

**Infra-Lite Containers:**

* Postgres (5432)
* Orthanc (4242, 8042)
* HIU-DB-Initializer (one-time seed)

**Start HIU Infra-Lite:**

```bash
cd ~/services/abdm-hiu-service
sudo docker-compose -f docker-compose-infra-lite.yml up -d
```

**Start HIU Keyfile Service:**

```bash
cd hiu-keyfile
sudo nodemon index.js
```

**Start HIU API (host 8003 → container 8080):**

```bash
cd ~/services/abdm-hiu-service
sudo docker-compose -f docker-compose-hiuapi.yml up -d --build
```

**Verify:**

```bash
docker ps -a | grep hiu
```

### HIU-UI Setup

**Path:** `~/services/abdm-hiu-ui`

**Files:**

* `docker-compose-hiu.yml`
* `Dockerfile`
* `image.tar`

**Start UI:**

```bash
cd ~/services/abdm-hiu-ui
docker build -t hiuui-withlogo . || true
sudo docker-compose -f docker-compose-hiu.yml up -d
```

**Verify:**

```bash
docker ps -a | grep hiu-ui
```

Open the UI in browser & confirm API connectivity to HIU Keyfile (port 8080 inside container).

**Database Setup (HIU Admin User)**

```bash
docker ps | grep postgres
docker exec -it <postgres_container> /bin/bash
psql -U postgres
```

Inside psql:

```plsql
CREATE DATABASE health_information_user;
\c health_information_user
INSERT INTO "user" (username, password, role, verified)
VALUES ('admin', '<bcrypt_hash>', 'ADMIN', true);
```

**Generate bcrypt hash:**

```javascript
const bcrypt = require('bcrypt');
bcrypt.hash('YourPassword', 10).then(console.log);
```

Or use [Browserling Bcrypt Tool](https://www.browserling.com/tools/bcrypt)

#### **Postgres Auth Workaround (Sandbox Only)**

**File:** `~/services/abdm-hiu-service/pg_hba.conf`

```bash
docker exec -it <postgres_container> /bin/bash
vi /var/lib/postgresql/data/pg_hba.conf
# Change md5 → trust
docker restart <postgres_container>
```

Revert to `md5` after testing. Never use `trust` in production.

**Stopping Services (Graceful)**

UI:

```bash
cd ~/services/abdm-hiu-ui sudo docker-compose -f docker-compose-hiu.yml down
```

HIU:

```bash
cd ~/services/abdm-hiu-service sudo docker-compose -f docker-compose-hiuapi.yml down sudo docker-compose -f docker-compose-infra-lite.yml down
```

HIP:

```bash
cd ~/services/abdm-hip-service sudo docker-compose -f docker-compose-hip.yml down sudo docker-compose -f docker-compose-infra.yml down
```

#### **Troubleshooting**

**HIP ↔ Mongo**

```bash
docker ps | grep mongo
docker logs mongo
```

**HIU ↔ Postgres**

```bash
docker logs <postgres>
```

If auth fails, check `pg_hba.conf` or verify `.env` credentials.
