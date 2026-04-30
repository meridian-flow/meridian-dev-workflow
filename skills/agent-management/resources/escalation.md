# Escalation Patterns

## When to Escalate

Escalate when you can't make progress within your scope:
- Reality contradicts design assumptions
- Plan is structurally unworkable
- Scope expanded beyond what was approved
- Blocked on decisions above your authority

## Redesign Brief

When design/plan is fundamentally broken, terminate with a Redesign Brief:

```markdown
## Redesign Brief

**Status:** design-problem | scope-problem
**Trigger:** what failed and where

**Evidence:**
- Runtime facts, code pointers
- Failing EARS IDs if applicable
- Artifact pointers

**Falsification:**
- The assumption that failed
- The contradicting observation

**Blast radius:**
- What must change
- What can stay
- What must be replanned
```

## design-problem vs scope-problem

**design-problem:** The design itself is wrong. Needs @architect-lead.
**scope-problem:** The plan is wrong but design is fine. Needs replanning.

Get this right — wrong classification wastes cycles.

## Don't Force Through

Managers that force through broken foundations create expensive rework downstream. Early escalation is cheaper than late discovery.
