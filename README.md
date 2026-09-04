# AI Coding Sessions — Akshay Anil

Two unedited transcripts of agentic coding sessions, submitted for the **Founding AI Engineer**
role at 5U AI (Munich).

> *"Send your CV and the one thing we care about most: an AI coding session."*

Both are real work on real projects. Session 1 is the longer and more representative of the two.

| | Session | Tool | Size | Subject |
|---|---|---|---|---|
| **1** | [**Anomaly Detection & GPU Infrastructure**](./Session_1_Anomaly_Detection_and_GPU_Infrastructure.md) | Claude Code | 110 prompts, 8 days | Standing up an SDK on rented GPU pods, then an eight-arm benchmark sweep on an industrial anomaly-detection dataset |
| 2 | [Adversarial Audit & Formal Proofs](./Session_2_Adversarial_Audit_and_Formal_Proofs.md) | Antigravity | 15 prompts | Auditing a passing test suite in a Unity project, and rebuilding its assertions around formal invariants |

See also: [what else I'm working on](./PROJECTS.md).

---

# Session 1 — Anomaly detection and GPU infrastructure

**[→ Read it](./Session_1_Anomaly_Detection_and_GPU_Infrastructure.md)** · 110 prompts over eight
days · Python, Docker, CUDA, PyTorch, NVIDIA DeepStream · produced
[defect-anomaly](https://github.com/akshay131996/defect-anomaly) and
[deepstream-projects](https://github.com/akshay131996/deepstream-projects)

Started as an SDK setup and became an eight-arm benchmark sweep across an industrial
anomaly-detection dataset. Roughly the first sixty prompts are infrastructure work: driver and CUDA
version conflicts, container-in-container constraints, network volumes, disk exhaustion, and GPU
pods dying mid-run and being rebuilt. The remainder is experiment design and evaluation — held-out
calibration splits, seed audits, corrected totals, and several rounds of results being revised.

### What the AI thinks I should highlight

*I asked an AI to read this transcript and pick the exchanges it found most revealing. These are its
picks and its summaries, not mine. Read the file and draw your own.*

**Prompt #28**
> *"you sure right, last time we tried this we ran into the conflict that runpod runs vm on docker
> so we cant run another docker or something like that, you sure it wont block us this time"*

A setup that had failed once before, queried before repeating it. The reply distinguishes the two
cases: the earlier attempt nested a container inside a running pod, this one changed which image
the pod itself came from.

**Prompt #76**
> *"why reseed?"*

Asked after a correction moved one arm's advantage from 32% to 9%. The reply argues the table's
honest state was *"B1 wins by an unknown amount, possibly none,"* and that re-seeding all eleven
categories was the wrong scope, since four accounted for 95% of the effect.

**Prompt #99**
> *"why did you flag the change in gpu when we changed it?"*

A warning given days earlier, revisited. The reply separates three reasons by strength and concedes
that the one it led with was never demonstrated.

**Prompt #67**
> *"why arent we going beyond resnet in terms of architecture? wouldnt the latest models help in
> getting more accurate patterns"*

Why a 2015 backbone remains the baseline in a 2026 paper, and what modern training recipes discard
that this task depends on.

### How it ends

With a job lost to a dead GPU pod. The closing message states which results survived, which need
re-running, gives an unproven hypothesis as a hypothesis rather than a finding, and names the
assistant's own mistake that killed two of three runs.

---

# Session 2 — Adversarial audit and formal proofs

**[→ Read it](./Session_2_Adversarial_Audit_and_Formal_Proofs.md)** · 15 prompts · Unity, C#,
Python, custom Model Context Protocol server

A project whose automated playtest reported 100% green while the build was visibly broken. The
session traces that to the harness rather than the code, then re-derives the suite's assertions from
invariants that can be checked mechanically.

### What the AI thinks I should highlight

*Same caveat as above — these are an AI's picks from reading the transcript.*

**Prompt #8**
> *"the playtest failed. i just played it. […] the playtest shouldnt teleport the chef, it should
> manually command to go in all four directions and then see if it can access all the interactable
> items. Please dont assume any facts that you dont know. always verify with the ground truth you
> can extract from the game scene data you have access to."*

The test teleported the character between checkpoints instead of walking it, so it validated a world
that could not actually be traversed. Underneath sat a scene file silently overriding the code's
dimensions with stale serialized values, and a tiling loop stepping by double its tile size that
left three quarters of the floor bare.

**Prompt #11**
> *"i seriously doubt the 100% Pass rate of all tests, create an subagent that argues that your game
> logic is wrong mathematically false, it will only say you are wrong if it can mathematically prove
> to you that you are wrong. […] but make sure not to break your current version of the game, so
> keep a back up incase we want to revisit this version"*

A second agent tasked only with breaking the invariants, and only allowed to claim a break it could
prove. It found six: a delivery exploit where uncooked input satisfied a completed-order check, a
race condition in a 1.5-second window that duplicated state on rapid input, and two resource leaks
that drained a finite pool until the system deadlocked with no error.

---

## Mistakes and wrong turns

Both files are long and deliberately unedited. The wrong turns are all still in them — setups that
failed and had to be rebuilt, results that were reported and then corrected, prompts where I misread
a screen or a settings page, a dataset downloaded twice by accident, and a benchmark run killed by a
timeout that should not have been set. Session 2 opens with several prompts spent on a compiler
error that turned out to be a single misconfigured assembly reference.

A transcript with only the good parts left in would not answer the question you actually asked.

**What was filtered from Session 1**, also stated at the top of that file: system-generated
background-task notifications dropped, tool output bodies dropped (the raw logs run to hundreds of
megabytes), and GPU pod hostnames and IP addresses redacted. No prompt or reply was edited.

**Session 2** is exported as-is from the Antigravity session log.

Both files were scanned against credential patterns covering access tokens, API keys, private keys,
bearer tokens and personal data. Session 2 contained none. Session 1's only matches were the pod
hostnames and addresses noted above.

---

## Environment

| | |
|---|---|
| **Agents** | Claude Code, Antigravity |
| **Languages** | Python, C#, Bash, SQL |
| **Infrastructure** | Docker, remote GPU pods, SSH tunnelling, NVIDIA Container Toolkit, custom MCP server |
| **ML / CV** | PyTorch, PatchCore, DINOv2/v3, WideResNet, TensorRT, NVIDIA DeepStream, MVTec AD & AD 2 |

---

## How I think about AI

Pre AI, the role of an engineer was to build the car from the components you got. I now think of AI
as the car and the driver, that just needs to be fed a destination, and you make sure it takes the
right exits from the freeway.

I was initially hesitant to give the keys to the coding sessions to the AI, because my gut feeling
after being a software developer for 6 years was to understand and trace each line of code that gets
hit in the execution, and then prepare an abstract high level view of the flow. That allowed me to
trust the working of the code, and to identify bugs and crashes when they happened.

But now it's a completely different story. It understands packages and dependencies like I never
did, and is able to write code in ways I could never think of, because of the huge knowledge base
this thing is trained on, compared to my puny little brain which needs to tackle the real life
problems like searching for jobs and earning money to keep myself fed, and play ranked games to
release the pent up stress when things don't go your way.

Now I understand why people are pulling out their pitchforks when they see AI generated content, but
in the long term this thing is exactly what a person who writes code would want. It just frees up so
much time and allows you to experiment with all the ideas and side projects you wanted to do. I made
3D animations and a couple of games, and all it needed was my theme and taste to build it, instead
of learning C# and Blender. (I do have a solid base in C, which was the first programming language I
learnt, and I understand the concepts and can trace code if needed.)

Anyway, if you've read until here, maybe I might get an interview with your company.

Cheers, and thanks for reading through not AI slop but human rant.

---

**Akshay Anil** · Berlin, Germany · [github.com/akshay131996](https://github.com/akshay131996) · [akshay131996.github.io](https://akshay131996.github.io/)
