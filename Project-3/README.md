# TechCorp Support Agent

🔗 **[Live Demo](https://hi3btvwjj6qqpn3mzv3htw.streamlit.app/)** — try test scenario **S4 · SLA violated 10 days** in the sidebar to see the full agent trace in action.

An AI-powered multi-agent customer support system that automates customer query resolution, account verification, feature validation, and ticket escalation using LLM-driven agents. Instead of a single model guessing at an answer, the system routes each query through specialized agents that investigate the actual account, contract, and feature data before responding — and escalates to a human team when it should.

---

## What it does

- **Understands the query** and routes it to the right specialist agents
- **Verifies the account** — plan, billing health, seat usage, enabled features
- **Validates features** — checks plan eligibility, setup requirements, and flags mismatches between what a customer expects and what their plan actually includes
- **Checks contract terms** — SLA response/resolution windows, and whether they've been breached
- **Escalates automatically** when needed — creates a ticket, assigns a team, sets priority, and explains why
- **Generates a context-aware final answer** grounded only in the real data collected, not a hallucinated guess

## Architecture

```
User Query
    │
    ▼
Orchestrator Agent  ──────────────────────────────────────┐
    │                                                       │
    ├──► Account Agent      (plan, billing, status, seats)  │
    ├──► Feature Agent      (eligibility, mismatches)       │
    ├──► Contract Agent     (SLA terms, breach detection)   │
    └──► Escalation Agent   (routing, ticket creation)      │
    │                                                       │
    ▼                                                       │
Final Answer (LLM-generated, grounded in agent findings) ◄──┘
```

Each agent has its own tools (`tools/`) for querying the underlying JSON knowledge base (`database/`), and the orchestrator decides which agents a given query actually needs — not every query touches every agent.

## Tech Stack

| Layer | Tools |
|---|---|
| Language | Python |
| Agent orchestration | LangChain, custom orchestrator |
| LLM providers | Groq API (Llama 3.3), Gemini 2.0 Flash, Ollama (local) |
| Observability | Langfuse (agent traces, token/latency tracking) |
| UI | Streamlit |
| Data layer | JSON-based knowledge base (customers, features, contracts, escalations) |

## Try it yourself

The live demo includes 5 preset test scenarios in the sidebar, or you can type a custom query:

| Scenario | What it tests |
|---|---|
| S1 · Enable dark mode | Simple feature lookup, no escalation |
| S2 · API on Starter plan | Feature/plan mismatch detection |
| S3 · Pro rate limit conflict | Contradiction between plan promise and actual behavior |
| S4 · SLA violated 10 days | Contract breach detection + automatic escalation |
| S5 · 15 users, only 10 seats | Licensing/seat mismatch investigation |

You can also switch the customer profile and LLM provider from the sidebar to see how responses change.

## Run it locally

```bash
git clone https://github.com/PureBliss-prog/TechCorp-Support-Agent.git
cd TechCorp-Support-Agent/Project-3
pip install -r requirements.txt
```

Create a `.env` file in `Project-3/` with:
```
GROQ_API_KEY=your-key-here
GEMINI_API_KEY=your-key-here
LANGFUSE_PUBLIC_KEY=your-key-here
LANGFUSE_SECRET_KEY=your-key-here
```

Then run:
```bash
streamlit run app.py
```

## Project structure

```
Project-3/
├── app.py                  # Streamlit UI
├── main.py                 # CLI entry point / query runner
├── config.yaml             # LLM provider + app config
├── agents/                 # Orchestrator + specialist agents
├── llm/                    # Provider adapters (Groq, Gemini, Ollama)
├── database/                # JSON knowledge base + data access layer
├── memory/                 # Conversation memory, shared context, state
├── monitoring/              # Langfuse tracing, metrics
├── tools/                  # Per-agent tool functions
└── tests/                  # Agent, scenario, and tool tests
```

## Author

**Jaee Deshmukh**
[LinkedIn](https://linkedin.com/in/jaee-deshmukh-8b1315259) · [GitHub](https://github.com/PureBliss-prog) · jaeedeshmukh2004@gmail.com
