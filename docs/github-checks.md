# The GitHub App's checks

The command line checks commits on your machine. The GitHub App checks each push. You can use one or both.

GitHub shows the App and its Check under the name **Runstory**.

## Connect it

Only you can do these 4 steps, because they happen in your GitHub account.

1. Open [your repositories on RunStory](https://runstory.specstory.com/account/repositories).
2. Press **Connect GitHub**.
3. Install the Runstory app on the repository.
4. Press **Set up checks**.

The App reads the repository's contents and metadata, and it writes checks. It posts no comment and no review on a pull request.

## When a push is checked

Each push to any branch gets one check, once the commit's CI passes. If CI fails, RunStory spends nothing on that push.

GitHub shows the Check on the commit and on its pull request.

## What each title means

| Title | What happened |
| --- | --- |
| `Waiting for CI` | RunStory starts after the other checks on this commit pass |
| `Checking with 2 agents` | The check is running |
| `No confirmed problems` | The check ended and found nothing it could repeat |
| `1 problem found` | The check ended with 1 confirmed finding, and the Check fails |
| `No new problems, 8 still open` | Nothing new, and 8 findings from earlier checks still stand |
| `Not checked because nothing that runs changed` | The push changed nothing that runs, e.g. only a README, so RunStory spent nothing. Press **Re-run** to check it anyway |
| `Not checked because CI did not pass` | Another check on the commit failed, so RunStory spent nothing |
| `Not checked because CI did not finish` | The other checks on the commit did not finish |
| `Not checked because a bot pushed this commit` | A bot made the push |
| `Not checked because automatic checks are off` | Checks of pushes are turned off for this repository |
| `Not checked until setup is fixed` | Setup needs fixing before a check can run |

## What a failed Check says

The summary lists each finding with its id, says how to fix and retest it, and says what the check cost.

```text
1 problem found

- f-3a9c41d27b6e The cart is empty after the delivery address changes during checkout · passed on the commit before

Put a finding's id in the commit message of its fix, and the next check retests it. Run `runstory show f-3a9c41d27b6e` to read the first one in full.

Runstory checked this commit, after the build and test checks passed.

The check used 6 credits, 3 for each agent.
```

## Not on GitHub?

You do not need the App. A check from the command line uploads the commit itself, so it works with GitLab, Bitbucket, or no host at all. To check each merge request in a pipeline, see [examples/](../examples).
