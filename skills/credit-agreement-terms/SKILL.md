---
name: credit-agreement-terms
description: >-
  Review a credit agreement or indenture and all amendments in a Continua
  Project Room for key lender terms, quoting each clause with a source citation:
  change of control, material adverse effect (MAE/MAC), liability management
  (LME) blockers such as J.Crew, Serta, and Chewy protections, sacred rights,
  and amendment history. Use for private credit loan document review, lender
  protection and LME risk scoring, or term sheet versus final documentation
  checks.
---

# Credit agreement key terms and LME protections

Uses the **Continua Project Room** MCP server (`https://app.askcontinua.com/mcp?subset=mini`), which this plugin connects through `.mcp.json`. If its tools are missing or return an authorization error, ask the user to connect and sign in to **Continua Project Room** (Claude: Settings → Connectors; Claude Code: `/mcp`; a Continua account is required), then retry. Research can consume credits under the user's plan.

## 1. Brief

The instruments (credit agreement, indenture, intercreditor, security documents, and every amendment); the user's position (existing lender, prospective lender, sponsor, borrower); and which modules to run (default all):

| Module | What to extract |
| --- | --- |
| Change of control | Definition, triggers (ownership thresholds, board control, permitted holders, sale of substantially all assets), consequence (default, mandatory prepayment, 101% put), portability |
| MAE / MAC | Definition, carve-outs, disproportionate-effect exceptions, "prospects" language, where it is used (conditions precedent, representations, drawstop) |
| LME blockers | J.Crew (transfers of material IP or assets to unrestricted subsidiaries), Chewy (guarantor release when a guarantor stops being wholly owned), Serta (lien or payment subordination without affected-lender consent; open-market purchase and pro rata sharing language), non-pro-rata uptiers, drop-downs, and unrestricted subsidiary and investment basket capacity |
| Sacred rights | Changes needing each affected lender's consent: principal, rate, fees, maturity, pro rata sharing and waterfall, release of all or substantially all collateral or guarantees, voting thresholds and the Required Lenders definition |
| Amendment history | Each amendment, waiver, and consent: date, parties, what changed, vote required |

## 2. Room

Find or create the deal Room (`list_project_rooms`, `get_project_room`, `create_project_room`) and `upload_project_files` the agreement and **all** amendments. Add the term sheet or house LME standard if the user has one, so the review compares against it.

For a single clause ("what does the change of control definition say?"), `search_project_room` then `read_project_file` is enough.

## 3. Research

`research_project_room` with a question like:

> Review the borrower's credit agreement or indenture with all amendments in this Room as of the date in the brief, from the user's stated position, naming the borrower, instrument, and position explicitly. For change of control, MAE, LME blockers, and sacred rights: quote the operative language and the defined terms that control it, with section and page, and show every amendment that changed it, with dates. For each LME protection (J.Crew, Chewy, Serta, uptier, drop-down), say whether it is present, partial, or absent, and describe any gap concretely. Calculate unrestricted subsidiary and investment basket capacity only from figures stated in the documents, showing the calculation and assumptions. List each amendment, waiver, and consent with what changed and the vote required. Compare against the term sheet or house standard in this Room, if one was supplied. Keep ambiguous drafting visible. End with an LME scorecard (protection · present/partial/absent · clause · comment) and points to raise with counsel.

Drop modules the user did not ask for.

## 4. Collect and check

Poll `get_project_research` until done. For every "absent" or "partial" protection, confirm through `findings.json` and `read_project_file` that the report quotes the actual clause or shows its absence. Deliver the scorecard, the three most significant gaps, and the `report_url`.

## Rules

- This is a document review to brief counsel and the credit team, not legal advice; say so once.
- Quote the documents; do not stretch operative language into conclusions it does not support.
- J.Crew, Serta, and Chewy name transaction patterns; do not predict how a court would rule.
