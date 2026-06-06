# Stock Signal Monitor

AI-powered spare parts inventory monitoring for EMEA depot operations. Reads 150+ parts across 10 European depots, calculates reorder signals, generates an AI analysis with Claude, and delivers a styled HTML email digest.

Built with [n8n](https://n8n.io) · [Claude AI](https://anthropic.com) · Google Sheets · Gmail

![Workflow](screenshots/workflow-canvas.png)

## Live Demo

*The production workflow runs on a daily schedule. A [live demo](#live-demo) version with a form trigger is available below.*

https://omar-ai0.app.n8n.cloud/form/0b2a4498-27ac-49bd-baad-689b33190d2f

---

## How It Works

Every weekday at 8 AM, the workflow:

1. Reads inventory data from Google Sheets (150 parts, 10 depots)
2. Calculates days-of-stock-remaining, lead time coverage, reorder quantities, and costs
3. Classifies each part by severity: STOCKOUT → CRITICAL → WARNING → WATCH → OK
4. Sends alert data to Claude AI for a plain-language executive summary
5. Builds a styled HTML email with KPI cards, severity tables, and the AI analysis
6. Emails the digest to the planning team

![Email Output](screenshots/email-output.png)

## Signal Logic

| Severity | Trigger |
|----------|---------|
| STOCKOUT | Stock = 0 |
| CRITICAL | Below threshold AND won't last through supplier lead time |
| WARNING | Below threshold, but stock covers lead time |
| WATCH | Above threshold but within 1.5× lead time of depletion |
| OK | Healthy |

Suggested order quantity: `threshold - current_stock + (daily_consumption × lead_time)`

## Architecture
