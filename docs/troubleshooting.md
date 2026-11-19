\# AFFiNE Server Troubleshooting and Technical Investigations



\## 1. Overview



This document records troubleshooting activities performed while deploying and maintaining a self-hosted AFFiNE server on Ubuntu 24.04.



The project provided hands-on experience investigating problems involving Docker, Linux networking, HTTPS connectivity, database backups, and third-party API integrations.



The most extensive investigation involved AFFiNE's Bring Your Own Key (BYOK) integration with Google's Gemini API.



\## 2. Troubleshooting Methodology



The general troubleshooting process used throughout the project involved:



1\. Identifying the reported symptoms.

2\. Verifying the status of relevant services.

3\. Reviewing application logs and error messages.

4\. Isolating individual components.

5\. Testing suspected causes.

6\. Comparing successful and unsuccessful results.

7\. Documenting findings and unresolved limitations.



This approach helped distinguish application-level problems from underlying networking or infrastructure failures.



\## 3. Docker Container Troubleshooting



AFFiNE runs in Docker alongside PostgreSQL and Redis.



Common diagnostic commands include:



```bash

docker ps

```



```bash

docker compose ps

```



```bash

docker logs --tail 100 affine\_server

```



```bash

docker logs -f affine\_server

```



These commands are useful for checking container status, reviewing startup failures, and investigating application errors.



When using Docker Compose, commands should be run from the deployment directory or supplied with the appropriate Compose file path.



\## 4. Network Connectivity Investigation



\### Problem



During server configuration, an Ethernet connectivity test resulted in the server becoming temporarily unreachable.



\### Investigation



The troubleshooting process focused on restoring network connectivity and confirming that the server could once again be accessed remotely.



\### Outcome



Connectivity was restored, and the server continued operating using its Wi-Fi connection.



\### Lessons Learned



\- Network interface changes can interrupt remote administration.

\- Remote connectivity should be verified before relying on a new network configuration.

\- Physical access or an alternative management method is valuable when changing server networking.

\- Network changes should be planned to minimize downtime.



\## 5. Tailscale and HTTPS Troubleshooting



Tailscale provides private connectivity between the Ubuntu server and authorized client devices.



Tailscale Serve provides HTTPS access to the AFFiNE application.



Useful diagnostic commands include:



```bash

tailscale status

```



```bash

tailscale ip -4

```



```bash

tailscale serve status

```



```bash

docker port affine\_server

```



When investigating access problems, it is important to determine whether the issue involves:



\- The client device's Tailscale connection.

\- The Ubuntu server's network connection.

\- Tailscale access permissions.

\- The HTTPS reverse proxy.

\- The AFFiNE container.

\- The application's configured external URL.



Separating these components helps narrow the scope of a connectivity issue.



\## 6. PostgreSQL Backup and Recovery Verification



\### Objective



Verify that the PostgreSQL database backup could be restored successfully.



\### Investigation



The backup system was checked to confirm that database backup files were being generated.



A PostgreSQL restore test was performed using backup data.



\### Outcome



The database restore test succeeded.



This demonstrated that the tested PostgreSQL backup could be restored.



\### Remaining Work



A complete disaster recovery exercise involving the entire AFFiNE environment has not yet been completed.



The distinction is important: successful database restoration does not automatically verify that every application file and configuration setting can be recovered.



\## 7. Google Gemini BYOK Integration Investigation



\### Objective



Configure AFFiNE's built-in AI features using a Google Gemini API key.



The goal was to evaluate whether Gemini could provide AI functionality inside the self-hosted AFFiNE workspace.



\### Environment



| Component | Details |

|---|---|

| Application | Self-hosted AFFiNE v0.27.4 |

| Operating system | Ubuntu 24.04 |

| Deployment | Docker Compose |

| AI provider | Google Gemini |

| Authentication | Google AI Studio API key |

| Integration method | AFFiNE BYOK |



\### Initial Problem



AFFiNE's AI provider verification did not complete successfully.



The failure occurred despite the Gemini API key being valid and capable of communicating directly with Google's API.



\### Investigation Step 1 — Verify API Connectivity



The Gemini API was tested independently of AFFiNE.



The direct API request authenticated successfully and returned an HTTP `200` response.



\*\*Result:\*\* Basic API connectivity and authentication were working.



This helped rule out an invalid API key or a complete inability to reach the provider.



\### Investigation Step 2 — Examine AFFiNE's Verification Process



AFFiNE's BYOK verification was investigated using its GraphQL API.



The `probeWorkspaceByokDraft` operation was used to inspect individual verification results.



The verification process included connection, chat, tool-calling, and structured-output checks.



\### Investigation Step 3 — Compare Verification Results



The following results were observed during testing:



| Verification test | Result |

|---|---|

| Connection | Passed |

| Chat | Passed |

| Tool calling | Passed |

| Structured output | Failed |



The structured-output check returned an `invalid\_response` error.



The failure was observed while investigating Gemini 3.5 Flash Lite and Gemini 3.1 Flash Lite.



\### Investigation Step 4 — Isolate the Failure



Because connection, chat, and tool-calling checks passed, the investigation focused on the structured-output verification rather than general API access.



A potential limitation involving AFFiNE's structured-output probe and a small output-token allowance was identified during the investigation.



However, the precise root cause was not conclusively established.



\### Outcome



The Gemini API itself was reachable, and several AFFiNE verification checks passed.



However, the structured-output check continued to fail, preventing successful completion of the BYOK verification process.



\*\*Status: Unresolved.\*\*



The Gemini integration was deferred pending further investigation or an upstream compatibility fix.



\### Upstream Issue References



The investigation was shared with the AFFiNE project through a GitHub issue discussion.



Relevant issues:



\- \[AFFiNE issue #15558](https://github.com/toeverything/AFFiNE/issues/15558)

\- \[AFFiNE issue #15552](https://github.com/toeverything/AFFiNE/issues/15552)



These references document the upstream discussion and related investigation. They do not establish that the issue has been resolved.



\### Skills Demonstrated



This investigation involved:



\- REST API connectivity testing.

\- HTTP response interpretation.

\- API key authentication troubleshooting.

\- GraphQL API investigation.

\- Comparing individual verification results.

\- Isolating application compatibility issues.

\- Documenting findings for an open-source project.

\- Recognizing when an issue requires upstream investigation.



\## 8. Lessons Learned



Several important lessons emerged from this project.



\*\*Verify components independently.\*\* A successful direct API request does not guarantee that an application's integration layer will work.



\*\*Use logs and structured results.\*\* Detailed diagnostic output is more useful than relying only on a general error message.



\*\*Separate infrastructure from application issues.\*\* Network connectivity, container health, and application behavior should be evaluated independently.



\*\*Test recovery procedures.\*\* A backup is more useful when restoration has been tested.



\*\*Document unresolved problems.\*\* A clear explanation of unsuccessful tests and remaining limitations is valuable technical documentation.



\## 9. Future Troubleshooting Improvements



Planned improvements include:



\- Adding structured maintenance and troubleshooting checklists.

\- Improving monitoring and error visibility.

\- Implementing backup failure notifications.

\- Testing complete application recovery.

\- Re-evaluating Gemini integration after relevant AFFiNE updates.



\---



\*\*Security note:\*\* All examples omit production API keys, authentication tokens, private network addresses, passwords, and sensitive application data.

