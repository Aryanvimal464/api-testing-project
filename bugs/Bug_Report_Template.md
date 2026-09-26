# Bug Report Template

> This file defines the bug report format used for this project. JSONPlaceholder is a
> stable, widely used mock API and no real defect was found in it during this testing
> effort. The example below is clearly a **sample/template only**, used to demonstrate
> bug reporting methodology for interviews — it does **not** describe a real bug.

## Template Fields

| Field | Description |
|---|---|
| Bug ID | Unique identifier, e.g. BUG-001 |
| Title | One-line summary of the issue |
| Module | Feature area, e.g. Posts, Users, Comments |
| Endpoint | Exact API path, e.g. `/posts/1` |
| Method | HTTP method used |
| Environment | e.g. QA_Environment (Postman), Newman CLI, local machine |
| Preconditions | State required before reproducing the issue |
| Steps to Reproduce | Numbered steps |
| Request | Request body/params sent |
| Expected Result | What should have happened |
| Actual Result | What actually happened |
| Status Code | HTTP status code received |
| Severity | Critical / Major / Minor / Cosmetic |
| Priority | High / Medium / Low |
| Evidence | Screenshot, response snippet, or Newman report link |
| Status | New / In Progress / Fixed / Closed / Won't Fix |

---

## SAMPLE (illustrative only — not a real defect in JSONPlaceholder)

| Field | Value |
|---|---|
| Bug ID | BUG-001 (SAMPLE) |
| Title | POST /posts accepts a non-numeric value for `userId` without validation |
| Module | Posts |
| Endpoint | `/posts` |
| Method | POST |
| Environment | QA_Environment, Postman v11, Newman CLI |
| Preconditions | None |
| Steps to Reproduce | 1. Send POST to `{{baseUrl}}/posts`<br>2. Body: `{ "title": "Invalid Data Type Test", "body": "...", "userId": "one" }`<br>3. Observe response |
| Request | `{ "title": "Invalid Data Type Test", "body": "userId sent as a string instead of a number", "userId": "one" }` |
| Expected Result | API should reject the request with `400 Bad Request` because `userId` is documented as a numeric reference to a user, or clearly document that no type validation is performed |
| Actual Result | API returns `201 Created` and echoes back `userId: "one"` unchanged, with no validation error |
| Status Code | 201 |
| Severity | Minor (this is a public mock/demo API with no real data-integrity requirement) |
| Priority | Low |
| Evidence | See Newman HTML report, request "POST with Invalid Data Type for userId" in folder `06 - Negative Tests` |
| Status | Won't Fix (by design — JSONPlaceholder intentionally performs no server-side validation, since it exists purely for prototyping) |

**Purpose of this sample:** to show, in an interview, that I understand how to write a
clear, reproducible, evidence-backed bug report — not to claim a real defect was found
in a third-party public API.
