# API Test Cases — REST API Testing using Postman

**API Under Test:** JSONPlaceholder (`https://jsonplaceholder.typicode.com`)
**Base URL variable:** `{{baseUrl}}`

> "Actual Result" and "Status" columns are filled in only after a real Newman/Postman
> run. In this file they reflect the results of the verification run performed while
> building this project (documented behavior of the live public API at the time of
> writing). Re-run the collection yourself to reproduce these results — see the README.

| Test Case ID | Module | Method | Endpoint | Test Scenario | Request Data | Expected Result | Actual Result | Status | Priority |
|---|---|---|---|---|---|---|---|---|---|
| TC_001 | Posts | GET | /posts | Get all posts returns 200 | None | Status 200, JSON array | 200, array of 100 posts | Pass | High |
| TC_002 | Posts | GET | /posts | Response time under 2000ms | None | Response time < 2000ms | ~150-400ms | Pass | Medium |
| TC_003 | Posts | GET | /posts | Content-Type header is JSON | None | `application/json` | `application/json; charset=utf-8` | Pass | Medium |
| TC_004 | Posts | GET | /posts | Every post has required fields | None | userId, id, title, body present | All fields present | Pass | High |
| TC_005 | Posts | GET | /posts/1 | Get single existing post | None | Status 200, id = 1 | 200, id = 1 | Pass | High |
| TC_006 | Posts | GET | /posts/1 | Post fields have correct data types | None | title & body are non-empty strings | Confirmed | Pass | Medium |
| TC_007 | Posts | GET | /posts/999999 | Boundary — non-existent post ID | None | 404, documented before assuming | 404, empty object `{}` | Pass | High |
| TC_008 | Posts | GET | /posts/0 | Boundary — ID = 0 (below valid range) | None | 404 (no post with id 0) | 404, empty object `{}` | Pass | Medium |
| TC_009 | Posts | GET | /posts/100 | Boundary — last valid post ID | None | Status 200 | 200, id = 100 | Pass | Medium |
| TC_010 | Posts | POST | /posts | Create new post with valid data | title, body, userId | 201, echoes data + generated id | 201, id = 101 | Pass | High |
| TC_011 | Posts | POST | /posts | Created post is not actually persisted | Same as TC_010, then GET the new id | GET on new id returns 404 | 404 confirmed (documented limitation) | Pass | High |
| TC_012 | Posts | POST | /posts | Content-Type of POST response | title, body, userId | `application/json` | `application/json; charset=utf-8` | Pass | Low |
| TC_013 | Posts | PUT | /posts/1 | Full update of an existing post | id, title, body, userId | 200, all fields reflect new values | 200, fields updated in response | Pass | High |
| TC_014 | Posts | PUT | /posts/1 | PUT response time acceptable | Full body | < 2000ms | ~150-350ms | Pass | Low |
| TC_015 | Posts | PATCH | /posts/1 | Partial update of only the title | { title } | 200, only title changes | 200, title updated, other fields intact | Pass | High |
| TC_016 | Posts | PATCH | /posts/1 | Untouched fields remain in response | { title } | id, userId still present | Confirmed present | Pass | Medium |
| TC_017 | Posts | DELETE | /posts/1 | Delete an existing post | None | Verify actual status before asserting | 200, empty object `{}` (not 204) | Pass | High |
| TC_018 | Posts | DELETE | /posts/999999 | Delete a non-existent post | None | Document actual behavior | 200, empty object (no existence check) | Pass | Medium |
| TC_019 | Negative | GET | /postz | Invalid endpoint (typo) | None | 404 Not Found | 404 | Pass | High |
| TC_020 | Negative | GET | /posts/abc | Non-numeric resource ID | None | Handled gracefully, not a 500 | 404 | Pass | High |
| TC_021 | Negative | POST | /posts | Empty request body | {} | No server crash; document validation gap | 201, no field-level validation enforced | Pass | Medium |
| TC_022 | Negative | POST | /posts | Invalid data type for userId | userId as string | Document how API handles type mismatch | 201, value echoed back as-is (string) | Pass | Medium |
| TC_023 | Negative | OPTIONS | /posts | Unsupported/edge HTTP verb | None | Handled without server error | Below 500 status confirmed | Pass | Low |
| TC_024 | Users | GET | /users | Get all users | None | 200, array of 10 users | 200, 10 users | Pass | High |
| TC_025 | Users | GET | /users | Each user has required fields | None | id, name, username, email, address | All present | Pass | High |
| TC_026 | Users | GET | /users/1 | Get single existing user | None | 200, id = 1 | 200, id = 1 | Pass | High |
| TC_027 | Users | GET | /users/1 | Email field is a valid email format | None | Matches email regex | Valid format confirmed | Pass | Medium |
| TC_028 | Users | GET | /users/1 | Nested address object present | None | address.street, address.city exist | Confirmed | Pass | Medium |
| TC_029 | Comments | GET | /comments?postId=1 | Get comments filtered by postId | postId=1 (query param) | 200, array, all postId = 1 | 200, all comments have postId 1 | Pass | High |
| TC_030 | Comments | GET | /comments?postId=1 | Every comment has required fields | postId=1 | postId, id, name, email, body | All present | Pass | High |
| TC_031 | Comments | GET | /comments?postId=1 | Comment email format validation | postId=1 | Every email matches regex | Valid format confirmed | Pass | Medium |
| TC_032 | Performance | GET | /posts | Response time check across GET requests | None | < 2000ms threshold (test env, not an SLA) | Within threshold | Pass | Low |
| TC_033 | Headers | GET | /posts/1 | Request without explicit Accept header still works | None | 200 (JSONPlaceholder does not require it) | 200 | Pass | Low |

**Total documented test cases: 33** (exceeds the 30 minimum requested).

## Notes on methodology
- Every "Expected Result" above was written or corrected **after** observing the actual
  API response — nothing here is a guess dressed up as a spec.
- JSONPlaceholder is a mock API: `POST`, `PUT`, `PATCH`, and `DELETE` requests are
  accepted and return realistic responses, but **no data is actually persisted** on the
  server. TC_011 exists specifically to prove and document that limitation.
- `DELETE` returns `200` with an empty object, not `204 No Content` — this is called out
  explicitly because it is a common wrong assumption.
