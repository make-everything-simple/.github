
# Workflow

A workflow is a configurable automated process made up of one or more jobs. You must create a YAML file to define your workflow configuration.

## Workflow Template

Create a standardized set of workflow templates specifically for your organization. Organization members can then reuse the templates when creating new workflows in the organizations' repositories to take time for coffee.

## How To

1. [Creat a workflow template](https://docs.github.com/en/actions/configuring-and-managing-workflows/sharing-workflow-templates-within-your-organization#creating-a-workflow-template)
2. [Use a workflow template](https://docs.github.com/en/actions/configuring-and-managing-workflows/sharing-workflow-templates-within-your-organization#using-a-workflow-template)

## Workflow syntax for GitHub Actions

- [Lean YAML in five minutes](https://www.codeproject.com/Articles/1214409/Learn-YAML-in-five-minutes)
- [YAML syntax for workflows](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobs)
- [Environment](https://docs.github.com/en/actions/configuring-and-managing-workflows/using-environment-variables)

```yml
name: Workflow Example

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  pull_request:
    branches: [ develop, qc, uat ]
  push:
    branches: [ master ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  job1:
    runs-on: ubuntu-latest
    # Map a step output to a job output
    outputs:
      output1: ${{ steps.step1.outputs.test }}
      output2: ${{ steps.step2.outputs.test }}
    steps:
    - id: step1
      run: echo "::set-output name=test::hello"
    - id: step2
      run: echo "::set-output name=test::world"
    - id: step3
      env:
        MY_VAR: Hi there! My name is
        FIRST_NAME: Mona
        MIDDLE_NAME: The
        LAST_NAME: Octocat
      run: |
        echo $MY_VAR $FIRST_NAME $MIDDLE_NAME $LAST_NAME.
  job2:
    runs-on: ubuntu-latest
    needs: job1 # dependence on job1
    steps:
    - run: echo ${{needs.job1.outputs.output1}} ${{needs.job1.outputs.output2}}
```
