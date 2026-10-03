# REST API Testing using Postman

A complete, beginner-friendly but industry-style API testing portfolio project. It uses
[JSONPlaceholder](https://jsonplaceholder.typicode.com), a free public mock REST API, to
demonstrate practical QA/API testing skills with **Postman** and **Newman**.

## 1. Project Overview
This project tests a real, publicly available REST API end-to-end: HTTP methods, status
codes, headers, JSON schema, response time, positive/negative/boundary scenarios, and
CLI-based regression execution with HTML reporting. It was built as portfolio evidence
for a Trainee Software Testing / QA Analyst role.

## 2. Objectives
- Practice designing and executing real API test cases (not toy examples).
- Demonstrate JavaScript-based assertions inside Postman.
- Show positive, negative, and boundary testing thinking.
- Automate execution via Newman for CI/CD-style regression runs.
- Produce QA documentation a real team would expect: test cases, strategy, bug template,
  summary report.

## 3. API Under Test
**JSONPlaceholder** — `https://jsonplaceholder.typicode.com`
A free fake REST API used for testing/prototyping. No authentication, no real data
persistence. Endpoints used: `/posts`, `/users`, `/comments`.

## 4. Tools & Technologies
- Postman
- Newman (CLI test runner)
- newman-reporter-htmlextra (HTML reports)
- JavaScript (Postman test scripts / Chai assertions)
- Node.js & npm
- Git & GitHub

## 5. HTTP Methods Tested
GET, POST, PUT, PATCH, DELETE, and OPTIONS (in a negative test).

## 6. Test Types
Positive testing, negative testing, boundary testing, data/schema validation, header
validation, response-time validation.

## 7. Project Structure
```
api-testing-postman/
│
├── postman/
│   ├── collections/
│   │   └── API_Testing_Collection.json   # The full Postman collection (8 folders)
│   └── environments/
│       └── QA_Environment.json           # baseUrl + shared variables
│
├── test-cases/
│   └── API_Test_Cases.md                 # 33 documented test cases
│
├── test-data/
│   └── test-data.json                    # Reusable sample request payloads
│
├── reports/
│   └── .gitkeep                          # Newman HTML reports are generated here
│
├── bugs/
│   └── Bug_Report_Template.md            # Bug report format + one labeled sample
│
├── docs/
│   ├── API_Testing_Strategy.md           # Scope, approach, risks, exit criteria
│   └── Test_Summary_Report.md            # Execution summary (fill in after a run)
│
├── README.md
├── package.json
├── .gitignore
└── LICENSE
```

## 8. Postman Collection
Collection name: **REST API Testing - QA Portfolio**, organized into 8 folders:

| Folder | Contents |
|---|---|
| `01 - GET Requests` | Get all posts, get single post, get non-existent post (boundary) |
| `02 - POST Requests` | Create a new post |
| `03 - PUT Requests` | Full update of an existing post |
| `04 - PATCH Requests` | Partial update of an existing post |
| `05 - DELETE Requests` | Delete an existing post |
| `06 - Negative Tests` | Invalid endpoint, invalid ID type, empty body, invalid data type, unsupported verb, delete non-existent |
| `07 - User APIs` | Get all users, get single user (with email/address validation) |
| `08 - Comments APIs` | Get comments filtered by `postId` |

Every request has JavaScript `pm.test()` assertions attached — see
`postman/collections/API_Testing_Collection.json`.

## 9. Environment Setup
The collection uses one variable, `{{baseUrl}}`, defined in the `QA_Environment`
environment:
```
baseUrl = https://jsonplaceholder.typicode.com
```

## 10. How to Run (Postman Desktop App)
1. Open Postman → **Import** → select `postman/collections/API_Testing_Collection.json`.
2. Import → select `postman/environments/QA_Environment.json`.
3. In the top-right environment dropdown, select **QA_Environment**.
4. Open the collection → click **Run** (Collection Runner) → Run.
5. Review pass/fail results per request.

## 11. Newman Execution (CLI)
Install dependencies:
```bash
npm install
```
Run the full collection:
```bash
newman run postman/collections/API_Testing_Collection.json -e postman/environments/QA_Environment.json
```
Or use the npm script:
```bash
npm test
```

## 12. HTML Reports
Generate an HTML report using `newman-reporter-htmlextra`:
```bash
newman run postman/collections/API_Testing_Collection.json \
  -e postman/environments/QA_Environment.json \
  -r cli,htmlextra \
  --reporter-htmlextra-export reports/api-test-report.html
```
Or:
```bash
npm run test:html
```
Open `reports/api-test-report.html` in a browser to view the report.

## 13. Test Case Summary
33 documented test cases across Posts, Users, Comments, and Negative testing — see
[`test-cases/API_Test_Cases.md`](test-cases/API_Test_Cases.md) for the full table
(Test Case ID, Method, Endpoint, Scenario, Expected/Actual Result, Status, Priority).

## 14. Sample Assertions
```javascript
// Status code
pm.test('Status code is 200', function () {
    pm.response.to.have.status(200);
});

// Response time
pm.test('Response time is below 2000ms', function () {
    pm.expect(pm.response.responseTime).to.be.below(2000);
});

// JSON schema / property existence
pm.test('Post has required fields', function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('userId');
    pm.expect(jsonData).to.have.property('title');
});

// Documented actual behavior (not assumed)
pm.test('Non-existent post returns 404', function () {
    pm.response.to.have.status(404);
});
```

## 15. Bug Reporting
See [`bugs/Bug_Report_Template.md`](bugs/Bug_Report_Template.md) for the bug report
format used on this project, including one clearly labeled sample entry (JSONPlaceholder
itself has no known real defects — the sample exists to demonstrate methodology).

## 16. API Limitations (documented, not treated as bugs)
- Data created/updated/deleted via `POST`/`PUT`/`PATCH`/`DELETE` is **not actually
  persisted** — JSONPlaceholder simulates the response only.
- No server-side input validation (wrong data types, empty bodies are accepted).
- `DELETE` returns `200` with an empty object, not `204 No Content`.
- No authentication to test, by design.

## 17. Skills Demonstrated
- API Testing
- REST API
- Postman
- JavaScript Assertions
- JSON Validation
- HTTP Methods
- HTTP Status Codes
- Positive Testing
- Negative Testing
- Boundary Testing
- Test Documentation
- Newman
- Test Reporting
- Git/GitHub

## 18. Future Improvements
- Add contract/schema validation with a JSON Schema validator (`tv4` / `ajv`).
- Integrate the Newman run into a GitHub Actions CI pipeline on every push.
- Add data-driven testing using Postman's CSV/JSON data file runner.
- Extend coverage to the `/albums` and `/todos` resources.

## 19. Author
*[Your Name]* — Final-year B.Tech CSE student, preparing for a Trainee Software
Testing / QA Analyst role.
Aryan vimal 
