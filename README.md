# PLE Agent Harness

A reproducible starting point for the [Physical-Engineering's Last Exam (PLE)](https://www.kaggle.com/competitions/physical-engineerings-last-exam) agent-harness competition.

**Status:** Official Google `minimal` baseline imported for development. Not yet benchmarked, scored, or submitted. The current `serve.py` is upstream starter code, not our own optimized agent.

## Baseline architecture

- `serve.py`: OpenAI-compatible model loop, shell/read/write tools, retry and deadline handling.
- `taskbridge.py`: WebSocket protocol bridge to the isolated evaluator task container.
- `submission.yaml`: Google-sponsored `google-cloud/gemini-3.8-flash` model, Python 3.12 environment, and serve command.
- `requirements.txt`: pinned Python dependencies.

The initial baseline **does not support all task MCP tools**. We will add planning, tool selection, verification, and MCP capabilities only after baseline measurements.

## Quick start (local development)

Prerequisites: Git, Docker, Python, [uv](https://docs.astral.sh/uv/), and the official [PLE evaluator](https://github.com/google-research/ple).

1. Accept the gated dataset terms at [Hugging Face PLE](https://huggingface.co/datasets/google/physical-engineerings-last-exam) and create a read-only HF token. Do **not** commit any tokens.
2. Clone both repositories into sibling folders:

```bash
git clone https://github.com/google-research/ple.git
git clone https://github.com/amin076/PLE-Agent-Harness.git
cd ple
uv tool install harbor==0.20.0 --with websockets
```

3. Configure environment variables in your terminal (never in Git). See the official [PLE quick start](https://github.com/google-research/ple#quick-start) for `HF_TOKEN`, the Google Cloud-backed model proxy, `MODEL_PROXY_URL`, and `MODEL_PROXY_KEY`. **Local Vertex AI usage may incur costs.**
4. From the `ple` directory, run:

```bash
uv run python -m kit check ../PLE-Agent-Harness
uv run python eval/run.py ../PLE-Agent-Harness
```

These steps require Docker and access to the model proxy. The evaluator produces an `artifacts/` directory. Before submission, commit legitimate generated artifacts and run the readiness check again.

**Do not substitute a personal model** for the required Google-sponsored model in an official submission. Local-only custom models are not competition-submittable.

## Development roadmap

1. Run the official `kit check` smoke tests and record exact outcomes.
2. Evaluate a small selection of the 29 public development tasks; record model cost, time, errors, and scored deliverables.
3. Add robust tool use (including task-local MCP integrations), calculation validation, and deadline-aware self-checking.
4. Compare improvements against the untouched baseline on the same tasks and model.
5. Produce genuine task artifacts, run readiness verification, and follow official submission instructions.

## Licensing and attribution

Original project material is MIT-licensed (see `LICENSE`). Files `serve.py`, `taskbridge.py`, and `submission.yaml` are copied from the [Google PLE minimal sample](https://github.com/google-research/ple/tree/main/sample_submissions/minimal), copyright Google LLC, and retain their Apache License 2.0 notices. See `THIRD_PARTY_NOTICES.md` and `licenses/Apache-2.0.txt`. These files are **not relicensed to MIT**.

This is a community competition entry, not an official Google product.
