# Dissection VR Project — ARCHITECTURE.md

## Repositories and Roles

**DissectionAssistant**
- Canonical source for schemas (steps, tools, rubrics), API_REFERENCE.md, model/part naming, and all project-wide documentation.
- Editing: Maintained by instructors/TAs; students do not edit directly during class.
- Contains:
  - `/docs/API_REFERENCE.md` (schema, naming, cross-file rules)
  - `/docs/ARCHITECTURE.md` (this guide)
  - `/data/steps.schema.json`, `tools.json`, `assessment_rubric.json`, `[specimens.json if needed]`

**DissectionAFrame** and **DissectionGodot**
- Engine-specific implementations (browser or Godot). Used for hands-on student work and course development.
- Editing: Students frequently modify and prototype (code, data, assets).
- Each contains:
  - `/data/steps.json`, `tools.json`, `assessment_rubric.json` (local copies for iteration)
  - `/models/` or `/assets/` (engine-compatible 3D/2D files)
  - Engine-specific code and config
  - `README` referencing canonical `/docs/API_REFERENCE.md` in DissectionAssistant (**do not copy or edit API reference locally**)

---

## Conventions

- All `/data/` files in engine repos must match schema from DissectionAssistant (but may be student-modified for prototyping).
- Canonical documentation (`API_REFERENCE.md`, schema) exists *only* in DissectionAssistant; all other repos link to it.
- Script naming:
  - Godot: `snake_case`
  - JavaScript (A-Frame): `camelCase`
- Model/part names in steps, assets, and schemas must match those in API reference.

---

## Workflow

- Students edit: `/data/` and engine code/assets in AFrame/Godot repos.
- Instructors/TAs manage: core schema, main documentation, and templates in DissectionAssistant repo.
- If schema or conventions change: update DissectionAssistant docs, then notify all teams to update their local engine copies.

---

## Contribution & Reference

**To author or revise content:**
1. Students: edit local `/data/steps.json` and related files in engine repos.
2. For schema or project-wide convention updates: instructors modify DissectionAssistant and communicate the changes.
3. *Always* refer to the canonical [`API_REFERENCE.md`](../DissectionAssistant/docs/API_REFERENCE.md) in DissectionAssistant (do **not** copy or edit in engine repos).

---

## Table: Ownership & Usage

| Repo                | Main Role        | Edited By          | Docs/Schema Location           |
|---------------------|------------------|--------------------|-------------------------------|
| DissectionAssistant | Canonical Docs   | Instructor/TA      | /docs (only here)             |
| DissectionAFrame    | Engine + Data    | Students/Instructors | Linked from DissectionAssistant |
| DissectionGodot     | Engine + Data    | Students/Instructors | Linked from DissectionAssistant |

---

*This document is intentionally concise. Update it if repos, conventions, or file locations change.*
