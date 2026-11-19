\# Self-Hosted AFFiNE Server Architecture



This diagram illustrates the infrastructure used to host AFFiNE on a repurposed Lenovo laptop running Ubuntu 24.04.



\## Infrastructure Diagram



```mermaid

flowchart TB

&#x20;   PC\["Windows PC"]

&#x20;   PHONE\["Galaxy Z Fold8"]



&#x20;   subgraph VPN\["Private Tailscale Network"]

&#x20;       TS\["Tailscale VPN"]



&#x20;       subgraph SERVER\["Lenovo Server - Ubuntu 24.04"]

&#x20;           HTTPS\["Tailscale Serve - HTTPS"]



&#x20;           subgraph DOCKER\["Docker Compose"]

&#x20;               AFFINE\["AFFiNE Application"]

&#x20;               POSTGRES\[("PostgreSQL 16")]

&#x20;               REDIS\[("Redis")]

&#x20;           end



&#x20;           BACKUP\["Bash Backup Scripts + Cron"]

&#x20;       end

&#x20;   end



&#x20;   SSD\[("External SSD - Local Backups")]



&#x20;   PC --> TS

&#x20;   PHONE --> TS

&#x20;   TS --> HTTPS

&#x20;   HTTPS --> AFFINE

&#x20;   AFFINE --> POSTGRES

&#x20;   AFFINE --> REDIS

&#x20;   POSTGRES -.-> BACKUP

&#x20;   AFFINE -.-> BACKUP

&#x20;   BACKUP --> SSD

```



\## Architecture Overview



\*\*Client devices:\*\* Authorized Windows and Android devices access the workspace through a private Tailscale network.



\*\*Server:\*\* A repurposed Lenovo laptop runs Ubuntu Linux and hosts the application infrastructure.



\*\*Application:\*\* AFFiNE runs inside Docker alongside PostgreSQL and Redis.



\*\*Secure access:\*\* Tailscale Serve provides HTTPS access without directly exposing AFFiNE to the public internet.



\*\*Backup system:\*\* Scheduled Bash scripts back up application data and database records to external SSD storage.



\## Security Notes



This diagram intentionally excludes private IP addresses, hostnames, credentials, and other sensitive infrastructure details.

