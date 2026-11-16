# Task Manager

Implementation of `Nextjs_CRUD_Step_by_Step_Guide.pdf` in the existing
`llorando-next` project, following its SQLite schema, API routes, component,
validation, and responsive stylesheet.

## Run locally

Use Node.js 22 or later, as required by the installed SQLite driver.

```powershell
npm.cmd install
npm.cmd run dev
```

Open http://localhost:3000. `npm.cmd` works when PowerShell blocks the unsigned
`npm.ps1` launcher; `npm` works when your shell permits it.

Tasks persist in `data/tasks.db`, created automatically with the `tasks` table
and WAL journaling. The `data/` directory is ignored by Git.

## Project structure

- `lib/db.ts`: server-only SQLite connection and table initialization.
- `app/api/tasks/route.ts`: GET lists tasks newest first; POST creates a task.
- `app/api/tasks/[id]/route.ts`: GET reads one task, PATCH edits its title and/or
  completion flag, and DELETE removes it with an empty HTTP 204 response.
- `components/TaskManager.tsx`: add, edit, cancel, complete, confirm deletion,
  task totals, empty state, busy state, and errors.
- `app/page.tsx`: renders the Task Manager.
- `app/globals.css`: the guide's layout, task styles, and mobile breakpoint.

Titles are trimmed and must contain 1–120 characters. Invalid JSON, invalid
fields, and invalid IDs return HTTP 400; missing tasks return HTTP 404.

## Necessary adjustments

- Setup steps reuse the existing Next.js installation and project directory.
- Step 14 omits its GET function declaration. Its handler body is enclosed in
  `export async function GET(_request: Request, context: Context)`.
- The starter's `cacheComponents` and `partialPrefetching` settings are removed
  so the guide's `runtime` and `dynamic` route exports work in this Next.js version.
- A narrow lint exception preserves the guide's mount-time fetch. Its state
  updates occur after the asynchronous request.

The optional due date, priority, and filter challenge remains an exercise.

## Verify

```powershell
npm.cmd run lint
npm.cmd run build
npm.cmd run start
```

Stop the development server before starting production on the same port. In
another PowerShell terminal, the guide's API demonstration is:

```powershell
Invoke-RestMethod http://localhost:3000/api/tasks
Invoke-RestMethod -Method Post -Uri http://localhost:3000/api/tasks -ContentType "application/json" -Body '{"title":"API demo"}'
Invoke-RestMethod http://localhost:3000/api/tasks
```

In the browser, add `Prepare class activity`, refresh, edit and save it, mark it
complete, refresh again, then delete and confirm. Each change should persist.

This is the guide's local teaching app. File-based SQLite requires a persistent
local disk; the guide recommends managed PostgreSQL for typical cloud deployment.
