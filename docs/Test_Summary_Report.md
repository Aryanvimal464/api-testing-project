# Test Summary Report

| Field | Value |
|---|---|
| Project | REST API Testing using Postman |
| API Tested | JSONPlaceholder (`https://jsonplaceholder.typicode.com`) |
| Testing Period | *[Fill in the date range you actually ran this, e.g. 26 Sep 2026 – 28 Sep 2026]* |
| Total Test Cases | 33 (see `test-cases/API_Test_Cases.md`) |
| Passed | *[Fill in after running `newman run ...` — see the HTML report in `reports/`]* |
| Failed | *[Fill in after running]* |
| Blocked | *[Fill in after running]* |
| Not Executed | *[Fill in after running]* |
| Defects Logged | 0 real defects; 1 sample/template entry in `bugs/Bug_Report_Template.md` |
| Tools Used | Postman, Newman, newman-reporter-htmlextra, Git/GitHub |

## Overall Observations
- JSONPlaceholder behaves consistently with its own public documentation for read
  operations (`GET`).
- Write operations (`POST`, `PUT`, `PATCH`, `DELETE`) return realistic, well-formed
  responses but do not persist data server-side — this is expected mock-API behavior,
  not a defect, and is documented wherever it is relevant (see TC_011, TC_018).
- No server-side request validation is performed (missing fields, wrong data types are
  all accepted) — documented in the Negative Tests folder rather than assumed.

## Known API Limitations
1. Created/updated/deleted resources are not actually persisted between requests.
2. No input validation — invalid data types and empty bodies are accepted.
3. `DELETE` returns `200` with an empty object rather than `204 No Content`.
4. No authentication mechanism to test (by design, as a public demo API).

> **Note:** This report intentionally leaves the pass/fail counts as placeholders. Fill
> them in with the real numbers from your own `newman run` execution and HTML report —
> do not invent execution numbers, per this project's testing principles.
