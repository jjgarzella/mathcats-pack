# mathcats

`mathcats` is a Gas City pack for autonomous work whose bottleneck is
mathematical semantic reasoning rather than mechanical implementation. Reach
for a mathcat when the central task is to preserve and reason about exact
mathematical content: statements, quantifiers, hypotheses, definitions,
notation, semantic choices, evidence, and open proof obligations.

Use a polecat for ordinary implementation work. Use a mathcat when the result
needs an honest mathematical verdict, a proof or disproof, a checked argument,
a counterexample, a formalization attempt, a translation of mathematical
content, a proof-dependency artifact, or a structured unresolved report.

## Layout

```
pack.toml
README.md
agents/mathcat/
  agent.toml
  prompt.template.md
formulas/
  mol-mathcat-work.toml
assets/scripts/
  worktree-setup.sh
```

## Wiring

Import the pack from the city composition or from any rig that should expose
mathcats:

```toml
[imports.mathcats]
source = "packs/mathcats"
```

For a rig-scoped import, use the same form under the rig:

```toml
[[rigs]]
name = "<rig>"
[rigs.imports.mathcats]
source = "packs/mathcats"
```

The mathcat agent declares `provider = "codex"` and explicitly sets both
`model = "gpt-5.5"` and `effort = "xhigh"`. In a city without a
`[providers.codex]` block, Gas City falls back to the built-in Codex provider;
because the agent declares both options, the pack does not need city-local
model or effort patches.

Do not override the mathcat model or effort with `[[patches.agent]]`. The pack
is the source of truth for those settings.

## Slinging

The mathcat's default sling formula is `mol-mathcat-work`, so a bare sling to a
mathcat target uses the mathcat lifecycle automatically:

```bash
gc sling <target-rig>/mathcats.mathcat <bead-id>
```

Use an explicit formula when you need to be precise during testing:

```bash
gc sling <target-rig>/mathcats.mathcat <bead-id> --on mol-mathcat-work
```

## Lifecycle

`mol-mathcat-work` is assignment-agnostic. It runs the seven-step spine:

1. `load-assignment`
2. `establish-contract`
3. `prepare-environment`
4. `execute-assignment`
5. `audit-deliverable`
6. `publish-result`
7. `drain`

The formula deliberately names no fixed mathematical role. Future assignment
formulas may extend this spine and override only `execute-assignment`, while
supplying an assignment prompt fragment and deliverable contract by formula
vars.

## Output Contract

Every mathcat deliverable carries the same eight metadata fields:

- `assignment`
- `result_status`
- `epistemic_status`
- `assumptions`
- `dependencies`
- `checks_performed`
- `findings`
- `open_obligations`

The deliverable specification can vary by assignment, but these fields are
invariant. Use explicit empty or unknown values when a field has no content;
do not drop the field.

## Outcomes

`gc.outcome` is a process outcome, not a mathematical verdict. A mathcat may
complete successfully while rejecting a proof, finding a counterexample, or
recording an unresolved result. Record the mathematical verdict in mathcat
metadata and in the durable artifact; reserve workflow failure for failures of
the lifecycle itself.

## Future Assignments

V1 ships no assignment formulas. Follow-up packs can add assignment formulas
that extend `mol-mathcat-work`, set assignment-specific prompt/output vars,
and override `execute-assignment` only. They must keep the seven-step spine
and the eight-field metadata schema intact.
