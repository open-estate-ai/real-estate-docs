# Legal Agent

## What it does

- Checks if a property is legally safe.
- Looks at RERA approval, land/registry details, ownership, disputes, and uploaded documents.
- Returns a simple “legal health report” for each property.

## How it fits in the system

- The Search Agent sends a task to the Legal Agent.
- The Legal Agent talks to official systems like RERA and land registry.
- It also scans any documents (like sale deeds) with OCR.
- After checks, it publishes a result message.
- The Orchestrator collects this and uses it in the final response.

## Input (example task)

```json
{
	"request_id": "req-001",
	"task_id": "t2_legal_001",
	"task_type": "legal_check",
	"payload": {
		"listing_ids": ["L-1001", "L-1002"],
		"region": "Sonipat"
	}
}
```

## Output (example result)

```json
{
	"request_id": "req-001",
	"task_id": "t2_legal_001",
	"status": "completed",
	"result": [
		{
			"listing_id": "L-1001",
			"rera_status": "approved",
			"ownership_match": "matched",
			"encumbrance_flag": "none",
			"litigation_flag": "none",
			"document_verified": true,
			"confidence": 0.93,
			"provenance": [
				{ "source": "rera_api", "id": "RERA-987", "fetched_at": "2025-09-13T09:00:00Z" }
			]
		}
	]
}
```

## What external systems it connects to

- **RERA API:** for project approvals
- **Land Registry:** for ownership and encumbrance checks
- **Court Records:** for disputes/litigation
- **OCR service:** for scanning uploaded documents
# Legal Agent