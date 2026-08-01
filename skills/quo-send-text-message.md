---
name: Send a text message with Quo
description: Send an SMS from a Quo workspace phone number to a recipient and confirm delivery.
api: Quo Public API v1
base_url: https://api.quo.com
auth: apiKey in Authorization header (no Bearer prefix)
operations:
  - listPhoneNumbers_v1
  - sendMessage_v1
  - listMessages_v1
---

# Send a text message with Quo

Send an SMS from one of your Quo workspace numbers.

## Steps

1. **Pick a sending number.** `GET /v1/phone-numbers` (`listPhoneNumbers_v1`) and choose the `phoneNumberId` (or its E.164 `phoneNumber`) to send from.
2. **Send the message.** `POST /v1/messages` (`sendMessage_v1`) with the sending number, `to` (an array of up to 10 E.164 recipients for group messaging), and `content`. MMS is not supported.
3. **Confirm.** Poll `GET /v1/messages` (`listMessages_v1`) filtered by phone number and participant, or subscribe to a message webhook (`createMessageWebhook_v1`) for delivery events instead of polling.

## Rules

- Auth: put the raw API key in the `Authorization` header — **no** `Bearer` prefix.
- Rate limit: 10 requests/second per API key; back off on `429`.
- Messaging is credit-based and billed per segment; keep bodies short to minimize segments.
- Recipients must be valid E.164 numbers or you get `400`.
- No Idempotency-Key is supported — do not blindly retry `sendMessage_v1` on a network error without first checking `listMessages_v1`, or you may double-send.
