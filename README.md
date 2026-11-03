# Frontstep Coder (showcase)

Public showcase for a private project. The source code is not published here.

Frontstep is a pipeline that finds small Brazilian businesses without a website and builds them a preview site automatically. Frontstep Coder is the builder part: it turns a short brief about a business into a finished preview site, checked by a quality gate before anyone sees it.

## The problem

Many local businesses (law firms, accountants, bakeries, florists, small industry) have no site, or a poor one. A custom site is too expensive to build speculatively by hand. The question was whether an AI builder could produce sites good enough that the owner says yes when shown a finished preview, without a human in the loop for each build.

## How it works

End to end, the pipeline is:

1. **Find.** Pick a niche and a city, look through local business listings, and keep businesses with no site, a mobile number, and enough rating and reviews.
2. **Reach out.** Small daily batches, a first message over WhatsApp, inside allowed hours.
3. **Handle replies.** Each reply is sorted as yes, no, opt-out or question.
4. **Brief.** For a yes, a brief is put together for that business and its niche.
5. **Build.** An AI builder writes the site from the brief and must pass the quality gate.
6. **Preview.** The site goes online at a preview link, which goes back to the owner over WhatsApp.
7. **Sale, payment and a short post-sale form.**

This project is step 5.

## Principles

- **Rules per niche.** What a site for a law firm needs is different from a florist site: required sections, tone, and the kind of visuals that fit.
- **A quality gate that can reject.** Sites are checked for things like missing content, copy that is too long, weak fonts and photo use. Some problems block the site, others only warn. The gate was tuned on real builds.
- **Avoid the "AI look".** Generic layouts and stock patterns are checked against.
- **A fact check.** A separate pass compares the text on the site against the brief.
- **The factory keeps running.** A new check must never take the whole pipeline down.
- **No repeated work.** Each build is recorded so nothing is built twice.

## Status

Running since October 2026. The pipeline builds and sends previews. See [frontstep](https://github.com/7thcapitalist/frontstep) for the public project overview.

## What is shown and what is kept private

Only this README is public. The source is private because it holds the outreach logic, lead handling and files made from real businesses.
