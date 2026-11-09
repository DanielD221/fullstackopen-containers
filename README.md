# Full Stack Open — Containers

Coursework for the containers section of [Full Stack Open](https://fullstackopen.com/en/). The repository contains a React todo interface, an Express/MongoDB backend, optional Redis connectivity, and Playwright end-to-end test scenarios.

This is an unfinished exercise repository. Dockerfiles, a Compose file, and an nginx configuration are not currently committed, although the CI workflow refers to them.

## Repository layout

| Directory | Contents |
| --- | --- |
| `answers/` | Written exercise answers |
| `todo-app/todo-backend/` | Express API, Mongoose model, and Redis client |
| `todo-app/todo-frontend/` | React and Vite todo interface |
| `todo-tests/` | Playwright UI and API test scenarios |
| `.github/workflows/` | Draft container-based E2E workflow |

## Run the backend locally

Requires Node.js 24, npm, and MongoDB. Redis is optional.

```sh
cd todo-app/todo-backend
npm ci
```

Set these environment variables in your shell or hosting configuration before starting:

| Variable | Purpose |
| --- | --- |
| `MONGO_URL` | MongoDB connection URI |
| `REDIS_URL` | Optional Redis connection URI |
| `PORT` | HTTP port; defaults to `3000` |

For example, in a POSIX shell:

```sh
MONGO_URL=mongodb://127.0.0.1:27017/todos PORT=3000 npm start
```

The backend mounts its todo routes at `/todos`.

## Run the frontend locally

In a second terminal, from the repository root:

```sh
cd todo-app/todo-frontend
npm ci
```

Create `.env.local` in that frontend directory:

```dotenv
VITE_BACKEND_URL=http://localhost:3000
```

```sh
npm run dev
```

Open the URL Vite prints. `npm run build` generates the frontend's `dist/` output.

## Current limitations

- The backend implements list and create routes. Single-item GET and PUT return `405`, so marking a todo as done is not implemented end to end.
- The delete route calls a document method that needs review for compatibility with the installed Mongoose version.
- Redis connectivity exists, but the statistics endpoint expected by the tests is absent.
- The Playwright suite expects an app at `http://localhost:8080` with API routes under `/api`; that reverse-proxy setup is not included.
- The E2E workflow runs on pushes to `xmain` and `xmaster`, not the default `main` branch, and refers to missing Docker/Compose files.

## Test scenarios

Once the expected app and proxy setup are available:

```sh
cd todo-tests
npm install
npx playwright install chromium
npm run test:e2e
```

The suite covers page loading, todo creation, completion interactions, and API expectations. These scenarios describe the intended behavior; they are not a claim that the unfinished implementation passes them.
