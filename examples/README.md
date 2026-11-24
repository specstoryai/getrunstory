# Examples

Files you can copy to check each pull request in a pipeline. The command line and the GitHub App need none of these. Use them when you want the check to be a step that can fail a job.

| File | Copy it to | For |
| --- | --- | --- |
| [github-actions/runstory.yml](github-actions/runstory.yml) | `.github/workflows/runstory.yml` | GitHub Actions on Linux |
| [github-actions/runstory-windows.yml](github-actions/runstory-windows.yml) | `.github/workflows/runstory.yml` | GitHub Actions, when your CI runs on Windows |
| [gitlab-ci/.gitlab-ci.yml](gitlab-ci/.gitlab-ci.yml) | `.gitlab-ci.yml` | GitLab CI/CD |

## Before you copy one

1. Run `runstory init` and `runstory setup` once on your own machine, and commit `.runstory/runstory.yml`. That file tells the job how to build and start your app.
2. Create an API key on [the keys page](https://runstory.specstory.com/account/keys). A key made while an org is selected acts in that org, and the org pays for its checks.
3. Save the key as the secret `RUNSTORY_API_TOKEN`. On GitHub:

   ```sh
   gh secret set RUNSTORY_API_TOKEN --repo owner/name
   ```

   On GitLab, store it as a masked variable with Protected off.

If your app needs secret values, pass each one in the job's `env`, next to the key.

## What the job does

- It fetches the whole Git history, so RunStory can find where the branch left `main`.
- It checks every commit the branch has that `main` does not, as one change.
- `runstory run --wait` exits 1 when the check has findings, so the job fails. It exits 2 when the check is incomplete.
- `--timeout 75m` covers a check of 60 minutes and the minutes a wait adds.

To make the job a gate on GitHub, require its check, named `check`, in the branch rule for `main`.

## Any other pipeline

A job needs 4 things: the whole Git history, the `runstory` command, the key in `RUNSTORY_API_TOKEN`, and a range to check.

```sh
curl -fsSL https://runstory.specstory.com/install.sh | sh
export PATH="$HOME/.local/bin:$PATH"
runstory run origin/main..HEAD --wait --timeout 75m
```

Always name the range. A plain `runstory run` checks only the newest commit.

The website has the full guide: [check every pull request before it merges](https://specstory.com/docs/runstory/workflows/merge-gate).
