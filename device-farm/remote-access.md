---
type: Concept
title: "Remote Access"
description: "Reaching into the pen yourself: gesture, swipe, and interact with a real device in real time, straight from your browser."
tags: [aws, device-farm, remote-access, manual-testing, appium]
sources:
  - id: aws-device-farm-remote-access
    resource: https://aws.amazon.com/device-farm/
    title: AWS Device Farm — Remote Access
  - id: aws-device-farm-appium-endpoint
    resource: https://docs.aws.amazon.com/devicefarm/latest/developerguide/appium-endpoint.html
    title: AWS Device Farm Developer Guide — Appium endpoint (client-side testing)
status: stable
generated:
  by: claude-code/sonnet-5
  at: 2026-09-15T00:00:00Z
---

# Remote Access — Reaching Into the Pen Yourself

## 🕹️ The mnemonic

Automated testing is the ranch hand walking the whole herd through the same course at once. **Remote Access** is you, personally, reaching into one pen and walking one animal on a leash — in real time, from your browser, on a real physical device sitting in an AWS data center.

## What it's for

Remote Access gives you an interactive session on a single real device: gestures, swipes, taps, and text input stream to the device live, and its screen streams back to you. There's no script to write. Use it when:

- You need to **manually reproduce** a bug a customer reported on one specific device/OS combination
- You want to **exploratory test** a new feature by hand before automating anything
- You need to inspect exactly what a real user would see — a permissions dialog, a keyboard layout, an OEM skin's quirks — that a script wouldn't catch

**Mnemonic:** *"Automated testing tells you a thousand animals ran the course; Remote Access lets you feel exactly what the fence felt like."*

## Client-side Appium testing, through an Appium endpoint

The rest of Device Farm's automated testing is **server-side execution** — you upload your app and tests, and Device Farm runs them in parallel across many devices using its own service-managed test hosts (see [Automated Testing Frameworks](automated-testing-frameworks.md)). Remote Access offers a complementary path built specifically for Appium users: every Remote Access session exposes the device through a **managed Appium endpoint** — an Appium server URL, scoped to that one device for the life of that one session.

Appium itself is a client-server framework: a local client sends commands to an Appium server, which drives the device through a platform driver (UiAutomator2 for Android, XCUITest for iOS), with every command following the **W3C WebDriver** standard. Device Farm's Appium endpoint puts the *server* half of that pair inside your Remote Access session, so your Appium *client* — running on your own machine, in whatever Appium client environment you already use — can connect straight to it.

Because the endpoint stays valid for the whole session, you can iterate against the same real device repeatedly — run a test, tweak a locator, run it again — with no re-setup cost between attempts. That's the practical payoff of **client-side execution**: fast local feedback loops, instead of a full upload-and-run cycle for every change.

**Mnemonic:** *"Server-side: the farm's own ranch hand runs the whole course for you, across every animal in the herd. Client-side: you're handed one animal on a leash and an open gate — walk it yourself, from your own laptop, as many times as you like."*

## Starting a session, in practice

Every run or session in Device Farm — automated or Remote Access — lives inside a **Project**, the container that holds your app uploads and run history. To open a Remote Access session from the console:

1. Create (or open) a **Project** under Mobile Device Testing.
2. On the **Remote access** tab, choose **Create remote access session**.
3. Pick a device from the list (or search for one), and give the session a name.
4. Optionally attach an app to the session — your own upload, or the built-in **Device Farm Sample App**. App uploads are scoped to the project and **expire after 30 days**, so an upload from last month may need re-uploading.
5. Optionally configure an **HTTP/S device proxy** under Advanced Configuration, applied for the duration of the session.
6. Confirm, and the session starts against a real device.

**Mnemonic:** *"Every visit to the farm starts by checking in at the project's front gate — pick your animal, name your visit, and bring your own gear (app) if the farm's sample tools won't do."*

The same flow is scriptable through the AWS CLI:

```bash
# 1. Find a device ARN (look for "remoteAccessEnabled": true)
aws devicefarm list-devices

# 2. Start a session against that device, inside your project,
#    optionally attaching your app and any auxiliary apps
aws devicefarm create-remote-access-session \
  --project-arn "$PROJECT_ARN" \
  --device-arn "$DEVICE_ARN" \
  --app-arn "$APP_ARN" \
  --configuration '{"auxiliaryApps": ["'"$AUXILIARY_APP_ARN"'"]}'

# 3. Poll until the session status flips to RUNNING
aws devicefarm get-remote-access-session \
  --arn "$SESSION_ARN" \
  --query 'remoteAccessSession.status' --output text
```

A freshly created session starts in `PENDING`; poll `get-remote-access-session` until it reports `RUNNING` (or bail out if it reports `STOPPING`/`COMPLETED` early) before you connect to it.

## Automated and manual, together

The two modes aren't a choice between one or the other — they solve different problems:

| | Automated testing | Remote Access |
|---|---|---|
| Scale | Every device in a pool, in parallel | One device, one session |
| Scripting | Required (or built-in Fuzz) | None — or client-side Appium if you want scripting |
| Best for | Regression coverage, CI/CD gates | Manual exploration, bug reproduction |

A typical workflow: automated tests across the whole pool catch *that* something broke on one particular device; Remote Access lets you sit down at that exact device and see *why*.

## Next up

There's a second, entirely separate stable on this farm — one for testing web apps in desktop browsers, not mobile devices: [Desktop Browser Testing](desktop-browser-testing.md).

[^aws-device-farm-remote-access]: AWS Device Farm, "Remote Access," aws.amazon.com/device-farm.
[^aws-device-farm-appium-endpoint]: AWS Device Farm Developer Guide, "Appium endpoint (client-side testing)."
