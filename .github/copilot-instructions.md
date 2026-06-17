# GitHub Copilot Instructions

This file provides guidance to GitHub Copilot when working with code in this repository.

## Repository overview

- **Source code**: `send_sms` and `receive_sms` contain the implementation.
- **Documentation**: README.md contains the project's documentation.
- **PR template**: `.github/PULL_REQUEST_TEMPLATE/pull_request_template.md` describes the information every PR must include.

## Project knowledge

### Repository structure

When suggesting file paths or navigation, follow this structure:

```bash
send-receive-sms-messages-php/
.
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md             # A GitHub template for reporting bugs
│   │   ├── feature_request.md        # A GitHub template for requesting new features
│   │   └── question.md               # A GitHub template for asking questions about the project
│   ├── PULL_REQUEST_TEMPLATE/
│   │   └── pull_request_template.md  # A GitHub template for creating pull requests
    └── copilot-instructions.md       # This file
├── AGENTS.md                         # Project guidance for most Agents, except for Claude and Copilot
├── CLAUDE.md                         # Project guidance for Claude Code
├── CONTRIBUTING.md                   # Instructions for contributing to the project
├── LICENSE.md                        # The project's license (MIT)
├── README.md                         # Main landing page with table of contents
├── send_sms                          # A small Rust application that shows how to send an SMS
└── receive_sms                       # A small Rust application that shows how to reply to an SMS
```

### Tech stack

- Cargo
- Rust

## Prerequisites

To run the app, the following is required:

- Rust and Cargo
- [ngrok][ngrok] and a free ngrok account
- A [Twilio account][twilio_signup] with an active phone number that can send SMS

## Set up instructions

### Send an SMS

In the _send_sms_ directory:

1. Rename the `.env.example` file to `.env`
1. Go to the [Twilio Console][twilio_console] and find your **Account SID**, **Auth Token**, and Twilio phone number.
1. Copy and paste those values into the placeholders in the `.env` file `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, and `SENDER`, respectively.
1. Set your phone number, in [E.164 format][e164_format] as the value of `RECIPIENT` in _.env_
   Save the file.

### Receive an SMS

1. Start your ngrok server:

   ```bash
   ngrok http 8080
   ```

1. Go to the [Active numbers][active_numbers] page in the Twilio Console.
1. Click your Twilio phone number.
1. Go to the **Configure** tab and find the **Messaging Configuration** section.
1. In the **A call comes in** row, select the **Webhook** option.
1. Paste your ngrok **Forwarding** URL in the **URL** field followed by "/receive/".
   For example, if your ngrok console shows Forwarding "<https://1aaa-123-45-678-910.ngrok-free.app>", enter "<https://1aaa-123-45-678-910.ngrok-free.app/receive/>".
   - To receive an SMS **without** responding to it, append "no-response" to the URL
   - To receive an SMS and respond to it, append "with-response" to the URL
1. Click **Save configuration**.
1. Start the Rust web app

   ```bash
   cargo run
   ```

## Commands you can use

**Lint the documentation:** `markdownlint-cli2 README.md`
**Check the code:**

- Check the _send_sms_ app: `cd send_sms && cargo clippy`
- Check the _receive_sms_ app: `cd receive_sms && cargo clippy`

**Run the code:**

- Run the _send_sms_ app: `cd send_sms && cargo run`
- Run the _receive_sms_ app: `cd receive_sms && cargo run`

## Boundaries

- ✅ **Always do:** Follow the style examples, run `cargo clippy` for Rust source files
- ⚠️ **Ask first:** Before modifying existing files in a major way
- 🚫 **Never do:** Modify code in `send_sms/` or `receive_sms`, edit config files, commit secrets

## Pull request expectations

PRs should use the template located at `.github/PULL_REQUEST_TEMPLATE/pull_request_template.md`.
Provide a summary, test plan and issue number if applicable, then check that:

- New tests are added when needed.
- Documentation is updated.
- The full test suite passes.

Commit messages should be concise and written in the imperative mood.
Small, focused commits are preferred.

## What reviewers look for

- Tests covering new behaviour.
- Consistent style: code formatted with [Clippy][cargo-clippy] and use statements sorted.
- Clear documentation for any public API changes.
- Clean history and a helpful PR description.

[active_numbers]: https://console.twilio.com/us1/develop/phone-numbers/manage/incoming
[cargo-clippy]: https://doc.rust-lang.org/stable/clippy/usage.html
[e164_format]: https://www.twilio.com/docs/glossary/what-e164
[ngrok]: https://ngrok.com/
[twilio_console]: https://console.twilio.com
[twilio_signup]: https://www.twilio.com/try-twilio
