# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

Two independent Rust applications demonstrating Twilio SMS messaging:

- `send_sms/` — CLI app that POSTs to the Twilio Messages REST API using `reqwest` with form-encoded parameters and basic auth.
  Reads credentials and phone numbers from a `.env` file via `dotenvy`.
- `receive_sms/` — Axum web server (port 4000) that handles Twilio webhook POST requests.
  Exposes two routes: `/receive/no-response` (returns empty TwiML) and `/receive/with-response` (responds with a TwiML `<Message>` element).

## Commands

**Lint documentation:** `markdownlint-cli2 README.md`

**Check code:**

- `cd send_sms && cargo clippy`
- `cd receive_sms && cargo clippy`

**Run:**

- `cd send_sms && cargo run`
- `cd receive_sms && cargo run`

## Boundaries

- Always run `cargo clippy` after changing Rust source files.
- Ask before making major changes to existing files in `send_sms/` or `receive_sms/`.
- Never commit secrets or modify `.env` files.

## Code style

Follows the [Rust Style Guide](https://doc.rust-lang.org/style-guide/). Keep `use` statements sorted. Comments must be complete sentences ending with a period.

## Commits and PRs

- Commit messages follow the [Chris Beams style](http://chris.beams.io/posts/git-commit/): imperative mood, concise, small focused commits.
- PRs use `.github/PULL_REQUEST_TEMPLATE/pull_request_template.md`.
  Every PR must answer: what changed, why, breaking changes, and any coordinated server PR.
