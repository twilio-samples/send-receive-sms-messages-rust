# Send & receive SMS messages with Rust

Demonstrates how to send and receive SMS messages using Twilio's Messaging API with two independent Rust applications.

## Environment Variables

Copy `.env.example` to `.env` in the `send_sms/` directory. Never commit `.env`.

```bash
cp send_sms/.env.example send_sms/.env
```

| Variable | Where to find | Format |
| -------- | ------------- | ------ |
| `TWILIO_ACCOUNT_SID` | Console homepage or Admin dropdown (top right) → Account Management → Keys & Credentials → API Keys & Tokens | Starts with `AC` |
| `TWILIO_AUTH_TOKEN` | Console homepage or Admin dropdown (top right) → Account Management → Keys & Credentials → API Keys & Tokens → click to reveal | 32-char string. Treat as a password. |
| `TWILIO_PHONE_NUMBER` | Console → Phone Numbers → Manage → Active Numbers | E.164 format: `+15551234567` |
| `RECIPIENT_PHONE_NUMBER` | The phone number you want to send the test SMS to | E.164 format: `+15551234567` |

## Commands

```bash
# Install dependencies (send SMS)
cd send_sms && cargo build

# Install dependencies (receive SMS)
cd receive_sms && cargo build

# Run send_sms (sends one SMS then exits)
cd send_sms && cargo run

# Run receive_sms (starts webhook server on port 4000)
cd receive_sms && cargo run

# Expose receive_sms webhooks locally
# Requires ngrok — install and authenticate at https://ngrok.com before running
ngrok http 4000
# Set the resulting URL + /receive/with-response as the SMS webhook in Twilio Console
```

## Project Structure

- `send_sms/src/main.rs` — CLI app that POSTs to Twilio Messages API using basic auth
- `receive_sms/src/main.rs` — Axum server exposing `/receive/no-response` and `/receive/with-response`
- `send_sms/.env.example` — template for required credentials

## Agent Boundaries

**Always:**
- Confirm `send_sms/.env` is configured before running any command
- Use the Environment Variables section to guide the user to each credential — don't ask them to find values without direction
- Confirm the app is running before asking the user to test it

**Never:**
- Run the app with missing or placeholder credentials
- Hardcode credentials or phone numbers in source files
- Skip the `cp send_sms/.env.example send_sms/.env` step

## Verify It's Working

**Send SMS:** After running `cd send_sms && cargo run`, the terminal should print `Your SMS with the body ...`. Check that the SMS arrives on the phone number set in `RECIPIENT_PHONE_NUMBER`.

**Receive SMS:** After starting `cd receive_sms && cargo run` and exposing port 4000 with ngrok, set the ngrok HTTPS URL + `/receive/with-response` as the SMS webhook in Twilio Console. Text your Twilio number (`TWILIO_PHONE_NUMBER`) the word `never gonna` — you should receive a reply with a line from "Never Gonna Give You Up".

## Twilio Resources

- [Twilio Console](https://console.twilio.com) — credentials, phone numbers, webhook configuration
- [Twilio Messaging API docs](https://www.twilio.com/docs/messaging/api)
- [TwiML for Messaging](https://www.twilio.com/docs/messaging/twiml)
