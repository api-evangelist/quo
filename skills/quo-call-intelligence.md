---
name: Retrieve Quo call summaries and transcripts
description: List calls and pull AI-generated summaries and transcripts for analysis.
api: Quo Public API v1
base_url: https://api.quo.com
auth: apiKey in Authorization header (no Bearer prefix)
operations:
  - listCalls_v1
  - getCallSummary_v1
  - getCallTranscript_v1
---

# Retrieve Quo call summaries and transcripts

Pull AI call intelligence (summaries + transcripts, including Sona-handled calls) for a Quo number.

## Steps

1. **List calls.** `GET /v1/calls` (`listCalls_v1`) for a Quo number and counterpart number; results are paginated (`maxResults` + `pageToken`).
2. **Get the summary.** For each `callId`, `GET /v1/call-summaries/{callId}` (`getCallSummary_v1`).
3. **Get the transcript.** `GET /v1/call-transcripts/{id}` (`getCallTranscript_v1`) for the full transcript.

## Rules

- Auth: raw API key in the `Authorization` header (no `Bearer`).
- Call summaries and transcripts are **only available on business and scale plans** — expect `403` otherwise.
- Data fields may be `null` while processing; poll or use a `createCallSummaryWebhook_v1` / `createCallTranscriptWebhook_v1` webhook to be notified on completion.
- Respect the 10 req/s per-key rate limit; back off on `429`.
