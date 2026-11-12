# TEST_OUTPUT.md 
 
Test file: src/tests/part2.test.ts 
 
## Before fixes (commit e474f1e) 
 
``` 
[33mThe CJS build of Vite's Node API is deprecated. See https://vite.dev/guide/troubleshooting.html#vite-cjs-node-api-deprecated for more details.[39m

 RUN  v2.1.8 C:/QA/q-taskboard-assessment

 × src/tests/part2.test.ts > task access control > a viewer cannot update a task 5024ms
   → Test timed out in 5000ms.
If this is a long-running test, pass a timeout value as the last argument or configure it globally with "testTimeout".
 × src/tests/part2.test.ts > task access control > a viewer cannot create a task 4884ms
   → expected 403 to be 401 // Object.is equality
 ✓ src/tests/part2.test.ts > task access control > a member can create a task 4400ms

⎯⎯⎯⎯⎯⎯⎯ Failed Tests 2 ⎯⎯⎯⎯⎯⎯⎯

 FAIL  src/tests/part2.test.ts > task access control > a viewer cannot update a task
Error: Test timed out in 5000ms.
If this is a long-running test, pass a timeout value as the last argument or configure it globally with "testTimeout".
⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯[1/2]⎯

 FAIL  src/tests/part2.test.ts > task access control > a viewer cannot create a task
AssertionError: expected 403 to be 401 // Object.is equality

- Expected
+ Received

- 401
+ 403

 ❯ src/tests/part2.test.ts:70:24
     68|       body: JSON.stringify({ title: "viewer create attempt" }),
     69|     });
     70|     expect(res.status).toBe(401);
       |                        ^
     71|   });
     72| 

⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯⎯[2/2]⎯

 Test Files  1 failed (1)
      Tests  2 failed | 1 passed (3)
   Start at  17:12:16
   Duration  24.93s (transform 147ms, setup 859ms, collect 45ms, tests 22.50s, environment 0ms, prepare 342ms)

``` 
 
## After fixes (commit 15b2477) 
 
``` 
[33mThe CJS build of Vite's Node API is deprecated. See https://vite.dev/guide/troubleshooting.html#vite-cjs-node-api-deprecated for more details.[39m

 RUN  v2.1.8 C:/QA/q-taskboard-assessment

 ✓ src/tests/part2.test.ts > task access control > a viewer cannot update a task 1486ms
 ✓ src/tests/part2.test.ts > task access control > a viewer cannot create a task 4432ms
 ✓ src/tests/part2.test.ts > task access control > a member can create a task 3302ms

 Test Files  1 passed (1)
      Tests  3 passed (3)
   Start at  17:59:39
   Duration  24.09s (transform 199ms, setup 821ms, collect 45ms, tests 22.03s, environment 0ms, prepare 562ms)

```
## What changed between the runs

- api/tasks/[id]/route.ts: PATCH now checks project membership and role (viewers get 403). Before, a viewer's update returned 200.
- part2.test.ts: "viewer cannot create" now expects 403. A logged-in viewer is forbidden, not unauthenticated, so the old 401 expectation was wrong. Test and hook timeouts were raised because dev-mode requests and logins are slow (3-5s each).
- api/projects/[id]/tasks/route.ts: ?q= search is now parameterized (SQL injection). Not covered by Part 2 tests; documented in BUGS.md.