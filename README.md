```markdown
# MyWorkflow
### Enterprise Workflow Orchestration & Workforce Intelligence Platform

> A configuration-driven platform that orchestrates organisational workflows in real time, detects operational bottlenecks, understands workforce skills and capacity, and provides bottleneck-aware allocation recommendations — designed for integration with enterprise systems such as SAP.

---

## Live Demo
https://ayushmaangangulywork-boop.github.io/My-Workflow/src/index.html

---

## What This Is

MyWorkflow is a **product prototype** — not a tutorial project, not a UI exercise.

It demonstrates a complete enterprise software architecture:

- Configuration-driven business logic engines
- Multi-signal bottleneck detection
- Skill-aware workforce capacity analysis
- Advisory-based work allocation with full audit trail
- Enterprise integration layer (SAP MM, SD, WM, FI, CRM, HRMS)
- Role-based access control model
- Real-time operational control tower

The product answers ten operational questions:

1. What work exists?
2. What workflow does it belong to?
3. Where is it in the process?
4. What is waiting, blocked, or overdue?
5. Where are organisational bottlenecks?
6. What skills are required?
7. What workforce capacity is available?
8. Which qualified capacity can relieve a bottleneck?
9. What should happen next?
10. What actually happened, and who decided?

---

## Architecture

```
CONFIGURATION STORE
        ↓
BUSINESS LOGIC ENGINES
        ↓
WORKFLOW EXECUTION
        ↓
UI / CONTROL TOWER
```

### Core Engines

| Engine | Responsibility |
|---|---|
| `WorkflowEngine` | Instance creation, stage transitions, state management |
| `BottleneckEngine` | Multi-signal detection — queue, ageing, SLA risk, utilisation, throughput |
| `CapacityEngine` | Workload calculation, utilisation, available hours |
| `AllocationEngine` | Configurable weighted scoring across skill fit, capacity, SLA pressure, bottleneck impact |
| `SLAEngine` | SLA monitoring, warning/critical thresholds, escalation rules |
| `AuditEngine` | Immutable decision log — every action, actor, timestamp, entity |

### Configuration-Driven Design

Nothing is hardcoded. All business entities live in `configStore`:

```javascript
configStore.departments           // Add/remove departments without code
configStore.workflowDefs          // Define new workflows without code
configStore.skills                // Extend the skill master without code
configStore.employees             // Add workforce without code
configStore.allocPolicy           // Change allocation weights without code
configStore.bottleneckThresholds  // Tune detection sensitivity without code
configStore.slaPolicy             // Adjust SLA rules per workflow without code
```

The engines consume configuration dynamically. Adding a new department, workflow, or skill never requires touching engine logic.

---

## Screens

| Screen | Purpose |
|---|---|
| **Control Tower** | Organisation-wide real-time snapshot — 7 KPIs, live workflow table, department health grid, bottleneck feed, activity stream |
| **My Work** | Personal task queue with Edit / Mark Done / Delete — tasks persist in runtime state |
| **Workflows** | All active instances with search, filter by status, sort by priority or SLA, click-through detail panel with stage advance |
| **Bottlenecks** | 5 active detections with symptom → analysis → cause → impact → Fix Advisory per bottleneck |
| **Allocation Advisory** | Scored candidate ranking, Accept/Override with audit-logged override reason, bottleneck relief impact scoring |
| **Workforce** | Employee capacity, utilisation gauges, skill proficiency profiles, department filter, live search |
| **Analytics** | 12-month trends across throughput, SLA compliance, bottleneck frequency, utilisation, skill constraint index, process cycle time |
| **Configuration** | All business entities editable — departments, workflows, skills, roles, SLA policies, allocation policy weights, workflow designer |
| **Integrations** | SAP MM/SD/WM/FI, CRM, HRMS, Email, ITSM — all active with live event counts and pulse indicators |
| **Audit Trail** | Full immutable decision log, filterable by entity type, full-text searchable |

---

## Everything That Is Clickable

- **Control Tower stat strip** — each KPI navigates directly to the relevant screen with filter pre-applied
- **Workflow rows** — opens a detail side panel with full stage flow, SLA, metadata, timeline, and working Advance Stage button
- **Bottleneck signal cells** — hover tooltips with exact values; skill tags navigate to Allocation Advisory
- **Department grid** — clicks through to Workflows
- **Activity feed** — navigates to Audit Trail
- **Employee rows** — shows utilisation and capacity summary; skill tags show proficiency level
- **Analytics bars** — hover every bar for month and exact value
- **Audit rows** — clickable with full entry detail
- **Integrations** — every row clickable with live sync stats
- **Admin tables** — every row clickable with contextual info; Add buttons write to config and re-render immediately
- **Allocation Advisory** — Accept logs to audit trail and shows confirmed state; Override requires written reason

---

## Key Design Decisions

**1. Configuration before UI**
The data model and engine logic were designed before the UI. The UI renders whatever the configuration contains — it has no opinion about what departments, workflows, or skills exist.

**2. Advisory, not automatic**
The allocation engine scores and ranks candidates but never assigns autonomously. Managers accept, override with a written reason, or dismiss. Every decision is recorded in the audit trail with actor and timestamp.

**3. Bottleneck ≠ big queue**
BN-05 (PO Creation, 14 items, 91% SLA risk) is correctly identified as a downstream symptom of BN-01, not an independent bottleneck. The engine evaluates five signals simultaneously. Resolving BN-01 automatically unblocks BN-05 — the system makes this dependency explicit rather than treating both as equal independent problems.

**4. Workload in hours, not task count**
A 15-minute approval task and a 6-hour quality inspection are not equivalent units. Utilisation is calculated as committed hours divided by available hours, producing meaningful capacity figures and better allocation recommendations.

**5. Integration abstraction**
SAP and other external systems connect through adapter layers, not directly into workflow engine logic. If the organisation changes ERP or adds a new system, the workflow engine is unaffected. Integration mappings are configuration-driven.

**6. Audit trail is append-only**
Every allocation decision, workflow advancement, configuration change, and system event is logged with actor, action, detail, entity, and timestamp. Override reasons are mandatory and permanently recorded. Nothing is ever deleted from the log.

**7. Single accent colour**
Amber is reserved exclusively for things requiring the user's attention — active states, advisories, warnings. Status colours are desaturated earth tones, not neon. Operations interfaces are used under cognitive load for extended periods.

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | Vanilla JavaScript, HTML5, CSS custom properties |
| Typography | IBM Plex Mono + IBM Plex Sans (Google Fonts) |
| Data | In-memory configuration store + runtime state |
| Build | None required — opens directly in any browser, works offline |

---

## Production Roadmap

| Phase | Scope | Estimate |
|---|---|---|
| 1 | Python FastAPI backend + PostgreSQL database | 6–8 weeks |
| 2 | React + TypeScript frontend + REST API | 4–6 weeks |
| 3 | SAP Integration Suite adapter + webhook layer | 4–8 weeks |
| 4 | WebSockets, SSO/SAML, multi-tenancy, mobile | Ongoing |

Full schema, all technical decisions, and detailed roadmap in `/docs`.

---

## Repository Structure

```
myworkflow/
├── src/
│   └── index.html                  # Complete application — open in browser
├── docs/
│   ├── ARCHITECTURE.md             # System layers, entity model, DB schema
│   ├── TECHNICAL_DECISIONS.md      # 10 documented decisions with rationale
│   └── ROADMAP.md                  # Production build phases
└── README.md
```

---

## Why This Exists

Built as a portfolio piece demonstrating enterprise product thinking — not just UI skills.

The problem this solves is real: work flows through mid-to-large organisations via email, WhatsApp, and spreadsheets. No one knows where anything is stuck, why it is stuck, or who has the capacity and skills to unstick it. Enterprise solutions like ServiceNow solve this but cost crores per year and take 12–18 months to implement. The mid-market has nothing.

MyWorkflow is a proof of concept for what a modern, lightweight, configurable alternative could look like.

---

*Designed and built by Ayushmaan Ganguly*
```
