# API Reference: Patient Clinical Data Ingestion (`v1/observations`)
> **AUTHENTICATION**  
> All API requests require a valid OAuth 2.0 Bearer Token in the HTTP `Authorization` header.  
> Required Scope: `write:patient_observations`
---
## Endpoint Overview
`POST /api/v1/observations/ingest`
Ingests structured physiological observation records (e.g., vital signs, lab results) from remote patient monitoring hardware or external EHR systems into the AuraHealth analytics engine.
---
## Request Headers

| Header Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `Authorization` | String | **Yes** | `Bearer <access_token>` |
| `Content-Type` | String | **Yes** | Must be `application/json` |
| `X-Organization-ID` | UUID | **Yes** | Target healthcare facility UUID: `e3b0c442-8801-4c12` |

---
## Request Body Schema (`application/json`)
* **`patient_id`** *(string, required)*: Unique encrypted patient identifier.
* **`observation_category`** *(string, required)*: Category of data (`vital-signs`, `laboratory`, `imaging`).
* **`code_system`** *(object, required)*:
  * **`system`** *(string)*: URI of coding system (e.g., `http://loinc.org`).
  * **`code`** *(string)*: LOINC or ICD-10 code (e.g., `8867-4` for Heart Rate).
  * **`display`** *(string)*: Human-readable label (e.g., `"Heart Rate"`).
* **`effective_datetime`** *(string, required)*: ISO 8601 timestamp in UTC (`YYYY-MM-DDTHH:mm:ssZ`).
* **`value_quantity`** *(object, required)*:
  * **`value`** *(number)*: Quantitative value recorded.
  * **`unit`** *(string)*: UCUM standard unit (e.g., `beats/min`).
---
## Code Samples
### Example Request
```json
{
  "patient_id": "PAT-99482-X",
  "observation_category": "vital-signs",
  "code_system": {
    "system": "[http://loinc.org](http://loinc.org)",
    "code": "8867-4",
    "display": "Heart rate"
  },
  "effective_datetime": "2026-09-21T14:30:00Z",
  "value_quantity": {
    "value": 72,
    "unit": "beats/min"
  }
}
---

```markdown
### Response (`201 Created`)

```json
{
  "status": "success",
  "data": {
    "observation_id": "obs_88f01a39-9d01",
    "patient_id": "PAT-99482-X",
    "ingested_at": "2026-09-21T14:30:02.104Z",
    "sync_status": "QUEUED_FOR_PROCESSING"
  }
}
