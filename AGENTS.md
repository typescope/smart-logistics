# Smart Logistics development

This is a Jo application using Harpe on Python.
This file guides development. Runtime instructions belong in
[prompts/watch.md](prompts/watch.md) and [prompts/plan.md](prompts/plan.md).
Treat runtime prompts and skills as application content.

## Commands

Run commands from the project root. See [README.md](README.md) for configuration.
Activate the environment in each shell before installing dependencies or running Jo.

- Create a virtual environment if missing: `python3 -m venv .venv`.
- Activate it: `. .venv/bin/activate`.
- Install dependencies: `python -m pip install -r requirements.txt`.
- Build the application: `jo build agent`.
- Build the watcher sandbox: `jo build --spec sandbox/watch/jo.toml guest`.
- Build the planner sandbox: `jo build --spec sandbox/plan/jo.toml guest`.
- Run the web application after configuration: `jo start`.
- Run tests: `jo run tests`. They use an offline model and need no provider API key.

Run the smallest affected check first. Run the suite after behavior changes.
Build each affected sandbox when changing its contract or implementation.
Inspect the diff before handing off. Report checks run and any that could not run.

## Development conventions

- Prefer colon call syntax for multiline and nested calls.
- Keep watcher writes limited to warnings and planner writes limited to drafts.
  Order approval and submission stay outside both guest APIs.
- Enforce supplier, case size, capacity, and duplicate checks in the trusted runtime.
  Prose checks guide the model and need human review.
- Keep the stock ledger append-only. Record corrections as new movements.
  Derive stock balances from the ledger.
- Update the schema and add a migration when changing existing tables.
  Follow [migrations/README.md](migrations/README.md). Keep applied migrations immutable.
- Keep Python FFI out of the API and guest modules.
  Expose database access through capabilities implemented in the trusted runtime.
- Change capability APIs, runtime bindings, and the `runTask` placeholders together.
  Update affected prompt and skill examples.
- Preserve loopback binding and host checks. The application has no login.
- Do not edit generated `.build/` output.

## References

- [Jo documentation](https://jo-lang.org/): syntax, capabilities, and build commands.
- [Jo GitHub](https://github.com/typescope/jo): implementation and tests.
- [Harpe documentation](https://harpe.typescope.ai/): concepts and extension patterns.
- [Harpe GitHub](https://github.com/typescope/harpe): APIs, examples, and tests.

Use local examples first. Consult documentation and source when unsure.
Check [jo.toml](jo.toml), [sandbox/watch/jo.toml](sandbox/watch/jo.toml), and
[sandbox/plan/jo.toml](sandbox/plan/jo.toml) for declared versions.
Use `jo.lock` files for resolved package versions.
Read source at the matching release tag or commit.
Current documentation and `main` may describe newer APIs.
