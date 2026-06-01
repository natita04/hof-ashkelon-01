# Hof Ashkelon — System Goal (one-pager)

**Client:** Hof Ashkelon Regional Council, Department of Social Services
**Role:** VTO (volunteer-from-tech) engineer
**Date:** 2026-04-28

## What they want me to build

A **mobile-first Hebrew-RTL tool for municipal social workers** that collects, documents, and analyzes emotional and social information about residents and communities, especially during emergencies and the long recovery that follows — so that decisions and community responses stop being ad-hoc.

Working title from the documents: **מערכת דינמית למיפוי חברתי תומכת עובדות סוציאליות** ("dynamic social-mapping system supporting social workers").

## Why

After October 7 and subsequent emergencies (incl. Op. "Am K'lavi"), three brittle realities collided:

1. **Shift handoffs lost knowledge.** Each social worker in evacuation hotels kept notes locally — notebook, personal Google Form, Excel. When she was unavailable, context was gone.
2. **No "ask once."** Many workers touched the same family → residents re-interviewed → residents disengage.
3. **No top-down or bottom-up visibility.** A community social worker could not see her village at a glance: themes, surfacing needs, families unknown to welfare.

## What the system must do

1. **Three-stage emergency questionnaires** on the resident's time axis:
   - Stage A — acute response & triage (urgent / immediate / not urgent)
   - Stage B — stabilizing function (~week 1+)
   - Stage C — continuing recovery + proactive "reaching out" to unknown-to-welfare residents
2. **Resident/family profiles** with continuity of care when workers rotate.
3. **Dynamic analysis** — themes, cross-cuts by age / village / stage / crisis type; simple charts; per-resident, per-village and regional reports.
4. **Actionable insights** — the system generates tasks from conversations, not just records them. Secure AI summarization of long conversations is an explicit requirement ("שתדע לתמצת שיחה ארוכה — למשימות, באמצעות AI מאובטח").
5. **Content management** so the program manager can edit questionnaires without a developer.
6. **Role-based access** for field worker / community worker / program manager / admin.

## Non-functional must-haves

- Hebrew, RTL, calm visual language (blues, greens, light gray)
- **Offline-capable** on phone/tablet in the field
- Pause-&-resume on questionnaires
- Password + OTP, encryption, daily backup
- Data residency in Israel
- Future integrations: Yahad (יחד), EPR/Shibolet, Power BI, resilience center

## The deeper framing

The community social worker should become the **"family doctor of the village"** — always synced, top-down and bottom-up, including on residents unknown to welfare. The tool must **reduce the documentation burden** so handoff is not a separate chore, and it must **generate actionable insights** that flow straight into the worker's task list.

## The hardest problem

**Adoption.** The source docs call this out explicitly ("החלק של ההטמעה מאד משמעותי"). If workers don't want to use it in the field, none of the rest matters. Design decisions should be biased to reducing data-entry burden and to feeling lighter than the current notebook-Google-Form flow.
