---
name: covenant-compliance
description: >-
  Reconstruct effective financial covenants after every amendment and waiver,
  test compliance, calculate headroom, and reconcile the compliance certificate,
  with each figure cited to the source documents in a Continua Project Room. Use
  for private credit covenant monitoring, DSCR, leverage, and interest cover
  tests, compliance certificate review, quarterly portfolio reviews, or
  comparing this quarter with the last using versioned files.
---

# Covenant compliance and headroom

Uses the **Continua Project Room** MCP server (`https://app.askcontinua.com/mcp?subset=mini`), which this plugin connects through `.mcp.json`. If its tools are missing or return an authorization error, ask the user to connect and sign in to **Continua Project Room** (Claude: Settings → Connectors; Claude Code: `/mcp`; a Continua account is required), then retry. Research can consume credits under the user's plan.

## 1. Brief

Borrower, facility, and test date / period (for example LTM to 30 June). Which documents govern: the credit agreement and every amendment, waiver, and consent, in order.

## 2. Room

Find the borrower's Room with `list_project_rooms` and `get_project_room`; otherwise `create_project_room` and `upload_project_files`. The Room must hold the agreement **and all amendments and waivers**, the compliance certificate, and the financials it relies on. If an amendment is referenced but not uploaded, report the gap; do not assume its terms.

Quick lookups (for example "where is the leverage definition?") can use `search_project_room` and then `read_project_file`. The full test needs research across the whole Room.

## 3. Research

`research_project_room` with a question like:

> As of the test date from the brief, reconstruct every financial covenant in the borrower's facility (name both explicitly) after all amendments, waivers, and consents. For each covenant, give the threshold, step-down schedule, governing clause, and amendment history with effective dates. Quote the defined terms that drive each test (EBITDA, total debt, fixed charges, cash netting, permitted add-backs and caps, equity cure rights, testing frequency). Recalculate each covenant for the test period ending on that date from the financials using those definitions, showing inputs, result, pass/fail, and headroom as an amount and a percentage. Show how far EBITDA can fall before each test fails. Reconcile the compliance certificate figure by figure and flag every difference. Note equity cures, waivers, and reservation-of-rights letters. List missing documents. Cite clause and page for every term, and source for every input.

## 4. Collect and check

Poll `get_project_research` about every 5 seconds until done. Use `findings.json` to map citations `[n]`, and open the sources behind any failed test or certificate discrepancy with `read_project_file` before reporting it.

Lead with a table: covenant · threshold · actual · pass/fail · headroom. Then certificate discrepancies, step-downs in the next two test dates, and missing documents. Share the `report_url`.

## 5. Next quarter

Upload the new financials and certificate to the same Room, then `research_project_room` with `previous_research_id` set to last quarter's research and ask for the re-test plus changes versus the prior quarter. Use `list_project_file_history` if you need to compare against an earlier version of a document.

## Rules

- Apply the agreement's own definitions, not market-standard ones.
- Whether a failed test is an Event of Default is for the lender's counsel; report "test failed on these calculations."
- Never invent thresholds or amendment terms.
