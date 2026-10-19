# Agentic Character Design

The hardest problem in AI-generated storytelling is **keeping characters recognisable across scenes**. If every scene prompt describes Arjuna slightly differently, the image model draws a different person each time.

This project solves it with a **chain of specialised LLM agents** that research, design, assemble, validate and correct one canonical appearance per character. Every scene then reuses that appearance.

## The agent chain

```
 Story text
     │
     ▼
 ┌──────────────────────────┐
 │ Character extraction     │  spaCy NER + custom gazetteer + name normalisation
 │ (who appears?)           │  → one entry per character, aliases merged
 └──────────────────────────┘
     │
     ▼
 ┌──────────────────────────┐
 │ 1. Scholar Researcher    │  Extracts only textual facts from canonical sources:
 │                          │  age, lineage, role, described features, weapons
 └──────────────────────────┘
     │ facts (JSON)
     ▼
 ┌──────────────────────────┐
 │ 2. Vedic Iconographer    │  Turns facts into a visual design using traditional
 │                          │  iconography: build, skin tone, attire, ornaments
 └──────────────────────────┘
     │ visual design (JSON)
     ▼
 ┌──────────────────────────┐
 │ 3. Cinematic Assembler   │  Compresses the design into a reusable, model-ready
 │                          │  image prompt fragment
 └──────────────────────────┘
     │ canonical prompt
     ▼
 ┌──────────────────────────┐
 │ 4. Visual Validator      │  STRICT MODE: checks the result against canonical
 │    (critic)              │  facts and flags anything wrong → "needs_correction"
 └──────────────────────────┘
     │ findings
     ▼
 ┌──────────────────────────┐
 │ 5. Visual Rewriter       │  Applies the validator's corrections and stores the
 │                          │  fixed profile
 └──────────────────────────┘
     │
     ▼
 Character profile store  ──▶  injected into every scene's image prompt
```

## Why separate agents?

| Single big prompt | Agent chain |
|---|---|
| Mixes research, design and prompt-writing, so errors are hard to trace | Each agent has one job and a narrow, checkable output |
| Tends to invent details ("hallucinated" jewellery, wrong weapons) | The researcher is limited to source facts; the designer may only interpret those facts |
| No self-check | A dedicated critic agent is instructed to be strict and reject when in doubt |
| Different output every run | The canonical profile is generated once, stored and reused |

## Supporting pieces

- **Global story analysis (memory):** before scenes are built, the full script is analysed once to map every alias and form ("Partha", "Dhananjaya" → Arjuna) to a single character. Scene prompts then always reference the right profile.
- **Canonical weapon descriptions:** signature weapons (e.g. Gandiva, Sudarshana Chakra) are enriched by their own agent step, so they look the same in every appearance.
- **Theme-aware prompts:** all agent prompts are templates with theme variables (visual style, source material). The same chain can serve a different mythology or series by changing configuration.
- **Incremental and resumable:** profiles are saved after each character, so a long batch can stop and resume without regenerating finished characters.
- **Prompt logging:** every agent prompt and response is logged, making it easy to see which agent introduced an error.

## Result

Characters keep the same face, build, attire and weapons across all scenes of a Short, and across different Shorts on the channel. That's what makes AI-generated storytelling watchable as a series.
