# Elicitation Plan: Customer Outage Request (Illustrative Case Study)

This plan describes how requirements for the outage scenario would be gathered and confirmed. It is illustrative: it shows the approach, and no real sessions were held. It builds on the draft requirements document and the BPMN, ERD and API artifacts.

## 1. Objective and scope

**Objective:** confirm the draft requirements, resolve the open questions, and agree priorities with the people who run and use the process.

**Open questions to resolve** (from section 11 of the requirements document):

| ID | Open question |
|---|---|
| OQ-1 | How is the SLA defined: time to respond, or time to restore service? |
| OQ-2 | What does the dispatcher decide after escalation, and how is it recorded? |
| OQ-3 | Should work be creatable directly on a technician, and with which checks? |
| OQ-4 | Are spare parts and inventory in scope, and at what level of detail? |

## 2. Stakeholders

| Stakeholder | Influence | What we need from them | Main technique |
|---|---|---|---|
| Service manager | High | SLA definition, priorities, performance targets | Interview, workshop |
| Dispatcher | High | How work is assigned, escalation handling, daily pain points | Observation, interview |
| Technician | Medium | Field constraints, status updates, missing-part handling | Interview, observation |
| Customer (representative) | Medium | How problems are reported, expected updates, confirmation | Survey, interview |
| Development team lead | Medium | Interfaces, authentication, performance and integration limits | Interface analysis, interview |

## 3. Techniques by phase

| Phase | Goal | Technique | Participants | Output |
|---|---|---|---|---|
| 1. Discovery | Understand the current process and goals | Document analysis, interviews | Service manager, dispatcher | Process notes, confirmed scope, answer to OQ-1 |
| 2. Detail | Capture exceptions and real behavior | Observation (job shadowing), interviews | Dispatcher, technician | Exception list, answers to OQ-2 and OQ-4 |
| 3. Rules and data | Make rules and data precise | Business rules workshop, data analysis (SQL on past activities), interface analysis | Dispatcher, service manager, development lead | Confirmed rules, data needs, answer to OQ-3 |
| 4. Validation | Check that the model matches reality | Prototype review (Figma screen), BPMN walkthrough, MoSCoW prioritization workshop | All stakeholders | Corrected models, prioritized requirements |
| 5. Confirmation | Agree the final set | Document review, sign-off | Service manager, dispatcher, development lead | Approved requirements |

## 4. Key questions by stakeholder

**Service manager**
- Does the SLA measure time to respond or time to restore service? What happens when it is breached? (OQ-1)
- Which measures matter most: SLA compliance, first-time fix, customer confirmation rate?
- What are acceptable response times for the system (NFR-1)?

**Dispatcher**
- After an escalation, how do you choose a technician and what do you record? (OQ-2)
- What information is usually missing when work is escalated?
- How often are valid requests left unmatched for 30 minutes?

**Technician**
- What most often blocks a repair, and how is a missing part handled? (OQ-4)
- How and when do you update the status of a job, and what if there is no connection?

**Customer representative**
- How do you report an outage today, and what updates do you expect?
- What does "confirmed" mean to you: a call, a message, a click?

**Development team lead**
- Which interfaces and authentication methods are available today?
- Can work be created directly on a technician, and where should the skill be checked? (OQ-3)

## 5. Schedule (illustrative)

| Session | Phase | Duration | Participants |
|---|---|---|---|
| 1. Goals and SLA interview | 1 | 45 min | Service manager |
| 2. Dispatcher interview | 1, 2 | 60 min | Dispatcher |
| 3. Dispatcher shadowing | 2 | 2 hours | Dispatcher |
| 4. Technician interview | 2 | 45 min | Technician |
| 5. Business rules and interface workshop | 3 | 90 min | Dispatcher, service manager, development lead |
| 6. Validation workshop (prototype and BPMN) | 4 | 90 min | All stakeholders |
| 7. Requirements review and sign-off | 5 | 60 min | Service manager, dispatcher, development lead |

A short customer survey runs in parallel with sessions 1 to 4.

## 6. Preparation checklist

- Objectives and an agenda per session, with the questions from section 4
- Materials: the BPMN diagram, ERD, draft requirements and the Figma prototype
- Pre-reading sent two days before workshops
- Agreement to take notes and, where allowed, record
- A clear owner for each open question

## 7. Documentation and follow-up

- Notes are written within 24 hours and sent to participants for confirmation.
- Every new or changed requirement gets an ID and a source (session and stakeholder), and is added to the traceability table.
- An issues log tracks open questions and decisions with owner and date.
- Conflicting views are recorded and resolved in the validation workshop, not by assumption.

## 8. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Key stakeholders have little time | Short, focused sessions; asynchronous questions for follow-up |
| Dispatcher and manager disagree on priorities | Joint workshop with MoSCoW prioritization and a named decision-maker |
| Stated process differs from actual practice | Observation and analysis of past activity data, not interviews alone |
| Scope grows during sessions | Compare each request with the scope in section 1 of the requirements document; park extras in the issues log |

## 9. Success criteria

- All four open questions are answered and recorded.
- Every user story has acceptance criteria that its stakeholders have reviewed.
- The requirements are prioritized and approved by the service manager, dispatcher and development lead.
