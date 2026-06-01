# PRD — Dynamic Social-Mapping System for Emergency & Continuing Recovery

**Client:** Hof Ashkelon Regional Council — Department of Social Services
**Working title (Hebrew):** מערכת דינמית למיפוי חברתי תומכת עובדות סוציאליות
**English working title:** Community Resilience & Case-Continuity Platform (CRCC)
**Audience for this document:** VTO dev team (me), product stakeholders at the council, welfare dept. leads
**Status:** v0.1 — synthesized from materials sent to the VTO team on 2026-04-28

---

## 1. Background & Problem

The Hof Ashkelon Regional Council's welfare department ran an internal accelerator ("מאיץ") to redesign how social workers (עו"ס) support residents during **emergencies and the long recovery that follows**. Two reference documents from the Home Front Command and the Digital Directorate are part of the regulatory envelope:

- **"מרכז סיוע ראשוני" (מס"ר) — Feb 2026** — the national doctrine for Local-Authority Primary Assistance Centers. Replaces the older "Family Center" doctrine based on lessons from Operation "Am K'lavi." Defines roles, workflow stages (declaration → operation → evacuation → closure), intake forms (Annex A), and a social-worker intervention/needs-mapping form (Annex B).
- **"מערכת יחד" (Yahad System) Guide for Local Authorities** — the Ministry of Finance + Digital Directorate evacuee reporting system. Defines three alternatives for reporting evacuees (municipal system / digital form / manual Excel) and the required 24-hour handoff to the Tax Authority.

### Pain points (from field interviews, "5 whys," and the mentoring session on 2.3.26)

1. **Lost knowledge between shifts.** After Oct 7, evacuation-hotel social workers rotated without handoff. Each kept notes locally (notebook, personal Google Form, personal Excel). When she was unavailable, the context was gone.
2. **Inconsistent documentation.** Workers document differently by generation, personality, and context (detailed vs. terse; phone vs. notebook). Aggregation is impossible.
3. **"Ask once" broken.** Many workers touch the same family during an emergency. Residents are re-interviewed repeatedly → they tire, refuse further engagement.
4. **No top-down or bottom-up visibility.** A community social worker cannot see her village at a glance — themes, emerging needs, who's known vs. unknown to welfare.
5. **Documentation feels like a separate chore.** Workers skip or defer it, especially when they're personally affected by the same event.
6. **Fragmentation across national systems.** Data in Shibolet, Amun Municipal, Yahad, EPR, arnona — none of it reaches the social worker in the field.
7. **Reaching residents unknown to welfare** — no mechanism in routine or in emergency.

### Vision

A single lightweight tool for social workers (primary user) and community/regional managers (secondary users) that:

- Is a **mobile/tablet-first Hebrew-RTL** tool you actually want to use in the field.
- Runs the **three-stage emergency questionnaire** on the resident/family's time axis.
- Gives the worker **continuity** — she opens any resident's card and sees the prior history and tasks.
- Produces **aggregated community dashboards** for the community social worker, regional manager, and village roundtables.
- Uses **AI (secure)** to summarize long conversations into tasks and themes, so documentation stops being an extra chore.

The deeper framing (from the mentoring notes): the community social worker should function like **"the family doctor of the village"** — always synced, top-down and bottom-up, including on residents unknown to welfare.

---

## 2. Goals & Non-Goals

### Goals (v1)

- G1. **Reduce time-to-intervention** by letting any on-shift worker see the resident's prior contacts instantly.
- G2. **Enforce "ask once"** via persistent resident/family profiles.
- G3. **Stop knowledge loss during handoffs** (shift change, rotation, personal unavailability).
- G4. **Give the council a real-time picture** per village and across villages during an event.
- G5. **Let non-developers** (a program manager) edit questionnaires.
- G6. **Work in the field**, including with weak or no connectivity.
- G7. **Be adoptable** — adoption is explicitly called out as the biggest risk ("החלק של ההטמעה מאד משמעותי").

### Non-Goals (v1)

- NG1. Not replacing the Ministry of Welfare's core case-management (תע"ס / Shibolet / EPR). We document and generate hand-off payloads; we don't try to own the full client record.
- NG2. Not replacing Yahad. We produce the Annex-A evacuee-report payload and, in future, push into Yahad via API.
- NG3. Not owning hotel-matching or Tax-Authority compensation — that lives in Yahad + the Tax Authority.
- NG4. No desktop-first admin UI in v1 (the CMS is web but still compact).
- NG5. No community map (2D geographic view) in v1 — listed as "future" in the source spec.

---

## 3. Users & Roles

| Role (Hebrew) | Role (English) | Key capabilities |
|---|---|---|
| עובדת סוציאלית יישובית / מרחבית | Field social worker (village / regional) | Run questionnaires, view & edit resident profiles, leave notes, generate personal tasks |
| עו"ס קהילתית (רכזת) | Community social worker | All of the above + village-level dashboards, theme reports, village roundtable view |
| מדר"צ / מנהלת תוכנית | Program manager / supervisor | Cross-village dashboards, regional roll-up, export reports |
| מנהל מערכת | System admin | Questionnaire CMS, user & permission management, tags, retention controls |
| (future) עו"ס מס"ר | Primary Assistance Center worker | Rapid intake using Annex-A + Annex-B forms at a מס"ר |

### Secondary readers (not users of the UI, but consumers of outputs)

- Home Front Command & the municipality's Operations Room — receive aggregated population-intelligence snapshots (תמ"ץ).
- Tax Authority (קרן הפיצויים) — receives evacuee lists within 24 h (via Yahad or Excel fallback).

---

## 4. Operating Model — the Three Stages

Directly from `מודל עבודה בחירום חוף אשקלון.pdf` and mirrored in the resident questionnaires. These are the three stages the system's questionnaires are keyed to.

### Stage 0 — Routine / Pre-emergency (שלב מקדים)

- Maintain resident rosters per village (known + unknown to welfare).
- Staff training on the model; each worker has a team lead.
- Budget pre-allocation for rapid aid at the worker's discretion.

### Stage 1 — Acute Emergency (חירום אקוטי)

- Declaration + deployment of standby team (village עו"ס, community עו"ס, resilience center).
- **Intake: the Acute questionnaire** — short, fast, triage-oriented.
- Triage: urgent / immediate / not urgent / no further care needed.
- Produces: per-family snapshot + village situation-picture (תמ"ץ) for the roundtable.

### Stage 2 — Stabilizing Function (ביסוס תפקוד, ~week 1+)

- Families flagged for follow-up in Stage 1 get a deeper interview.
- Logged as formal service requests in EPR.
- Decision on "open a case file" happens within ~3 months, with the supervisor.

### Stage 3 — Continuing Recovery / Resilience (שיקום מתמשך)

- **Reaching-out questionnaire** to the whole village to surface unknown-to-welfare families.
- Families needing a plan: full intake + care plan.
- Community aggregation → village roundtable (עו"ס קהילתית leads) including resilience center, community leads, relevant role-holders.

---

## 5. Functional Requirements

### 5.1 Questionnaire engine

- **Three seeded questionnaires** (Stages 1, 2, 3) built from `שאלוניים לתושבים (1).docx`.
- **Question types:** rating 1–5, open text, single-choice, multi-choice, checkbox, date, scripted branches (decision trees — "known to welfare?", "evacuated / at home / alone / injured?").
- Required vs. optional flags.
- **Pause & resume** (save partial, finish later).
- Auto-coding into categories (emotional / community / material / rights / bereavement / special needs / security-service family / other).
- **CMS**: program manager edits question text, options, ordering, required/optional, categories — without dev involvement.
- **Versioning**: every questionnaire edit creates a new version; prior responses stay pinned to the version they were captured in.

### 5.2 Resident & family profile

- Anonymous internal ID (local authority decides whether it's linked to national ID or opaque).
- Link all encounters back to one profile (person + family unit).
- Household structure: head of family + partner + children (ages, special needs, evacuation status per child).
- **Ask-once ledger**: previously collected answers visible in read-only form; the worker can "carry forward" instead of re-asking.
- Filters: age, gender, village, support circle, crisis type (personal / community), stage, known-to-welfare flag.
- **Continuity of care**: when the worker on shift changes, the new worker sees the last 3 contacts, the open tasks, and the "handoff note" field.

### 5.3 Field workflow (mobile)

- Home → pick resident (search or QR) → pick questionnaire (Stage 1 / 2 / 3) → conduct → worker summary → actions.
- "Emergency mode" shortcut — pre-opens Stage 1 with only the fastest fields.
- Offline-first: capture works with no connectivity; syncs when online. Conflict resolution: last-writer-wins at field level with audit trail.
- Icons next to each question (to aid eye contact with the resident during the conversation).

### 5.4 Task engine (actionable insights)

- Every questionnaire ends with "tasks for the social worker" free-text + checkboxes.
- Tasks are first-class objects: assignee, due date, status, link to resident.
- Tasks can be generated automatically from the AI summary (with the worker reviewing before commit).
- **Optional sync with the worker's calendar / EPR** — flagged as "check feasibility" in the source spec.

### 5.5 AI conversational summary (secure)

- After a Stage-2 or Stage-3 long conversation, the worker can dictate or paste free text → the system proposes a summary, a set of tasks, and themes/tags.
- The worker edits and commits. Nothing goes out without a human in the loop.
- Must run in an approved Israeli-government environment (see §7 Security).

### 5.6 Community & regional dashboards

- **Village view** (community social worker): heatmap of needs by age bracket, crisis-type distribution, newly-surfaced unknown-to-welfare families, open tasks by worker.
- **Regional view** (program manager): same slices cross-village, theme trends over time (Stage 1 → 2 → 3).
- **Drill-down**: any chart → filtered list of residents (with permission check).
- Simple chart types only (pie, bar, line). No geographic map in v1.

### 5.7 Reports & exports

- Per-resident report.
- Per-village report.
- Regional roll-up.
- Export: **Excel, PDF, CSV**.
- **Annex-A payload export** — aligned with the Tax-Authority format required by Yahad (so the 24-hour evacuee report is one click, not a separate manual Excel).
- **Annex-B export** — printable intervention/needs-mapping form to give to a מס"ר social worker.

### 5.8 Content & reference data

- Dynamic lookups: needs list, special-needs list, family statuses, evacuation statuses, crisis-type taxonomy.
- Free tagging by the worker, plus a curated controlled vocabulary maintained by the admin.
- Themes produced by auto-tagging (NLP over open-text answers) + manual tags.

### 5.9 Identity & access

- Login: password + OTP (SMS or authenticator app).
- Role-based access as in §3.
- **Row-level security**: village social workers see their village; community social workers see their village(s); program managers see the region; system admins see everything.
- **Consent & visibility**: a resident record can carry a consent flag that controls whether info is visible across departments (see §7 privacy notes).
- **Audit log**: who viewed / edited / exported what.

---

## 6. Integrations (phased)

| System | Phase | Direction | Purpose |
|---|---|---|---|
| **Yahad (יחד)** — Digital Directorate | Phase 2 | Push | 24-hour evacuee reports replace the manual Excel flow (currently 3 alternatives are documented; we'd become "Alternative A — municipal system" with proper integration) |
| **EPR / Shibolet / תע"ס** — Ministry of Welfare | Phase 2 | Pull + push | Pull known-client flags; push service requests to the ministry system of record |
| **Municipal arnona / residents roster** | Phase 1 late | Pull | Seed the resident list so workers don't start from blank; surface unknown-to-welfare residents |
| **Power BI** | Phase 2 | Push | Advanced analytics for regional HQ |
| **Resilience Center (מרכז חוסן)** | Phase 2 | Shared tenancy | Resilience center staff as named users; shared case visibility |
| **מס"ר intake** | Phase 2 | Replace paper Annex A / B | The מס"ר team uses our mobile app at the physical center; forms are the Annex-A and Annex-B forms from the Home Front Command doctrine |

---

## 7. Non-Functional Requirements

### 7.1 Localization & a11y

- **Hebrew-first, RTL** across every screen.
- All labels, errors, and exports in Hebrew; English strings only for diagnostic logs.
- Font sizes scalable. Target WCAG 2.1 AA.
- Tablet-friendly touch targets; one-hand mobile operation for Stage-1 (acute).

### 7.2 Visual design

- Calm palette: blues, greens, light gray (per the source spec).
- Icons beside text on every question — "conversation-supportive UI."
- No dense spreadsheet-style screens for field workers; analytical density is fine on the dashboards.

### 7.3 Offline

- Must capture a full questionnaire offline.
- Must queue tasks offline.
- Must sync automatically when connectivity returns.
- Must warn the worker which resident records are stale on the device.

### 7.4 Performance

- Resident lookup ≤ 300 ms online, ≤ 100 ms offline.
- Stage-1 intake form fully interactive within 2 s on a mid-range 2024 Android tablet over 3G.

### 7.5 Security & privacy

- Encryption in transit (TLS 1.3) and at rest (AES-256).
- OTP for login; mandatory re-auth after 30 min idle.
- **Data residency in Israel** (government cloud or a vendor approved for public-sector social-welfare data).
- Daily backups with 30-day retention; quarterly restore drills.
- **Consent model** aligned with "חובת סודיות / מעבר מידע בין הרשויות ובין מחלקות המועצה" — which the source doc flags as an open problem. v1 will implement:
  - Per-resident consent flag for sharing beyond the assigned worker.
  - Explicit audit trail on every cross-department read.
- Mandatory legal review before go-live. This is called out as "לפצח" (to crack) in the notes — not solved yet.

### 7.6 Adoption & change management (explicit requirement)

- In-app coach marks for the first two weeks.
- A "light mode" that only shows the three core flows.
- Training kit for the department rolled out alongside the tool.
- Success metric: in the next exercise, ≥ 80% of Stage-1 interviews are captured in the system rather than on paper / Google Forms.

---

## 8. Priority Matrix (from `אפיון כלי מיפוי טכנולוגי.docx`, with my additions)

| Capability | Must-have? | Priority |
|---|---|---|
| Add a custom questionnaire without a developer | ✅ | Critical |
| Compare by age / time / village | ✅ | Critical |
| Offline mode | ✅ | Very important |
| Automatic theme analysis | ✅ | Critical |
| Free tagging by the worker | ✅ | Important |
| Intuitive charts | ✅ | Important |
| External-system integrations (Yahad, EPR, Power BI) | ⏳ | Phase 2 |
| AI conversational summary → tasks | ✅ | Critical (mentoring notes) |
| Continuity of care across shift changes | ✅ | Critical (mentoring notes) |
| Reaching-out / unknown-to-welfare workflow | ✅ | Important (Stage 3) |
| Community map (geo) | ⏳ | Future |

---

## 9. Release Plan (proposed)

### M0 — Discovery & design (2 weeks)

- Confirm scope with the department (3 one-hour workshops).
- Freeze data model + wireframes (this repo already contains a first cut).
- Legal review of the consent model.

### M1 — "Paper replacement" MVP (6–8 weeks)

- Mobile web PWA.
- Three seeded questionnaires (Stage 1, 2, 3).
- Resident profile + family.
- Task engine (manual tasks, no AI yet).
- Offline capture + sync.
- Role-based access (field worker / community worker / program manager / admin).
- Hebrew-RTL across the board.
- Excel + PDF export.

### M2 — Aggregation & analytics (4 weeks)

- Village and regional dashboards.
- Theme auto-tagging (rule-based first, NLP model second).
- Annex-A export aligned with Yahad format.

### M3 — AI summarization & integrations (6 weeks)

- AI conversational summary behind a human-in-the-loop gate.
- Yahad integration (if approved by Digital Directorate).
- EPR one-way push (service-request creation).

### M4 — Pilot & rollout

- 1 village pilot → 3 villages → region.
- Exercise ("תרגיל") that measures paper vs. in-system capture rate.

---

## 10. Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Adoption — workers default to paper & Google Forms | High | High | In-app coach marks; light mode; training kit; measurement in exercise |
| Legal — cross-department info-sharing blocks the feature | Medium | High | Legal review in M0; consent model upfront; audit trail |
| Integration scope creep (Yahad / EPR / Shibolet) | Medium | Medium | Phase 2 and only after MVP proves ROI |
| AI produces wrong tasks | Medium | High | Human-in-the-loop — no task is committed without worker review |
| Connectivity is worse than assumed in the field | Medium | High | Offline-first architecture (not offline-tolerant) — design the happy path for no-connectivity |
| Privacy incident | Low | Very high | Data residency + encryption + RBAC + audit log + yearly pentest |

---

## 11. Open Questions (for the client)

1. Is the resident identifier linked to national ID, or fully opaque? (The source spec says "at the council's discretion.")
2. Which Ministry of Welfare system are we integrating with first — EPR, Shibolet, or the legacy תע"ס? (They're referenced interchangeably.)
3. Is Hof Ashkelon ready to be the anchor pilot, or should we include a second regional council?
4. Does the council already have an approved cloud vendor (gov cloud vs. Azure IL / AWS IL)?
5. Who is the legal owner of the consent model — the council's legal adviser, or the Ministry of Welfare?
6. Is the community social worker ("רופא משפחה של הישוב") a formal role today, or a target role introduced by this program? That changes permissions design.
7. מס"ר integration — does the council want to replace Annex A and Annex B on day one, or in Phase 2?

---

## 12. Appendix — Source Documents

Stored under `../source-materials/`:

- `אפיון כלי מיפוי טכנולוגי.docx` — the team's own initial AI-assisted spec (UX, functions, tech stack, priorities).
- `כתיבה אינטואיטיבית.docx` — 25 needs/definitions, 5-whys analysis, actionable insights, future-state personas (community עו"ס hat, evacuated resident hat).
- `תיעוד מאיץ - סמינר 2 (003).docx` — accelerator seminar #2 synthesis: role redefinition, milestones, sub-team assignments.
- `שאלוניים לתושבים (1).docx` — the three questionnaire drafts (Stage 1, 2, 3). These are the authoritative source for §5.1.
- `קישור למצגת המסבירה את הקשר בין שיבולת לא.docx` — pointer to `goextra.gov.il/265` and the "Data in Welfare" body of work, including a municipal data-sharing guide.
- `מודל עבודה בחירום חוף אשקלון.pdf` — the council's own three-stage model with the target future state ("tech tool in the palm of the hand").
- `מנחה מקצועי מרכז סיוע ראשוני (מס"ר) — פברואר 2026.pdf` — Home Front Command doctrine for primary-assistance centers, incl. Annex A (evacuee report) and Annex B (social-worker intervention & needs-mapping form).
- `מערכת יחד — מדריך לרשויות המקומיות.pdf` — Digital Directorate guide for the Yahad evacuee-reporting system.
- `חוברת פרויקטים פרויקטים נבחרים.ppsx` — project portfolio (not synthesized here; informational).
