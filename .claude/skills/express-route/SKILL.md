---
name: express-route
description: Use when adding or changing an API endpoint or resource in this Express project (a new route file, a new GET/POST/PUT/DELETE handler, or its tests). Encodes how routes, store helpers, validation, error responses and tests are written here. Not for general questions or non-route code.
---

# Writing routes in this project

Follow these steps every time a route is added or changed.

1. **One router file per resource** in `routes/<resource>.js`, exporting an Express router. Mount it in `server.js` under its base path (`app.use('/things', thingsRouter)`).
2. **All data goes through `db/store.js`.** Add helper functions there (and export them); routes never hold state. Keep `reset()` restoring seed data.
3. **Validate input in the route.** Missing/invalid body fields -> `res.status(400).json({ error: 'name and email are required' })`. Missing record -> `res.status(404).json({ error: 'User not found' })`. Use `return` on every early response.
4. **Error shape is always** `{ "error": "message" }` — never plain text, never other keys.
5. **Status codes:** `200` read/update, `201` create, `400` bad input, `404` missing record.
6. **Convert path ids with `Number(req.params.id)`** before calling the store.
7. **Tests** go in `tests/<resource>.test.js` using `node:test`, `node:assert` and `supertest` against `require('../server')`, with `test.beforeEach(() => store.reset())`. Cover the happy path, the `400` case and the `404` case.
8. **Document it** in `docs/api.md` in the same style as existing endpoints.
9. Finish by running `npm test` and `npm run lint`.
