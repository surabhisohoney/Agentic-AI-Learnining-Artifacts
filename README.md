# Agentic-AI-Learning-Artifacts

Runnable Colab notebooks from my agentic AI learning sprint: RAG with decision records, tool-using agents, memory, MCP and A2A, evaluation gates, red teaming, and versioned deployment with rollback.

Each notebook is meant to be **simple, testable and explainable**: it has a clear outcome, tests that prove it, and a short "how to explain this" section.

## Notebooks

| # | Notebook | What it demonstrates |
|---|---|---|
| 1 | `sprint01_rag_agent_triad.ipynb` | RAG agent with cited sources, RAG triad scoring (context relevance, groundedness, answer relevance), and a SKILL.md handover file. Runs on open models in Colab. |
| 2 | `sprint02_tool_recovery.ipynb` | Tool failure recovery: retries, a polite fallback, and a step limit. |
| 3 | `sprint03_pause_resume_memory.ipynb` | Pause for human approval, checkpoint to disk, resume, and persistent memory. |
| 4 | `sprint04_mcp_scoped_permissions.ipynb` | MCP-style tool server with scoped tokens; a read-only token cannot write. |
| 5 | `sprint05_a2a_supervisor_specialist.ipynb` | A2A supervisor delegating to a specialist, including a failing specialist. |
| 6 | `sprint06_sft_gemini_automatic_evaluation.ipynb` | Supervised fine-tuning with integrated Gen AI evaluation on Vertex AI. Adapted from Google Cloud's public sample (see Credits). Needs a GCP project. |
| 7 | `sprint07_eval_gate_trace.ipynb` | Evaluation report that blocks a release on regression, plus a trace walk to the first differing step. |
| 8 | `sprint08_redteam_model_armor_style.ipynb` | Red team test without leaks: input and output screening, tool policy, layered defence, scoreboard. |
| 9 | `sprint09_agent_runtime_dashboards.ipynb` | Agent behind a runtime with latency, cost and error dashboards and an SLO release gate. |
| 10 | `sprint10_rag_decision_record_embeddings.ipynb` | RAG with embeddings: experiment grid over chunk size, k and similarity cutoff; a decision record generated from the evidence; SKILL.md. |
| 11 | `sprint11_connected_agent_eval_deploy.ipynb` | One connected agent. **Part B:** recovery, pause/resume, four memory kinds, MCP scopes, A2A, traces. **Part C:** eval gate, red team, dashboards and SLOs, model card, versioned-config deploy with automatic rollback. |

Notebooks 10 and 11 are the best starting points: together they cover the whole path from a RAG agent to a released, monitored one.

## How to run

1. Open a notebook in Google Colab (File > Open notebook > GitHub, or upload it).
2. Run all cells. Most notebooks start in **mock mode** (`BACKEND = "mock"`): a built-in fake model, no API key, deterministic results, so the tests can pass.
3. To use a real model, set `BACKEND = "real"` and add `GEMINI_API_KEY` in Colab (left sidebar > Secrets). Calls go through Gemini's OpenAI-compatible endpoint, and only one function (`ask_model()`) talks to a model.

In real mode, treat test failures as findings about the model, not necessarily bugs. A full real-mode run of notebook 11 makes several hundred calls, so use mock for the full run.

Notebook 10 downloads a small embedding model (`all-MiniLM-L6-v2`, about 90 MB) on first run. Set `RETRIEVER = "tfidf"` to avoid the download.

## Design notes

- **Mock first.** A deliberately gullible fake model makes tests deterministic and shows that safety comes from code around the model, not from the model refusing.
- **Evidence over opinion.** Decisions (chunk size, k, release gates) are made by a written rule and recorded, with the record generated from the measured table.
- **Layered defence.** Input filter, output filter, token scopes, refund cap and human approval each stop different attacks.
- **Simulated numbers.** In mock mode latency is simulated and prices are assumed. Measure again with a real model and your real prices.

## Limits

- Evaluation sets are small (about 10 to 20 cases). Treat small differences as ties.
- Filters are pattern-based and can be bypassed by rephrasing; tool-side controls are the backstop.
- Routing in the connected agent uses a keyword, not a classifier.

## Credits

Notebook 6 is adapted from the public Google Cloud sample "Supervised Fine-Tuning with integrated Gen AI Evaluation" (GoogleCloudPlatform/generative-ai), licensed under Apache 2.0. Keep its original header when redistributing.

## Author

Surabhi Sohoney, Senior Machine Learning Engineer (Applied AI).
