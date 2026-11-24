# RunStory

RunStory is an AI QA agent. It builds and starts your web app or CLI from a commit, uses it the way a customer would, and hands each failure it can repeat back to your coding agent.

It is in beta, and your first 10 checks are free.

[Website](https://specstory.com/runstory) · [Docs](https://specstory.com/docs/runstory) · [How it compares](https://specstory.com/runstory/compare) · [Pricing](https://specstory.com/runstory/pricing) · [Sign in](https://runstory.specstory.com/sign-in)

## Set up

Paste this into your coding agent. It works in Claude Code, Codex and Cursor.

```text
Set up runstory for me. Fetch https://runstory.specstory.com/agents.md and follow it.
Ask me before spending money. I'll handle sign-in and secrets myself.
```

Your agent runs each step itself. It stops when you need to:

- approve the sign-in in your browser
- say yes before the first check uses your credits
- install the GitHub App, if you want it

The prompt sends your agent to one page. You can [read that page first](agents/agents.md).

### Or install it yourself

On macOS and Linux:

```sh
curl -fsSL https://runstory.specstory.com/install.sh | sh
```

On Windows, in any terminal:

```powershell
powershell -ExecutionPolicy Bypass -c "irm https://runstory.specstory.com/install.ps1 | iex"
```

Then, in your repository:

```sh
runstory init     # save this repository's settings and sign in
runstory setup    # boot the app in a sandbox, so you see that it starts
runstory run      # check a commit with paid agents
```

The installer checks the download against a signed manifest, uses no sudo, and puts `runstory` in `~/.local/bin`. The Windows build is not signed yet, so Smart App Control in enforcement mode blocks it. Both scripts are in [install/](install) if you want to read them before you run them.

## What happens

1. **You sign in, and your app boots.** RunStory reads the install, build and start commands from your repository and boots the app in a sandbox. You see it start before anything is checked.
2. **You commit. A check starts.** A check can start at each commit, when you or your agent run `runstory run`, at a push through the GitHub App, or as a step in your pipeline.
3. **RunStory builds your app and uses it.** In its own sandbox, it builds the commit and starts it. Two agents, each on a different frontier model, work through what you asked for the way a customer would. For a web app, that is a real browser.
4. **It keeps only what it can repeat.** A failure counts once its script fails 3 more times in a row and a second agent confirms it. Then the same script runs on the commit before yours. If it passed there, your change caused it.
5. **Your agent gets it, and asks you first.** At the end of its turn, your coding agent receives the finding: what was expected, where to look, and the script. It shows you and waits for your answer before it changes any code.
6. **The fix is retested, for free.** Your agent commits a fix that names the finding. RunStory runs the finding's script again, 3 times, in the sandbox. Three passes close it.

You write no tests and set up no preview deployment. [What to expect](docs/what-to-expect.md) shows each step with the text the command line prints.

## What a result looks like

```text
1 problem found
check 59 · 30f9a6c, the change from 7be29d3, with 1 file changed
https://runstory.specstory.com/runs/d61299e6-4e82-4811-934e-3a99d3c48f8f

Claude Opus 5.5  1 finding
OpenAI Sol       nothing

F1  The cart is empty after the delivery address changes during checkout
    f-3a9c41d27b6e · found by Claude Opus 5.5
    Before this change  passed on 7be29d3

Next: runstory show f-3a9c41d27b6e   read F1, with how it was found
```

Each finding comes with a script under `.runstory/repro/` that reproduces it. The check's page on the web adds each agent's account of what it did and, for a web app, a video of the browser.

## What it tests

| | In the beta | Not yet |
| --- | --- | --- |
| Apps | Web apps, command line tools | Desktop apps, mobile apps, browser extensions |
| Where your app runs | A Linux sandbox | macOS, Windows, iOS, Android |
| Languages | Node, Python, Go, Ruby, Java and Kotlin, .NET, Rust, PHP | |
| Services it starts for your app | PostgreSQL, MySQL and MariaDB, MongoDB, Redis, S3 storage, SMTP, RabbitMQ, Kafka | |
| Coding agents | Claude Code, Codex, Cursor | |
| Git hosts | GitHub, GitLab, Bitbucket, or no host at all | |

The command line tool itself runs on macOS, Linux and Windows. A check runs on the models you pick: Claude Opus 5.5, Codex Sol 6.1 and Gemini 4 Neon. A default check runs 2 of them side by side. [What it tests](docs/what-it-tests.md) has the details.

## It reports on GitHub too

Install the Runstory app on a repository, and each push gets a Check on its commit once the commit's CI passes. The Check lists each finding, and it shows on the pull request. RunStory posts no comments. See [the GitHub App's checks](docs/github-checks.md).

## Price

Your first 10 checks are free, and they never expire. After that, plans start at $39 a month. Retesting a fix is free, and so is booting your app the first time. See [pricing](https://specstory.com/runstory/pricing).

## In this repository

RunStory is a hosted service, and its source is not here. This repository holds what you and your agent read, and it is where you tell us what went wrong.

| Where | What is there |
| --- | --- |
| [docs/what-to-expect.md](docs/what-to-expect.md) | Setup, a check, a finding and a fix, with the text the command line prints |
| [docs/what-it-tests.md](docs/what-it-tests.md) | The apps, languages and services a check supports, and what it does not support yet |
| [docs/commands.md](docs/commands.md) | The commands you will use |
| [docs/github-checks.md](docs/github-checks.md) | The GitHub App, and what each Check title means |
| [docs/your-code-and-data.md](docs/your-code-and-data.md) | What leaves your machine for a check, and what stays |
| [agents/](agents) | The page your agent follows during setup, and the rules it keeps |
| [skills/runstory](skills/runstory) | The RunStory skill for coding agents |
| [install/](install) | Copies of the 2 install scripts |
| [examples/](examples) | Pipeline files for GitHub Actions and GitLab CI/CD |
| [CHANGELOG.md](CHANGELOG.md) | What changed that you will notice |

The workflow guides are on the website: [find your workflow](https://specstory.com/docs/runstory/workflows).

## Tell us what went wrong

RunStory is in beta, and reports from real projects are what improve it.

- [Something went wrong](https://github.com/specstoryai/getrunstory/issues/new?template=bug.yml) in the command line, the web app or the GitHub App.
- [RunStory could not boot my app.](https://github.com/specstoryai/getrunstory/issues/new?template=setup.yml)
- [A finding was wrong, or RunStory missed a bug.](https://github.com/specstoryai/getrunstory/issues/new?template=finding.yml)
- [I want RunStory to test something it does not test yet.](https://github.com/specstoryai/getrunstory/issues/new?template=request.yml)

For a quick question, ask in [Discord](https://discord.gg/E47yQyEUd3). To report a security problem, see [SECURITY.md](SECURITY.md).

## From the makers of SpecStory

[SpecStory](https://github.com/specstoryai/getspecstory) saves the prompts and decisions from your coding agent sessions. RunStory reads what you asked for and checks that the software does it. Each works without the other.
