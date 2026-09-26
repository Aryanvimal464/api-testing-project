# API Testing Strategy

## 1. Scope
This strategy covers functional API testing of the public JSONPlaceholder REST API
(`https://jsonplaceholder.typicode.com`), specifically the `/posts`, `/users`, and
`/comments` resources, using Postman and Newman.

## 2. Objectives
- Validate correct behavior of GET, POST, PUT, PATCH, and DELETE operations.
- Verify status codes, response structure, and response time.
- Demonstrate positive, negative, and boundary testing techniques.
- Produce a repeatable, CLI-runnable regression suite (Newman) with HTML reporting.

## 3. API Under Test
- **Name:** JSONPlaceholder
- **Base URL:** `https://jsonplaceholder.typicode.com`
- **Type:** Public fake/mock REST API for testing and prototyping — no authentication,
  no real data persistence.
- **Resources tested:** posts, users, comments.

## 4. Test Types
- Functional testing (GET/POST/PUT/PATCH/DELETE)
- Positive testing
- Negative testing
- Boundary testing (ID = 0, ID = 100, ID = 999999, non-numeric ID)
- Response schema / data validation
- Response header validation
- Response time (non-functional) checks

## 5. Test Environment
- Postman Desktop App (latest)
- Newman CLI (Node.js) for command-line execution
- `QA_Environment` Postman environment (`baseUrl` variable)
- No authentication required (public API)

## 6. Tools
| Tool | Purpose |
|---|---|
| Postman | Manual test design, exploratory testing, collection authoring |
| Newman | CLI execution of the collection, CI/CD-friendly |
| newman-reporter-htmlextra | HTML test reports |
| Git/GitHub | Version control and portfolio hosting |

## 7. Entry Criteria
- API endpoints are publicly reachable.
- Postman collection and environment are prepared and reviewed.
- Test data is defined in `test-data/test-data.json`.

## 8. Exit Criteria
- All planned test cases in `test-cases/API_Test_Cases.md` have been executed.
- Newman HTML report generated in `reports/`.
- All failures triaged: either fixed, documented as a known API limitation, or logged
  using the bug report template.

## 9. Risks
- JSONPlaceholder does not persist created/updated/deleted data — chained,
  multi-request workflows that depend on real persistence are not possible and are
  explicitly called out rather than faked.
- Being a shared public demo API, response times may vary with network conditions and
  third-party load; response-time assertions use a generous threshold and are treated
  as environment-relative, not a production SLA.
- API behavior could change over time since it is not under this project's control;
  test cases were verified against the live API's actual behavior at the time of
  writing and should be re-verified periodically.

## 10. Assumptions
- No authentication/authorization is required by this API.
- Test execution happens against the public internet-hosted instance, not a local mock.
- Fresher-level scope: this suite favors clarity and correctness over exhaustive
  coverage of every possible edge case.

## 11. Test Data Strategy
- Static, version-controlled sample payloads live in `test-data/test-data.json`
  (valid post, updated post, invalid-type post, empty post, boundary IDs).
- No real personal data is used; all names/emails encountered come from
  JSONPlaceholder's own public seed data.

## 12. Defect Management
- Any unexpected behavior is first re-verified manually (not assumed) before being
  logged.
- Defects are logged using `bugs/Bug_Report_Template.md`.
- Since JSONPlaceholder is stable third-party infrastructure, no fabricated "real"
  defects are reported — the template includes one clearly labeled sample entry only.

## 13. Reporting
- Newman generates an HTML report per run using `newman-reporter-htmlextra`, saved to
  `reports/api-test-report.html`.
- A high-level `docs/Test_Summary_Report.md` is maintained separately for a quick,
  human-readable pass/fail overview.
