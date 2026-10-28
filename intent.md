# Intent: results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md`
(discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17
and is history only: nothing in this intent is approved on its authority alone.

## Problem

- **I-1** The fictional clinic receives avoidable calls from patients checking whether their results are
  ready. (PRD, "Problem and outcome")

## Proposed outcome

- **I-2** A patient is told that an update is available once their result has been reviewed and released
  by a doctor, and is directed to the existing authenticated portal to view it. (PRD, "Problem and
  outcome"; "Approved constraints")
- **I-3** "Ready" means doctor-reviewed and released, not that the laboratory upload finished. (Discovery
  notes, product owner)
- **I-4** Success will be observed by comparing these against a baseline measured during discovery:
  - status-only results calls, counted separately from clinical questions (PRD; discovery notes, support
    lead);
  - delivery failures (PRD, "Problem and outcome").
- **I-5** No baseline value or numerical target is approved yet. The PRD says both will be agreed during
  discovery; this intent sets neither.

## Affected users and systems

- **I-6** Patients: the approved recipients of the notification. Referring doctors may also become
  recipients, but only if OPEN-2 approves it; they are not approved now.
- **I-7** Doctors: their review and release of a result is the gate for any notification.
- **I-8** Support staff: they handle the calls being measured and separate status-only calls from
  clinical questions.
- **I-9** The existing authenticated portal and its existing result route, which the notification points
  to. Engineering considers the existing portal path adequate. (PRD, "Scope"; discovery notes, engineering
  lead)
- **I-10** The notification and delivery mechanism and its logs. The channel is not yet decided (OPEN-1).

## Out of scope

- **I-11** Portal redesign. (PRD, "Scope")
- **I-12** A new or rebuilt patient dashboard: a separate project. (Discovery notes, engineering lead)
- **I-13** Appointment scheduling. (PRD, "Scope")
- **I-14** Clinical interpretation of results. (PRD, "Scope")
- **I-15** Notifying third parties. (PRD, "Scope") The referring doctor is a separate, open decision
  (OPEN-2), not approved.
- **I-16** Excluded because they came only from the superseded request: sending every result as soon as
  the lab uploads it, putting the test result in the message, copying the referring doctor, rebuilding
  the patient dashboard and a 90% call reduction in one week. SMS as a channel is neither approved nor
  excluded: it remains part of OPEN-1.

## Constraints

- **C-1** A doctor must review and release the result before any notification is eligible. (PRD,
  "Approved constraints"; reaffirmed by the product owner in the discovery notes)
- **C-2** The message only says that an update is available and directs the patient to the existing
  authenticated portal. No diagnosis, test result, test name, clinical measurement or treatment detail
  appears in the message or in notification or operational logs. (PRD; discovery notes, privacy owner)
- **C-3** Access to results is authenticated in the existing portal. No result is attached and no public
  result link is introduced. (PRD, "Approved constraints")
- **C-4** Prevent duplicate notifications for the same released result version. (PRD)
- **C-5** Handle delivery failures without sending the patient duplicate messages. (PRD)
- **C-6** Until OPEN-2 is decided, no recipient other than the patient is approved. Whether the
  referring doctor is notified is left to the clinical and privacy owners and is not inferred from the
  superseded request. (PRD, "Scope" and "Unresolved decisions"; discovery notes, clinical owner)

## Open questions and owners

- **OPEN-1 Channel** (SMS, email or portal push). Owners: product owner and privacy owner. They decide
  after the consent and delivery review, which is still under way. (PRD, "Unresolved decisions";
  discovery notes, privacy owner)
- **OPEN-2 Referring-doctor notification.** Owners: clinical owner and privacy owner. They must decide
  whether the referring doctor is an authorized recipient. Until then it is not approved. (PRD,
  "Unresolved decisions"; discovery notes, clinical owner)

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
