# VitalsWatch

A simulated agent-to-equipment health monitoring demo — a monitoring agent watches a single wearable sensor, reports to a doctor agent, and escalates clearly when something looks critical. Built with the [Claude API](https://docs.claude.com).

## ⚠️ This is a simulated portfolio demo, not a real device

Every reading, the doctor agent's responses, and the "911 call" are entirely fake and generated in-browser for demonstration purposes. **Nothing is actually monitored, dialed, or contacted.** This is a prototype of an agent coordination *pattern* — not a certified medical device, and it must never be used, or relied on, for real home health monitoring or real emergencies. A real version of this concept would require certified medical-grade sensors, real telephony integration, regulatory clearance (e.g. FDA), and continuous human clinical oversight — none of which a browser demo can honestly provide.

## Live demo

**[Try it here](https://claude.ai/artifact/GixyPdQKdUj2moJWT5Tb1i)** — note: trying the interactive part requires a free Claude account to sign in with (a platform requirement for any page that calls Claude live, not specific to this project).

## The problem

Remote/in-home patient monitoring is a real, growing category — wearables that watch vitals and alert someone when something's wrong, so an at-risk person (elderly, chronically ill, recovering from surgery) can live independently with a safety net. The interesting technical problem isn't the sensor — it's the coordination: who gets notified, in what order, based on what thresholds, and what happens if it's a real emergency versus a false alarm. This project models that coordination logic directly.

## What it does

Simulates a single wrist-worn sensor (heart rate, SpO2, fall/motion detection) and:

1. **A Monitoring Agent checks in on a regular interval**, classifying status as normal, watch, urgent, or critical — a detected fall is always treated as at least critical, since an unresponsive person can't call for help themselves
2. **Reports to a Doctor Agent** whenever something's urgent or worse — a second agent that responds with its own instruction, a real agent-to-agent handoff, not just one agent talking to itself
3. **Escalates audibly** — a heartbeat tick during normal monitoring (paced to the live heart rate), switching to an alarm tone on critical status
4. **Simulates a 911 call** on critical escalation — a dialing sequence, an AI-generated automated dispatcher message, and a scripted acknowledgment — with "SIMULATED, NOT A REAL CALL" labeling repeated multiple times on screen, deliberately redundant given what this demo depicts

Four scenario buttons (Normal, Irregular Heart Rhythm, Low Oxygen, Fall Detected) let you trigger different situations and watch the full pipeline respond — including a discrete fall event versus vitals that gradually drift into concerning territory.

## Why the safety framing is part of the design, not an afterthought

A demo simulating emergency response is the one category in this portfolio where looking *too* convincing would be a real problem, not just a quality bar to clear. The repeated, high-visibility "simulated" labeling on the call screen specifically — not just a one-time disclaimer buried in a README — was a deliberate design decision, made with the understanding that this pattern could plausibly involve a vulnerable person in a real deployment.

## How it works

- A client-side simulation engine drifts fake vitals toward a scenario target over time (or triggers an instant fall event)
- One Claude API call per monitoring interval for status classification; a second call to the Doctor Agent persona when something's flagged
- A third call generates the automated dispatcher message for the simulated 911 sequence
- All audio (heartbeat, alarm) is synthesized in-browser via the Web Audio API — no audio files

## Tech

- HTML / CSS / JavaScript (single file, no build step)
- Claude API, via Anthropic's Artifact runtime
- Multi-agent handoff pattern (monitor → doctor → escalation) applied to a safety-critical simulated domain
