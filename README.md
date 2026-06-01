# Hof Ashkelon — VTO Project Workspace

This repo is a working folder for my VTO engagement with the **Hof Ashkelon Regional Council** (מועצה אזורית חוף אשקלון). The council asked me to help their social-services department design and build a **dynamic social-mapping system for municipal social workers** — a mobile-first Hebrew-RTL tool to collect, document, and analyze resident and community information, especially during emergencies and continuing recovery.

## What's in here

```
hof-ashkelon/
├── README.md                     ← you are here
├── docs/
│   └── system-goal.md            ← one-pager: what are we building and why
├── spec/
│   └── PRD.md                    ← full product requirements document (English)
├── data-model/
│   ├── schema.md                 ← entities, fields, RLS, offline sync
│   ├── stage1-acute.json         ← questionnaire: acute emergency
│   ├── stage2-stabilize.json     ← questionnaire: stabilizing function
│   ├── stage3-recovery.json      ← questionnaire: continuing recovery / reaching out
│   ├── msr-annex-b.json          ← questionnaire: Primary Assistance Center (מס"ר)
│   └── needs-taxonomy.json       ← shared option lists (needs, villages, crisis types)
├── mockups/
│   └── index.html                ← static Hebrew-RTL mobile mockups (open in browser)
└── source-materials/             ← original Hebrew documents from the council
    ├── אפיון כלי מיפוי טכנולוגי.docx
    ├── כתיבה אינטואיטיבית.docx
    ├── תיעוד מאיץ - סמינר 2.docx
    ├── שאלוניים לתושבים (1).docx
    ├── קישור למצגת המסבירה את הקשר בין שיבולת.docx
    ├── מודל עבודה בחירום חוף אשקלון.pdf
    ├── מנחה מקצועי מרכז סיוע ראשוני (מס"ר).pdf
    ├── מערכת יחד — מדריך לרשויות המקומיות.pdf
    ├── חוברת פרויקטים נבחרים.ppsx
    └── _pdf-extracted.txt        ← plain-text dump of all PDFs (for search)
```

## How to use this

1. **Start with `docs/system-goal.md`** — the one-pager answering "what are we building?"
2. Open `mockups/index.html` in a browser to see six screens: home, resident profile, Stage-1 questionnaire, AI summary + proposed tasks, tasks list, village dashboard.
3. Read `spec/PRD.md` end to end before the next council meeting.
4. Share `data-model/*.json` with the team reviewing the questionnaires — these are direct translations of the three Hebrew questionnaires in `שאלוניים לתושבים (1).docx`, plus Annex B of the Home Front Command doctrine.

## Open questions to ask the council

Listed in `spec/PRD.md` §11. Highlights:

- Is the resident identifier linked to national ID, or opaque?
- Which Ministry of Welfare backend integrates first — EPR, Shibolet, or legacy תע"ס?
- Who legally owns the cross-department consent model?
- Day-one scope for מס"ר integration vs. Phase-2?

## Status

- **v0.1** — Synthesis of the materials the council sent on 2026-04-28.
- Nothing built yet. No real data. Mockups are static HTML.
- Next step: 3 x 1-hour workshops with the department to confirm scope, then M1 paper-replacement MVP (see §9 of the PRD).
