Failure_Post-Mortem_Template.md

Moon Ice Crawler — Phase 3

Purpose

This template defines how failures are investigated, documented, and learned from.

It exists to prevent:

blame theater

PR-driven distortion

shallow “lessons learned”

leadership evasion

repeated mistakes

A failure post-mortem is not punishment.
It is institutional learning.

If a failure happens and the organization learns nothing,
that is the worst failure.

🚨 FAILURE POST-MORTEM REPORT
1️⃣ Failure Overview

Date / Time:
Location / Operational Context:
Phase of Operation: (planning / transit / extraction / transfer / shutdown / deployment / EOL etc.)
Authority in Command at Time:
Operators Present:
Engineering On-Call / Active:
Oversight Status: (present / remote / inactive)

Short Plain-Language Summary (no spin, no engineering jargon):

What happened?

What was the immediate visible effect?

Was safety compromised?

2️⃣ Incident Classification

Failure Category:

 Benign Failure

 Mission-Ending but Safe Failure

 Governance Failure

 Unacceptable Failure

Operational Outcome:

 Self-recovered

 Required operator intervention

 Required governance intervention

 Permanent retirement / EOL triggered

Severity Assessment (justify):

Technical Severity:

Operational Severity:

Governance Impact:

Safety Risk:

3️⃣ Timeline of Events

Provide a precise, chronological account with times.

Time	Event	Source (telemetry/operator/report)
		
		
		

Timeline must show:

what happened

what was known when

what actions were taken

how long decisions took

No vague approximations unless truly unavoidable.
If times are unknown, state that and explain why.

4️⃣ Data & Evidence

Attach or reference:

telemetry logs

operator communications

system health records

environment data

governance decisions / approvals

relevant screenshots / extracts

If data is missing:

explicitly state what data is absent

explain why

treat missing data as its own failure

Evidence must support conclusions.
No evidence = no confidence.

5️⃣ Root Cause Analysis
5.1 Primary Cause (Direct Failure Mechanism)

What physically or operationally failed?

5.2 Contributing Factors

(list all, with explanation)

technical

operational

environmental

governance

cultural

5.3 Why Did This Happen? (Five-Whys or Equivalent)

Drive downward, not sideways.

6️⃣ Governance & Doctrine Evaluation

Did governance function?

Did operators follow doctrine?

Did escalation follow rules?

Was stop authority exercised correctly?

Did independent oversight behave appropriately?

Was reporting honest?

If any governance layer failed:

document how

document why

treat governance failure as seriously as technical failure

7️⃣ Risk Posture Impact

Did this incident:

change our understanding of risk?

reveal hidden risk?

expose underestimated risk?

invalidate previous assumptions?

challenge reliability expectations?

If the risk map changes,
record how the map changes.

8️⃣ Immediate Response Evaluation

What did the team do right?

list disciplined responses

list actions that prevented worse outcomes

Where did response fall short?

slow decisions?

bad assumptions?

hesitation?

pressure?

breakdown in communication?

This section must be honest.
Not flattering.
Not performative.

9️⃣ Outcome

Final State:

returned to normal ops

reduced mode continuing

temporarily halted program posture

retirement / EOL declared

additional review required

Residual Risks Remaining:
(list clearly)

🔟 Corrective Actions
Technical Corrections

Required changes to:

design

limits

monitoring

thresholds

software behavior

Include:

responsible owner

completion deadline

verification method

Operational Corrections

Required changes to:

procedures

checklists

execution rhythm

training

crew discipline

Include:

responsible owner

completion deadline

verification method

Governance Corrections

Required changes to:

authority enforcement

oversight behavior

reporting cadence

doctrine clarity

Include:

responsible owner

completion deadline

verification method

1️⃣1️⃣ Lessons Learned (Institutional)

Short, blunt statements capturing real truths the program learned.

Examples format:

“We discovered X belief was wrong because…”

“We assumed Y but reality showed…”

“We must never again…”

“We underestimated…”

If lessons learned are vague,
the program will repeat the mistake.

1️⃣2️⃣ Verification & Closure

Has corrective action been implemented?

 Yes

 No

 Partially

Has independent oversight confirmed closure?

 Yes

 No

Has the system been revalidated for operation?

 Yes

 No

Final Closure Statement:
(Short, clear, accountable)

Signatures / Approvals Required:

Strategic Authority

Operations Command

Engineering Oversight

Independent Oversight

Cultural Rule

This report is not a punishment tool.
It is a truth tool.

If people fear this report,
governance already failed.

Success Criteria

This template succeeds when:

failures are documented honestly

leadership is accountable

learning is real

repeated failures decrease

trust increases

operations become calmer and safer

If the organization treats post-mortems as paperwork,
Phase-3 doctrine has collapsed.
