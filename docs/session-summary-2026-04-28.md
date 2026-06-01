# Session Summary — 2026-04-28

**Project:** Hof Ashkelon VTO — Dynamic Social-Mapping System for Municipal Social Workers
**Location:** `/Users/nasher/aiSuite/hof-ashkelon/`

## What we did

1. **Ingested the materials the council sent.**
   A ZIP of 9 files in Hebrew (5 .docx, 3 .pdf, 1 .ppsx). Extracted with UTF-8-safe Python (the shell's `unzip` mangled Hebrew filenames). Parsed all 3 PDFs with `pypdf`.

2. **Identified the system goal.**
   A mobile-first Hebrew-RTL tool for municipal social workers to collect, document, and analyze resident/community info during emergencies and recovery — "family doctor of the village" model. Working title: מערכת דינמית למיפוי חברתי תומכת עובדות סוציאליות.

3. **Produced a working project folder** at `/Users/nasher/aiSuite/hof-ashkelon/`:

   ```
   hof-ashkelon/
   ├── README.md                              index + open questions
   ├── docs/system-goal.md                    one-pager
   ├── docs/session-summary-2026-04-28.md     ← this file
   ├── spec/PRD.md                            full English PRD (~20 KB)
   ├── data-model/
   │   ├── schema.md                          ERD + entities + RLS + offline sync
   │   ├── stage1-acute.json                  Stage A questionnaire
   │   ├── stage2-stabilize.json              Stage B questionnaire
   │   ├── stage3-recovery.json               Stage C "reaching out" questionnaire
   │   ├── msr-annex-b.json                   Primary Assistance Center intake form
   │   └── needs-taxonomy.json                shared option lists
   ├── mockups/index.html                     6 Hebrew-RTL mobile mockup screens
   └── source-materials/                      all 9 original files + _pdf-extracted.txt
   ```

## Key content produced

- **`spec/PRD.md`** — background, 3-stage operating model (Acute → Stabilize → Recovery), roles, functional & non-functional requirements, integrations roadmap (Yahad, EPR/Shibolet, Power BI), 4-milestone release plan, risks, open questions for the council.
- **`data-model/*.json`** — 4 questionnaires as executable JSON specs. The 3 stages are direct translations of the council's own Hebrew drafts; the מס"ר form is from Annex B of the Home Front Command doctrine.
- **`mockups/index.html`** — static HTML, RTL, calm blue/green palette, 6 screens:
  1. Home (emergency CTA, search, today's tasks)
  2. Resident profile with timeline and AI summary
  3. Stage-1 acute questionnaire
  4. End-of-conversation AI summary + proposed tasks
  5. Tasks list
  6. Village dashboard (triage donut, common-needs bars, AI themes)

## Decisions recorded

- Phase-1 MVP = "paper replacement" PWA with offline-first capture for the three stage questionnaires.
- Phase-2 adds AI conversational summarization + Yahad/EPR integrations.
- Adoption is the biggest named risk. Design bias is toward reducing data-entry burden.
- Consent model for cross-department info-sharing is an open legal question (flagged).

## Open questions parked for the council

See `spec/PRD.md` §11. Highlights:

- Linked-to-national-ID or opaque resident identifier?
- Ministry of Welfare backend — EPR, Shibolet, or legacy תע"ס?
- Legal owner of the consent model?
- מס"ר intake — day-one or Phase-2?
- Cloud vendor decision (gov cloud vs. Azure IL / AWS IL)?

## Next action

- Open `mockups/index.html` in a browser to show stakeholders.
- Schedule 3 × 1-hour scope workshops (PRD §9 M0).
- Confirm village list in `data-model/needs-taxonomy.json` with the council.
