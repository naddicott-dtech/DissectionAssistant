# Dissection VR Platform — API Reference (v0.2 Minimal Version)

## Core JSON Files and How They Relate

- **steps.json**: Main workflow. Each step references tool IDs (tools.json) and optionally rubric IDs (assessment_rubric.json).
- **tools.json**: All tool IDs, names, models, and core interaction types.
- **assessment_rubric.json**: Defines criteria and rubric IDs for assessment. All rubrics use a fixed 4-point scale (1=emerging, 4=advanced).

## Key Step JSON Fields

| Field                  | Type     | Description                                                        |
|------------------------|----------|--------------------------------------------------------------------|
| id                     | string   | Step identifier (unique)                                           |
| title                  | string   | Short display name                                                 |
| description            | string   | Instructions for the user                                          |
| required_tools         | [string] | Tool IDs from tools.json required for the step                     |
| valid_tool_combinations| [[string]]  | Each is a valid array of tool IDs for this step                    |
| on_wrong_tool          | object   | Message/hint to user + (optional) score change                     |
| on_success             | {actions:[]} | Array of actions to trigger on correct completion                 |
| final_state            | dict     | What each model/part should look like at end (e.g. visible, highlight)|
| assessment.rubric_ids  | [string] | List of rubric IDs (must exist in assessment_rubric.json)          |

## Tool Object Fields (tools.json)

| Field         | Type     | Description                  |
|---------------|----------|------------------------------|
| display_name  | string   | Name for UI                  |
| model_path    | string   | Path to glb/bin asset        |
| interaction_types | [string] | Actions supported (cut, grip, probe, etc.) |
| default_hand  | string   | 'primary' or 'secondary'     |

## Assessment Rubric Fields

| Field         | Type     | Description                  |
|---------------|----------|------------------------------|
| id            | string   | Rubric key (referenced in steps) |
| display_name  | string   | UI label                     |
| criteria      | string   | Brief description            |

> All assessment rubrics are 4-point scale:  
> 1 = emerging, 2 = developing, 3 = proficient, 4 = advanced

## Cross-file Requirements

- Every `required_tools`/`valid_tool_combinations` entry must appear in tools.json.
- Every step `assessment.rubric_id` must match an entry in assessment_rubric.json.
- If model/part names are referenced in steps/final_state, asset naming must match.

## Conventions

- Use only fields/types as defined here unless a PR to schema/docs is approved.
- Update this reference with any schema evolution.
