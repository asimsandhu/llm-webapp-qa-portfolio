# BUG-021 — Model Returns Fabricated URLs Without Uncertainty Indicator

| Field | Value |
|---|---|
| **Bug ID** | BUG-021 |
| **Severity** |  High |
| **Priority** | P1 |
| **Module** | Response Quality / UX |
| **Reported By** | Asim Masood (QA Engineer) |
| **Date Reported** | 2025-04-02 |
| **Status** |  In Progress |

---

## Summary

When asked for external resource links (documentation URLs, reference pages), the model generates plausible-looking but completely fabricated URLs with full confidence and no uncertainty disclaimer. Users who click these links land on 404 pages or unrelated websites.

---

## Steps to Reproduce

1. Start a new chat session
2. Ask: *"Can you give me a link to the official Next.js documentation for the App Router?"*
3. Observe the model's response
4. Click the provided URL

---

## Expected Result

Model should either:
- Provide the correct URL (`https://nextjs.org/docs/app`) with a note to verify
- State it cannot provide external links but describe where to find the resource
- Add a disclaimer: *"I may hallucinate URLs — please verify before clicking"*

---

## Actual Result

Model responded:
> "Sure! Here is the official Next.js App Router documentation: https://nextjs.org/documentation/app-router/getting-started/overview"

The URL returns a 404. The correct URL is `https://nextjs.org/docs/app`.

**The model presented a fabricated URL with 100% confidence and no caveat.**

---

## Impact

- Users following broken links lose trust in the application
- Could be dangerous in medical, legal, or financial use cases where sources matter
- Affects user experience significantly — reported by 3 beta users independently

---

## Recommendation

1. Add a system-level instruction: "When providing URLs, always add: *'Please verify this link as I may produce incorrect URLs.'*"
2. Implement a URL validation layer that checks URLs before rendering them as clickable links
3. Consider flagging all URLs with a  icon and tooltip warning
