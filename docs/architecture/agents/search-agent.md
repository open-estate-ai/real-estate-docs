# Search Agent

The Search Agent is a simple service that helps find property listings based on user filters.  
It doesn’t make decisions—it just finds and returns the best matches for other agents to check.

---

## What does the Search Agent do?

- Takes search filters from the Orchestrator (like property type, price, location).
- Runs queries against your databases and APIs.
- Returns a ranked list of candidate properties with useful details.

---

## How does it work?

### 1. Input

The Orchestrator sends a search task to the Search Agent.  
Example input:

```json
{
	"request_id": "req-001",
	"task_id": "t1_search",
	"task_type": "search_listings",
	"payload": {
		"property_type": "plot",
		"max_price_inr": 8000000,
		"near": "metro",
		"radius_km": 5,
		"freshness_days": 30,
		"max_candidates": 20
	}
}
```

---

### 2. Step-by-step Processing

- **Resolve location:**  
	If the filter is "near metro", Search Agent calls a MAPS MCP (Model Context Protocol Server) to get metro station coordinates.

- **Run database query:**  
	Search Agent queries Athena (linked to your data lake) for listings matching the filters.

- **Run fuzzy search:**  
	For requirements like "good for investment", Search Agent uses a Vector DB to find listings with similar descriptions or reviews.

- **Merge and rank:**  
	Combines results from Athena and Vector DB, removes duplicates, and ranks by price, freshness, distance, and relevance.

- **Check freshness:**  
	If most listings are old, Search Agent can ask the Orchestrator to trigger fresh data ingestion.

---

### 3. Output

Search Agent sends results back to the Orchestrator:

```json
{
	"request_id": "req-001",
	"task_id": "t1_search",
	"status": "completed",
	"result": {
		"candidates": [
			{
				"listing_id": "L-1001",
				"price": 7800000,
				"lat": 28.994,
				"lon": 77.023,
				"rera_status": "approved",
				"source": "99acres",
				"fetch_time": "2025-09-13T10:00:00Z",
				"score": 0.91
			},
			{
				"listing_id": "L-1002",
				"price": 7500000,
				"lat": 28.991,
				"lon": 77.030,
				"rera_status": "pending",
				"source": "magicbricks",
				"fetch_time": "2025-09-12T15:00:00Z",
				"score": 0.87
			}
		]
	}
}
```

---

## What does it connect to?

- **Athena (Data Lake):**  
	Finds listings using structured filters (price, type, location).

- **Vector DB:**  
	Finds listings using semantic similarity for fuzzy search.

- **MAPS MCP:**  
	Resolves location filters like "near metro" into coordinates.

- **External Portals:**  
	Uses data from MagicBricks, 99acres, etc. (via periodic ingestion).

---

## How does it fit in the flow?

1. User asks a question.
2. Orchestrator sends a search task to Search Agent.
3. Search Agent queries Athena, Vector DB, and MAPS MCP.
4. Search Agent ranks and returns candidate listings.
5. Orchestrator sends candidates to Legal, Valuation, and Verification Agents.

---

## In short

The Search Agent is the “retrieval brain” of your system.  
It finds the best property matches and passes them on for deeper checks.

---

## Example Request

```json
{
	"request_id": "req-002",
	"task_id": "t1_search",
	"task_type": "search_listings",
	"payload": {
		"property_type": "flat",
		"max_price_inr": 6000000,
		"near": "Sector 14",
		"radius_km": 2,
		"freshness_days": 15,
		"max_candidates": 10
	}
}
```

## Example Response

```json
{
	"request_id": "req-002",
	"task_id": "t1_search",
	"status": "completed",
	"result": {
		"candidates": [
			{
				"listing_id": "F-2001",
				"price": 5900000,
				"lat": 28.990,
				"lon": 77.025,
				"rera_status": "approved",
				"source": "Housing.com",
				"fetch_time": "2025-09-13T09:00:00Z",
				"score": 0.93
			}
		]
	}
}
```

---

If you have questions or want to see more examples, check the guides or reach out on [Slack](https://join.slack.com/t/open-estate-ai/shared_invite/zt-3dk65gu4h-SmBeySssL732C3ReHL_ejQ) or open an issue on [GitHub](https://github.com/open-estate-ai/real-estate-docs)!
