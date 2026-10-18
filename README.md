# 🥐 Furn Teta – Agent-Powered Business Assistant

EECE 503P/798S – Assignment C3: Introduction to Agents

**Zeina** is the virtual host of *Furn Teta*, a fictional family-run Lebanese bakery in Hamra, Beirut.
She answers customer questions, collects leads for catering and workshops, and logs feedback and
questions she can't answer.

🎥 **Demo video:** https://youtu.be/ry5ukHw4w8o

## Features
- **Persona + Instructions**: clearly separated: *who* Zeina is vs. the rules she must follow
- **Router**: a lightweight LLM call classifies each message as `general`, `lead`, or `feedback`
- **Tools** (function calling):
  - `record_customer_interest(email, name, message)`: saves a lead
  - `record_feedback(question)`: logs unanswered questions and feedback
- **Agent loop**: runs tool calls and feeds the results back to the model
- **Short-term memory**: the chat history is resent on every turn
- **(Bonus) Reflection**: a reviewer call checks each draft against the instructions and revises it if needed; draft and final reply are logged
- **Gradio** chat interface

## Architecture
```
user message → router (general / lead / feedback)
             → agent: system prompt (persona + instructions + business docs) + history + tools
             → tool loop (record_customer_interest / record_feedback)
             → reflection (approve or revise draft)
             → reply in Gradio
```

## Tech stack
- **Gemini** via its OpenAI-compatible endpoint (`openai` Python library)
- `gemini-3.x-flash` for the agent, `gemini-3.5-flash-lite` for the router and reflection
- `pypdf` to read the business profile, `gradio` for the UI

## Project structure
```
├── me/
│   ├── about_business.pdf      # full business profile
│   └── business_summary.txt    # short summary
├── business_agent.ipynb        # main notebook
├── requirements.txt
├── .env.example                # template, real .env is NOT committed
└── README.md
```

## How to run (Google Colab)
1. Get a Gemini API key from [Google AI Studio](https://aistudio.google.com).
2. In Colab, add it under 🔑 **Secrets** as `GEMINI_API_KEY` (enable notebook access).
3. Open `business_agent.ipynb`, run all cells, and upload the two files from `me/` when prompted.
4. Chat with Zeina in the Gradio interface at the bottom.

Running locally instead: `pip install -r requirements.txt`, copy `.env.example` to `.env` and add your key.

## Logs
Every lead, feedback item, and reflection is printed and appended to `logs.jsonl`
(one JSON object per line). In production this would be a database or CRM instead.
