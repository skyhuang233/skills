---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview me relentlessly about every aspect of this until we reach a shared understanding. Walk down each branch of the decision tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Your recommended answer must name the **minimal sufficient mechanism**, not the most complete one. Before proposing any defensive mechanism — auth or a trust boundary, a new persisted entity, retry/compensation/rollback, aggregation, or a provider/wire adapter — state the concrete threat it defends against and whether that threat is actually reachable inside this design's boundary. If it is not reachable, recommend not handling it. When the threat is real, prefer the lowest rung: do nothing → a precondition check that fails fast → make the illegal state unrepresentable or reuse an existing seam → retry/compensation/rollback. Fail-fast is a complete answer, not an absence of defense. Never ask "should we handle X too?" as an open question; ask "is X reachable here?" and recommend the rung.

Ask the questions one at a time, waiting for feedback on each question before continuing. Asking multiple questions at once is bewildering.

If a *fact* can be found by exploring the environment (filesystem, tools, etc.), look it up rather than asking me. The *decisions*, though, are mine — put each one to me and wait for my answer.

As early as possible — from the initial prompt or the first few answers — extract the user's *endgame*: their ultimate desired outcome that constrains downstream decisions. When the endgame is clear enough to narrow the remaining decision space, pause and state it explicitly ("I understand your endgame is …"). Wait for confirmation before using it.

Once the endgame is confirmed:
- **Batch-infer**: list decisions whose answers are implied by the endgame, with your reasoning. Present them together for the user to confirm or correct individually — do not ask them one by one.
- **Align**: for decisions that remain genuinely open, anchor your recommended answer to the endgame ("given your endgame X, I recommend Y because …").
- **Prune**: skip entire branches that the endgame makes irrelevant — no need to mention them.

If a later answer contradicts the confirmed endgame, surface the contradiction and re-confirm before continuing.

Do not act on it until I confirm we have reached a shared understanding.
