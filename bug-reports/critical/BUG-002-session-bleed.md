# BUG-002 — Conversation Context Bleeds Across User Sessions

| Field | Value |
|---|---|
| **Bug ID** | BUG-002 |
| **Severity** | 🔴 Critical |
| **Priority** | P0 |
| **Module** | Session Management |
| **Reported By** | Asim Masood (QA Engineer) |
| **Date Reported** | 2025-03-18 |
| **Status** | ✅ Fixed (v1.4.3) |

---

## Summary

When two users are active simultaneously, User B's conversation occasionally receives context injected from User A's ongoing session. This means one user can see fragments of another user's private conversation history.

---

## Steps to Reproduce

1. Open two separate browser profiles (simulating two distinct users)
2. Log in as User A in Profile 1; start a conversation: *"My company name is ACME Corp and I need help with invoicing."*
3. Within 3 seconds, log in as User B in Profile 2; start a new conversation: *"What is my company name?"*
4. Observe User B's model response

---

## Expected Result

User B's model has no knowledge of User A's session. Response should be something like:
> "I don't have access to your company information. Could you please tell me your company name?"

---

## Actual Result

User B's model response included:
> "Based on our conversation, your company name is ACME Corp. Would you like help with invoicing?"

User B received User A's private conversation context — **a complete data privacy breach**.

---

## Impact

- **Privacy**: Users can access other users' private conversation data
- **Compliance**: Potential GDPR / data protection violation
- **Trust**: Catastrophic if discovered by end users

---

## Root Cause

Context window was stored in a shared in-memory cache keyed only by session timestamp, not by user ID + session ID composite key. Under concurrent load, keys collided.

---

## Evidence

- Full reproduction video recorded (internal link: QA-2934)
- Reproduced 4/10 attempts under concurrent load conditions