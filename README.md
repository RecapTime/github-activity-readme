# GitHub Activity in Readme

Updates a GitHub user's `README.md` with the recent GitHub activity. The only difference with this fork is Node.js 16.x compartibility
and default `user.name` and `user.email` for the generated commit.

<img width="735" alt="profile-repo" src="https://user-images.githubusercontent.com/25279263/87703301-3aa4a500-c7b8-11ea-8eb6-245121997a7b.png">

> **Remember that using this fork will commit your updated README as the [Recap TIme Bot](https://github.com/RecapTimeBot).**
> That account is managed by [Recap Time Squad](https://recaptime.eu.org) and will count it as an contribution to your README repo.
> If you want, add that account to your collaborators to allow us debug issues.

---

## Instructions

**NOTE**: We recommend to have an `your-username/your-username` repo on your personal GitHub account and not on your org (they're unsupported by design).

- Add the comment `<!--START_SECTION:activity-->` (entry point) within `README.md`. You can find an example
[here](https://github.com/ajhalili2006/blob/ajhalili2006/main/README.md).

- It's the time to create a workflow file called `.github/workflows/update-readme.yml`. The above job runs every half an hour, you can change it as you
wish based on the [cron syntax](https://jasonet.co/posts/scheduled-actions/#the-cron-syntax).
You can find an example [here](https://github.com/ajhalili2006/ajhalili2006/blob/ajhalili2006/main/.github/workflows/pull-gh-activity.yml).


```yml
name: GitHub activity feed to README

on:
  schedule:
    - cron: '* */2 * * *' # 30 minus to probably too much for API ratelimits

jobs:
  generate-feed:
    runs-on: ubuntu-latest
    name: Generate feed

    steps:
      - uses: actions/checkout@v2

      # Pin the version to the latest stable version.
      # If you want to live on edge, use 'master' branch.
      - uses: RecapTime/github-activity-readme@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

The above job runs every two hours, you can change it as you wish based on the [cron syntax](https://jasonet.co/posts/scheduled-actions/#the-cron-syntax).

Please note that only those public events that belong to the following list show up:

- `IssueEvent`
- `IssueCommentEvent`
- `PullRequestEvent`

You can find an example [here](https://github.com/jamesgeorge007/jamesgeorge007/blob/master/.github/workflows/update-readme.yml).

### Override defaults

Use the following `input params` to customize it for your use case:-

| Input Param | Default Value | Description |
|--------|--------|--------|
| `COMMIT_MSG` | :zap: Update README with the recent activity | Commit message used while committing to the repo |
| `MAX_LINES` | 5 | The maximum number of lines populated in your readme file |


```yml
name: Update README

on:
  schedule:
    - cron: '*/30 * * * *'
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    name: Update this repo's README with recent activity

    steps:
      - uses: actions/checkout@v2
      - uses: jamesgeorge007/github-activity-readme@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          COMMIT_MSG: 'Specify a custom commit message'
          MAX_LINES: 10
```

_Inspired by [JasonEtco/activity-box](https://github.com/JasonEtco/activity-box)_
