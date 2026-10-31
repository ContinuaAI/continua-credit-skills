# Continua Credit Skills

Due diligence and credit underwriting skills for **private credit teams**, built on Continua Project Rooms.

Continua gives Claude up to **1B tokens of working context** to investigate an entire borrower package or data room: financial statements, contracts, invoices, and supporting records. These skills turn that evidence into credit memos, covenant tests, diligence findings, loan document reviews, and EBITDA analysis, with every material figure traceable to its source.

Reviewed conclusions go back into the Room's **Enterprise Second Brain**, a shared LLM wiki of team knowledge, policies, and decisions, with versioned files and member access controls, so the next deal starts from what your team already knows.

## Requirements

This plugin depends on the **Continua Project Room** MCP server and requires a Continua account. Access and usage follow your account permissions and plan.

```text
https://app.askcontinua.com/mcp?subset=mini
```

The plugin declares this server in [`.mcp.json`](.mcp.json) (Streamable HTTP with OAuth), so installing the plugin adds the connection:

1. Install the plugin. In Claude Code:

   ```text
   /plugin marketplace add ContinuaAI/continua-credit-skills
   /plugin install continua-credit-skills@continua-credit-skills
   ```

2. Sign in to **Continua Project Room**: in Claude, **Settings → Connectors**; in Claude Code, run `/mcp` and select `continua-project-room`. Users who already added Continua Project Room from the [Claude directory](https://claude.ai/directory/continua) are connected to the same server.

The skills call these Continua Project Room tools: `list_project_rooms`, `get_project_room`, `create_project_room`, `update_project_room`, `upload_project_files`, `search_project_room`, `read_project_file`, `research_project_room`, `get_project_research`, `list_project_file_history`, `restore_project_file`.

## Skills

| Skill | Use it for | What you get |
| --- | --- | --- |
| [`/continua-credit-skills:credit-memo`](skills/credit-memo/SKILL.md) | Underwriting, renewals, and annual reviews from a borrower package | Cited credit memo with ratios, risks, policy exceptions, and open items |
| [`/continua-credit-skills:covenant-compliance`](skills/covenant-compliance/SKILL.md) | Effective covenants after amendments, compliance tests, headroom, certificate reconciliation | Covenant table (threshold, actual, pass/fail, headroom) and discrepancies |
| [`/continua-credit-skills:data-room-diligence`](skills/data-room-diligence/SKILL.md) | Full data room review | Red flags, cross-document conflicts, missing documents, diligence request list |
| [`/continua-credit-skills:credit-agreement-terms`](skills/credit-agreement-terms/SKILL.md) | Change of control, MAE, LME blockers (J.Crew, Serta, Chewy), sacred rights, amendment history | Quoted terms and an LME protection scorecard |
| [`/continua-credit-skills:ebitda-addbacks`](skills/ebitda-addbacks/SKILL.md) | QoE-style test of adjusted EBITDA | Item-by-item accept / adjust / reject view and diligence-adjusted EBITDA |

Every skill follows the same flow: find or create the Room → upload sources → `research_project_room` → poll `get_project_research` → check citations against `findings.json` and the source files → deliver the report and its link. Follow-ups continue the same research with `previous_research_id`, and reviewed decisions can be saved back to the Room as text.

## Example requests

- "Underwrite Acme Ltd for a $25m term loan as of 30 June using the Acme Room."
- "Test Q3 covenants for Acme after the second amendment and reconcile the compliance certificate."
- "Run buy-side diligence on Project Falcon's data room and draft the request list."
- "Score the LME protections in this credit agreement and all amendments."
- "Test the sponsor's adjusted EBITDA bridge against the covenant definition and its cap."

## Layout

```text
continua-credit-skills/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── .mcp.json              # Continua Project Room MCP (subset=mini)
├── skills/
│   ├── credit-memo/SKILL.md
│   ├── covenant-compliance/SKILL.md
│   ├── data-room-diligence/SKILL.md
│   ├── credit-agreement-terms/SKILL.md
│   └── ebitda-addbacks/SKILL.md
├── .gitignore
├── LICENSE
└── README.md
```

## Local development

```bash
claude --plugin-dir .
claude plugin validate .
```

## Support and policies

- Product and documentation: [Continua Project Room](https://askcontinua.com/project-room) · [Help Center](https://askcontinua.com/help#projects)
- Support: [askcontinua.com/help](https://askcontinua.com/help)
- [Privacy policy](https://app.askcontinua.com/privacy-policy) · [Terms of service](https://app.askcontinua.com/terms-of-service) · [Trust & Security](https://askcontinua.com/trust)

Outputs are drafts for credit and deal teams to review. They are not credit decisions or legal, tax, accounting, or investment advice.

[Continua AI](https://askcontinua.com)
