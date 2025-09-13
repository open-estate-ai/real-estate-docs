# Valuation Agent

## What it does

- Estimates a property’s fair price.
- Gives simple scores for investment and residential suitability.
- Combines recent sales (comps), time-series trends, location features, legal flags, and builder signals to produce a price range, scores, and an uncertainty measure.

## How it fits in the system

- The Search Agent sends a valuation task to the Valuation Agent.
- The Valuation Agent gathers data from internal stores and external services (comps, timeseries, maps, builder DB).
- It runs models or heuristics to compute price and scores.
- It returns structured results with model version, feature contributions, confidence, and provenance.
- The Orchestrator uses these results when composing the final response.

## Input (example task)

```json
{
	"request_id": "req-001",
	"task_id": "t3_valuation_001",
	"task_type": "valuation",
	"payload": {
		"listing_ids": ["L-1001", "L-1002"],
		"goal": "investment",
		"model_version": "valuation-v1.2"
	},
	"context": { "user_id": "u-123", "region": "Sonipat" },
	"retries": { "attempt": 0, "max_attempts": 2 }
}
```

## Output (example result)

```json
{
	"request_id": "req-001",
	"task_id": "t3_valuation_001",
	"task_type": "valuation",
	"status": "completed",
	"result": {
		"valuations": [
			{
				"listing_id": "L-1001",
				"fair_price_range": { "low": 7300000, "mid": 7600000, "high": 7900000 },
				"investment_score": 0.78,
				"residential_score": 0.62,
				"estimated_rental_yield": 0.045,
				"uncertainty": 0.07,
				"model_version": "valuation-v1.2",
				"feature_contributions": [
					{ "feature": "distance_to_metro", "impact": 0.12 },
					{ "feature": "rera_status", "impact": -0.30 }
				],
				"provenance": [
					{ "source": "comps_service", "id": "comp-234", "fetched_at": "2025-09-12T08:00:00Z" },
					{ "source": "timeseries_db", "range": "2018-2025" }
				]
			}
		]
	},
	"timestamps": { "started_at": "2025-09-13T10:05:00Z", "completed_at": "2025-09-13T10:05:03Z" }
}
```
