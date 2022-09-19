# 🔨 Hands-on: Reusable workflows

In this hands-on lab you will create a reusable workflow and a workflow that consumes it.

This hands on lab consists of the following steps:
- [Creating a reusable workflow](#creating-a-reusable-workflow)
- [Adding an output parameter](#adding-an-output-parameter)


## Creating a reusable workflow

1. Create a [new file](/../../new/main) `.github/workflows/reusable.yml` (paste the file name with the path in the box).
2. Set the name to `Reusable workflow`.

<details>
  <summary>Solution</summary>
  
```YAML
name: Reusable workflow
```
  
</details>

3. Add a `workflow_call` trigger with an input parameter `who-to-greet` that is required.

<details>
  <summary>Solution</summary>
  
```YAML
  workflow_call:
    inputs:
      who-to-greet:
        description: 'The person to greet'
        type: string
        required: true
        default: World
```
  
</details>

4. Add a job named `reusable-job` that runs on `ubuntu-latest` that echos "Hello <input parameter>" to the console. 

<details>
  <summary>Solution</summary>
  
```YAML
jobs:
  reusable-job:
    runs-on: ubuntu-latest
    steps:
      - name: Greet someone
        run: echo "Hello ${{ inputs.who-to-greet }}"
```
  
</details>

## Adding an output parameter

1. Add an additional step that uses a workflow command to set an output parameter 
named `current-time` to the current time (use `$(date)` to get current time).

<details>
  <summary>Solution</summary>
  
```YAML
      - name: Set time
        id: time
        run: echo "::set-output name=current-time::$(date)"
```
  
</details>

2. Add an output parameter called `current-time` to `workflow_call` and set it to the outputs of the workflow command.

<details>
  <summary>Solution</summary>
  
```YAML
    outputs:
      current-time:
        description: 'The time when greeting.'
        value: ${{ jobs.reusable-job.outputs.current-time }}
```
  
</details>


<details>
  <summary>Complete Solution</summary>
  
```YAML
name: Reusable workflow

on: 
  workflow_call:
    inputs:
      who-to-greet:
        description: 'The person to greet'
        type: string
        required: true
        default: World
    outputs:
      current-time:
        description: 'The time when greeting.'
        value: ${{ jobs.reusable-job.outputs.current-time }}
        
jobs:
  reusable-job:
    runs-on: ubuntu-latest
    outputs:
      current-time: ${{ steps.time.outputs.current-time }}
    steps:
      - name: Greet someone
        run: echo "Hello ${{ inputs.who-to-greet }}"
      - name: Set time
        id: time
        run: echo "::set-output name=current-time::$(date)"
```
  
</details>

## Summary

In this lab you have learned to create and protect environments in GitHub and use them in a workflow. You have also learned to conditionally 
execute jobs or steps and to chain jobs using the `needs` keeword.

You can continue with the [README](../README.md#part-4--cicd-and-automation).
