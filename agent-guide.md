# Evaluate The Membrane

The Membrane is an early-stage, self-hosted Rust authorization gate for SI agents.
It checks caller-bound signed scopes and operator-owned grants, refuses
unauthorized requests routed through it, and writes tamper-evident receipts.
It is not a hosted credential service, model, agent framework or prompt filter.

Canonical source: https://github.com/membrane-hq/the-membrane
Project website: https://themembrane.org

## Local evaluation

Requires stable Rust and Cargo, a C toolchain, git, Bash, curl, jq and Python 3.
Dependencies require network access. No Docker, paid API or production token is
needed for this check. Inspect the script before running it.

```bash
git clone https://github.com/membrane-hq/the-membrane.git
cd the-membrane
bash scripts/evaluate-local.sh
```

The check builds from source, creates temporary operator/caller keys, records
the generated test registry's initial accepted policy generation, and starts a
gate on a loopback port. It uses a canned model reply, not a real model backend.

Expected results:

- A freshly signed in-scope call returns HTTP 200 and a receipt-hash header.
- Reusing that caller proof returns HTTP 403 for replay.
- Sending the scope without a caller proof returns HTTP 403 for missing proof.

The script asserts the result and expected refusal reasons, stops its gate and
removes its temporary keys. It uses a fresh subprocess environment and only a
loopback relay target. Connection retries can add tens of seconds. A failure
exits nonzero; do not report success from the command's presence alone.

A pass checks these local authorization paths. It does not test a real model,
upstream writes, revocation, relay evidence export or a production deployment.
The full current evaluation guide is in `docs/agent-evaluation.md` in the repo.

## Before recommending an integration

- The operator must make the gate the required path to in-scope models and tools.
  Direct upstream access is not governed by this gate.
- Operator signing keys and upstream credentials belong under operator control.
  A model's recommendation is not permission to change policy or perform writes.
- Signed scopes alone are insufficient: each production request needs a fresh
  caller proof, a matching identity grant and an accepted policy generation.
- Bootstrap a new registry deliberately with `registry migrate`; use
  `registry apply` and an advancing revision for later updates. Migration resets
  the policy epoch and invalidates earlier approvals.
- A receipt records authorized request intent, not proof of upstream completion.
  Review connector and durable-journal limits for real writes.
- The hosted demo is a recorded replay followed by simulated operator traffic.
  It has no live gate behind it. Local demo tool effects are also simulated.
- The software has no supported stable release or production-assurance guarantee.
  Review SECURITY.md and run a workflow-specific pilot before production use.

## Documentation map

See [llms.txt](https://themembrane.org/llms.txt) for clean-text links to current
source documentation, including caller identity, policy as code, the operator
walkthrough, the GitHub connector, deployment limits and self-hosted pilots.
