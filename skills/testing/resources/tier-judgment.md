# Tier Judgment

Choosing the right tier is usually the difference between a test that catches real bugs and a test that either misses them or flags noise.

## Decision Diagram

```mermaid
flowchart TD
    START[New behavior to test] --> Q1{"Pure logic?<br/>No I/O?"}
    Q1 -->|yes| UNIT["UNIT<br/>@coder + unit-test"]
    Q1 -->|no| Q2{"Components composing<br/>with fakeable boundaries?"}
    Q2 -->|yes| INT["INTEGRATION<br/>@coder + integration-test"]
    Q2 -->|no| Q3{"Real runtime behavior<br/>with real systems?"}
    Q3 -->|yes| SMOKE["SMOKE / E2E<br/>@probe"]
    Q3 -->|no| REFACTOR["Code is probably mixing<br/>decisions and I/O —<br/>consider refactoring first"]

    UNIT --- U_P["Fast (ms)<br/>Exhaustive edge cases<br/>No fakes needed"]
    INT --- I_P["Medium (100ms–1s)<br/>Fakes at external boundaries<br/>Tests composition"]
    SMOKE --- S_P["Slow (seconds+)<br/>Real processes / APIs<br/>User-visible behavior"]
```

## Tradeoffs by Tier

| Dimension | Unit | Integration | Smoke / E2E |
|---|---|---|---|
| Speed | milliseconds | 100ms – 1s | seconds, sometimes more |
| Fidelity to production | low | medium | high |
| Isolation on failure | sharp | medium | diffuse |
| Cost to write | low | medium | high |
| Cost to maintain | low if targeting behavior | medium | high |
| Answers question | "does the logic hold?" | "do pieces compose?" | "does it work for a user?" |

## Heuristics

- **Default to the lowest tier that answers the question.** Lower tiers are faster feedback and sharper failure signals.
- **Move up a tier when the question is about collaboration or integration.** Unit tests cannot answer "do these modules agree on the contract."
- **Move up a tier when mocking starts dominating.** A unit test with five mocks is usually an integration test wearing a disguise.
- **Don't duplicate coverage across tiers.** If the smoke test proves a code path works, a unit test that reasserts the same thing adds maintenance without protection.
- **Critical paths deserve redundancy.** Authentication, billing, data loss risks — test at multiple tiers because the cost of a false negative is high.

## Common Sizing Mistakes

- **Unit tests that mock everything** — should probably be integration tests, or the code should be refactored to have a testable core.
- **Integration tests that spin up real databases** — probably should be smoke tests, or the database client should be wrapped in a fake-able interface.
- **Smoke tests that verify string formatting** — should be unit tests. Smoke tests are expensive; reserve them for behavior only visible end-to-end.
