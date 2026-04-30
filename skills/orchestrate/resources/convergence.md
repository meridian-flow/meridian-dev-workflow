# Convergence Patterns

## When to Stop

A loop converges when no new substantive findings emerge. Not "N iterations" — actual convergence.

Substantive = would change behavior, catch a bug, or block ship.
Non-substantive = style, minor suggestions, things already deferred.

## Review Loops

```
spawn reviewers → collect findings → route to fix → re-review affected areas
```

Stop when:
- No blocking findings remain
- New findings are all non-substantive
- Remaining items explicitly deferred with reasoning

## Fix Cycles

```
finding → scoped fix → verify fix → continue or re-review
```

Route findings to the smallest scope that can fix them. Don't re-run the full loop on every iteration — re-run affected lanes only.

## Explicit Deferral

Deferral is a valid exit. When something can't be resolved in this scope:
1. Log the deferral with reasoning
2. Note what would trigger revisiting
3. Continue — don't block on it

## Loop Guards

If a loop isn't converging after reasonable iterations:
- Check if findings are actually being addressed
- Check if new work is creating new findings faster than fixes
- Consider whether scope is too large
- Escalate to caller if stuck
