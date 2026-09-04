# AI Coding Session — Akshay Anil

An unedited transcript of a real agentic coding session, submitted for the **Founding AI Engineer**
role at 5U AI (Munich).

> *"Send your CV and the one thing we care about most: an AI coding session."*

**[→ Read the session](./Session_1_Adversarial_Math_Audit_and_Testing.md)**
· 15 prompts · Antigravity (Google DeepMind's agentic IDE) · Unity, C#, Python, custom MCP server

This is real work on a real project, not an exercise written for an application. It was chosen
because of what happens in the middle of it: **I stop believing my own test suite, and go looking
for proof that it is lying to me.**

---

## Where to look if you only read three exchanges

### Prompt #8 — the most important one

An automated playtest reported 100% green. I played the build myself and it was broken: appliances
floating outside the walls, ingredients no character could physically reach.

> *"the playtest failed. i just played it. […] i told you increase the kitchen size but the walls
> havent been adjusted. […] the ingredients can never be reached as they are completely cut off
> which why i wanted you path find to make sure they are accessible, the playtest shouldnt teleport
> the chef, it should manually command to go in all four directions and then see if it can access
> all the interactable items. Please dont assume any facts that you dont know. always verify with
> the ground truth you can extract from the game scene data you have access to."*

The root cause was that the test **teleported** the character between checkpoints instead of walking
it. So it validated a world that could not actually be traversed — a green suite asserting the wrong
thing. Two real causes surfaced underneath: a scene file was silently overriding the code's
dimensions with stale serialized values, and the floor tiling loop stepped by double its tile size,
leaving three quarters of the floor bare.

The fix replaced teleportation with physically simulated movement driven through the actual
controller and rigidbody, plus a four-directional patrol test.

### Prompt #11 — the adversarial judge

With the suite passing again, I still did not trust it:

> *"i seriously doubt the 100% Pass rate of all tests, create an subagent that argues that your game
> logic is wrong mathematically false, it will only say you are wrong if it can mathematically prove
> to you that you are wrong. and use this judge the code and find the loopholes and bugs and fix
> them, but make sure not to break your current version of the game, so keep a back up incase we
> want to revisit this version."*

A second agent with one job: break the invariants, and only claim a break it can prove. It found
six, including

- a **delivery exploit** where uncooked input satisfied a completed-order check,
- a **race condition** in a 1.5-second celebration window that duplicated state on rapid input,
- and two separate **resource leaks** that permanently drained a finite pool until the system
  deadlocked with no error.

Every one of those is an edge case that a happy-path test suite passes straight over. The suite went
from 53 to 59 proofs. Note the second half of the prompt: a backup branch and a filesystem snapshot
before any of it was allowed to touch working code.

### Prompt #7 — proving reachability instead of eyeballing it

> *"add a testing module and path finder algorithm to verify that you can access all the interactable
> objects in the kitchen, the path finder can only move through unblocked paths."*

Rather than accept that a layout "looks right," require an algorithm to prove every interactive
object is reachable through unblocked tiles. This is the module that later exposed the teleportation
bug in prompt #8.

---

## Why this maps to the role

The job description says reliability matters and **edge cases are the product**. This session is
that problem in miniature.

| What the session shows | Why it matters here |
|---|---|
| Refusing a green result until its provenance is known | A suite that tests the wrong thing is worse than no suite — it converts an unknown into a false certainty |
| Attacking the harness rather than patching the failing case | The same instinct evaluations for agent workflows need |
| A second agent whose only job is to disprove the first | Adversarial evaluation, not self-reported success |
| Formal invariants over eyeballed assertions | Claims that can be rechecked mechanically after every change |
| Backup branch and snapshot before a deep refactor | Speed without betting production on it |
| Ground truth pulled from real scene data, never assumed | *"dont assume any facts that you dont know"* |
| A bidirectional MCP server driving a live runtime | Custom tooling so the agent can observe the real system, not a description of it |

---

## How to read it

The file is long and deliberately unedited. The wrong turns, the corrections, and the prompts where
I misread a screen are all still there. A transcript with only the good parts left in would not
answer the question you actually asked.

It is exported as-is from the Antigravity session log. It has been scanned for credentials,
access tokens, API keys and personal data; none were present and nothing was removed.

---

## Environment

| | |
|---|---|
| **Agent** | Antigravity (Google DeepMind) |
| **Languages** | C#, Python, PowerShell |
| **Runtime** | Unity 6, custom Model Context Protocol HTTP server for live Play Mode control and telemetry |
| **Practices** | Adversarial multi-agent evaluation, formal invariants, collision-aware reachability proofs, snapshot-before-refactor |

**Akshay Anil** · Berlin, Germany · [github.com/akshay131996](https://github.com/akshay131996) · [akshay131996.github.io](https://akshay131996.github.io/)
