# Restaurant Review Sentiment Analysis with a Local LLM

LLM-based sentiment and aspect classification of restaurant reviews using
`llama3.2:3b` via Ollama, evaluated against star ratings as ground truth.

## What it does

1. Classifies each review as Positive, Negative or Neutral (free text, then JSON)
2. Extracts aspect-level sentiment (food quality, ambience) as structured JSON
3. Evaluates predictions against star ratings (5 = Positive, 3 = Neutral, 1 = Negative)
4. Tests an LLM-as-judge for consistency against an exact code check

## Results (20 labelled reviews)

| Prompt | Accuracy |
|---|---|
| Sentiment, JSON output | 100% |
| Sentiment + aspects, JSON output | 100% |

## Findings

- **Prompt formatting matters.** Without a delimiter between the instruction and the review, the aspect prompt dropped to 95%: one 5-star review was labelled Negative.
- **Use code for exact checks.** An LLM judge checking whether three labels agreed scored 0/20 on raw outputs and 13/20 on extracted labels, versus an exact pandas comparison (20/20). Small models are unreliable judges; save LLM-as-judge for open-ended outputs.
- **Parse structured output, not prose.** JSON responses parsed cleanly on all 20.

## Limitations

20 short, clearly labelled reviews. Next step: a larger dataset and a comparison across models and prompts.

## Run

```bash
ollama pull llama3.2:3b
uv sync
jupyter notebook review_sentiment.ipynb
```

## Next steps
- Scale to ~1,000 Yelp reviews (map 1–2 stars to Negative, 3 to Neutral, 4–5 to Positive)
- Compare llama3.2:3b, llama3.1:8b and qwen2.5:7b on accuracy, latency and JSON parse-failure rate