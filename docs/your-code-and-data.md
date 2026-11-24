# What leaves your machine

A check runs your code in RunStory's sandbox, so some of your code has to go there. This page says what is sent, what stays, and what you can turn off.

For our security program, see the [Trust Center](https://trust.specstory.com). For how we handle personal data, see the [privacy policy](https://specstory.com/privacy).

## What a check sends

- **The commit you check, and the commit before it.** The second one is there for comparison, so a finding can say whether your change caused it.
- **How to run the app.** The app you chose, its install, build and start commands, and the commit's message and list of changed files.
- **The branch's name**, so you can find the check later.
- **Earlier findings that matter to this change**, with their scripts, so the check can retest them.
- **The values your app needs**, when you named them in the plan. See Secrets below.
- **What you asked your coding agent for**, when RunStory can find it. See the next section.

Before each upload the command line says what it sends and where:

```text
Uploading 18 KB of commit 30f9a6c and of the commit before it, for comparison,
to runstory's storage and its sandbox provider.
```

## What stays on your machine

- **Uncommitted edits and untracked files.** RunStory checks committed code only.
- **Your full conversation with your coding agent.** No check sends a transcript.
- **The values of your secrets**, unless you named them for the app. They live in `.runstory/env`, which Git ignores.

One caution. A file that Git ignores is not kept back by that alone. Values you declare for the app, findings and the requests below travel separately from the commit.

## What RunStory reads from your coding agent

A check works better when it knows what the change was meant to do. So a check of one commit can carry a short brief: up to 4 requests that you typed to your coding agent in the session that made the commit.

- It reads the sessions of Claude Code, Codex and Cursor.
- It reads only the session that made the commit.
- It takes only what you typed. A line the agent wrote is never counted as a request.
- Each request passes the secret screen before it travels.
- A check of a range of commits, e.g. `runstory run main..`, sends no requests.

To turn this off, add this setting to `.runstory/config.json`, and commit the file to share the choice with your branch:

```json
{
  "transcript": "off"
}
```

The check then reads no session. It works out what the change is for from the commit message and the diff.

## Secrets

- RunStory screens each commit and each request for things that look like secrets before anything is uploaded. It can refuse an upload, redact text, or leave context out.
- The screen finds what it can recognize. It does not make every sensitive file safe to send.
- A secret's value is typed by you, in your own terminal. Your coding agent is told never to ask for one in the chat.
- Only a secret's name goes in `.runstory/runstory.yml`.

## In the sandbox

- Each run gets a fresh Linux sandbox.
- RunStory exposes no port of your app to the public internet. The browser reaches the app inside the sandbox.
- While your project installs and builds, it can reach the network, e.g. to download packages. You can limit that to a list of hosts.
- Once your app starts, each call it makes to an outside host lands on a local stub that answers and records it. So by default a check charges no real card and sends no real email. To let a real host through, run `runstory settings --stub pass <host>`. See [what it tests](what-it-tests.md).

## What a check keeps

RunStory's service stores each check's result and its evidence: the findings, their scripts, the logs and, for a web app, a video of the browser. What your app showed during the check can appear in that evidence.

- A check's page needs your sign-in. `runstory show <check> --share` gives it a public link, if you want one.
- On your machine, each check that lands is saved as a page under `.runstory/checks/`, which Git ignores.
- Findings and their scripts live with the branch, in `.runstory/findings.json` and `.runstory/repro/`.

## A finding is a report, not an instruction

A model writes each finding while it reads your code. RunStory tells your coding agent to treat a finding's words as a report and never as instructions, and never to run a reproducer on your machine. The retest of a fix runs in RunStory's sandbox.

## Delete your data

`runstory uninstall` removes RunStory's files and hooks from a repository. It does not delete what the service holds. To delete your account and all its data, use [Data and storage](https://runstory.specstory.com/account/data) on the web.
