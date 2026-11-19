\# Network Configuration and Secure Remote Access



\## 1. Overview



This document explains the network architecture used to provide secure remote access to a self-hosted AFFiNE server.



The server runs Ubuntu 24.04 on a repurposed Lenovo laptop. Rather than exposing AFFiNE directly to the public internet, access is provided through a private Tailscale network.



Tailscale Serve provides HTTPS connectivity to the application.



\## 2. Network Architecture



The environment includes:



| Component | Role |

|---|---|

| Ubuntu server | Hosts AFFiNE and its supporting services |

| Docker Compose | Manages the application containers |

| Tailscale | Provides private network connectivity |

| Tailscale Serve | Provides HTTPS access |

| Windows PC | Authorized desktop client |

| Galaxy Z Fold8 | Authorized mobile client |

| Home router | Provides local network and internet connectivity |



The Windows PC and Android phone connect to the Ubuntu server through the same private Tailscale network.



See the \[architecture diagram](../diagrams/architecture.md) for a visual overview.



\## 3. Why Tailscale?



Tailscale was selected to simplify secure access to a privately hosted application.



Benefits include:



\- Private connectivity between authorized devices.

\- Encrypted connections using WireGuard-based networking.

\- No requirement to expose AFFiNE through public router port forwarding.

\- Access from devices on different networks.

\- Support for Windows, Linux, and Android.



This approach allows the server to remain privately accessible while still supporting remote productivity.



\## 4. Installing and Configuring Tailscale



Tailscale was installed on the Ubuntu server and authorized client devices.



For a new deployment, use the official installation instructions:



https://tailscale.com/download



Verify Tailscale status on Ubuntu:



```bash

sudo tailscale status

```



View the server's assigned Tailscale IPv4 address:



```bash

tailscale ip -4

```



Tailscale assigns private network addresses to connected devices.



Actual addresses and tailnet identifiers are intentionally excluded from this public repository.



\## 5. Restricting AFFiNE to the Private Network



AFFiNE uses port `3010` for its application service.



The Docker deployment was configured to bind the published application port to the Ubuntu server's Tailscale IPv4 address rather than to all network interfaces.



An illustrative Docker Compose port mapping is:



```yaml

ports:

&#x20; - "${TAILSCALE\_IP}:3010:3010"

```



Here, `TAILSCALE\_IP` represents the server's private Tailscale IPv4 address.



This configuration helps prevent direct access to AFFiNE through the server's ordinary LAN interface.



\*\*Important:\*\* Binding to a Tailscale address is one layer of protection. Tailscale access policies, host firewall rules, and Docker networking behavior should also be reviewed.



\## 6. Configuring HTTPS with Tailscale Serve



Tailscale Serve was used to provide an HTTPS endpoint for AFFiNE.



The configuration follows this general pattern:



```bash

sudo tailscale serve --bg --https=443 http://TAILSCALE\_IP:3010

```



Replace `TAILSCALE\_IP` with the server's actual Tailscale IPv4 address when configuring a new deployment.



Tailscale Serve acts as a reverse proxy, forwarding HTTPS requests to the AFFiNE application.



The resulting HTTPS address is accessible to authorized devices on the tailnet.



Check the Serve configuration:



```bash

tailscale serve status

```



Tailscale Serve is distinct from Tailscale Funnel. This project uses private Serve access, not public Funnel exposure.



\## 7. Application URL Configuration



AFFiNE was configured to use its private HTTPS address as the application's external URL.



This is important for consistent browser access and application-generated links.



The production URL is intentionally omitted because it contains private network information.



\## 8. Client Device Access



\### Windows PC



The Windows workstation runs the Tailscale client and connects to the same tailnet as the Ubuntu server.



After connecting, AFFiNE can be opened in a browser using the configured private HTTPS address.



\### Android Phone



The Galaxy Z Fold8 also runs Tailscale.



This allows the phone to reach the private AFFiNE server when connected through the authorized tailnet.



Mobile application functionality can differ from the desktop experience, so successful network access does not necessarily mean every AFFiNE feature is available on mobile.



\## 9. Troubleshooting Network Connectivity



The following commands help diagnose common connectivity problems.



Check whether Tailscale is connected:



```bash

sudo tailscale status

```



Check the assigned Tailscale address:



```bash

tailscale ip -4

```



Check the HTTPS proxy configuration:



```bash

tailscale serve status

```



Check whether the application container is running:



```bash

docker ps

```



Inspect application logs:



```bash

docker logs --tail 100 affine\_server

```



Inspect the application's published Docker ports:



```bash

docker port affine\_server

```



If the application is unreachable, investigate:



1\. Whether the Ubuntu server is powered on and connected to the network.

2\. Whether Tailscale is running on both the server and client.

3\. Whether the client is authorized to reach the server under the tailnet's access policies.

4\. Whether the AFFiNE container is running.

5\. Whether Docker is publishing the application port to the intended interface.

6\. Whether Tailscale Serve is forwarding requests correctly.

7\. Whether the application's external URL matches its configured HTTPS endpoint.



\## 10. Security Considerations



The network design uses several protective measures:



\- Private tailnet access rather than public port forwarding.

\- Encrypted connectivity between Tailscale devices.

\- HTTPS for browser access.

\- Restricted application port binding.

\- No publication of private IP addresses, hostnames, or credentials.



Tailscale device authorization and access policies remain important because devices with permitted tailnet access may be able to reach the application.



\## 11. Skills Demonstrated



This portion of the project demonstrates experience with:



\- VPN and overlay networking.

\- Linux network troubleshooting.

\- Docker port publishing and interface binding.

\- Reverse proxy configuration.

\- HTTPS.

\- Remote device connectivity.

\- Network access controls.

\- Secure self-hosted service design.



\---



\*\*Security note:\*\* All network addresses, tailnet names, and application URLs have been replaced with placeholders or omitted to protect the production environment.

