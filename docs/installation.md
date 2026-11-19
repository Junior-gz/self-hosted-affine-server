\# AFFiNE Server Installation and Deployment



\## 1. Project Overview



This document describes the deployment of a self-hosted AFFiNE workspace on a repurposed Lenovo laptop running Ubuntu 24.04.



The application was deployed using Docker Compose, with PostgreSQL and Redis providing supporting services.



The goal was to create a private, self-managed productivity environment while gaining practical experience with Linux system administration, containerization, and application deployment.



\## 2. Server Environment



| Component | Configuration |

|---|---|

| Hardware | Repurposed Lenovo laptop |

| Operating System | Ubuntu 24.04 |

| Container platform | Docker and Docker Compose |

| Application | AFFiNE |

| Database | PostgreSQL 16 |

| Supporting service | Redis |

| Networking | Tailscale |

| HTTPS | Tailscale Serve |

| Backup storage | External SSD |



The deployment uses the `/opt/affine` directory for application configuration and supporting scripts.



\## 3. Preparing Ubuntu



The server was prepared for hosting containerized applications.



Typical preparation commands include:



```bash

sudo apt update

sudo apt upgrade -y

```



The system was configured for continuous operation, including adjustments to laptop power-management behavior so the server could continue running with its lid closed.



\## 4. Installing Docker



Docker provides the container runtime used to host the application and its supporting services.



For a new deployment, Docker should be installed using the official Ubuntu installation instructions:



https://docs.docker.com/engine/install/ubuntu/



Verify the installation:



```bash

docker --version

docker compose version

```



Confirm that the Docker service is running:



```bash

sudo systemctl status docker

```



\## 5. Application Deployment



AFFiNE was deployed using Docker Compose.



The deployment consists of the following services:



| Service | Function |

|---|---|

| AFFiNE | Main web application |

| PostgreSQL | Persistent application database |

| Redis | Supporting application service |

| Migration job | Initializes or updates database structure |



The application uses the official AFFiNE container image:



```text

ghcr.io/toeverything/affine:stable

```



Docker Compose manages service configuration, container startup, and communication between the application components.



The deployment configuration is stored under:



```text

/opt/affine

```



Actual production configuration files are intentionally excluded from this public repository.



\## 6. Managing the Containers



The following commands illustrate common container administration tasks.



View running containers:



```bash

docker ps

```



View application logs:



```bash

docker logs affine\_server

```



Follow application logs in real time:



```bash

docker logs -f affine\_server

```



View the Docker Compose services:



```bash

cd /opt/affine

sudo docker compose ps

```



Restart the application stack:



```bash

cd /opt/affine

sudo docker compose restart

```



These commands help with monitoring, maintenance, and troubleshooting.



\## 7. Persistent Storage



Application data must remain available even when containers are restarted or recreated.



The deployment uses persistent storage for:



\- PostgreSQL database records.

\- AFFiNE application files and uploads.

\- Application configuration.



Persistent storage is an important consideration when designing containerized services.



\## 8. Network Configuration



The AFFiNE application is accessible through a private Tailscale network.



Instead of exposing the application directly to the public internet, authorized devices connect through Tailscale.



Tailscale Serve provides HTTPS access to the application.



The networking configuration is documented separately in `networking.md`.



\## 9. Initial Application Setup



After deployment, the application was configured for internal use.



Setup activities included:



\- Creating the initial administrator account.

\- Creating the business workspace.

\- Configuring application access.

\- Configuring SMTP for account verification emails.

\- Testing access from authorized devices.



Sensitive account details, SMTP credentials, and private network information are excluded from this documentation.



\## 10. Backup Preparation



The deployment includes a backup system using Bash scripts, PostgreSQL database dumps, scheduled cron jobs, and external SSD storage.



Backup operations cover application files, configuration data, and database records.



A PostgreSQL restore test was also performed to verify that a database backup could be restored.



The backup process will be documented in `backups.md`.



\## 11. Verification and Troubleshooting



The following checks are useful when verifying a deployment:



```bash

docker ps

docker compose ps

docker logs affine\_server

```



Additional verification includes:



\- Confirming that the application loads through its private HTTPS address.

\- Confirming that PostgreSQL and Redis containers are running.

\- Checking application logs for startup errors.

\- Confirming that backup files are being generated.

\- Testing access from authorized client devices.



\## 12. Skills Demonstrated



This deployment provided hands-on experience with:



\- Ubuntu Linux administration.

\- Docker and Docker Compose.

\- Container lifecycle management.

\- PostgreSQL and Redis services.

\- Persistent application storage.

\- Private networking.

\- HTTPS configuration.

\- Bash scripting and scheduled backups.

\- Application troubleshooting.



\---



\*\*Security note:\*\* This guide documents the deployment process without publishing production configuration files, credentials, private network addresses, or customer data.

