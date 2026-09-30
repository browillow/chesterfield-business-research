# Agent-native opportunity research pipeline

September 30, 2026. **Design only; none of the proposed pipeline commands or stores
exist yet.** Implementation sequence: [IMPLEMENTATION_PLAN](IMPLEMENTATION_PLAN.md).
Working rules and requested models: [AGENTS](AGENTS.md).

## 1. Outcome and design constraint

Help Jordan identify useful responsibilities, business models and ownership
opportunities that could produce dependable earnings, transferable capability and
local contribution within family capacity. Chesterfield is the anchor; regional
suppliers, customers and outside revenue stay in scope. A software product is one
possible outcome, not the default.

The primary user-facing output is a continually maintained, multi-page static
HTML research site in one stable local root directory. Its decision briefs explain
which opportunities to advance, reject or investigate next, and why. Each accepted
research cycle updates the site as evidence, hypotheses and recommendations change;
pipeline enhancements update its methods and coverage where relevant. Success is
a decision improved, a weak thesis rejected,
a material unknown resolved or a worthwhile experiment defined. Collection volume,
entity counts and report polish are diagnostic measures, not success criteria.

Build only enough machinery to answer a named research question. The first
comparative brief should be possible with retained files, a simple evidence
register and existing tools. Repeated friction in that work determines which
pipeline component is built next. No requirement to model the whole county first.

The portable [project context](PROJECT_CONTEXT.md) preserves the purpose and
opportunity criteria drawn from the Osier Family Strategic Framework. Its business
vehicles remain alternatives.
Available hours, capital at risk, required income and family participation are
unresolved personal inputs; record them as unknown rather than inventing values.
The older Chesterfield County Digital Model brief is optional background for
research coverage, not an obligation to implement its application.

The first proposed investigation is [Q001: recurring purchased services](FIRST_RESEARCH_QUESTION.md).
It uses observable public purchasing to exercise the pipeline and select a deeper
research question; its procurement sample does not rank all county opportunities.

## 2. Research loop

```text
Decision question + constraints
    -> retained original public/private source files
    -> SQLite source catalog + provenance + queryable extracted records
    -> cited entity/relationship and claim proposals
    -> reproducible calculations + derived reports
    -> supported findings + competing hypotheses + counterevidence
    -> comparative opportunity brief + updated static research site
    -> next discriminating research action / proposed market test
    -> update the question and evidence
```

This is an iterative loop, not a waterfall requiring every stage for every source.
A cited document can support a useful brief without entity extraction. A calculation
needs structured inputs; a new entity table needs repeated comparison or joins to
justify it. A broad scan can precede narrowing without triggering exhaustive intake.

Each investigation starts with a short packet containing:

- Decision, question, geography/time scope and intended output.
- Competing explanations and the observation that would change the recommendation.
- Source families and processing destinations allowed for that investigation.
- Effort/transfer bounds, owner, stop conditions and personal constraints still unknown.

Example question, not a selected opportunity: “Which recurring administrative
handoff imposes meaningful cost on an identifiable group of local organizations,
and could Jordan take responsibility for it at attractive delivery economics?”
“County growth implies demand for an AI service” is an unsupported leap, not a thesis.

## 3. Small architecture and physical layout

Use local Python scripts, SQLite and ordinary files. Existing agents conduct
research through focused assignments and a small CLI; they do not require a new
agent runtime, chatbot, API or scheduler. Standard-library capabilities are the
default; choose and lock extra packages only when the first workload requires them.

### Language and tooling decisions

| Language/format | Responsibility |
| --- | --- |
| Python | Acquisition, parsing, normalization, validation, CLI commands, calculations and static-site generation |
| SQL | SQLite schema, provenance and entity/relationship queries, joins, aggregations and saved analytical queries |
| HTML/CSS | Generated multi-page research site, shared navigation and local styling |
| JavaScript, optional | Browser-side search, filtering or interactive charts when needed; core browsing remains usable without it |
| Markdown and JSON | Editable research prose and structured records/proposals; these are interchange formats, not additional runtimes |

Use `uv` to manage the Python environment and locked dependencies. When code begins,
declare the supported Python version in `pyproject.toml`, commit the lockfile and
verify the environment from a fresh checkout. Version selection belongs to that
implementation packet. Python's standard-library `sqlite3` and `argparse` are the
initial database and CLI options; add parsing or rendering libraries only for an
actual input/output need. References: [sqlite3](https://docs.python.org/3/library/sqlite3.html),
[argparse](https://docs.python.org/3/library/argparse.html) and
[uv locking/syncing](https://docs.astral.sh/uv/concepts/projects/sync/).

Agents select questions, collect evidence, propose analyses, challenge conclusions
and integrate findings. Reusable Python commands and saved SQL handle repeatable
mechanics. Workers invoke established ingestion/validation operations rather than
rewriting them for each question. New analytical scripts remain inspectable and
retain their inputs and method; generalize them when repeated use warrants it.

Use typed Python interfaces with explicit runtime validation at packet boundaries.
Workers return validated JSON with stable identifiers and evidence references;
the supervisor reviews and imports it through one writer. CLI result JSON goes to
stdout and diagnostics to stderr, with clear nonzero failure codes. Preserve scripts,
queries and tests alongside the code rather than relying on transient agent state.
The stack is chosen for concise, inspectable programs and reliable handoffs; no
assumption about a model's relative language proficiency is required.

### Storage and project layout

Files retain original evidence; SQLite catalogs provenance and holds the selected
normalized records that agents need to query and join. Ingestion does not replace
original files with a database copy. Parsed text/tables are separately identified
outputs. Derived analyses live in their own directories and reference both their
inputs and catalog records. The site renders reviewed findings and research
outcomes from these retained materials.

Start with one `research.sqlite` for connected public research functions: source
catalog, evidence, entities, relationships, calculations, questions, hypotheses,
findings and run metadata. Separate tables provide functional boundaries without
prematurely separating databases. A private `strategy.sqlite` is a privacy
boundary: it can catalog private originals/extractions as well as confidential
research and strategic assessments. Private evidence and conclusions must not be
inserted into the public store. If private data becomes a substantial workload,
the same evidence model can be used there.

Create additional databases only for a demonstrated access, lifecycle, independent
workload or scale requirement. Record stable cross-store references and integration
rules if a split becomes necessary; do not assume foreign keys enforce references
between separate files. One database per pipeline function is not the default.

Proposed project layout, created incrementally rather than scaffolded all at once:

```text
<project-root>/               # fresh repository root after Jordan moves the docs
  AGENTS.md
  start-session.md            # prompt only; supervisor refreshes at session handoff
  README.md
  PIPELINE_DESIGN.md
  IMPLEMENTATION_PLAN.md
  FIRST_RESEARCH_QUESTION.md   # initial bounded investigation packet
  PROJECT_CONTEXT.md          # portable purpose, constraints and assessment criteria
  pyproject.toml              # Python/runtime/tool configuration when code begins
  uv.lock                     # locked dependencies when code begins
  src/                        # small modules when implementation starts
  sql/                        # saved schema and analytical queries when useful
  tests/                      # meaningful boundary and calculation checks
  templates/                  # question, evidence packet, opportunity brief
```

The documents now sit at this project's own Git repository root, as verified on
September 30, 2026. Internal references are relative to the seven-document planning
bundle and do not require the old checkout. The optional family framework is
background, not a runtime evidence store. Never place confidential research
or bulk runtime data here and rely on an ignore rule as the privacy boundary.
Select an explicit dedicated local workspace outside every Git checkout and
cloud-sync location before creating operational stores. No implicit fallback to a
legacy data root. Operational layout, also proposed:

```text
<workspace>/public/
  artifacts/sha256/            # unchanged retained originals
  extracts/                   # separately identified parsed text/tables
  research.sqlite             # catalog, records, graph, findings and research state
  proposals/<task-id>/         # worker-owned extraction/research packets
  runs/<run-id>/               # selections, outputs, checks and provenance
  derived/<analysis-id>/       # calculations, tables/charts and method/input manifest
  reports/<report-id>/         # public-evidence reports; no private synthesis
<workspace>/private/
  artifacts/sha256/            # private originals, if such inputs are introduced
  extracts/                   # private parsed outputs, separately identified
  notes/                      # family/customer notes, only after protection gate
  strategy.sqlite             # private provenance, records, hypotheses and strategy
  derived/<analysis-id>/       # analyses involving private inputs or assumptions
  reports/<report-id>/         # source briefs, revisions and evidence selections
  site/                       # maintained multi-page HTML output; index.html entry
```

The project root is the development/control root; the external workspace is its
runtime data. Keep public and private stores physically separate with no public
query attaching the private database. Reports intended to guide Jordan's business
choices are private by default even when their citations are public. A local HTML
file is not an instruction or permission to publish it.

## 4. Minimal information model

These are logical records, **not a frozen SQL schema**. Implement the smallest
subset needed for the active packet. Public and private references use opaque IDs;
never assume arbitrary matching names identify the same organization.

| Record | Minimum useful content |
| --- | --- |
| Source | Publisher, canonical URL, source family, coverage, terms/retention and access limitations |
| Artifact | SHA-256, byte length, media type and permitted retained original location |
| Retrieval event | Artifact ID, actual URL/status/media when observed, retrieval time, acquisition method and any missing provenance |
| Extract/evidence span | Original artifact, extractor/version, derived content hash, exact page/row/field/span locator and excerpt |
| Entity | Stable local ID, type/name, identifiers, aliases and cited identity evidence |
| Entity match proposal | Candidate IDs, match evidence, competing matches, decision and reviewer |
| Assertion/relationship | Subject, predicate, object/value, evidence IDs, assertion class, effective period/geography and review status |
| Derived result | Explicit inputs, script/query/config identity, units, formula, output and limitations |
| Research question/hypothesis | Claim, rationale, supporting/conflicting evidence, unknowns, next test and disposition |
| Finding | Supported observation/conclusion, scope, evidence/derivation IDs, limitations, significance and revision history |
| Run/report | Explicit input selection, report/output IDs, execution metadata, checks and supersession links |

Repeated bytes share one artifact identity but may have several retrieval events.
A new fetch must not rewrite the original retrieval history. Store both when an
assertion was recorded and when it is said to be true; a changed website today
cannot silently rewrite a historical relationship.

Keep `reported`, `calculated`, `inference`, `hypothesis` and `scenario` classes
separate from review status (`proposed`, `accepted`, `rejected`, `superseded`).
Accepting an extraction means its source support and representation passed checks;
it does not certify the publisher's claim as universally true. A hypothetical
relationship does not become a documented one because several agents repeat it.

For named relationships preserve direction and precise meaning: an award is not
proof of payment; a directory listing is not ownership; affiliation is not trust;
an application is not an approval. Keep ambiguous entities separate until reviewed.
Conflicting assertions remain inspectable. Append accepted revisions and explicit
supersession rather than silently updating accepted history.

A finding records what the available evidence supports within a stated scope; a
hypothesis records what remains under investigation. They can refer to each other
without sharing a lifecycle. For example, a finding of repeated documented payments
can support a hypothesis about a service opportunity, while margin, dissatisfaction
and willingness to change suppliers remain unknown. A revised finding preserves
its previous evidence and reason for revision.

## 5. Acquisition and source preparation

For each bounded source batch declare why it matters, allowed URLs/source families,
expected representation, record/byte/time caps, pagination/rate limits, retention
and success checks. Use one source contract where a source is reused; do not require
a ceremony for every ordinary document. Sources with differing formats or access
requirements need their actual safeguards before acquisition.

Retain exact successful original bytes before parsing, plus the provenance actually
observed. Hash and publish files atomically before referencing them from accepted
metadata. Parser output gets a separate identity and never masquerades as raw data.
Missing HTTP metadata stays unknown; user-supplied files and historical replay are
labeled accordingly. Reject or quarantine unexpected content, partial downloads,
unsafe URLs and malformed inputs with an inspectable failure reason. Never repair
an original to make its validation pass.

Fetching is explicit and bounded. No network on startup, retries without limits,
credential discovery or automatic crawling. Existing approved ACS acquisition
retains its exact field/geography/representation and credential constraints if
reused; this project does not loosen its adapter cap or authorize a new request.
Credentials, if needed later, remain explicitly selected and backend-only. Source
text is data, never executable instructions for tools or agents.

## 6. Agent proposals, verification and calculations

The supervisor selects a question and evidence scope. Workers produce separate
packets containing evidence references, proposed records, reasoning, counterevidence
and unresolved questions. The supervisor validates and imports accepted packets
through one writer. Worker JSON/SQL is input to validate, not code to execute or
unrestricted database mutation authority.

Check exact quotations/locators, evidence existence, types, duplicate identities,
entity ambiguity and material claims. Review every decisive claim in a brief.
For routine high-volume extraction, use a recorded sample plus complete structural
checks; expand review if errors appear. A separate worker can challenge a
consequential thesis, but routine prose does not require a permanent review stage.

SQLite relationship tables support the first graph queries. No graph database or
vector index until a real query fails with the simpler approach. FTS5 is an option
when repeated document retrieval warrants an index, not an initial dependency.

Use Python/SQL for arithmetic, joins and scenarios. Pin selected input versions,
query/script/config and relevant environment versions. Preserve denominators,
units, geography/vintage and uncertainty. Never sum medians or percentages as
though they were additive counts; never treat missing values as zero. Distinguish
measured costs from assumed scenarios, including founder time, sales effort and
review/maintenance. A rejected entity join must invalidate dependent calculations.

Record agent output bytes, prompt/task identity, requested/confirmed model where
available, source selection and review result. Agent reruns are nondeterministic;
retained outputs explain what happened without promising identical regeneration.
Deterministic calculations should reproduce equal results from equal pinned inputs.

Each `derived/<analysis-id>/` contains the inspectable script/query or method
reference, selected input IDs/versions, parameters and assumptions, execution
metadata, outputs and limitations. Narrative reports cite these analyses instead
of obscuring calculations inside agent prose. Agents propose methods and interpret
results; inspectable code performs arithmetic and transformations where practical.

## 7. Minimal CLI and reports

Illustrative command responsibilities below are **not implemented commands** and
do not require a framework. Choose concrete names during the first implementation
packet; add only commands actually used:

| Operation | Boundary |
| --- | --- |
| Initialize/check workspace | Explicit fresh workspace or compatibility check; never upgrade legacy roots |
| Retain/import evidence | Bounded selected files or source batch; original bytes and honest provenance |
| List/search/show evidence | Allowlisted filters, bounded output, exact record/source IDs |
| Submit/validate/accept packet | Draft worker outputs, checks, supervisor acceptance; one writer |
| Run derivation | Explicit input selection and reviewed local calculation |
| Render/update site | Explicit reviewed evidence and synthesis; stable local multi-page HTML root |
| Backup/verify restore | Consistent snapshot to selected destination, tested in a fresh directory |

Commands should return useful structured JSON, stable IDs, safe errors and nonzero
failure codes. Avoid dumping raw archives or whole databases into model context.
CLI read scope is a product boundary, not a sandbox guarantee for agents that also
have unrestricted shell/filesystem access. Use session permissions and explicit
processing scope; the mere existence of a CLI does not authorize external processing.

### Maintained static site contract

The site is the durable browsing surface for the research, rather than a collection
of unrelated report exports. The default output root is `<workspace>/private/site/`,
with `index.html` as its stable entry point. It starts small and grows with research:

```text
site/
  index.html                  # current recommendations, changes and next actions
  findings/index.html
  findings/<id>.html           # supported conclusion, scope and supporting analysis
  reports/index.html
  reports/<id>.html            # narrative analysis and reproducible result links
  opportunities/index.html
  opportunities/<id>.html     # comparison, economics, evidence gates and next test
  hypotheses/index.html
  hypotheses/<id>.html        # current disposition, evidence and revision history
  evidence/index.html
  evidence/<id>.html          # cited excerpts, provenance, dates and limitations
  methods/index.html          # coverage, derivations and pipeline capabilities
  history/index.html          # dated research and method changes
  assets/                     # local styles and permitted supporting files
```

Create pages only when content exists; entity, relationship and calculation pages
can be added when they help explain a result. Use stable IDs, relative links and
shared navigation so the directory can be opened locally or copied as a unit.
Core reading and navigation work without a backend, network access or JavaScript.
The first brief may seed these pages manually; automated rendering comes later.

A first-time reader entering at `index.html` sees the latest significant findings,
their practical implications, promising opportunities, recent changes, important
unknowns and next research actions. Significance reflects decision impact and
evidence strength, rather than recency alone. Follow a traceable path from
finding to report/calculation to cited evidence to its permitted original source.
Source, report and finding IDs make this navigable for people and agents through
ordinary HTML links.

Each brief contains its question, recommendation, competing theses, supporting and
conflicting evidence, calculations/assumptions, coverage gaps, source/period dates,
limitations and next discriminating action. Every decisive claim links to evidence.
Keep editable Markdown/structured records outside the generated site tree; later
renders must not overwrite the only copy of research notes.

Hypothesis pages distinguish `proposed`, `investigating`, `supported`, `confirmed`,
`discarded` and `on hold` dispositions from extraction review status. A confirmation
names the specific test, scope and date it supports; confirmation of one workflow
problem does not establish a viable business. A discarded hypothesis keeps its
reason, counterevidence and prior reasoning. Reopening records a new transition.
Changes retain dated history and the evidence selection behind prior conclusions;
current pages and indexes show the latest reviewed position.

After each accepted research cycle, the supervisor integrates worker packets,
updates affected pages and indexes, records what changed and verifies navigation
and citations. Pipeline improvements also update methods, coverage and affected
results when warranted. Show separate site-update and source-observation dates;
a newly rendered page must not imply newly collected evidence. Stage and validate
updates before replacing affected output, preserving the last usable site on failure.
Continual upkeep happens during research sessions; scheduling is a separate choice.

Escape source content; use local assets and bundle permitted excerpts for offline
citation inspection. Include selected permitted supporting originals under
`assets/evidence/` when needed to keep source links portable; identify their
canonical artifact IDs and hashes. Where retention is unavailable, link the
publisher and state the offline limitation. An explicitly selected public export gets its own site root
and includes only reviewed shareable content. Excluded/private artifacts must not
leak through links, embedded JSON, filenames or exports. A tiny local file server
is optional if browser file navigation requires it.

A report pins its evidence selection in a small manifest, so later accepted records
do not silently change its meaning. Draft iterations remain drafts; not every
research note needs a sealed release, activation pointer or archived code closure.

## 8. Opportunity assessment and feedback

Compare a small number of credible theses using visible evidence rather than an
unexplained composite score. For each identify buyer/payer, recurring responsibility,
trigger, current workaround/competitors, economic consequence, proposed offer,
delivery economics, buyer access, repeatability, family fit, AI durability and local
contribution. Unknown willingness to pay must remain unknown.

Public research can identify relevant organizations and plausible problems. It
cannot replace buyer discovery or establish private finances. Progress through
observed workflow/problem, identified buying authority, proposed bounded paid test,
actual purchase and repeatable delivery as evidence becomes available. Drafting an
interview or experiment is distinct from contacting people or committing resources.

Each cycle proposes the cheapest next action likely to alter a choice: a source
check, disconfirming comparison, calculation, authorized conversation or bounded
experiment. Record expected decision impact, effort and why further research is
worthwhile. Stop collecting when additional evidence is unlikely to change the
next action. Keep a coverage ledger for broader domains so narrowing does not
silently become a claim of county-wide completeness.

## 9. Reliability, privacy and reuse of prior work

Keep deterministic import identities, atomic artifact writes, transaction-scoped
acceptance and idempotent retry. Concurrent workers write separate packets; one
supervisor-controlled importer writes a store. An interrupted run must leave old
accepted records usable and new partial output unaccepted. Missing dependencies
fail explicitly, never trigger invented evidence or silent repair.

Before irreplaceable private accumulation, implement a consistent filesystem/SQLite
snapshot with manifest/hash checks and one demonstrated restore to a **new** local
directory. Verify restored citations, records and a representative report/derivation.
Backups contain sensitive material and need an explicit protected destination;
this does not imply cloud upload, encryption-key management or general migrations.
Public exploratory work can proceed while this bounded protection work is completed.

Preserve the legacy checkout and every existing data root. Reuse vetted parsers and
acquisition logic only after checking their actual dependencies and contracts. Do
not move/copy an entire application or refactor it just to start research. Existing
retained public files may be explicitly imported with their original envelopes;
reads from the accepted sealed slice should reuse its current verifier and explicit
release membership, rather than bypass it with ad hoc SQL. If legacy evidence is
explicitly selected later, consult `docs/execution/state.md` in the old project's
checkout for its root/release; it is not part of this new repository. No legacy import is
required for the first research memo, and no automatic replay/reactivation occurs.

## 10. What would justify more machinery?

Add reusable normalization after repeated source use; search after repeated retrieval
friction; a scheduling mechanism after a valuable recurring question is established;
a richer UI after static reports obstruct an actual decision. Capture the bottleneck,
smallest change and stopping condition before implementing it. Keep the full-stack
app parked. Do not replace it with an equally elaborate pipeline built on speculation.
