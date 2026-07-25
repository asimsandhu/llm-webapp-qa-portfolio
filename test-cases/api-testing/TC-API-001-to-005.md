# Test Cases — API Integration Testing

**Module:** Backend API  
**Author:** Asim Masood  
**Tool:** Postman / cURL  

---

## TC-API-001 — Valid Prompt Returns 200 with Response Body

**Endpoint:** `POST /api/v1/chat`  
**Request Body:**
```json
{ "message": "Hello", "session_id": "test-123" }
```
**Expected:** HTTP 200, response body contains `{ "reply": "<non-empty string>", "session_id": "test-123" }`

---

## TC-API-002 — Missing session_id Returns 400

**Request Body:** `{ "message": "Hello" }` (no session_id)

**Expected:** HTTP 400, error message: `"session_id is required"`

---

## TC-API-003 — Empty message Returns 422

**Request Body:** `{ "message": "", "session_id": "test-123" }`

**Expected:** HTTP 422, validation error: `"message cannot be empty"`

---

## TC-API-004 — Rate Limit Enforcement

**Steps:** Send 100 requests within 60 seconds from same IP/user

**Expected:** After the configured rate limit threshold, receive HTTP 429 with `Retry-After` header

---

## TC-API-005 — Response Time Under Normal Load

**Steps:** Send 10 sequential single-turn prompts (non-concurrent)

**Expected:** Each response arrives within 8 seconds (P95 latency target)