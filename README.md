# Opportunity research

A new agent-native research project for Jordan's business and economic opportunity
discovery, rooted in Chesterfield with relevant regional and national connections.

The user-facing deliverable is a continually maintained, multi-page static HTML
research site with a stable `index.html` entry point. It brings together opportunity
briefs, hypotheses, evidence, methods and change history. Each accepted research
cycle updates the current conclusions while preserving why hypotheses were
confirmed, discarded or reopened. The proposed local output root is
`<workspace>/private/site/`, separate from project code and retained evidence.

The process is retained source files → SQLite provenance and queryable records →
entities/relationships → reproducible analyses and reports → findings/hypotheses
→ the maintained site. Start with one connected public research database and a
separate private store; use tables for functional boundaries. Additional databases
need a demonstrated reason. A reader should be able to follow each significant
finding through its report and evidence to the original source.

The selected stack is **Python + SQL**, with **HTML/CSS** for the generated site
and optional JavaScript for browser-side enhancements. `uv` manages the Python
environment and locked dependencies. Agents handle research and judgment; reusable
commands and saved queries handle repeatable operations. Workers exchange validated
JSON packets; Markdown and structured records remain the editable research sources.

**Current state: design and implementation planning only.** There is no research
CLI, database, acquisition job or generated-report browser in this directory yet.
The legacy app and its retained evidence are preserved; its roadmap is not the
roadmap for this project. No source acquisition or private research was performed
as part of creating these plans.

To begin a fresh session, select `gpt-6.1-sol` and submit the complete prompt in
[start-session.md](start-session.md). The supervisor coordinates bounded subagent
work, updates verified project state, and rewrites that prompt at a clean stopping
point so the next session can resume without the old chat. The prompt file itself
is not a running job. [AGENTS](AGENTS.md#fresh-sessions-and-handoff) defines the
standing handoff rules.

The supervisor also surfaces [manual tasks that unblock the team](AGENTS.md#manual-tasks-that-unblock-the-team)
promptly, with concrete steps and a verification check, while continuing independent
work. Pending tasks carry forward into the next session; credentials stay out of chat.

Reference documents (load only what the active packet needs):

1. [Pipeline design](PIPELINE_DESIGN.md): research loop, files/SQLite, evidence and
   entity/relationship model, derivations, hypotheses, reports and boundaries.
2. [Implementation plan](IMPLEMENTATION_PLAN.md): first ready packet, phased
   deliverables, dependencies, acceptance checks and working templates.
3. [Project instructions](AGENTS.md): `gpt-6.1-sol` supervisor and workers, exclusive
   ownership, integration and continuity rules. These instructions are scoped to
   this project and supersede the parent's Astra/web-app workflow here.
4. [First research question](FIRST_RESEARCH_QUESTION.md): a bounded public-purchasing
   pilot to select two or three recurring service responsibilities for investigation.
5. [Project context](PROJECT_CONTEXT.md): portable purpose, personal unknowns,
   opportunity criteria and evidence gates.

Start by producing a comparative opportunity brief and a minimal set of linked
HTML pages using existing tools. Introduce
reusable pipeline code only when that work reveals a concrete bottleneck. The
acceptance question is “What can Jordan decide or test now?”

This directory is now its own Git repository root. The portable planning bundle is
this README, start-session.md and the five reference documents above; their internal links remain valid
without the old checkout. The additional `osier_family_strategic_framework.md` is
optional personal background, not a required file to copy or publish. Code and safe
templates belong in this project; runtime evidence, databases and private synthesis belong in
an explicitly selected external local workspace, described in the design. The
plan's proposed directories/commands are not claims of implemented capability.

Use the [project context](PROJECT_CONTEXT.md), derived from the family strategic framework, for
purpose and assessment criteria. Hours, capital at risk and family participation
remain unresolved; agents must not invent them. Existing-agent research is the
intended workflow; a web application, API, chatbot or general agent framework is
not required.
