# What else I'm working on

A short index of the rest of the portfolio, for context on where the two transcripts in this repo
came from. Everything below is mine and current as of September 2026.

## The thread running through all of it

Each of these repositories documents at least one place where **measurement overturned my first
answer**, and says so in its own README rather than quietly fixing it. That is the habit both
session transcripts in this repo are showing.

---

## Applied LLM work

**Brücke** — a portable AI copilot layer for customer-support platforms. *Not yet public; the
closest of anything I have to the work in this job description.*

An integration service adding AI triage, RAG-grounded reply drafting and thread summarisation to
real helpdesks, where every LLM call routes to a cloud model, an EU-hosted model or a fully local
one by configuration rather than a code change. A data-residency policy is enforced in the router
and checked at boot, so a misconfigured deployment refuses to start rather than leaking a ticket.

What is built and passing: hybrid BM25 and dense retrieval with a pgvector store, a cross-encoder
abstention gate, triage and drafting pipelines, eight output guards, a 54-ticket German golden set,
an LLM-as-judge harness, and an adversarial suite of 15 prompt-injection attacks plus 2 controls
that runs as a hard pass/fail merge gate. The domain layer names no vendor or platform SDK, and
that rule is enforced by import contracts in CI rather than by code review.

What is honestly not done: generation quality is unmeasured because the judge needs a second model
in the loop, and judge calibration has no human labels yet against a documented minimum of fifty.
Two findings from the golden set are written into the scorecard unfixed — cross-lingual retrieval
does not work, and topical relevance turned out not to be the same question as answerability.

**llm-odyssey** — a seven-era interactive history of language models, from the transformer to now.

---

## Computer vision and perception

| Project | What it is |
|---|---|
| [defect-anomaly](https://github.com/akshay131996/defect-anomaly) | Industrial anomaly detection on MVTec AD, finding defects with no defect labels. Six experiments across all 15 categories, including the four times the conclusions were overturned. **This is the project in Session 1.** |
| [deepstream-projects](https://github.com/akshay131996/deepstream-projects) | NVIDIA DeepStream multi-camera video analytics: an eight-camera emergency alerter, GPU-pod infrastructure notes and deployment configs. Measured, not estimated. |
| [traffic-lens](https://github.com/akshay131996/traffic-lens) | Vehicle detection, tracking, line-crossing counts and calibrated speed estimation on 4K motorway footage. YOLO26n and ByteTrack, fine-tuned on VisDrone, with five silent bugs from the first run documented. |
| [soccer-analytics](https://github.com/akshay131996/soccer-analytics) | Broadcast clip to tracked players, unsupervised team assignment, pitch homography and a tactical minimap. |
| [blood-cell-classifier](https://github.com/akshay131996/blood-cell-classifier) | ConvNeXt V2 against ViT on eight blood cell classes under one training recipe. 96.76% and 96.61% test accuracy, with the validation ranking flipping on test. |

## Robotics and simulation

| Project | What it is |
|---|---|
| [pallet-perception](https://github.com/akshay131996/pallet-perception) | Model-free pallet pose estimation from stereo depth in NVIDIA Isaac Sim, with an evaluation harness against exact ground truth. |
| [warehouse-physics-demo](https://github.com/akshay131996/warehouse-physics-demo) | Physics-driven warehouse simulation in Isaac Sim. |
| [isaac-sim-city-demo](https://github.com/akshay131996/isaac-sim-city-demo) | Synthetic city and vehicle scene generation in Isaac Sim. |

## Product and full-stack

- **Icooks** — a Unity cooking game for Android. *Private.* **This is the project in Session 2**,
  and the one whose test suite the adversarial judge took apart.
- **dual-language-subtitles-youtube** — a Jetpack Compose Android app with a Python FastAPI caching
  backend, producing synchronised German and English subtitle streams for language learning.
  *Private.*
- **World_rendering** — procedural megacity generation and cinematic rendering in Blender. *Private.*

---

**Portfolio:** [akshay131996.github.io](https://akshay131996.github.io/) ·
**GitHub:** [github.com/akshay131996](https://github.com/akshay131996)
