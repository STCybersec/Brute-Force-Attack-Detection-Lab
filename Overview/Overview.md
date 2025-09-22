# Overview & Reproduction Guide — Brute-Force Detection Lab

**Project:** SSH Brute-Force Detection Lab (Splunk)

## Purpose / Overview

This lab demonstrates detection, alerting, and response for SSH brute-force attacks using Splunk. It is built to be reproducible, interview- and portfolio-ready, and demonstrates skills valuable to companies of any size: detection engineering, incident response, and measurable risk reduction.

## What you’ll get from this repo

* **Reproducible dataset** (`Dataset/Auth_log_labeled.csv`) with labeled failed/success events.
* **Copy-paste SPL queries** (`Queries/*.spl`) for detection panels.
* **Dashboard JSON** (`Dashboard/Brute_force_login_attempts_Splunk.json`) to import exact visualizations.
* **Incident response playbook & case study** (`Incident_Response`).
* **Screenshots** to show results to fellow professionals/hiring managers.

---

## A quick value note (Why this matters to companies)

* **Reduces account compromise risk:** detects brute-force attempts and prevents successful logins with automated blocking or escalation.
* **Measurable outcomes:** reductions in successful brute-force logins, MTTD (Mean Time To Detect), and MTTC (Mean Time To Contain).
* **Low friction:** dataset + SPL enable quick PoC in any environment (small business to enterprise).
* **Cross-functional value:** useful for SOC analysts, IT ops, DevOps (CI/CD pipeline hardening), and compliance teams.

### Suggested KPIs to present for a company

* Mean Time To Detect (MTTD) for brute-force alerts.
* Mean Time To Contain (MTTC) after confirmation.
* Number of blocked IPs per week.
* False positive rate (alerts that were benign).

---

## Prerequisites

* Splunk Enterprise or Splunk Free (local install) with access to Search & Reporting and Dashboards.
* A browser to access Splunk Web (`http://localhost:8000`).
* The repository files available locally or uploaded to GitHub (you should have `data/`, `queries/`, `dashboards/`, `screenshots/`, `cases/`).

**Optional:**

* A Kali / Linux host to test live SSH attacks (for advanced labs).
* Wazuh manager/agent for host-based alerts (future integration).

---
