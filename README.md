# AI Coding Sessions — Akshay Anil

Two unedited transcripts of real agentic coding sessions, submitted for the **Founding AI Engineer**
role at 5U AI (Munich).

> *"Send your CV and the one thing we care about most: an AI coding session."*

Both are real work on real projects, not exercises written for an application. **Start with Session
1.** Session 2 is here because it shows the same habit from a different angle: I do not accept a
green result until I know what produced it.

| | Session | Tool | Size | What it shows |
|---|---|---|---|---|
| **1** | [**Anomaly Detection & GPU Infrastructure**](./Session_1_Anomaly_Detection_and_GPU_Infrastructure.md) | Claude Code | 110 prompts, 8 days | Production debugging on remote GPU infrastructure, then an eight-arm experiment sweep where measurement repeatedly overturned the first answer |
| 2 | [Adversarial Audit & Formal Proofs](./Session_2_Adversarial_Audit_and_Formal_Proofs.md) | Antigravity | 15 prompts | Refusing a 100% pass rate, spawning an adversarial judge agent to break my own test suite, and grounding the fixes in formal invariants |

See also: [**what else I'm working on**](./PROJECTS.md).

---

# Session 1 — Anomaly detection and GPU infrastructure

**[→ Read it](./Session_1_Anomaly_Detection_and_GPU_Infrastructure.md)** · 110 prompts over eight
days · Python, Docker, CUDA, PyTorch, NVIDIA DeepStream · produced
[defect-anomaly](https://github.com/akshay131996/defect-anomaly) and
[deepstream-projects](https://github.com/akshay131996/deepstream-projects)

Started as an SDK setup and became an eight-arm benchmark sweep across an industrial
anomaly-detection dataset. The first sixty prompts are pure infrastructure incident work: driver and
CUDA version conflicts, container-in-container constraints, network volumes, disk exhaustion, and
GPU pods dying mid-run and being rebuilt. The last fifty are evaluation discipline.

### Where to look if you only read four exchanges

**Prompt #28 — refusing to repeat a known failure.**

> *"you sure right, last time we tried this we ran into the conflict that runpod runs vm on docker
> so we cant run another docker or something like that, you sure it wont block us this time"*

I will not re-run a setup that failed before until the difference is explained structurally, not
reassuringly. The distinction turned out to be real: the earlier attempt nested a container inside a
running pod, this one changed which image the pod itself came from. Same-sounding operation,
different privilege path.

**Prompt #76 — making the model justify spend before it spends.**

> *"why reseed?"*

A correction had just moved one arm's advantage from 32% to 9%. Nine percent is close enough to zero
that the remaining runs could erase it or restore it, so the honest state of the table was *"B1 wins
by an unknown amount, possibly none."* Either result is publishable. Not knowing which is true is
not. The answer also pushed back on my framing: re-seeding all eleven categories was the wrong job,
because four of them accounted for 95% of the effect.

**Prompt #99 — auditing a warning I was given days earlier.**

> *"why did you flag the change in gpu when we changed it?"*

The reply separates three reasons by strength and concedes that the one it led with was never
actually demonstrated. Getting a model to retract its own weak reasoning is worth more than getting
it to agree with me.

**Prompt #67 — pushing past a default.**

> *"why arent we going beyond resnet in terms of architecture? wouldnt the latest models help in
> getting more accurate patterns"*

Why a 2015 backbone is still the baseline in a 2026 paper, and what modern training recipes throw
away that this particular task depends on.

### How it ends

With a job lost to a dead GPU pod. The final message states which results survived, which must be
re-run, gives an unproven hypothesis as a hypothesis rather than a finding, and names the
assistant's own mistake that killed two of three runs. That is the reporting standard I want in a
system where **edge cases are the product**.

---

# Session 2 — Adversarial audit and formal proofs

**[→ Read it](./Session_2_Adversarial_Audit_and_Formal_Proofs.md)** · 15 prompts · Unity, C#,
Python, custom Model Context Protocol server

A project with a passing test suite. I did not believe the suite.

**Prompt #8 — the most important prompt in either session.** An automated playtest reported 100%
green. I played the build and it was broken: appliances outside the walls, ingredients no character
could reach.

> *"the playtest failed. i just played it. […] the playtest shouldnt teleport the chef, it should
> manually command to go in all four directions and then see if it can access all the interactable
> items. Please dont assume any facts that you dont know. always verify with the ground truth you
> can extract from the game scene data you have access to."*

The root cause: the test **teleported** the character between checkpoints instead of walking it, so
it validated a world that could not actually be traversed. Underneath sat a scene file silently
overriding the code's dimensions with stale serialized values, and a tiling loop stepping by double
its tile size that left three quarters of the floor bare.

**Prompt #11 — the adversarial judge.**

> *"i seriously doubt the 100% Pass rate of all tests, create an subagent that argues that your game
> logic is wrong mathematically false, it will only say you are wrong if it can mathematically prove
> to you that you are wrong. […] but make sure not to break your current version of the game, so
> keep a back up incase we want to revisit this version"*

A second agent whose only job was to break the invariants, and only claim a break it could prove. It
found six: a delivery exploit where uncooked input satisfied a completed-order check, a race
condition in a 1.5-second window that duplicated state on rapid input, and two resource leaks that
permanently drained a finite pool until the system deadlocked with no error. Every one is an edge
case a happy-path suite passes straight over. Note the second half of the prompt: a backup branch
and a filesystem snapshot before any of it touched working code.

---

## How to read these

Both files are long and deliberately unedited. The wrong turns, the corrections and the prompts
where I misread a screen are all still in them. A transcript with only the good parts left in would
not answer the question you actually asked.

**Session 1** was mechanically filtered in three ways, all stated at the top of the file:
system-generated background-task notifications dropped, tool output bodies dropped (the raw logs run
to hundreds of megabytes), and GPU pod hostnames and IP addresses redacted. No prompt or reply was
edited.

**Session 2** is exported as-is from the Antigravity session log.

Both files were scanned against credential patterns covering access tokens, API keys, private keys,
bearer tokens and personal data. Session 2 contained none. Session 1's only matches were the pod
hostnames and addresses noted above, which are redacted.

---

## Environment

| | |
|---|---|
| **Agents** | Claude Code, Antigravity |
| **Languages** | Python, C#, Bash, SQL |
| **Infrastructure** | Docker, remote GPU pods, SSH tunnelling, NVIDIA Container Toolkit, custom MCP server |
| **ML / CV** | PyTorch, PatchCore, DINOv2/v3, WideResNet, TensorRT, NVIDIA DeepStream, MVTec AD & AD 2 |
| **Practices** | Held-out calibration splits, seed audits, adversarial evaluation, formal invariants, reachability proofs, snapshot-before-refactor |

**Akshay Anil** · Berlin, Germany · [github.com/akshay131996](https://github.com/akshay131996) · [akshay131996.github.io](https://akshay131996.github.io/)
