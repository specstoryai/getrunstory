# What it tests

RunStory is in beta. This page says what a check supports today and what it does not support yet.

## At a glance

| | In the beta | Not yet |
| --- | --- | --- |
| Apps | Web apps, command line tools | Desktop apps, mobile apps, browser extensions |
| Where your app runs | A Linux sandbox | macOS, Windows, iOS, Android |
| Languages | Node, Python, Go, Ruby, Java and Kotlin, .NET, Rust, PHP | |
| Services it starts for your app | PostgreSQL, MySQL and MariaDB, MongoDB, Redis, S3 storage, SMTP, RabbitMQ, Kafka | |
| Coding agents | Claude Code, Codex, Cursor | |
| Git hosts | GitHub, GitLab, Bitbucket, or no host at all | |

If RunStory does not test your app yet, [tell us what you want it to test](https://github.com/specstoryai/getrunstory/issues/new?template=request.yml).

## Apps

Setup decides what kind of app you have from how it boots.

| Kind | How setup knows | What a check's agents do |
| --- | --- | --- |
| Web app | Its start opens a port that answers | They drive it in a recorded browser |
| Command line tool | Its boot opens no port | They run its commands in a shell |

An app that builds only for macOS or Windows cannot run, because every sandbox is Linux on x64. The command line tool `runstory` itself runs on macOS, Linux and Windows.

## The sandbox

Each run gets a fresh Linux x64 sandbox with 8 GiB of memory. After a project's first run, later runs start from that project's saved snapshot.

RunStory exposes no port of your app to the public internet. The browser reaches the app inside the sandbox.

## Languages and tools

Setup finds the toolchain for Node, Python, Go, Ruby, Java and Kotlin, .NET, Rust and PHP from your repository's own files. It reads, for example, `engines.node` and `.nvmrc`, `go.mod`, `.python-version`, `Gemfile`, `global.json`, `rust-toolchain.toml`, `composer.json` and the `FROM` line of a `Dockerfile`. It also reads how you run the app, e.g. from a development compose file or a `Procfile`.

`runstory setup --plan` prints the commands it would use and why, before anything runs:

```text
This is a web app, because npm run dev runs next, a web framework's dev server (package.json line 1).
These commands are guesses from your files, in the order a run uses them.
Install       npm install
Build         npm run build
Start         npm run dev, or if that fails, npm start
```

## Services

RunStory starts a fresh stand-in for each service your app needs, inside the sandbox: PostgreSQL, MySQL, MariaDB, MongoDB, Redis, S3 compatible storage, SMTP, RabbitMQ and Kafka. Supabase's own local stack can start too.

Once your app starts, every other call to an outside host lands on a local stub that answers and records it.

- It answers `api.stripe.com` and `api.openai.com` in each vendor's documented shape.
- It keeps an email your app sends, so the agent can read the code or the link in it.
- It answers any other host with 200 and an empty object.

To let a real host through, run `runstory settings --stub pass <host>`.

## Signing in to your app

Setup finds your app's sign-in page and picks how a check gets past it. A check can sign up a test person through the app's own pages, or use a test login that you give it.

## Secrets

A secret's value stays in `.runstory/env` on your machine, which Git ignores. Only its name goes in `.runstory/runstory.yml`. You type the value in your own terminal. A check from the GitHub App takes its values from the repository's settings page.

Every commit is screened for secrets before it is uploaded.

## Models

A check runs 1 to 3 agents, each on a different model. You pick which.

| Model | From | Notes |
| --- | --- | --- |
| Claude Opus 5.5 | Anthropic | Runs in a default check |
| Codex Sol 6.1 | OpenAI | Runs in a default check. The command line prints it as `OpenAI Sol` |
| Gemini 4 Neon | Google | Runs when you name it |

On the Free plan a check runs 1 agent, Claude Opus 5.5 by default. `runstory settings` changes which models a check runs.

## Time

A check runs for up to 60 minutes by default. You can set the limit from 15 to 120 minutes. The longest is 60 minutes on Free and 120 on Pro and Team.

## Coding agents

RunStory hands findings to Claude Code, Codex and Cursor at the end of the agent's turn. It also reads what you asked the agent for in the session that made the commit, so the check knows what the change was meant to do. [What leaves your machine](your-code-and-data.md) says exactly what is read and how to turn it off.

Two limits today:

- Codex runs its hook only after you approve it with `/hooks` in Codex.
- In Cursor on Windows the hook gives the agent nothing, so the agent reads findings with `runstory findings`.

A commit you make by hand is checked too. The agents then work out what the change is for from the commit itself.

## Git hosts

A check from the command line uploads the commit itself, so it does not depend on where the repository is hosted. It works with GitHub, GitLab, Bitbucket, or no host at all. The [GitHub App](github-checks.md) adds one more way to start a check, which is a push to a repository on GitHub.
