# anthropics/launch-your-agent — 7/10

**Date:** 2026-07-03
**Source URL:** https://github.com/anthropics/launch-your-agent
**Score:** 7/10
**Category:** Official Anthropic skill (anthropics/ org, Apache 2.0)

## What it is

An official Anthropic Claude Code skill that takes you from a rough idea to a fully deployed Managed Agent in a single session. Four stages: Interview (turns your idea into a concrete build sheet), Stage/Launch (fires the API calls to deploy it as a Managed Agent), Grade/Iterate (builds an eval scaffold with test cases), Schedule (sets up recurring cron). 633 stars, 120 forks, from the `anthropics/` GitHub org — this is Anthropic's own reference implementation.

## Why you'd want it (specific to your stack)

Your current path from "I want a new Jarvis skill" to "it's running on a cron as a Managed Agent" involves multiple manual steps that you redesign each time. This skill standardizes that path to one command — and the eval scaffold it generates directly helps you test whether the new agent actually works before you rely on it for overnight novel runs or digest collection.

## Why I think it's worth your attention

This is Anthropic's own answer to "how do you go from idea to production agent" — reading the stages gives you their mental model for what a well-deployed agent looks like, independent of whether you install the skill itself.

## What to do

Read the README at the link below to see the 4-stage workflow. If you want to try it: it installs as a standard Claude Code skill and requires a Managed Agents subscription. Given you already run Jarvis as a scheduled CCR agent, the "Schedule" stage is the most directly applicable part.

🔗 https://github.com/anthropics/launch-your-agent
