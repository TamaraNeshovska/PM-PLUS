# PM-PLUS

A multi-agent system that monitors a team's daily check-ins and automatically flags risks.

## How It Works

When the app runs, two agents start simultaneously:

**Risk Analyzer** — the main brain. When a team member checks in (reports their status/blockers), it:
1. Asks the Reporter for that person's history (Loop 1)
2. Sends everything to an LLM to assess if there's a risk (overload, blocker, etc.)
3. If the person is overloaded, asks a Resource Balancer who else can take the task (Loop 2)
4. If a risk is found, posts an alert to the PM's room and waits for approval (Loop 3)

**Reporter** — the memory keeper. It:
- Responds to history queries from the Risk Analyzer
- Archives all check-ins and events to a state store
- Can generate a weekly summary report on demand

**How they talk to each other:** via messages using `@mentions` (e.g. `@risk_analyzer {...}`). The `MessageRouter` handles sending and parsing these messages.

**The LLM** (GPT-4o by default) is used to summarize history and decide whether a risk exists, called through a simple wrapper in `core/llm.py`.

## Setup

### Prerequisites
- Python 3.9+
- [GitHub CLI](https://cli.github.com/) 

### Install dependencies

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Project Structure

```
PM-PLUS/
├── src/
│   ├── agents/
│   ├── core/
│   ├── main.py
│   └── mock_collector.py
├── agent_config.yaml
├── requirements.txt
└── .gitignore
```

## Notes

- Active branch: `agent_architect`
