# Claude for Legal — Plugins & Agents

Reference agents, skills, and data connectors for the legal workflows we see most — in-house commercial, privacy, product, corporate, employment, litigation, regulatory, AI governance, and IP, plus the learning side of the practice (law-school clinics and students). Install the plugins that match your work, run each one's cold-start interview, then tune them to how your team operates.

> [!IMPORTANT]
> **Every output from these plugins is a draft for attorney review — not legal advice, not a legal conclusion, and not a substitute for a lawyer.** They are built with guardrails that reflect that: source attribution on every citation, conservative defaults on privilege and subjective legal calls, jurisdiction assumptions surfaced, and explicit gates before anything is filed, sent, or relied on. A licensed attorney reviews, verifies, and takes professional responsibility for anything that leaves the building. These plugins make that review faster; they do not replace it. They do not represent Anthropic's legal positions. See [Important Risks Using These Plugins](#important-risks-using-these-plugins).

## What's in here

Thirteen plugins, grouped by where the work sits:

- **Advisory & transactional (8)** — `commercial-legal`, `corporate-legal`, `privacy-legal`, `product-legal`, `employment-legal`, `ai-governance-legal`, `regulatory-legal`, `ip-legal`.
- **Litigation (1)** — `litigation-legal`.
- **Learning & practice (2)** — `law-student`, `legal-clinic`.
- **Ecosystem (1)** — `legal-builder-hub` (discover and install community legal skills behind a trust gate).
- **Partner-built (1)** — `cocounsel-legal` by Thomson Reuters (Westlaw Deep Research).

Each plugin ships its skills, scheduled "watcher" agents, and the data connectors its workflows use. Skills are invoked as `/<plugin>:<skill>` (e.g. `/commercial-legal:review`) or fire automatically when your request matches.

## Installation (manual upload)

These steps cover uploading the plugins by hand — no GitHub connector required. Prebuilt, ready-to-upload zips live in [`claude-upload-packages/individual-plugins/`](./claude-upload-packages/individual-plugins) — one per plugin.

> [!NOTE]
> The upload dialog validates **exactly one `plugin.json` per zip**, so you upload each plugin individually and group them under one marketplace as you go. A single archive containing all plugins is rejected.

**Upload the first plugin — create the marketplace**

1. In claude.ai, go to **Admin settings → Plugins → Add plugins → Upload plugin**.
2. Choose **Upload to a new marketplace** and give it a name (e.g. `claude-for-legal`).
3. Drag in your first plugin zip (e.g. `commercial-legal.zip`) and click **Upload**.

**Add the remaining plugins to that marketplace**

4. Open the dialog again, choose **Add to an existing marketplace**, and select the marketplace you just created.
5. Drag in the next zip and click **Upload**. Repeat for each plugin you want.

When you're done, every uploaded plugin appears in that marketplace as **Available to install**; install the ones you want from the list.

> [!IMPORTANT]
> **After installing, run the cold-start interview first** (`/<plugin>:cold-start-interview`) and **connect a research tool** (see [MCP connectors](#mcp-connectors)). The interview writes your practice profile that every other skill reads; the research connectors are what let Claude verify citations against authoritative databases instead of relying on training knowledge.

### Building a zip from source

The prebuilt zips are convenient, but you'll want to repackage a plugin after you customize it. Each plugin directory carries its own `.claude-plugin/plugin.json`; zip the directory and upload it:

```bash
zip -r commercial-legal.zip commercial-legal -x '*.DS_Store'
```

### Other ways to install

- **Cowork (from this repo):** Settings → Plugins → Add plugin → paste the repo URL, then pick plugins from the marketplace list.
- **Claude Code:** `claude plugin marketplace add <your-org>/<repo>`, then e.g. `claude plugin install commercial-legal@claude-for-legal`.
- **Claude Managed Agents:** several plugins also ship as [Managed Agent templates](./managed-agent-cookbooks) for headless deployment.

## How to use these plugins

- **Skills via `/<plugin>:<skill>`** — type `/` in a session to see what a plugin exposes, then pick one (e.g. `/litigation-legal:chronology`). Skills are the building blocks; many also fire automatically when your request matches.
- **Just describe the task** — plainly state what you need and attach the documents; the matching skill activates.
- **Scheduled "watcher" agents** — each plugin includes background agents that run on a cadence (renewal-watcher, docket-watcher, leave-tracker, reg-change-monitor, …) and post to the channel you configure. Trigger them on demand too ("check renewals", "any new filings").
- **Connectors** — plugins read from the systems legal teams live in (Slack, Google Drive, CourtListener, iManage, Ironclad, Everlaw, and more). Connect a research tool first so citations are verified and tagged by source.

## All plugins at a glance

| Plugin | Group | What it's for |
|---|---|---|
| `commercial-legal` | Advisory | Playbook review of NDAs/MSAs/SaaS, amendment tracing, renewal alerts, escalation routing |
| `corporate-legal` | Advisory | M&A diligence (cited tabular review), disclosure schedules, board consents, entity compliance |
| `privacy-legal` | Advisory | PIA/DPIA triage, PIA generation, DPA review, DSAR response, policy-drift monitoring |
| `product-legal` | Advisory | Launch review, marketing-claims check, "is this a problem?" triage |
| `employment-legal` | Advisory | Hire/termination review, worker classification, leave tracking, investigations |
| `ai-governance-legal` | Advisory | AI use-case triage, impact assessments, vendor AI review, reg-gap analysis |
| `regulatory-legal` | Advisory | Reg-feed watcher, policy diff, gap tracker, Monday-morning digest |
| `ip-legal` | Advisory | TM clearance, FTO triage, C&D/DMCA, OSS compliance, portfolio deadlines |
| `litigation-legal` | Litigation | Matters/holds/demands; chronologies, claim charts, depo prep, privilege logs, briefs |
| `law-student` | Learning & practice | Socratic drills, case briefs, outlines, IRAC grading, bar prep — learning mode |
| `legal-clinic` | Learning & practice | Clinic setup, intake, deadlines, memos, client letters, semester handoff (ABA Op. 512) |
| `legal-builder-hub` | Ecosystem | Discover/install community legal skills behind a security & trust gate |
| `cocounsel-legal` | Partner (Thomson Reuters) | Westlaw Deep Research — fully cited, linked reports across jurisdictions |

## Important Risks Using These Plugins

AI-generated legal work is a starting point, not a finished work product. Every output must be reviewed by a licensed attorney who takes professional responsibility for it before it is filed, sent, signed, or relied on. The points below apply to all of the plugins in this guide.

- **Not legal advice; drafts for attorney review** — These tools do not provide legal advice, do not create an attorney-client relationship, and do not substitute for a lawyer's judgment. Every output is a draft for a qualified attorney to review, verify, and own.
- **Verify every citation against an authoritative source** — AI can fabricate or misstate case law, statutes, holdings, deadlines, and quotations. Confirm every authority exists, is current/good law, and stands for what the output claims, using a primary source or research database — not the model's say-so. Cites not verified through a research connector are flagged `[verify]`; treat them as unconfirmed.
- **Connect your research tools first** — The plugins are most trustworthy wired to authoritative sources (CourtListener, Westlaw/CoCounsel, and others). Connected, they pull from current databases and tag each cite by source; without one, outputs lean on training knowledge and must be checked independently.
- **Never the sole basis for a legal decision or action** — Do not file, send, sign, advise on, or rely on any AI-generated work product without independent attorney analysis and the required sign-off. Use these tools to accelerate review, not to replace it.
- **Confidentiality and privilege** — These tools handle privileged and confidential client material. Follow your duty of confidentiality, your firm's data-handling and information-barrier policies, and your retention obligations, and take care that AI use and connector configuration do not waive privilege.
- **Jurisdiction-specific and evolving law** — The law differs by jurisdiction and changes constantly. Outputs surface jurisdiction assumptions, but you must confirm the governing law, the current rules and deadlines, and any local variations before relying on them.
- **Unauthorized practice of law** — These tools are for use under the supervision of a licensed attorney. They are not legal representation; non-lawyers should consult a qualified attorney rather than rely on output. `legal-clinic` is built within ABA Formal Opinion 512.
- **Deadlines are aids, not a system of record** — Renewal, filing, leave, DSAR, and comment-period tracking help you spot what's coming; they are not a docketing system of record. Verify every computed date against the controlling rule and your authoritative calendar — a missed deadline can be malpractice.
- **Data accuracy and AI errors** — Outputs can miscalculate, misread a document, omit material facts, or state confidently incorrect details ("hallucinations"). Clean formatting is not evidence of accuracy; read the underlying documents.
- **Templates to tune, not turnkey** — These are reference templates reflecting general practice and do not represent Anthropic's legal positions. Run each plugin's cold-start interview, adapt it to your jurisdiction, playbook, and house style, and confirm behavior before depending on it.

## Plugin reference

Run `/<plugin>:cold-start-interview` first in any plugin — it tailors the rest to your practice.

### Advisory & transactional

#### commercial-legal

**What it does:** Playbook-aware review of vendor agreements, NDAs, and SaaS subscriptions; amendment tracing across base and amendments; a renewal register with cancel-by alerts; escalation routing; and business-stakeholder summaries.

**How to use:** `/commercial-legal:cold-start-interview`, then `:review`, `:amendment-history`, `:renewal-tracker`, `:escalation-flagger`. Scheduled agents: renewal-watcher, deal-debrief, playbook-monitor.

**Try this:** `/commercial-legal:review` — review this vendor MSA against our purchasing playbook and flag deviations with fallback positions. *(attach the agreement)*

#### corporate-legal

**What it does:** M&A diligence at scale with cited, one-row-per-document tabular review; disclosure schedules and closing checklists; board consents and minutes in house format; an entity-compliance tracker across jurisdictions; and post-close integration tracking.

**How to use:** `/corporate-legal:cold-start-interview` (optionally `--new-deal`), then `:tabular-review`, `:diligence-issue-extraction`, `:material-contract-schedule`, `:closing-checklist`, `:written-consent`, `:entity-compliance`, `:integration-management`. Scheduled agent: dataroom-watcher.

**Try this:** `/corporate-legal:tabular-review` — review the VDR contracts folder, one row per document with every cell cited to its source.

#### privacy-legal

**What it does:** Privacy triage (PIA vs GDPR DPIA vs proceed), PIA generation, DPA review that auto-detects controller vs processor, DSAR response within statutory timelines, and a policy monitor that watches drift between policy and practice.

**How to use:** `/privacy-legal:cold-start-interview`, then `:use-case-triage`, `:pia-generation`, `:dpa-review`, `:dsar-response`, `:reg-gap-analysis`, `:policy-monitor`.

**Try this:** `/privacy-legal:dsar-response` — walk this access request: verify identity, locate the data, assess exemptions, and draft the response within the deadline.

#### product-legal

**What it does:** Launch review against your house risk calibration, marketing-claims checks for statements that need substantiation, fast "is this a problem?" triage for the quick-question channel, and feature risk assessments.

**How to use:** `/product-legal:cold-start-interview`, then `:is-this-a-problem`, `:launch-review`, `:marketing-claims-review`. Scheduled agent: launch-watcher (flags upcoming launches before product counsel gets surprised).

**Try this:** `/product-legal:marketing-claims-review` — check this landing-page copy for claims that need substantiation.

#### employment-legal

**What it does:** Hire and termination review with jurisdiction-specific risk flags, worker classification against the controlling state test, a leave tracker (FMLA/CFRA/PFL/ADA) that fires decision-point alerts, internal investigations, and policy drafting with state supplements.

**How to use:** `/employment-legal:cold-start-interview`, then `:hiring-review`, `:termination-review`, `:worker-classification`, `:wage-hour-qa`, `:policy-drafting`, the investigation suite (`:investigation-open` / `:investigation-add` / `:investigation-memo` / `:investigation-query` / `:investigation-summary`), and `:leave-tracker` / `:log-leave`. Scheduled agent: leave-tracker.

**Try this:** `/employment-legal:termination-review` — review this proposed termination in California for high-risk flags.

#### ai-governance-legal

**What it does:** Triage of proposed AI use cases against your registry, impact assessments across the regimes in scope, vendor AI-terms review (training-on-data, liability gaps), reg-to-policy gap analysis, and an EU AI Act per-system inventory.

**How to use:** `/ai-governance-legal:cold-start-interview`, then `:use-case-triage`, `:aia-generation`, `:vendor-ai-review`, `:reg-gap-analysis`, `:ai-inventory`, `:policy-starter`, `:policy-monitor`.

**Try this:** `/ai-governance-legal:use-case-triage` — classify this proposed AI feature (approved / conditional / no) against our registry and the EU AI Act.

#### regulatory-legal

**What it does:** A regulatory feed watcher, policy diffs against new rules, a gaps tracker, NPRM comment-period tracking, and the Monday-morning digest your team actually reads.

**How to use:** `/regulatory-legal:cold-start-interview`, then `:reg-feed-watcher`, `:policy-diff`, `:gaps`, `:policy-redraft`, `:comments`. Scheduled agent: reg-change-monitor (materiality-filtered digest).

**Try this:** `/regulatory-legal:policy-diff` — diff this new rule against our policy library and surface the open gaps.

#### ip-legal

**What it does:** First-pass trademark clearance and freedom-to-operate triage, invention-disclosure screening, cease-and-desist drafting and triage, DMCA takedown and counter-notice, open-source license compliance, IP clause review, and portfolio/renewal tracking.

**How to use:** `/ip-legal:cold-start-interview`, then `:clearance`, `:fto-triage`, `:invention-intake`, `:cease-desist`, `:takedown`, `:infringement-triage`, `:ip-clause-review`, `:oss-review`, `:portfolio`. Scheduled agent: ip-renewal-watcher.

**Try this:** `/ip-legal:clearance` — run a first-pass trademark clearance for the mark "Northwind" in class 9.

### Litigation

#### litigation-legal

**What it does:** Manages the litigation portfolio (matters, deadlines, legal holds, demands, outside counsel) and does the work — claim charts (patent and civil), chronologies, deposition prep, privilege-log review, and brief drafting. Adapts to in-house, firm, or solo practice.

**How to use:** `/litigation-legal:cold-start-interview`, then portfolio skills (`:matter-intake`, `:portfolio-status`, `:legal-hold`, `:demand-draft`, `:demand-received`, `:subpoena-triage`, `:oc-status`) and work product (`:claim-chart`, `:chronology`, `:deposition-prep`, `:privilege-log-review`, `:brief-section-drafter`). Scheduled agent: docket-watcher.

**Try this:** `/litigation-legal:chronology` — build a chronology from the uploaded exhibits and the complaint, with a source cited for each event.

### Learning & practice

#### law-student

**What it does:** A learning-mode study partner: Socratic drilling, case briefing, outline building, IRAC grading, cold-call prep, flashcards, jurisdiction-tuned bar prep, exam forecasting, and study planning. It never writes the answer for you.

**How to use:** `/law-student:cold-start-interview`, then `:socratic-drill`, `:case-brief`, `:outline-builder`, `:irac-practice`, `:legal-writing`, `:cold-call-prep`, `:bar-prep-questions`, `:flashcards`, `:exam-forecast`, `:study-plan`.

**Try this:** `/law-student:socratic-drill` — drill me on personal jurisdiction; ask, let me answer, then push back on my reasoning.

#### legal-clinic

**What it does:** Stands up a law-school clinic: professor setup and per-practice-area guides, student semester ramp, structured intake with cross-area issue spotting, malpractice-aware deadline tracking, memo scaffolds, client letters (routine and plain-language), and semester handoffs — built within ABA Formal Opinion 512.

**How to use:** Professors: `/legal-clinic:cold-start-interview`, `:build-guide`, `:supervisor-review-queue`. Students: `:ramp`, `:client-intake`, `:research-start`, `:memo`, `:draft`, `:client-letter`, `:status`, `:deadlines`, `:semester-handoff`.

**Try this:** `/legal-clinic:client-intake` — run a structured intake for a new housing matter and spot issues across practice areas.

### Ecosystem

#### legal-builder-hub

**What it does:** Finds, evaluates, and installs community legal skills with a real trust layer — watched registries, a QA framework, SHA-pinned updates, a deployment-aware license gate, and a mandatory security review before anything lands in your environment.

**How to use:** `/legal-builder-hub:cold-start-interview`, then `:registry-browser`, `:skill-installer`, `:skills-qa`, `:related-skills-surfacer`, `:auto-updater`, `:disable`, `:uninstall`. Scheduled agent: registry-sync.

**Try this:** `/legal-builder-hub:registry-browser` — find community skills for trademark prosecution and run the trust check before installing anything.

### Partner-built

#### cocounsel-legal (Thomson Reuters)

**What it does:** Westlaw Deep Research that delivers fully cited reports — caselaw, statutes, regulations, Practical Law, and secondary sources — with inline, linked citations, across up to three U.S. jurisdictions per run. Built and supported by Thomson Reuters; requires a CoCounsel Legal subscription with the MCP connector enabled (support: cocounselsupport@tr.com).

**How to use:** `/cocounsel-legal:deep-research`.

**Try this:** `/cocounsel-legal:deep-research` — research the enforceability of non-competes for software engineers in California, New York, and Texas.

## MCP connectors

> [!IMPORTANT]
> **Connect a research tool first.** Connected, Claude pulls from authoritative sources and verifies its citations against current databases instead of relying on training knowledge; cites are tagged by source. Citations from model knowledge alone are flagged `[verify]`, and if no research tool is connected the reviewer note records that sources weren't verified. The connectors are what make the cites trustworthy — set them up before anything else.

| Connector | What it gives Claude | Plugins | Notes |
|---|---|---|---|
| **Slack** | Read channels, search, send messages and canvases | all plugins | Your workspace |
| **Google Drive** | Read docs, sheets, slides; fetch by link | all plugins | Your account |
| **CoCounsel Legal (Thomson Reuters)** | Westlaw Deep Research — cited reports across caselaw, statutes, regulations, Practical Law | `cocounsel-legal` | Subscription; OAuth |
| **Box** | Read files and folders in VDRs and matter rooms | `corporate-legal` | Your tenant |
| **Ironclad** | Read the contract register, renewal dates, clauses | `commercial-legal` | Subscription |
| **DocuSign / DocuSign CLM** | Envelope status, executed contracts, CLM metadata | `commercial-legal` | Subscription |
| **iManage** | Read from the DMS — matter workspaces, document versions | `commercial-legal`, `corporate-legal` | Subscription |
| **Everlaw** | E-discovery productions, tagged sets, chronologies | `litigation-legal` | Subscription |
| **CourtListener** | Federal dockets and opinions | `legal-clinic`, `ip-legal`, `litigation-legal`, `law-student` | Public; optional API key |
| **Trellis** | State court dockets and motions | `litigation-legal` | Subscription |
| **Aurora** | Clinic-style matter management and calendaring | `litigation-legal` | Subscription |
| **Definely** | In-document drafting and defined-terms checks | `commercial-legal`, `corporate-legal` | Subscription |
| **Lawve AI** | Contract review assist and clause libraries | `legal-builder-hub` | Subscription |
| **Courtroom5** | Self-represented litigant workflow | `legal-clinic` | Subscription |
| **Descrybe** | Case law research and summarization | `legal-clinic`, `ip-legal`, `law-student` | Subscription |
| **Solve Intelligence** | Patent drafting and prosecution | `corporate-legal`, `ip-legal` | Subscription |
| **TopCounsel** | Matter routing and outside-counsel panel | `commercial-legal`, `corporate-legal`, `litigation-legal` | Subscription |
| **Linear / Atlassian (Jira) / Asana** | Launch and issue tracking | `product-legal` | Your workspace |

> Connectors marked "subscription" need the customer's own account and API key. Configure them in each plugin's `.mcp.json` or via `claude mcp`. Building a connector? See [CONNECTORS.md](./CONNECTORS.md).

## Making It Yours

These are reference templates — they get better when you tune them to how your team works, and the customization mechanism is the plugin itself, not a buried config file.

- **Run the cold-start interview** — it *is* the customization mechanism. It asks how your practice works, reads your seed documents, and writes your practice profile; every other skill reads from it. A `/commercial-legal:cold-start-interview` with five signed MSAs, your playbook, and your escalation matrix makes the review skills noticeably sharper.
- **Edit the practice profile** — it lives at `~/.claude/plugins/config/claude-for-legal/<plugin>/CLAUDE.md`. Edit it directly for small fixes; it survives plugin updates.
- **Swap connectors** — point `.mcp.json` at your CLM, DMS, e-discovery platform, launch tracker, or HRIS. Skills fall back gracefully when a connector isn't configured.
- **Bring your playbook and templates** — drop your terminology, house style, and branded templates into the plugin's `CLAUDE.md` and `references/`.
- **Fork skills for house style** — every skill is a markdown file under `<plugin>/skills/`; edit the steps, gates, and output format.
- **Add scheduled agents** — the agents under `<plugin>/agents/` are markdown with a cron-style schedule; add your own watchers.

## Repository layout

```
<plugin>/                      # one directory per plugin (commercial-legal, litigation-legal, …)
  .claude-plugin/plugin.json   #   plugin manifest
  skills/<skill>/SKILL.md      #   skills, invoked as /<plugin>:<skill>
  agents/<name>.md             #   scheduled "watcher" agents
  .mcp.json                    #   data connectors for this plugin
external_plugins/              # partner-built plugins (cocounsel-legal)
managed-agent-cookbooks/       # Managed Agent cookbooks for headless deployment
references/                    # shared reference material
claude-upload-packages/        # prebuilt, ready-to-upload plugin zips (manual install)
scripts/                       # repo tooling
```

## Claude for Microsoft 365

Lawyers live in Word and Excel. Every contract-touching skill here is authored to work in the **Claude for Word sidebar, with tracked changes as the output mode** — `commercial-legal:review`, `commercial-legal:amendment-history`, `ip-legal:ip-clause-review`, `ai-governance-legal:vendor-ai-review`, `privacy-legal:dpa-review`, and the diligence extraction in `corporate-legal`. Excel-facing skills produce clean workbooks: `corporate-legal:tabular-review`, `litigation-legal:claim-chart`, `corporate-legal:entity-compliance`, and `commercial-legal:renewal-tracker`.

Install Claude for Microsoft 365 from [Microsoft AppSource](https://marketplace.microsoft.com/en-us/product/office/wa200010453); the skills from any enabled plugin are then available from the sidebar via `/`. For IT admins deploying the add-in against your own cloud (Vertex AI, Bedrock, or an internal gateway) rather than Anthropic's API, see the separate `claude-for-msft-365-install` tooling. Firms using Claude directly from Anthropic's hosted service don't need that step.

## Contributing

Everything here is markdown and JSON — fork, edit, PR.

- **New skill** → add `<plugin>/skills/<skill-name>/SKILL.md` with `name`, `description`, and `argument-hint` frontmatter; keep the description under 1024 characters (it's the trigger signal). It's invokable as `/<plugin>:<skill-name>`. Mark pure-reference skills `user-invocable: false`.
- **New agent** → add `<plugin>/agents/<name>.md` with scheduling frontmatter and the system prompt, plus a matching `managed-agent-cookbooks/<name>/` for headless deployment.
- **Community skills** → use `/legal-builder-hub:skill-installer`; the hub runs `/legal-builder-hub:skills-qa` against every skill before installing.
- **Validate cookbooks** → `bash scripts/test-cookbooks.sh` before pushing.

## License

Licensed under the [Apache License, Version 2.0](LICENSE).

