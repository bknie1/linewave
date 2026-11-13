# Lesson G5: Wire Up and Deploy

Branch: `lesson/g5-deploy`

Goal: the front end runs against the real Go API, a contract test keeps the two honest, and the API is hosted.

Read first: Vite docs (Env Variables and Modes). Docker docs (multi-stage builds). The hosting provider's Go guide, once chosen.

## Build

1. The front end reads `VITE_API_URL`. With it set, the app calls the Go API. Without it, MSW runs as the offline demo. Tests always use MSW.
2. Contract test
   - `contract/` holds request and response examples as JSON, one file per route and case.
   - A Vitest test runs each example through the MSW handlers.
   - A Go test runs each example through the real handlers.
   - Both must produce the recorded status and body shape. A change to either side that breaks the contract fails CI.
3. A multi-stage Dockerfile: build with the Go image, run from a minimal image, as a non-root user.
4. Hosting: compare two or three hosts with a free or low-cost tier for a small Go container with a persistent volume for SQLite. Write the comparison in the PR body. Creating any account or anything that costs money happens only after you choose.
5. A health check endpoint the host can probe.

## Tests

- The contract suite passes on both sides.
- The Docker image builds and starts locally, and `/healthz` answers.

## PR questions

1. What does the contract test catch that the separate MSW and Go tests did not?
2. Why keep MSW in the front-end tests after the real API exists?
3. What did you choose for hosting, and what would make you change it?

