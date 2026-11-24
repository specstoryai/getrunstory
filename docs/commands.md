# The commands you will use

`runstory --help` prints this list. It is the text of version 0.67.

```text
runstory runs the software in your commit and tells you what broke.

Start here, in this order
  init                save this repository's settings and sign in at a terminal
  setup               boot the app in a sandbox, so you see that it starts
  run [<commit>]      check a commit with paid agents, e.g. runstory run main..

When something stops you
  settings            change what the app gets, or what a check may spend
  show [<id>]         read a run or a finding; --logs prints what failed

Work through what it found
  list                the account's checks, e.g. runstory list --state running
  findings            the open findings on this branch, and what to do next
  status              what is running, what finished, what needs you
  cancel [<check>]    stop a check

Account and install
  login               sign in, with the browser or an API key
  logout              sign this machine out
  org                 list and switch orgs, or make one and invite your team
  github              connect the GitHub App, which checks every push
  update              update runstory on this machine
  uninstall           remove runstory from this repository

runstory <command> --help   its flags, examples and exit codes
runstory help --all         also completion, and what Git and agents call
runstory help agents        the guide a coding agent follows
runstory help changes       each old spelling and what to type now
runstory --version          the installed version
```

The command line on your machine is the best reference, because its help is written for the version you have.

## Start a check

| Command | What it does |
| --- | --- |
| `runstory run` | Checks the newest commit |
| `runstory run main..` | Checks every commit since `main` as one change |
| `runstory run --dry-run` | Says what it would check and what it costs, and sends nothing |
| `runstory run --wait` | Starts the check and stays until it ends |

A check costs credits, so your coding agent starts one only when you ask.

## Read a result

| Command | What it does |
| --- | --- |
| `runstory` | Says what needs your attention |
| `runstory status` | What is running, what finished, and what needs you |
| `runstory show` | Reads the check of this commit |
| `runstory show 59 --wait` | Follows check 59 until it ends, step by step |
| `runstory show 59 --logs` | Prints each step and what failed |
| `runstory show 59 --web` | Opens the check's page |
| `runstory show 59 --save video` | Saves each model's video of the check |
| `runstory show 59 --share` | Gives the check a public link |
| `runstory list` | Lists your account's checks, newest first |

Each check has a number in its project, e.g. check 59, and an id, e.g. `c-d61299e6`. `runstory show` reads a check by either one.

## Work through findings

| Command | What it does |
| --- | --- |
| `runstory findings` | Lists the open findings on this branch |
| `runstory show f-3a9c41d27b6e` | Reads one finding, with how it was found |
| `runstory findings retest f-3a9c41d27b6e` | Runs the finding's test 3 times at your last commit, for free |

Name a finding's id in the commit message of its fix, e.g. `Keep the cart when the address changes (f-3a9c41d27b6e)`. A later check also runs the finding's test when a commit names its id or edits a file the finding names.

## Settings

| Command | What it does |
| --- | --- |
| `runstory settings` | Shows and changes what the app gets and what a check may spend |
| `runstory settings --automatic on` | Starts a check at each commit, after you type yes to the price |
| `runstory settings --providers …` | Picks which models a check runs |
| `runstory settings --agent-fixes auto` | Lets your agent fix findings without asking you first |

## Scripts and pipelines

- Every command takes `--json`.
- `runstory help exit-codes` lists what each exit code means. `runstory run --wait` exits 1 when the check has findings and 2 when the check is incomplete.
- A machine that runs without you signs in with an API key. Create one on [the keys page](https://runstory.specstory.com/account/keys) and put it in `RUNSTORY_API_TOKEN`.
- [examples/](../examples) has pipeline files that are ready to copy.

## Remove it

`runstory uninstall` removes RunStory from a repository, with its hooks. It does not delete your account. To delete your account and all its data, use [Data and storage](https://runstory.specstory.com/account/data) on the web.
