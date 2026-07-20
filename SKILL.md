---
name: use-case-to-user-story
description: 'Turn a vague customer use case into one compelling, end-to-end Port user story that a Sales Engineer or TSM can build in a live demo. This is a CONVERSATION, not a one-shot — it asks the user for the raw use case, then runs a short three-question discovery (industry, tech stack, the "before" cost), stopping to wait for real answers before drafting. It then drafts the story to a high quality bar (real persona, real value, Port-differentiated) and always proposes an agentic extension: an autonomous, conditional Port workflow that acts on its own, labeled natural-fit or stretch. Outputs the story plus acceptance criteria as OUTCOMES, not a data model, so Cursor and the port-* skills design the build. Use at the very start of any vibe-build, before plan mode, before touching the Port MCP. Trigger on any fuzzy customer ask like "they want visibility into their services" or "developers should self-serve environments".'
---

# Use case → user story

Takes one fuzzy sentence from a customer and produces a demo-ready user story: persona +
goal + measurable value, pushed up Port's differentiation ladder, with an agentic extension
that turns an action into an autonomous, conditional workflow. It stops at the story and
acceptance *outcomes* — it does NOT design blueprints, properties, or a data model. That's
the job of Cursor + the `port-*` skills. Handing over a finished schema muzzles them;
handing over strong outcomes sets them free.

## How to run — two gates, then output

This skill is a conversation. You MUST stop and wait for the user at the two gates below.
Never guess, auto-fill, or hallucinate the user's answers — if you invent the industry or
tech stack, the whole story is fiction.

### Gate 1 — Intake
Ask for the raw customer use case in one line, exactly as the customer said it. Don't
refine it yet. **STOP. Wait for the user's reply before continuing.**

### Gate 2 — Discovery
Ask these three questions in one clean message, then **STOP and wait for the answers**:
1. **Customer industry?** (e.g. retail bank, insurance, e-commerce)
2. **Tech stack relevant to this use case?** (source control, CI/CD, cloud, ticketing, plus
   anything central like K8s, Terraform, observability)
3. **What does this cost them today?** — the "before": how long, how often, who's blocked.
   Push for a real number. If the user doesn't have one, offer a typical industry baseline
   as an anchor (e.g. "devs waiting ~3 days on an environment ticket") and label it clearly
   as an ASSUMPTION — never present an invented number as the customer's real figure.

Do not proceed to drafting until the user has answered.

### Internal work — no gate, do this silently, then print
With intake + discovery in hand, work through these against the quality bar at the bottom
(do NOT show this working to the user):
- **Draft** the story: *As a [persona], I want [goal], so that [measurable outcome].*
  Run the structural self-check and fix anything it catches.
- **Push differentiation** up the ladder to at least ownership/RBAC territory — not
  visibility-only. If the raw ask is pure visibility, find the action hiding inside it and
  center the story there.
- **Agentic extension** — take the strongest action in the story and add an autonomous,
  conditional workflow (see "What makes a story agentic" below). Label **[Natural fit]** if
  the story already has an action to enhance, **[Stretch]** if you had to introduce one —
  and even a Stretch must be a real workflow, never a chatbot.
- **Acceptance criteria as OUTCOMES** — what "done" looks like from the user's side. No
  property names, enums, cardinality, or data model.
- **POC-fit check** — score the story against the five POC-fit dimensions below. All five
  must PASS. If a dimension fails but is fixable (scope too broad, value not tied to the
  named before-cost, weak demo moment), fix the story and re-check silently. If a dimension
  carries a STRUCTURAL risk you cannot fix (not buildable from the chosen stack's native
  blueprints inside the timebox, or it needs a Port capability that doesn't exist), still
  print the best story you can AND surface the honest POC-fit note in the output — never ship
  a polished story that can't be built or demoed without warning the user.

### Output
Print the Output Template below, filled in, and NOTHING else. Specifically: do NOT print
the text of the Structural self-check, the Differentiation ladder, the ladder score, the
POC-fit dimensions, or the Agentic quality bar — those are internal grading criteria only.
The user sees only the finished template.

The ONE exception: if the POC-fit check found a structural risk you could not fix, add the
optional POC-fit note (shown at the top of the template) above the user story. If all five
POC-fit dimensions pass, omit that block entirely.

---

## Output template

```
[Optional — include ONLY if a structural POC risk remains after the fix pass; otherwise omit this whole block:]
## ⚠ POC-fit note
Risk: [the specific risk in one line — buildability in the timebox, or a needed capability that doesn't exist]
Nearest buildable alternative: [the closest use case that WOULD land live in the timebox]

## User story
As a [persona], I want [goal], so that [measurable outcome].

## Before → After
Before: [pain today, with a number if given — mark any industry baseline as an assumption]
After:  [measurable improvement]

## Why this lands for Port
[one line: where it sits on the ladder and why Backstage/Cortex can't easily match it]

## Acceptance criteria (outcomes — what "done" looks like)
1. ...
2. ...
3. ...

## Agentic extension  [Natural fit | Stretch]
Enhances: [the base action this builds on]
Trigger: [the request or event that starts the workflow]
Autonomous action: [what the platform does by itself, no human in the loop]
Guardrail: [the condition that bounds the autonomy — e.g. dev -> auto, prod -> approval; owner-only]
Outcome: [the concrete result]
Pillars: [which 2+ of: autonomous workflow · guardrails/policy · skills catalog · MCP]
Adds to acceptance criteria:
  - [1-2 extra outcomes the workflow introduces]

## Build note
Reuse Port's existing / native blueprints and relations wherever they fit. Create a new
blueprint or relation ONLY if the one you need does not already exist — do not duplicate
what's already in the catalog. Hand the story + criteria to Cursor plan mode; let the
port-* skills design or extend the model.
```

---

## Reference: internal quality bar (grading criteria — never printed to the user)

### What makes a story AGENTIC (read this first)
An agentic user story enhances an existing action or flow with a Port workflow/automation
that ACTS ON ITS OWN — it triggers on a request or event, branches on context, and
autonomously performs a downstream action to reach an outcome. The agentic part is the
PLATFORM ACTING, conditionally and autonomously — not a human being assisted.

Shape to aim for:
- Base story: a self-service action to provision a cloud resource.
- Agentic enhancement: a Port workflow that, when the request is for a DEV environment,
  automatically opens the PR and provisions it — while PROD requests route to approval.
  The platform did the work; the dev-vs-prod condition is the guardrail.

NOT agentic — never propose these as the agentic story:
- "Ask the catalog / context lake questions in natural language" → a Port FEATURE and a
  read, not a story. No workflow, no outcome.
- "A chatbot / assistant that recommends what to do" → assistance, not autonomous action.
- Anything that only surfaces information or advises a human.

### Agentic extension — how to build one
Take the strongest action already in the story and wrap it in an autonomous workflow:
- **Trigger** — the request or event that starts it.
- **Autonomous action** — what the platform does by itself for the eligible path (open a PR,
  provision, register, remediate). No human step in the happy path.
- **Guardrail** — the condition that bounds the autonomy: which cases run automatically vs
  escalate to a human (dev -> auto, prod -> approval; owner-only; below a risk threshold).
- **Outcome** — the concrete result.
Must hit 2+ pillars: autonomous workflow · guardrails/policy · skills/agent catalog · MCP.

### Agentic pattern map
- **Provisioning / self-service action** → workflow auto-executes the safe path (dev request
  -> auto-open PR + provision) and routes risky paths (prod) to approval.
- **Scorecard / compliance** → automation that, when a rule fails, auto-opens a fix PR or
  applies the remediation for known-safe cases and escalates the rest.
- **Incident / operational** → automation that, on an event, pulls blast radius from the
  catalog and auto-runs a known-safe remediation, escalating otherwise.
- **Pure visibility** → not agentic by itself. Find the action hiding inside it and enhance
  THAT. If there is genuinely no action, it is a [Stretch]: introduce a small real action
  (e.g. auto-assign an owner when a new unowned service is detected) and let the workflow act.

### Structural self-check (fix before output)
- No persona → add one.
- No "so that" value → add the outcome.
- Written as a system requirement ("the solution should allow RBAC") → rewrite from the
  user's point of view with a real outcome.
- "So that" is vague or a feature, not a business outcome → sharpen it, add the number.
- No scope boundary ("surface everything an engineer needs") → force an explicit scope.

### Differentiation ladder (score 1–10 — the story must land at 8 or higher)
Score the story by the highest rung it genuinely reaches. 8+ means Moat or Top, and that is
the pass line. If it scores below 8, climb: find the action hiding in the ask and center it.
- **Weak — catalog / visibility only (1–3):** competitors do this natively. Push up.
- **Better — basic action or scorecard (4–5):** competitors approximate with plugins.
- **Strong — multi-step action (6–7):** repo creation + catalog registration + scorecard trigger.
- **Moat — ownership / RBAC-scoped (8–9):** owner-only triggers, catalog-driven inputs,
  team-scoped logic. No competitor does this natively. **This is the target.**
- **Top — autonomous conditional workflow (10):** the platform acts on its own, bounded by a
  guardrail condition, using catalog/request context.

### Agentic quality bar (what makes it count)
- Must perform an autonomous, conditional ACTION using catalog/request context.
- A read-only NL query, a catalog chatbot, or an "assistant that recommends" does NOT
  qualify — that is a feature or assistance, not an agentic story.
- The guardrail is the condition that bounds autonomy (dev vs prod, owner-only, approval on
  risky) — not "the agent only advises".
- Must hit 2+ pillars: autonomous workflow · guardrails/policy · skills/agent catalog · MCP.
- A [Stretch] must still be a real workflow that acts, never a chatbot.

### POC-fit check (does this use case actually work as a live POC?)
A story can be well-formed, differentiated, AND agentic and still be a bad POC. This is a
different question from story quality. Score these five as PASS / FAIL — all five must PASS.

1. **Buildable in the timebox** — realizable in ~50 min of live building, mostly from the
   native / OOTB blueprints and relations of the integrations named in discovery, with
   minimal net-new modeling and standard Port actions/automations. FAIL if it needs heavy
   custom modeling, data the chosen stack can't provide, or more than the session allows.
   *This is the dimension most likely to fail — judge it against the discovery tech stack.*
2. **One clear demo moment** — a single visible "wow" to narrate, ideally the agentic
   workflow firing (dev path auto-runs while prod escalates). FAIL if the payoff is invisible
   or is just "look, a catalog".
3. **Feature-real** — every part maps to a Port capability that exists today. FAIL if it
   leans on an aspirational or non-existent feature.
4. **Tied to the named before-cost** — the "after" measurably attacks the specific cost the
   user gave in discovery, not a generic benefit. FAIL if the value is unanchored.
5. **Right scope** — exactly one story and one flow, buildable and demoable on its own. FAIL
   if it's really a platform rebuild or three stories in a trench coat.

Handling failures:
- **Fixable** (scope, anchoring, weak demo moment) → fix the story and re-check silently. No
  note to the user.
- **Structural** (fails #1 or #3 and cannot be fixed) → keep the best story you can, add the
  POC-fit note to the output, name the risk plainly, and offer the nearest use case that
  WOULD land in the timebox. Honesty is the point: a demo that can't be built is worse than a
  smaller one that can.
