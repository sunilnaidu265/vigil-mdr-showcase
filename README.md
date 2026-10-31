<div align="center">

# 🛡️ Vigil MDR

### A personal Managed Detection and Response lab, built end to end.

**From raw endpoint telemetry to an analyst decision.**

![Status](https://img.shields.io/badge/Status-Lab_Project-1A56B0?style=for-the-badge)
![AI](https://img.shields.io/badge/AI-Runs_Locally-0A1F44?style=for-the-badge)
![Analyst](https://img.shields.io/badge/Human-In_The_Loop-1A56B0?style=for-the-badge)
![ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK-0A1F44?style=for-the-badge)

</div>

> **This is a showcase.** The source code is private. Every screenshot uses invented demo data.

---

## 🎯 What it does

Vigil watches a test endpoint, pulls in detections automatically, connects related alerts into
incidents, asks a local AI model for a first opinion, and gives an analyst one place to review it all.

The AI assists. A person makes the final decision.

## 🖥️ Incident queue

Critical work first, with failed AI analysis flagged for manual review.

![Incident queue with demo data](screenshots/incident-queue.png)

## 🔍 Incident investigation

The verdict, attack chain, indicators and process tree sit together. Each AI claim links back to
the evidence behind it, so nothing has to be taken on trust.

![Incident detail page with demo data](screenshots/incident-detail.png)

## 📊 Security dashboard

Detection trends, severity, AI review results, top rules and activity patterns.

![Security dashboard with demo data](screenshots/security-dashboard.png)

## 🧭 How it works

![Vigil MDR architecture](docs/architecture.svg)

## 🪜 Step by step

1. **Something happens on the endpoint.** The LimaCharlie sensor records activity such as a new process.
2. **A rule raises an alert.** Sigma and managed rules turn suspicious events into detections mapped to MITRE ATT&CK.
3. **Vigil collects it.** Every 5 minutes, a background job securely pulls new detections. If the laptop was off, it catches up from LimaCharlie's history.
4. **Secrets get masked.** Passwords and tokens in command lines are hidden before anything is stored or sent to the AI.
5. **Related alerts are connected.** Detections on the same host are grouped by real process links, or by being close together in time.
6. **The incident gets a score.** Rule severity, the number of techniques, the variety of rules and sensitive targets such as LSASS all count. Known tools lower the score.
7. **Vigil waits briefly.** It holds each incident for a few minutes, so late alerts can join before analysis starts.
8. **The prompt is built.** Vigil combines core investigation rules with the playbooks that match the incident's ATT&CK techniques.
9. **The local AI investigates.** Ollama reviews the evidence and returns structured JSON, all on the laptop.
10. **Code checks the answer.** The format is validated, and invented evidence references are rejected. The model gets one retry, then the incident is flagged as failed.
11. **Results are stored.** Incidents, evidence, verdicts and decisions go into PostgreSQL.
12. **An analyst decides.** They check the AI's claims against the evidence, then approve, edit or reject the verdict.

## 🤖 How the AI model works

- **Runs locally:** the model runs through Ollama, so detection data stays on the machine.
- **Picks the right guidance:** a small local model can't hold everything at once, so it only gets playbooks relevant to the incident.
- **Separates trust levels:** trusted context from Vigil stays apart from evidence, which may contain text written by an attacker.
- **Treats evidence as data:** instructions hidden in logs are never followed, and attempts to steer the AI count as suspicious.
- **Must show its work:** every claim has to cite event IDs from the evidence.
- **Returns a fixed format:** verdict, confidence, severity, summary, attack chain, claims, indicators, evidence to collect and recommended actions.
- **Stays deterministic where it counts:** scoring, grouping, timelines, masking and defanging are done by code, not by the model.
- **Doesn't get the last word:** its verdict is a starting point for a person, not an automatic action.

## ✨ Highlights

- **Automated collection:** pulls detections from LimaCharlie every 5 minutes, and catches up after downtime
- **Correlation:** groups alerts by process links and time into scored incidents
- **Local AI triage:** runs on Ollama, so detection data doesn't go to an outside AI service
- **Playbook-driven analysis:** picks investigation guidance from MITRE ATT&CK techniques
- **Output validation:** rejects evidence references the AI made up
- **IOC defanging and secret masking:** handled in code, not left to the model
- **Process trees:** built from real parent and child links
- **Analyst workflow:** approve, edit or reject, with reasons and an audit trail
- **Customer reports:** clean PDFs without raw events, available only after review

## 🧰 Tech stack

| Area | Tools |
|---|---|
| Detection | LimaCharlie, Sigma, MITRE ATT&CK |
| Pipeline | Python, Windows Task Scheduler |
| AI | Ollama, running locally |
| Storage | PostgreSQL |
| Portal | FastAPI, Jinja, HTMX |

## 🔐 Security by design

- Attacker-controlled text is kept apart from trusted context, and escaped on every page
- Portal login with hashed passwords, signed sessions, lockouts and cross-site protection
- Least-privilege, read-only access to detection data
- Secrets stay out of the code

## 🚧 Honest status

This is a working lab project, not a production security service. It doesn't block attacks.

The pipeline runs end to end, but AI verdicts are still unreliable and accuracy hasn't been
measured, which is exactly why a person reviews every incident. Next up: code-based guardrails,
an evaluation set, rule tuning, and a feedback loop from analyst decisions.

---

<div align="center">

Built by [Sunil Chikkala](https://github.com/sunilnaidu265)

</div>
