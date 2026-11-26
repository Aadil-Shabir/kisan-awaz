# Kisan Awaz

A WhatsApp voice assistant that answers wheat farming questions in Urdu and Sindhi, with every answer grounded in official guides and every part of the system measured.

> Status: in development. This is a prototype and research project, not a replacement for an agriculture extension officer.

## The problem

Most farmers in Sindh do not have easy access to an agriculture expert. Advice exists in official guides, but those guides are long PDF documents, often in English or formal Urdu, and many farmers are more comfortable speaking than reading.

Voice AI could close that gap, but speech models perform much worse on Urdu and Sindhi than on English. Nobody building a product for these farmers knows how reliable the current tools really are until they measure it.

## What this project does

1. A farmer sends a voice note on WhatsApp, in Urdu or Sindhi.
2. The system converts the voice note to text.
3. It searches a library of official wheat farming guides for the relevant passage.
4. An LLM writes a short, simple answer using only that passage.
5. The answer is converted to voice and sent back on WhatsApp.

## What makes it different

This is not a demo that works on three hand-picked examples. The project includes a test set of 100 real farmer questions with correct answers, 50 of them recorded as voice notes by different speakers. Each stage is scored separately:

- Speech to text: word error rate, for Urdu and Sindhi separately
- Retrieval: did the system find the right passage
- Answer: is the final answer correct and complete
- Refusal: does the bot decline when the guides do not cover the question

Results, failures and fixes are published in this README as the project progresses.

## Scope

In scope:

- One crop: wheat
- One region: Sindh, Pakistan
- Two languages: Urdu first, Sindhi measured alongside
- Topics: sowing time, seed varieties, irrigation, fertilizer timing, common pests and diseases, harvesting

Out of scope:

- Other crops
- Market prices and weather forecasts
- Diagnosing disease from photos
- Any advice that is not in the source guides

## Safety rules

- The bot only repeats what the official guides say. It does not invent advice.
- It never makes up pesticide or chemical doses. If the guides do not state a dose, it says so.
- When it is unsure or the question is outside the guides, it tells the farmer to contact their local agriculture extension officer.

## Architecture

```
WhatsApp voice note
        |
        v
FastAPI webhook
        |
        v
Speech to text  --->  LangGraph agent  --->  Text to speech
                        |        ^
                        v        |
                Supabase (pgvector)
                official wheat guides
        |
        v
WhatsApp voice reply
```

## Tech stack

| Part | Tool |
|---|---|
| Messaging | WhatsApp Cloud API |
| Backend | Python, FastAPI |
| Agent flow | LangGraph |
| Knowledge library | Supabase with pgvector |
| Speech to text | To be chosen after benchmarking (see Results) |
| Text to speech | To be chosen after testing |
| Evaluation | Python scripts in `evals/` |
| Demo page | Next.js |

## Repository structure

```
kisan-awaz/
├── app/              FastAPI app and WhatsApp webhook
├── agent/            LangGraph flow: retrieve, answer, refuse
├── ingest/           Scripts to split and store the guides
├── speech/           Speech to text and text to speech wrappers
├── evals/
│   ├── questions.csv     100 questions with correct answers
│   ├── audio/            50 recorded voice notes
│   └── run_evals.py      Scoring script
├── data/guides/      Source documents (with links to originals)
├── web/              Next.js demo page
├── .env.example
└── README.md
```

## Results

Results will be added here as each stage is measured.

| Stage | Metric | Baseline | Current |
|---|---|---|---|
| Speech to text (Urdu) | Word error rate | TBD | TBD |
| Speech to text (Sindhi) | Word error rate | TBD | TBD |
| Retrieval | Correct passage found | TBD | TBD |
| Answers | Correct answers out of 100 | TBD | TBD |
| Refusals | Correctly declined | TBD | TBD |

## Roadmap

- [ ] Setup and scope
- [ ] Knowledge library of wheat guides
- [ ] Test set: 100 questions, 50 voice recordings
- [ ] Text-only bot and baseline score
- [ ] Speech to text benchmark
- [ ] Text to speech
- [ ] WhatsApp integration
- [ ] Failure analysis and fixes
- [ ] Final results, demo video and demo page

## Running locally

Setup instructions will be added once the first version is working.

## Author

Built by Aadil Memon, full-stack and AI agent developer.

## License

MIT
