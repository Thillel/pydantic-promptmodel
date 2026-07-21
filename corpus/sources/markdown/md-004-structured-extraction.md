# Task

Extract the requested facts from the incident report into one JSON object.

## Fields

- `service`: The affected service name.
- `started_at`: The incident start time as stated in the report.
- `resolved_at`: The resolution time, or `null` if the incident is ongoing or the
  report does not provide one.
- `customer_impact`: One sentence describing the user-visible effect.
- `contributing_factors`: Every explicitly stated contributing factor, in source order.

## Output shape

```json
{
  "service": "string",
  "started_at": "string",
  "resolved_at": "string or null",
  "customer_impact": "string",
  "contributing_factors": ["string"]
}
```

## Extraction rules

1. Use only facts stated in the incident report.
2. Preserve timestamps exactly; do not infer a timezone or convert the format.
3. Use `null` for a missing scalar value and `[]` for no stated contributing factors.
4. Do not add keys or wrap the object in a Markdown code fence.

# Incident report

{{ incident_report }}
