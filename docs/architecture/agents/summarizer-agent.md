# Summarizer Agent

## What it does

- Takes outputs from the Search, Legal, Valuation, and Verification Agents.
- Combines them into a clear, user-friendly response.
- Explains why a property is good or bad, highlights risks, and attaches supporting evidence (citations).

## How it fits in the system

- The Orchestrator waits until Search, Legal, Valuation, and Verification results are ready.
- The Orchestrator creates a summarize_response task and sends it to the Summarizer Agent.
- The Summarizer Agent collects these inputs (or references to them).
- It uses an LLM + RAG (retrieves supporting docs if needed) to generate a plain-text explanation.
- It attaches provenance so the system can prove each claim.
- The final summary goes back to the Results Queue, where the Orchestrator picks it up and runs Output Guardrails.

## Input (example task)

```json
{
	"request_id": "req-001",
	"task_id": "t5_summarize_001",
	"task_type": "summarize_response",
	"payload": {
		"user_query": "Find me RERA-approved plots under 80 lakh near metro",
		"search_results": { /* candidates from Search Agent */ },
		"legal_results": [ /* results from Legal Agent */ ],
		"valuation_results": [ /* results from Valuation Agent */ ],
		"verification_results": [ /* results from Verification Agent */ ]
	}
}
```

## Output (example result)

```json
{
	"request_id": "req-001",
	"task_id": "t5_summarize_001",
	"status": "completed",
	"result": {
		"summary_text": "2 plots found near metro. Plot L-1001 is RERA-approved, fairly priced at ~₹76L, and has a strong investment score (0.78). Verified ownership. Plot L-1002 is cheaper but RERA approval is pending and ownership is unverified, making it riskier.",
		"candidates": [
			{ "listing_id": "L-1001", "final_score": 0.82, "recommendation": "good" },
			{ "listing_id": "L-1002", "final_score": 0.55, "recommendation": "risky" }
		],
		"citations": [
			{ "listing_id": "L-1001", "source": "RERA API", "fetched_at": "2025-09-13T09:00:00Z" },
			{ "listing_id": "L-1001", "source": "Timeseries DB", "fetched_at": "2025-09-12T10:30:00Z" }
		]
	}
}
```

