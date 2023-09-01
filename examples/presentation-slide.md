# GitHub Action
<img src="https://1000logos.net/wp-content/uploads/2021/05/GitHub-logo.png" width="150" height="100">
<img src="https://avatars.githubusercontent.com/u/44036562?s=280&v=4" width="150" height="150">

### Prajwol Amatya
---
## Contents
- Introduction
- Components of GitHub Action
- Workflows
- Events
- Jobs
- Actions
- Runners
---
## Introduction
- continuous integration and continuous delivery (CI/CD) platform
- allows to automate your build, test and deployment pipeline
- lets you run workflows that build and test every pull request to your repository
- or deploy merged pull requests to production
- for example, every time someone creates a pull request for a repository, you can automatically run a command that executes a software testing script
---
## Components of GitHub Action
1. Workflows
2. Events
3. Jobs
4. Steps
5. Actions
6. Runners

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1629613850021/Hzlju3ctU.png?auto=compress,format&format=webp">
---
## Workflows
- configurable automated process that will run one or more jobs
- defined by YAML file
- runs when triggered by an event
- defined in the <mark style="font-weight:bold;background:rgba(0,0,0,0);">.github/workflows</mark> directory
- a repository can have multiple workflows
---
## Example Workflow
```yml [1|3-7|9|10|11|12-32|13-17|19-20|22-26|28-29|31-32|34-51|35|50-51]
name: CI

on:
  push:
    branches:
      - master
  pull_request:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Cancel previous runs
        uses: styfle/cancel-workflow-action@0.11.0
        with:
          all_but_latest: true
          access_token: ${{ github.token }}

      - name: Checkout
        uses: actions/checkout@v2

      - name: Setup NodeJs
        uses: actions/setup-node@v3
        with:
          node-version: '16'
          cache: 'npm'

      - name: Install dependencies
        run: npm install

      - name: Run ESLint
        run: npm run lint

  e2e:
    needs: lint # run sequentially
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
          cache: 'npm'

      - name: Install dependencies
        run: npm install

      - name: E2E Tests
        run: npm run test:e2e
```
---
## Events
- specific activity that triggers a workflow
- activity can be originated from commit push or opening an issue or creating pull request
---
## Jobs
- set of steps
- workflow with multiple jobs will run those jobs in parellel
- can also be configured to run jobs sequentially
---
## Steps
- individual task that runs commands in a job
- can be either an action or a shell command
- each step in a job executes on the same runner, allowing the actions in that job to share data with each other
---
## Actions
- standalone commands that are combined into steps to create a job
- smallest portable building block of a workflow
- you can create your own actions, or use actions created by GitHub community
- to use an action in a workflow, you must include it as a step
---
## Runners
- server that has the GitHub actions runner application installed
- you can use a runner hosted by GitHub, or you can host your own
- a runner listens for available jobs, runs one job at a time, and reports the progress, logs, and results back to GitHub
---
# Thank you!