# Change Request CR-001: Assign a technician in the manual path

Illustrative change request for the Customer Outage Request case study. It documents a known simplification in the executable process and analyzes the options to fix it.

| Field | Value |
|---|---|
| ID | CR-001 |
| Title | Assign a technician in the manual path |
| Raised by | Oleksiy Milner (business analyst) |
| Date | 2026-10-23 |
| Type | Defect in process design (found during execution) |
| Priority | Should (the demo runs without it, but the result is inconsistent) |
| Status | Proposed |

## 1. Problem

When the skills check fails (HTTP 422), the process routes to the manual task **Clarify and manually Assign to Technician**. That task does not assign anyone. The process then continues through **Start Activity** and **Complete Activity**, so the activity ends `completed` while still assigned to the bucket.

Evidence from the executable demo (database after both runs):

| Activity | Path | Status | Assigned to |
|---|---|---|---|
| 6 | Happy path | completed | technician 31 |
| 7 | Manual path | completed | resource 25 (bucket) |

This contradicts the intent of US-5 (escalated work gets manually assigned) and US-6 (assignment follows the skill rule), and leaves completed work attached to a queue instead of a technician.

## 2. Proposed outcome

After a mismatch or escalation, the dispatcher enters a technician. The system retries the reassignment with the same skill validation (RULE-3). Only a successful assignment lets the process continue to Start and Complete.

## 3. Options

| Option | Description | Assessment |
|---|---|---|
| A. Loop back (recommended) | The manual task collects a `technicianId` and the flow returns to **Reassign Activity To Technician**. A repeated mismatch returns to the dispatcher. | Reuses the existing reassignment call and its validation; no API change; consistent end state. |
| B. Separate assignment call | After the manual task, add another service task that calls `PATCH` with the dispatcher's choice. | Duplicates the reassignment step and needs its own error handling. |
| C. End the process after the manual task | The process ends with "Assigned manually"; handling continues outside it. | Avoids the wrong end state, but status tracking stops and the activity stays in the bucket. |
| D. Leave as is | Keep the documented simplification. | No effort; inconsistent data remains. |

**Recommendation:** Option A.

## 4. Impact analysis (Option A)

| Area | Impact | Change | Effort (estimate) |
|---|---|---|---|
| Requirements | New requirement and updated acceptance criteria | Add FR-14: after a mismatch or escalation the dispatcher can enter a technician and the system retries the reassignment (Must). Extend US-5 with that criterion. Partially answers OQ-2. | Small |
| BPMN process | Flow and task changes | Link a form with a numeric field `technicianId` to the manual task. Add a flow from the manual task back to Reassign. Remove the "Technician is Assigned" gateway, as the happy path no longer merges. | Medium |
| OpenAPI spec | None | `PATCH` already accepts `resourceId` and documents 422. | None |
| Data model / ERD | None | No new entities or attributes. | None |
| Mock service | None | Skill validation already runs on every reassignment. | None |
| Tests | New scenarios | Mismatch, then manual entry of a qualified technician, ends with resource 31 and status `completed`. A second mismatch returns to the manual task. | Small |
| Documentation | Updates | Remove the known simplification from the executable README, add new evidence, and update the traceability table. | Small |

Estimated total effort: about half a day (assumption).

## 5. Risks and dependencies

| Risk | Mitigation |
|---|---|
| Repeated mismatches create a loop | Each loop needs human input; add a dispatcher option to cancel the request if needed. |
| A new process version does not change running instances | Instances started before deployment finish on the old version; run new tests on new instances. |
| The form must be linked correctly to the user task | Verify by completing the task in Tasklist and checking the `technicianId` variable. |

No external dependencies.

## 6. Acceptance criteria for the change

- Given `technicianId` 32 at start, when the dispatcher enters 31 in the manual task, then the activity is assigned to technician 31 and finishes `completed`.
- Given the dispatcher enters an unqualified technician, then the response is 422 and the process returns to the manual task.
- After any run, no completed activity remains assigned to a bucket.

## 7. Traceability

Affects US-5, US-6, FR-5, FR-6, FR-10, RULE-3 and OQ-2, and the BPMN steps **Reassign Activity To Technician** and **Clarify and manually Assign to Technician**.

## 8. Approval

| Role | Name | Decision | Date |
|---|---|---|---|
| Requestor | Oleksiy Milner | Submitted | 2026-10-23 |
| Reviewer | Development team lead | Pending | |
| Approver | Service manager | Pending | |
