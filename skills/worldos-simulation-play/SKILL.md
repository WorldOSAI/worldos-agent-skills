---
name: worldos-simulation-play
description: Create and advance real owner-scoped WorldOS Simulation saves through the WorldOS MCP, inspect bounded player-visible results, and run controlled regression scenarios. Use when a user explicitly wants an agent to start a formal save, play one or more real turns, test the normal player, billing, counter, and statistics path, or verify a fix in a fresh owned save; do not use for isolated creator playtests or read-only review.
---

# WorldOS Simulation Play

Run a bounded scenario through the same persistent save and turn path used by a player. Real play is not a disposable authoring preview: it occupies a save slot, may spend Zaps, contributes to normal counters and accrued statistics, and remains in the authorized user's account.

## Confirm the live contract and consent

1. Call `get_authoring_guide` and confirm that `create_owned_save` and `play_owned_save_turn` are available.
2. Treat the live guide and current tool schemas as authoritative. If save-play access is unavailable, ask the user to reconnect WorldOS MCP and approve the updated permission screen. Never request an access token in chat.
3. Use only the authorized account, Simulations available to it, and saves it owns. Do not impersonate an email address or use another account's save.
4. Confirm that the user requested real play and understands that turns can spend Zaps and affect normal counters and statistics. Authorization to review, author, or run an isolated playtest does not authorize a real turn.

Use `worldos-authoring` and its temporary playtest tools for isolated creator testing that should be free, non-counting, and disposable. Use `worldos-simulation-review` when the request is read-only.

## Define a bounded run

Before creating or advancing a save, identify:

- the world ID, slug, or URL;
- the intended locale and any material setup or player-character choices;
- the behavior under test and its player-visible expected result;
- the maximum number of real turns authorized for this run.

Prefer a fresh, clearly named save for regression work. Do not experiment on a valuable existing save unless the user explicitly designates it. Use deterministic player actions and the fewest turns that can establish the result; model prose may vary even when persistence is correct.

## Create the real save

1. Generate a stable idempotency key of 8–160 characters for the exact world, setup, character choices, locale, and save name.
2. Call `create_owned_save`. Reuse the key only when retrying identical inputs; change it when any material input changes.
3. Let documented defaults fill omitted setup fields. If the live tool reports a required choice with no default, ask the user rather than guessing a consequential role or character.
4. Record the returned save ID, `simUrl`, `updatedAt`, turn count, resolved setup values, and player-visible opening.

Creation is complete only when the tool returns the owned save. A formal save cannot be deleted through this MCP workflow; tell the user it remains in their account and can be managed in the product.

## Play one turn at a time

1. Select an interaction accepted by the live schema: main input, chat, map action, or a public `player_action` exposed by another surface. Use natural player interactions, never engine operations or state paths.
2. Call `play_owned_save_turn` with the save's exact latest `updatedAt`.
3. Inspect the returned narrative and detailed player-visible surface changes. Carry the newly returned `updatedAt` into the next turn.
4. Stop as soon as the expected behavior is established, a defect is reproduced, the authorized turn limit is reached, or the next action would require a material user choice.

Never run real turns concurrently. A turn is non-idempotent: if the transport outcome is uncertain, call `get_owned_save` and `get_owned_save_turns` before considering another call. Do not blindly retry an action that may already have been saved.

If the tool reports that the save changed, fetch the latest save, compare its turn count and visible state with the test plan, then either continue from the new exact version or stop for user direction. Never bypass optimistic concurrency or overwrite another session's progress.

## Verify the result

Use player-visible evidence from the turn result, then re-fetch the save and relevant turn history to confirm persistence. For regression work, compare the observed surfaces and turn count with the expectation defined before play.

Distinguish these outcomes:

- **Pass:** the expected visible change appears and persists after re-fetch.
- **Reproduced defect:** the action completed, but the expected visible change is absent, stale, or inconsistent on a later fetch.
- **Inconclusive:** model behavior did not exercise the intended mechanic, a required permission or balance blocked the turn, or bounded responses do not expose enough evidence.

Do not infer a platform defect from prose variation alone. When a model-dependent action misses the mechanic, use one controlled rephrasing only if it remains within the authorized turn budget. Do not expose raw operations, hidden state paths, prompts, provider details, or other system internals.

## Report and hand off

Report:

- the Simulation and real save name, ID, and URL;
- setup and player-visible actions used;
- real turns consumed versus the authorized maximum;
- the expected and observed player-visible result;
- whether the result persisted after re-fetch;
- pass, reproduced defect, or inconclusive status and any limitations;
- that the save remains in the user's account and may have affected Zaps, counters, and statistics.

Do not claim an exact charge unless the live tool returns it. Do not modify the world while diagnosing a save unless the user separately authorizes an authoring change.

## Hard boundaries

- Do not create or play a real save without explicit real-play authorization.
- Do not exceed the agreed turn limit or continue after sufficient evidence exists.
- Do not mutate, repair, rewind, rename, transfer, or delete a real save through private APIs, a database, or repository internals.
- Do not treat an isolated authoring playtest as proof that billing, counters, accrued statistics, or formal-save persistence work.
- Do not bypass an MCP refusal through another account or data source.
