# Data Model — CRCC Platform

Conceptual schema for v1. Neutral to storage (Postgres, Mongo, or Firestore all viable). Names are English for code; Hebrew display labels are stored in a label catalog so the CMS can edit them without a deploy.

---

## 1. Core entities (ERD, text form)

```
Resident ─────< FamilyMembership >───── Family
   │                                       │
   │                                       │
   └──< Encounter >──── Questionnaire ─────┘
              │
              ├──< Answer
              ├──< Task
              └──< AIsummary (optional)

Village ──< Resident
Village ──< WorkerAssignment >── User(worker)

Tag ──< ResidentTag >── Resident
Tag ──< AnswerTag    >── Answer   (for theme analysis)

Questionnaire ──< QuestionnaireVersion ──< Question ──< QuestionOption
```

---

## 2. Entities

### Resident

Anonymous internal ID. Linkage to national ID is configurable per local authority.

| Field | Type | Notes |
|---|---|---|
| id | UUID | internal |
| national_id | string \| null | nullable; council decides |
| first_name | string | |
| last_name | string | |
| gender | enum(`male`,`female`,`other`,`undisclosed`) | |
| date_of_birth | date | |
| phone | string | |
| village_id | FK → Village | |
| current_address | string | |
| known_to_welfare | bool | Surfaces Stage-3 "reaching out" list |
| health_fund | enum(`kupat_holim_*`) | Hebrew: קופ"ח |
| consent_cross_dept | bool | Controls §7.5 data-sharing flag |
| special_needs_flags | string[] | from controlled vocabulary |
| active_case_worker_id | FK → User | the on-duty worker |
| created_at, updated_at | timestamp | |

### Family

Household unit. One resident can be the head; spouse + children link via `FamilyMembership`.

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| status | enum(`single`,`partnered`,`married_no_kids`,`married_with_kids`,`common_law_no_kids`,`common_law_with_kids`,`divorced`,`widowed`,`adult_kids_out_of_home`) | mirrors the Stage-1 questionnaire |
| num_children | int | |
| extended_family_nearby | bool | mirrors Stage-1 |
| current_address | string | may differ from individual resident address |
| primary_contact_resident_id | FK → Resident | |

### FamilyMembership

| Field | Type | Notes |
|---|---|---|
| family_id | FK | |
| resident_id | FK | |
| role | enum(`head`,`spouse`,`child`,`other`) | |
| evacuation_status | enum(`at_home`,`evacuated_with_family`,`evacuated_alone`,`injured`,`serving`,`other`) | per child per the Stage-1 form |
| special_need_or_urgent_need | string | free text + tag |

### Village

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| name_he | string | |
| local_authority | string | e.g., "חוף אשקלון" |
| population | int | optional |

### User (worker)

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| full_name | string | |
| phone | string | for OTP |
| email | string | |
| role | enum(`field_worker`,`community_worker`,`program_manager`,`admin`,`msr_worker`) | see PRD §3 |
| assigned_villages | FK[] → Village | row-level security scope |
| is_active | bool | |

### Questionnaire (template) & QuestionnaireVersion

`Questionnaire` is the logical template (e.g., "Stage 1 — Acute"). `QuestionnaireVersion` is an immutable snapshot.

| Questionnaire field | Type | Notes |
|---|---|---|
| id | UUID | |
| code | enum(`STAGE_1_ACUTE`,`STAGE_2_STABILIZE`,`STAGE_3_RECOVERY`,`MSR_INTAKE`,`CUSTOM_*`) | |
| title_he | string | |
| description_he | text | |
| stage | enum(`1`,`2`,`3`,`msr`,`ad_hoc`) | drives UI affordances |
| is_active | bool | |

| QuestionnaireVersion field | Type | Notes |
|---|---|---|
| id | UUID | |
| questionnaire_id | FK | |
| version_number | int | monotonic |
| published_at | timestamp | |
| published_by | FK → User | |

### Question

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| questionnaire_version_id | FK | |
| group_he | string | e.g., "רגש", "קהילה", "צורך", "תפקוד" |
| order | int | within version |
| label_he | text | question wording |
| help_he | text \| null | |
| type | enum(`rating_1_5`,`open_text`,`single_choice`,`multi_choice`,`checkbox`,`date`,`number`,`phone`,`nested_table`) | |
| required | bool | |
| category | enum(`emotional`,`community`,`material`,`rights`,`bereavement`,`special_needs`,`security_service_family`,`functional_routine`,`other`) | auto-coding bucket |
| conditional_on | JSON | branching: `{ question_id, op: 'eq', value }` |
| icon_name | string | renders beside the question |

### QuestionOption

For `single_choice`, `multi_choice`, `checkbox`.

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| question_id | FK | |
| label_he | string | |
| order | int | |
| category_tag | string \| null | auto-tag if this option is selected |

### Encounter

One sit-down with a resident (or their household rep). Links to one `QuestionnaireVersion`.

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| resident_id | FK | |
| family_id | FK \| null | |
| questionnaire_version_id | FK | |
| worker_id | FK → User | the worker on shift |
| stage | enum(`1`,`2`,`3`,`msr`,`ad_hoc`) | |
| started_at | timestamp | |
| paused_at | timestamp \| null | "pause & resume" |
| completed_at | timestamp \| null | |
| status | enum(`in_progress`,`paused`,`completed`,`abandoned`) | |
| location | enum(`in_home`,`msr`,`hotel`,`shelter`,`phone`,`other`) | |
| offline_captured | bool | for audit |
| worker_reflection_he | text | "worker summary" screen |
| overall_urgency | enum(`urgent`,`immediate`,`not_urgent`,`no_followup_needed`) | Stage-1 triage outcome |
| followup_recommendation | text | |

### Answer

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| encounter_id | FK | |
| question_id | FK | |
| value | JSON | shape depends on `Question.type` |
| carried_forward_from_encounter_id | FK \| null | enforces "ask once" |
| edited_at | timestamp | |

### Task

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| encounter_id | FK \| null | may also be standalone |
| resident_id | FK | |
| assignee_id | FK → User | |
| title_he | string | |
| description_he | text | |
| due_date | date | |
| status | enum(`open`,`in_progress`,`done`,`cancelled`) | |
| source | enum(`manual`,`ai_suggested_accepted`,`ai_suggested_edited`) | |
| priority | enum(`low`,`normal`,`high`,`urgent`) | |

### AiSummary

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| encounter_id | FK | |
| raw_transcript | text | |
| summary_he | text | AI output |
| proposed_tasks | JSON[] | surfaced to worker for accept/edit |
| proposed_tags | string[] | surfaced to worker |
| accepted_at | timestamp \| null | null = draft |
| accepted_by | FK → User \| null | |

### Tag / ResidentTag / AnswerTag

| Field | Type | Notes |
|---|---|---|
| Tag.id | UUID | |
| Tag.label_he | string | |
| Tag.kind | enum(`controlled`,`free`) | admin manages controlled |
| Tag.category | enum(same as `Question.category`) | |

### WorkerAssignment

| Field | Type | Notes |
|---|---|---|
| user_id | FK | |
| village_id | FK | |
| role_at_village | enum(`primary`,`backup`,`community`) | supports shift coverage |
| effective_from | timestamp | |
| effective_to | timestamp \| null | |

### AuditEvent

| Field | Type | Notes |
|---|---|---|
| id | UUID | |
| actor_id | FK → User | |
| action | enum(`view`,`edit`,`export`,`share`,`login`,`consent_change`) | |
| target_type | enum(`resident`,`encounter`,`answer`,`task`,`export`) | |
| target_id | UUID | |
| context_ip | string | |
| at | timestamp | |

---

## 3. Row-level security

- `field_worker`: sees residents where `resident.village_id ∈ user.assigned_villages`.
- `community_worker`: sees residents in her village(s) + aggregated theme data across them.
- `program_manager`: sees all villages in the regional council.
- `admin`: sees everything + manages CMS.
- `msr_worker`: sees only encounters created at `location = msr` in the past 72 hours.

Every cross-department read (i.e., a worker viewing a resident from another village or another department) requires `resident.consent_cross_dept = true` **and** emits an `AuditEvent`.

---

## 4. Offline & sync

- Client stores encounters + answers in IndexedDB.
- Sync protocol: on connectivity, POST new encounters, then PATCH answers.
- Conflict resolution: field-level last-writer-wins with an `edited_at` timestamp. A server-side audit record captures every overwrite.
- The sync worker maintains a local staleness map so the UI can show "⚠ offline copy — last synced 2h ago" per record.

---

## 5. Seed data — the three questionnaires

Full JSON definitions are in `stage1-acute.json`, `stage2-stabilize.json`, `stage3-recovery.json` in this directory. They are a direct translation of the three Hebrew questionnaires in `source-materials/שאלוניים לתושבים (1).docx`.

The מס"ר intake form is in `msr-annex-b.json` — translated from Annex B of the Home Front Command doctrine.
