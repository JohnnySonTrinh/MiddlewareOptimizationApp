Middleware Optimization App — a friendly middleware sandbox
=========================================================

This small ASP.NET Core minimal app is a playground for exploring middleware behavior. It's purposely simple and easy to run so you can try ordering, short-circuiting, header/cookie behavior, and basic input validation without extra infrastructure.

Why I built this
-----------------
- To learn and demonstrate how middleware ordering affects request handling (for example: when responses are written, when cookies/headers are available, and how short-circuiting works).
- To have a lightweight testbed for quick experiments and unit/integration tests.
- To show common pitfalls (e.g., writing to response before headers, Secure cookies on HTTP, unsafe input handling).

What this project does
----------------------
- Listens on localhost:5294 (HTTP) using Kestrel for quick local testing.
- Middleware pipeline includes:
  - Logging middleware: prints a simple security event when the response status is >= 400.
  - Simulated HTTPS enforcement: expects `?secure=true` in the query string.
  - Input validation: rejects non-alphanumeric input or input containing `<script>`.
  - Short-circuit middleware for `/unauthorized` that returns 401.
  - Authentication simulation: expects `?authenticated=true`, and appends a cookie when authenticated.
  - An asynchronous middleware that writes a short line and simulates async work.
  - A final terminal middleware that sends a default response if nothing else did.

Quick start — run locally
-------------------------
Prerequisites: .NET SDK 9.x (or whichever SDK the project targets). Get it at https://dotnet.microsoft.com/download

1. Clone the repository:

   git clone https://github.com/JohnnySonTrinh/MiddlewareOptimizationApp.git
   cd MiddlewareOptimizationApp

2. Build and run the app:

   dotnet build
   dotnet run

3. Try requests from another terminal (examples):

   # Missing secure param -> should be rejected by simulated HTTPS enforcement
   curl -i "http://localhost:5294/"

   # Secure but not authenticated -> rejected by auth middleware
   curl -i "http://localhost:5294/?secure=true"

   # Secure and authenticated -> should set a cookie and return processed messages
   curl -i "http://localhost:5294/?secure=true&authenticated=true"

   # Unauthorized path (short-circuited)
   curl -i "http://localhost:5294/unauthorized?secure=true&authenticated=true"

How I plan this project — roadmap
---------------------------------
This repo is intentionally minimal. Here's a simple plan I use to extend it or turn it into a teaching repo:

MVP (current):
- Minimal app with middleware examples (this repository).

Next steps (short term):
- Add unit tests for each middleware component using the ASP.NET Core TestServer or WebApplicationFactory.
- Add integration tests that run the app and assert behavior (responses, cookies, logs).
- Add better input validation and demonstrate common sanitizers.

Stretch goals (later):
- Add a small web UI to visualize middleware pipeline and live requests.
- Add a Dockerfile + docker-compose for consistent environments.
- Create a set of lesson notebooks or markdown lessons for teaching middleware concepts.

Contributing and fork/clone
---------------------------
1. Fork this repository on GitHub.
2. Clone your fork:

   git clone https://github.com/<your-username>/MiddlewareOptimizationApp.git
   cd MiddlewareOptimizationApp

3. Create a branch, make changes, and open a PR:

   git checkout -b feat/your-feature
   # make changes
   git add .
   git commit -m "Describe your change"
   git push origin feat/your-feature

4. Open a Pull Request against `main` and add a short description of what you changed and why.

Notes and recommendations
-------------------------
- This project runs over HTTP locally for simplicity. If you switch to HTTPS, set `CookieOptions.Secure = true` so cookies are only sent over secure connections.
- The input validation shown is intentionally simple; use a proper sanitizer or stricter validation rules for real applications.
- If you want, I can add tests, CI (GitHub Actions), or a Dockerfile — tell me which and I'll scaffold them.

License
-------
No license is included by default. Add a LICENSE file if you want to allow reuse and contribution.

Contact / Questions
-------------------
Open an issue or a PR on GitHub and I'll take a look. I'm happy to help expand the lessons or make this a small guided tutorial.

