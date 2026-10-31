---
name: ebitda-addbacks
description: >-
  Test adjusted EBITDA and each add-back against financial statements, ledgers,
  contracts, and invoices in a Continua Project Room, quality-of-earnings (QoE)
  style, with traceable source citations. Use for private credit due diligence
  and credit underwriting when reviewing a sponsor's EBITDA bridge, pro forma or
  run-rate synergies, normalization adjustments, or covenant EBITDA definitions
  and add-back caps.
---

# EBITDA add-back review

Uses the **Continua Project Room** MCP server (`https://app.askcontinua.com/mcp?subset=mini`), which this plugin connects through `.mcp.json`. If its tools are missing or return an authorization error, ask the user to connect and sign in to **Continua Project Room** (Claude: Settings → Connectors; Claude Code: `/mcp`; a Continua account is required), then retry. Research can consume credits under the user's plan.

## 1. Brief

Whose EBITDA (management, sponsor CIM, or the credit agreement's covenant definition); the LTM period and comparison years; the governing definition with any add-back caps and look-forward periods; house policy on accepted categories, if any.

## 2. Room

Use the deal Room (`list_project_rooms`, `get_project_room`) or create one and `upload_project_files`. It should hold the EBITDA bridge, at least 3 years of audited and management accounts, the GL or trial balance if available, invoices or contracts behind one-off items, restructuring and transaction cost records, synergy plans, and the credit agreement if covenant EBITDA matters.

## 3. Research

`research_project_room` with a question like:

> Review the EBITDA bridge for the target and LTM period from the brief, naming both explicitly. Rebuild reported EBITDA from the financial statements and reconcile it to the bridge's starting point. For each add-back give the amount, category (non-recurring cost, owner or related-party normalization, restructuring, transaction cost, stock compensation, pro forma acquisition, run-rate synergy or cost saving, other), and supporting documents. Test recurrence by looking for the same or similar items in prior periods. Test support from invoices, ledger entries, contracts, and approvals. For run-rate savings, say whether the actions are taken or only planned and whether timing fits any look-forward period. Check each item against the governing definition and caps from the brief (the credit agreement's EBITDA definition or the policy in this Room) and compute capped and uncapped EBITDA. Give total add-backs as a percentage of reported EBITDA. For each item give a diligence view (accept / adjust / reject) with the reason, then diligence-adjusted EBITDA beside management's figure, and open questions for management. Cite every amount.

## 4. Collect and check

Poll `get_project_research` until done. Check the sources behind every rejected or reduced item through `findings.json` and `read_project_file`. Lead with: management EBITDA → diligence-adjusted EBITDA, the difference, and the three items that drive most of it. Share the `report_url`; build a workbook with source references if the user wants one.

If the adjusted figure feeds a covenant test or memo, continue in the same Room with the `covenant-compliance` or `credit-memo` skill, passing this research as `previous_research_id`.

## Rules

- "Accept / adjust / reject" is a diligence view, not an audit or QoE opinion.
- An add-back needs evidence in this record; "common in the market" is not support.
- Keep management's figures intact and show the diligence view beside them.
