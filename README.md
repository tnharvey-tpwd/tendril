# Tendril Alpha

A zero-build, browser-local prototype of the Tendril personal stewardship system.

## What is included

- Quick Capture with optional source metadata
- Clarify flow with 1 to 3 processing results
- Actions, waiting status, poke dates, and follow-up creation
- Threads and resume notes
- People, relationship entries, and encouragement logging
- Today and Operations lenses
- Daily Review and Weekly Stewardship Review walkthroughs
- IndexedDB local storage
- JSON backup and restore
- Installable PWA shell and offline cache

## Run locally

A service worker and IndexedDB work best when served over HTTP rather than by opening the file directly.

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Publish on GitHub Pages

Upload the contents of this folder to a repository and configure GitHub Pages to deploy from the repository root. No build step is required for this alpha.

## Important alpha limitations

- Data does not synchronize between browsers or devices.
- Reviews are workflow prototypes, not yet fully interactive decision engines.
- Areas, Roles, and Commitments are reserved for the next development pass.
- Recurrence, calendar integration, and external automations are not included.
- This prototype intentionally avoids external libraries so it can run in restricted environments.

## Architecture direction

The prototype uses native IndexedDB behind small storage functions. A production build can migrate these stores to the planned React/TypeScript architecture and formal storage provider without changing the core domain concepts.
