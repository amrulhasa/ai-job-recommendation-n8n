# Architecture

## High-level flow

```text
Form Submission
    │
    ▼
Basic LLM Chain
    │
    ├── OpenRouter Chat Model
    └── Structured Output Parser
    │
    ▼
Code in JavaScript1
    │
    ▼
HTTP Request → Serper API
    │
    ▼
Code in JavaScript
    │
    ▼
Code in JavaScript2
    │
    ▼
Code in JavaScript3
    │
    ▼
Basic LLM Chain2
    │
    ├── OpenRouter Chat Model1
    └── Structured Output Parser1
    │
    ▼
Code in JavaScript4
    │
    ▼
Gmail
```

## Stage 1 — Candidate intake

The form collects:

- Name
- Email
- Skills
- Preferred location
- Experience level
- Work preference

## Stage 2 — Search planning

The first LLM analyzes the candidate profile and creates 3–5 suitable job roles. Each role receives a specific search query containing relevant candidate constraints and a current-job-opening intent.

The output parser keeps the result in a predictable JSON structure before JavaScript transforms it into individual search items.

## Stage 3 — Job discovery

Each generated query is sent to `https://google.serper.dev/search`. The workflow uses a secure n8n Header Auth credential for the `X-API-KEY`; the actual secret is not stored in this repository.

The response is flattened into job records containing title, link, snippet, and search position.

## Stage 4 — AI matching

The workflow combines the candidate profile with the collected job records. A second LLM ranks the jobs and returns the top five recommendations with:

- job title
- company
- location
- match score
- reason
- application/view link
- mismatch or concern when applicable

## Stage 5 — Delivery

JavaScript converts the structured recommendations into an HTML email. The Gmail node sends that email to the address submitted through the form.

## Design principles

- Use structured AI output instead of relying on free-form text.
- Keep candidate-specific processing inside the workflow.
- Do not invent job listings, companies, salaries, requirements, or URLs.
- Keep third-party API credentials outside the public workflow JSON.
- Separate data transformation from AI reasoning where practical.