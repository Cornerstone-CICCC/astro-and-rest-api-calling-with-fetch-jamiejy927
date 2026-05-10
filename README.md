# WAD202 - Posts Manager

**Welcome!**
This challenge will help you practice REST APIs, the Fetch API, and Astro by building a small frontend that talks to a real running backend.

## The Challenge

Build a **one-page Astro app** that calls every endpoint of the Express backend in this repo using `fetch`. The backend exposes a `/posts` resource with full CRUD support. Your users should be able to:

- <strong>View the full list of posts.</strong>
- <strong>View a single post by id.</strong>
- <strong>Create a new post by filling out a form.</strong>
- <strong>Replace a post entirely (PUT) by filling out a form.</strong>
- <strong>Partially update a post (PATCH) by filling out a form.</strong>
- <strong>Delete a post by id.</strong>
- <strong>See the server's response (or error) on the page after every action.</strong>

## Requirements

- One Astro page under `src/pages/` that calls **all six** endpoints listed in the [API](#api) section.
- For **GET** endpoints, a button is enough — no input fields required.
- For **POST**, **PUT**, and **PATCH** endpoints, provide **input fields** so the user can type the body values.
- For **DELETE**, an input for `id` plus a button.
- Display the server's response (status + JSON) on the page after every action. Show error messages too (400, 404, etc.).
- Use Tailwind CSS for styling.
- Use `fetch` with `async/await`. No external HTTP libraries.

## Getting started

1. Start the backend in one terminal:
   ```
   cd backend && npm run dev
   ```
   It runs at `http://localhost:3001` and has CORS enabled.
2. Start the Astro app in another terminal (`cd client && npm run dev`).
3. Test each endpoint in **Postman first** — see [postman.html](postman.html).
4. Wire up one endpoint at a time. Watch the backend terminal — every request prints a log so you can confirm it landed.

## Hints

- Always set `Content-Type: application/json` and `JSON.stringify(...)` the body when sending POST / PUT / PATCH.
- `response.json()` returns a Promise — `await` it.
- Read `response.status` and show it to the user. It's the fastest way to see if your call worked.
- If the backend terminal doesn't log your request, it never reached the server (check the URL and the port).

<hr>

# API

The backend lives in [backend/src/index.ts](backend/src/index.ts) and runs on `http://localhost:3001`. **Do not modify the backend** — treat it as a black box. Full Postman walkthrough: [postman.html](postman.html).

| Method | URL | Body | Purpose |
|---|---|---|---|
| GET | `/posts` | — | List all posts |
| GET | `/posts/:id` | — | Get one post |
| POST | `/posts` | `title`, `body`, `userId` | Create a post |
| PUT | `/posts/:id` | `title`, `body`, `userId` (all) | Replace a post |
| PATCH | `/posts/:id` | any subset | Partial update |
| DELETE | `/posts/:id` | — | Delete a post |

### Fetch — GET (no body)

```js
const response = await fetch("http://localhost:3001/posts");
const data = await response.json();
console.log(response.status, data);
```

### Fetch — POST / PUT / PATCH (with body)

```js
const response = await fetch("http://localhost:3001/posts", {
  method: "POST", // or "PUT" / "PATCH"
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    title: "My new post",
    body: "Hello from the frontend",
    userId: 1,
  }),
});

if (response.ok) {
  const data = await response.json();
  // do something with data
} else {
  console.error("Fetch error:", response.status, response.statusText);
}
```

### Fetch — DELETE

```js
const response = await fetch("http://localhost:3001/posts/1", {
  method: "DELETE",
});
const data = await response.json();
```

### Status codes

| Code | When |
|---|---|
| `200` OK | Successful GET / PATCH / DELETE / PUT (when the post existed) |
| `201` Created | Successful POST, or PUT against a new id |
| `400` Bad Request | POST or PUT missing a required field |
| `404` Not Found | GET / PATCH / DELETE against an id that doesn't exist |

<hr>

## Notes

- The backend stores posts **in memory only**. Restarting it resets to the seed data — that's expected.
- The requirements above are the floor, not the ceiling. Stretch goals: auto-refresh the list after a write, confirm before DELETE, validate forms client-side, replace prompts with inline edit forms.
