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
