# Verification Agent

## What it does

- Makes sure the property details and the owner are genuine.
- Checks ownership documents, scans them for accuracy, and may also confirm contact details of the seller or builder.
- Returns a clear “verification status” for each property.

## How it fits in the system

- The Search Agent sends a task to the Verification Agent.
- The Verification Agent looks at the documents linked to the listing.
- It uses OCR to read scanned documents and compares them with official records.
- It can also send SMS/phone/email checks to verify the owner’s contact info.
- After checks, it publishes a result message.
- The Orchestrator collects this result and uses it in the final summary for the user.

## Input (example task)

```json
{
	"request_id": "req-001",
	"task_id": "t3_verify_001",
	"task_type": "verification",
	"payload": {
		"listing_id": "L-1001",
		"owner_docs": ["doc-567"],
		"contact_number": "+91-9876543210"
	}
}
```

## Output (example result)

```json
{
	"request_id": "req-001",
	"task_id": "t3_verify_001",
	"status": "completed",
	"result": {
		"listing_id": "L-1001",
		"owner_verified": true,
		"docs_verified": ["doc-567"],
		"contact_verified": true,
		"confidence": 0.88,
		"provenance": [
			{ "source": "ocr_service", "doc_id": "doc-567", "fetched_at": "2025-09-13T10:00:00Z" }
		]
	}
}
```

## What external systems it connects to

- **OCR Service:** to read and check uploaded ownership documents
- **Registry / Legal DB:** to cross-check the names and IDs from documents
- **SMS/Email/Phone Verification Service:** to verify contact details
- **Builder/Owner Database:** to confirm identity and past history
# Verification Agent
