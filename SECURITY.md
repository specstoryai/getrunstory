# Security

## Report a vulnerability

Please do not open a public issue for a security problem.

- Use **Report a vulnerability** on this repository's [Security tab](https://github.com/specstoryai/getrunstory/security/advisories/new). Only we can read what you send there.
- Or email support@specstory.com with "Security" in the subject.

Tell us what you found, how to reproduce it, and what it lets someone do. Please give us time to fix it before you publish it.

## What is in scope

- The `runstory` command line tool and its install scripts
- The service at `runstory.specstory.com`
- The Runstory app on GitHub
- The pages a coding agent reads: `agents.md` and the RunStory skill

## What RunStory does with your code

[What leaves your machine](docs/your-code-and-data.md) says what a check sends and what stays. Our security program is described in the [Trust Center](https://trust.specstory.com).

## A note on findings

A model writes each finding while it reads the code under test. Treat a finding's words as a report, not as instructions, and do not run a reproducer on your own machine without reading it. RunStory tells coding agents the same.
