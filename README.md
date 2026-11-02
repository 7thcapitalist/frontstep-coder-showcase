# Frontstep Coder (showcase)

Public showcase for a private project. The source code is not published here.

Frontstep is a pipeline that finds small Brazilian businesses without a website and builds them a preview site automatically. Frontstep Coder is the builder part: a Claude Code skill, a worker service and a quality gate that turn a brief into a deployed preview.

## The problem

Many local businesses (law firms, accountants, bakeries, florists, small industry) have no site, or a poor one. A custom site is too expensive to build speculatively by hand. The question was whether an AI builder could produce sites good enough that the owner says yes when shown a finished preview, without a human in the loop for each build.

## How it works

End to end, the pipeline is:

1. **Capture.** A workflow takes a niche and a city, scans a maps listing, and keeps businesses with no site, a mobile number, and enough rating and reviews.
2. **Outreach.** Limited daily batches, first message over WhatsApp, inside allowed hours.
3. **Reply handling.** Replies are classified as yes, no, opt-out or question.
4. **Brief.** For a yes, a brief is assembled, with a visual effect and a premium feature chosen by business niche.
5. **Build.** The worker runs Claude Code headless with the skill. The result must pass the QA gate.
6. **Deploy and preview.** The worker deploys to a preview URL, and the link goes back over WhatsApp.
7. **Sale, payment, post-sale form.**

This project is step 5.

## Design notes

- **The skill is mostly written rules.** It defines an 8-step process, then reference files per topic: niche rules (mandatory sections per business type), creative direction, typography and banned fonts, copy budget, motion (what to animate and what not), signature effects, and anti-"AI look" checks. The headless builder reads files instead of calling other skills, because skills it cannot reach would silently not run.
- **A QA gate that can reject.** About 46 checks cover things like empty CSS, a missing portrait for one-person businesses, copy length, fonts, hero choice and photo use. Some are hard failures, others warnings. A calibration set and an audit of recent builds were used to tune it, including finding a false negative (a pattern that missed one niche's spelling).
- **Fail open, but quietly.** If the gate cannot run, it exits with a distinct code and the worker continues. A new feature must never take the factory down.
- **Restricted tools.** The build agent runs with an allowlist of tools, never with permission bypass.
- **Push, not poll.** The automation plan has a monthly execution cap, so nothing polls it.
- **Idempotence and durable reports.** The worker records builds, avoids duplicate work and keeps reports that survive restarts. It also checks the live deployment after publishing and checks Lighthouse results.
- **Judge step.** A separate pass checks facts in the generated copy against the brief.
- **Secrets stay out of the repo.** They live in the service environment on the machine, never in version control.

## Architecture

n8n workflows (capture, outreach, replies, brief, preview, sale) push jobs to a Node worker on a small always-on Linux machine, managed by systemd. The worker invokes Claude Code with the skill and a hook that records which skills were used, runs the gate, deploys to Vercel and reports back. Sites are built from a per-niche Astro starter. An install script backs up, syntax-checks and copies the repo to production without restarting the service.

## Stack

Node.js, Astro, Claude Code (headless), n8n, Vercel, a WhatsApp API gateway, systemd, Lighthouse, Playwright.

## Status

Running on a mini PC since October 2026 (about 70 commits). The pipeline builds and sends previews. I am not claiming paying customers or revenue here.

## What is shown and what is kept private

Only this README is public. The source is private because it holds the outreach logic, the lead-handling flow, calibration files made from real businesses, and production operations scripts.
