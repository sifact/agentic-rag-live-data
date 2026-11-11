# Agentic RAG over Live Data

Agentic RAG over live sources (Gmail, Notion, Jira via MCP): plans sub-questions, calls tools, collects evidence and answers with citations. Short and long-term memory, tracing and eval CI.

> **Status: in progress.** The repository is set up and the design is being written. The code, results and demo are added as each step on the roadmap below is finished. All numbers in this README come from evaluation runs in this repo; until a run exists, a metric is listed as planned, not reported.

## The problem
Answers about a project live across email threads, Notion pages and Jira tickets that change every day. A static RAG over a PDF cannot answer "What changed in project X this quarter, and what are the risks?"

## What it does
- Breaks a question into sub-questions (goals, tasks due, changed deadlines, reported blockers).
- Calls tools over live sources through MCP connectors to collect evidence.
- Writes one answer with citations back to the exact email, page or ticket.
- Remembers useful facts across sessions ("project X is our top priority") with clear rules for what is worth keeping.
- Shows its full working: question, rewrites, tools called, evidence, memory used, answer.
- Reuses the guardrails and eval CI from enterprise-rag.

## How it works
```
question ─► planner ─► sub-questions ─► tool calls (MCP: Gmail / Notion / Jira) ─► evidence
                 ▲                                                            │
           memory (short + long term) ◄──────────────── synthesizer ◄─────────┘ ─► cited answer
```

## Tech stack (planned)
- Python, LangGraph
- MCP connectors (Gmail, Notion, Jira)
- LLM with tool calling (provider to be chosen)
- Vector store for long-term memory
- Langfuse or LangSmith tracing
- DeepEval + GitHub Actions

## How it will be evaluated
- Golden set of multi-hop questions over a seeded demo workspace
- Answer correctness, faithfulness and citation precision
- Tool-call accuracy and number of steps per question
- Memory: recall of facts from earlier sessions without storing noise
- Latency and cost per question; injection tests on retrieved emails and tickets

## Roadmap
- [ ] Seed a demo workspace (synthetic emails, Notion pages, Jira tickets)
- [ ] MCP connectors
- [ ] Planner and multi-hop tool loop
- [ ] Cited synthesis
- [ ] Short and long-term memory
- [ ] Tracing
- [ ] Golden set, guardrails and eval CI
- [ ] Web demo showing the agent's working

## Project layout
```
src/agentic_rag/
├── connectors/
├── planning/
├── retrieval/
├── memory/
├── evaluation/
├── serving/
tests/
```

## Related projects
- [enterprise-rag](https://github.com/sifact/enterprise-rag): company assistant with role-based access, hybrid search, guardrails and evals.
