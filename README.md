\# Self-Hosted AFFiNE Server



A self-hosted productivity and knowledge management server built on Ubuntu Linux, using Docker Compose, PostgreSQL, Redis, and Tailscale for secure remote access.



This project was created as part of my hands-on experience with Linux system administration, containerization, networking, and self-hosted infrastructure.



\## Project Overview



The goal of this project was to deploy and maintain a private AFFiNE workspace for business documentation and knowledge management without relying entirely on a cloud-hosted service.



The server runs on a repurposed Lenovo laptop, providing an opportunity to build practical experience with Linux, Docker, secure networking, and backup and recovery procedures.



\## Project Documentation



The following guides document the implementation, architecture, security, maintenance, and troubleshooting of this self-hosted environment.



| Documentation | Description |

|---|---|

| \[Server Architecture](diagrams/architecture.md) | Infrastructure diagram showing the server, Docker containers, Tailscale, and backup storage |

| \[Installation \& Deployment](docs/installation.md) | Ubuntu setup, Docker Compose deployment, and container administration |

| \[Networking \& Security](docs/networking.md) | Private Tailscale connectivity, HTTPS, and network troubleshooting |

| \[Backup \& Recovery](docs/backups.md) | PostgreSQL backups, Bash automation, cron scheduling, and restore testing |

| \[Troubleshooting](docs/troubleshooting.md) | Technical investigations, including AFFiNE's Gemini BYOK integration |



All documentation uses sanitized examples to protect the production environment.



\## Technology Stack



| Technology | Purpose |

|---|---|

| Ubuntu Server 24.04 | Linux operating system |

| Docker \& Docker Compose | Containerized application deployment |

| AFFiNE | Self-hosted productivity workspace |

| PostgreSQL | Application database |

| Redis | Supporting application service |

| Tailscale | Private remote network access |

| HTTPS | Encrypted browser connections |

| Bash \& Cron | Automated backup scripts and scheduling |

| External SSD | Local backup storage |



\## System Architecture

\[View the Server Architecture Diagram](diagrams/architecture.md)



The environment consists of:



\- \*\*Ubuntu host:\*\* Runs the Docker infrastructure.

\- \*\*AFFiNE container:\*\* Provides the web-based workspace.

\- \*\*PostgreSQL container:\*\* Stores application database records.

\- \*\*Redis container:\*\* Provides supporting application functionality.

\- \*\*Tailscale:\*\* Connects authorized devices to the private server.

\- \*\*Tailscale Serve:\*\* Provides HTTPS access to AFFiNE.

\- \*\*External SSD:\*\* Stores scheduled backups and recovery data.



The service is accessible from authorized devices, including a Windows workstation and an Android smartphone, without exposing the application directly to the public internet.



\## Deployment and Configuration



The deployment involved:



1\. Preparing Ubuntu Linux on repurposed hardware.

2\. Installing and configuring Docker and Docker Compose.

3\. Deploying AFFiNE with PostgreSQL and Redis.

4\. Configuring persistent application storage.

5\. Setting up private network access through Tailscale.

6\. Configuring HTTPS through Tailscale Serve.

7\. Setting up application administration and email verification.

8\. Implementing automated backups to external storage.

9\. Testing database backup and recovery procedures.



\## Security Considerations



Security was a primary consideration throughout the project.



\- Restricted application access to the private Tailscale network.

\- Used HTTPS for encrypted browser connections.

\- Kept application credentials and secrets outside public documentation.

\- Limited exposure of internal services.

\- Maintained backup copies on external storage.



\*\*Note:\*\* This repository contains documentation and sanitized configuration examples only. It does not contain production credentials, private network identifiers, database dumps, or business information.



\## Backup and Recovery



The server uses Bash scripts and scheduled cron jobs to automate backup operations.



The backup strategy includes:



\- PostgreSQL database dumps.

\- Application file backups.

\- Configuration backups.

\- Scheduled copying to an external SSD.

\- Backup verification and database restore testing.



\## Troubleshooting and Learning



This project provided hands-on experience diagnosing application, networking, and configuration issues.



Examples include:



\- Docker container configuration and application connectivity.

\- Remote access and HTTPS configuration.

\- Network connectivity troubleshooting.

\- PostgreSQL backup and restoration.

\- Investigating compatibility issues between AFFiNE's BYOK AI integration and Google's Gemini API.



The Gemini integration investigation included API connectivity testing and examination of AFFiNE's AI configuration verification behavior. Full integration remains unresolved.



\## Future Improvements



\- Add server monitoring and management dashboards.

\- Improve backup redundancy with an offsite backup destination.

\- Expand disaster recovery testing.

\- Document system maintenance procedures.

\- Evaluate future application upgrades and AI integration options.



\## Skills Demonstrated



\*\*Linux Administration · Docker Compose · Networking · VPNs · HTTPS · PostgreSQL · Bash Scripting · Backup \& Recovery · Troubleshooting · Self-Hosting\*\*



\---



\*This repository documents my personal hands-on infrastructure work. AFFiNE is an open-source application developed by its respective maintainers.\*

