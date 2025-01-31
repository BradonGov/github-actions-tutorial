# github-actions-tutorial
## GitHub Actions Overview
### What is GitHub actions: 
GitHub Actions allows you to **automate your build, test, and deployment pipeline.** You can create workflows that **build and test every pull request to your repository**, or **deploy merged pull requests to production**. GitHub Actions lets you **run workflows when other events happen in your repository**.

### Components of GitHub Actions
You can configure workflow to start when an event occurs in your repository, such as a pull request. Your workflow has one or more jobs it runs (sequentaly or in parallel). Each job will run inside its own virtual machine runner (or container). Each job has one or more steps where it either runs scripts or run an action.

### Workflow
A workflow is a **configurable automated process** that will run one or more jobs. Workflows are a **YAML** file that's inside your repository and **will run when triggered by an event in your repository**, or they can be **triggered manually**, or **at a defined schedule**.

Workflows are in the ```.github/workflows``` directory in a repository. A repository can have multiple workflows, each of which can perform a different set of tasks such as:

1. Building and testing pull requests
2. Deploying your application every time a release is created
3. Adding a label whenever a new issue is opened

### Events
An **event** is a **specific activity** in a repository that **triggers a workflow** run. For example, an activity can originate from GitHub when someone creates a pull request, opens an issue, or pushes a commit to a repository.

### Jobs
A **job** is a **set of steps** in a workflow that is executed on the same runner. Each step **is either a shell script that will be executed**, or **an action that will be run**.

### Actions
An action is a custom application for the GitHub Actions platform that performs a **complex but frequently repeated task**. Use an action to **help reduce the amount of repetitive code** that you write in your **workflow files**.

### Runners
A runner is a server that runs your workflows when they're triggered. Each runner can run a single job at a time.

## Workflow syntax For Github Actions
Github actions uses YAML (serialisation language designed to be writable and readable) to create the workflows  
### Common syntax rules for YAML: Based on this [article](https://learnxinyminutes.com/yaml/) 
```YAML
# Comments in YAML look like this.

key: value
another_key: Another value goes here.
a_number_value: 100

# The number 1 will be interpreted as a number, not a boolean. 
# If you want it to be interpreted as a boolean, use true.
boolean: true
null_value: null
another_null_value: ~
key with spaces: value

# Yes and No (doesn't matter the case) will be evaluated to boolean 
# true and false values respectively.
# To use the actual value use single or double quotes.
no: no            # evaluates to "no": false
yes: No           # evaluates to "yes": false
not_enclosed: yes # evaluates to "not_enclosed": true
enclosed: "yes"   # evaluates to "enclosed": yes

# Notice that strings don't need to be quoted. However, they can be.
however: 'A string, enclosed in quotes.'
'Keys can be quoted too.': "Useful if you want to put a ':' in your key."


# Special characters must be enclosed in single or double quotes
special_characters: "[ John ] & { Jane } - <Doe>"

# Multiple-line strings.
# Literal block turn every newline within the string into a literal newline (\n).
literal_block: |
  This entire block of text will be the value of the 'literal_block' key,
  with line breaks being preserved.

  The literal continues until de-dented, and the leading indentation is
  stripped.

# Nesting uses indentation. 2 space indent is preferred (but not required).
a_nested_map:
  key: value
  another_key: Another Value
  another_nested_map:
    hello: hello

# Maps don't have to have string keys.
0.25: a float key

# Sequences (equivalent to lists or arrays) look like this
# (note that the '-' counts as indentation):
a_sequence:
  - Item 1
  - Item 2
  - 0.5  # sequences can contain disparate types.
  - Item 4
  - key: value
    another_key: another_value
  - - This is a sequence
    - inside another sequence
  - - - Nested sequence indicators
      - can be collapsed

# Since YAML is a superset of JSON, you can also write JSON-style maps and
# sequences:
json_map: { "key": "value" }
json_seq: [ 3, 2, 1, "takeoff" ]
```

### Common YAML syntax for github actions
Based on this: [GitHub Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions)
| Syntax | Definition | More Info | Ex. |
|----------|----------|----------|----------|
| ```name``` | The name of the workflow.  |  `name: Update Test Server`   | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#name) |
| ```run-name``` | The name for workflow runs generated from the workflow. | ```run-name: Deploy to ${{ inputs.deploy_target }} by @${{ github.actor }}```   |  [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#run-name)  |
| ```on``` |To automatically trigger a workflow, use on to define which events can cause the workflow to run.  |  ```on: push```, ```on: [push, pull]```   | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#on)  |
| ```env``` | A map of variables that are available to the steps of all jobs in the workflow.  | <pre>env:<br/> &nbsp; SERVER: production</pre> | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#env) |
| ```jobs``` | A workflow run is made up of one or more jobs, which run in parallel by default. |    | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobs) |  
| ```jobs.<job_id>``` | Use jobs.<job_id> to give your job a unique identifier. The key job_id is a string and its value is a map of the job's configuration data. |   <pre>jobs: <br>  job1:<br>    name: My first job<br>  deployment:<br>    name: Deploy to Development Server </pre>   | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#) |  
| ```jobs.<job_id>.runs-on``` | define the type of machine to run the job on. |   <pre>jobs: <br>  job1:<br>    name: 1st job<br>    runs-on: ubuntu-latest </pre> | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idruns-on) |  
| `shell` | Use shell to define the shell for a step. (see link for use cases/context)  |  `shell: powershell`, `shell: cmd`, `shell :sh`   | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_iddefaultsrunshell) |   
| ```defaults``` | Use defaults to create a map of default settings that will apply to all jobs in the workflow.  |     | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#defaults) |
| ```defaults.run``` | You can use defaults.run to provide default shell and working-directory options for all run steps in a workflow.  |  <pre>defaults:<br/>  run:<br/>    shell: bash <br/>    working-directory: ./scripts </pre>   | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#defaultsrun) |
| ```working-directory``` | Specify the working directory of where to run the command.   | <pre>jobs: <br>  job1:<br>    runs-on: ubuntu-latest<br>    defaults:<br>      run:<br>        shell: bash<br>        working-directory: ./scripts </pre>   | [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_iddefaultsrunworking-directory) |  
| ```jobs.<job_id>.steps``` | A job contains a sequence of tasks called steps.   | <pre>jobs: <br>  job1:<br>    name: 1st job<br>    runs-on: ubuntu-latest<br>    steps:<br>      - name: Print a greeting<br>        env:<br>          MY_VAR: Hi there! My name is <br>          FIRST_NAME: Ben<br>          LAST_NAME: Jerry<br>      run: (insert stright veriticle line here)<br>           echo $MY_VAR $FIRST_NAME $LAST_NAME. </pre>| [Docs](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idsteps) |  
#### Example
---
```YAML
name: Update Test Server #The name of the workflow

on: # Run this workflow when...
  push: # A push has been made to ...
    branches: # The following Branches
      - main 
      - test
      - bradon_test_branch

jobs: # Mulitiple Jobs can run at the same time
  deployment: # The first job name
    runs-on: self-hosted # self-hosted is running this job on a local machine
      steps: #steps are executed from top down withing each job.
      - name: Test
        working-directory: "C:/Users/blbarfuss/Documents/WRi_API" #where commands will be executed
        run: |
          echo "Pulling lastest updates"
          git pull origin bradon_test_branch
          echo "pulled from test"
      - name: Execute batch script
        shell: powershell
        working-directory: "C:/Users/blbarfuss/Documents/WRi_API/.github/workflows"
        run: |
          echo "Running script"
          ./script.ps1
          echo "Script complete"
 
  greetings-job2:
    name: Greeting People
    runs-on: ubuntu-latest # Run workflow jobs using github runners/servers
    steps:
      - name: Print a greeting
        env:
          MY_VAR: Hi there! My name is 
          FIRST_NAME: Ben
          LAST_NAME: Jerry
      run: (insert stright veriticle line here)
           echo $MY_VAR $FIRST_NAME and $LAST_NAME.
      - name: Print a goodbye message
        env: # Enviroment Variables within this step
          MY_VAR: Bye, it was nice meeting you  
          FIRST_NAME: Ben
          LAST_NAME: Jerry
      run: (insert stright veriticle line here)
           echo $MY_VAR $FIRST_NAME and $LAST_NAME.
 
```

## Add a workflow to you repo  (Incomplete)
*note, the first step may not be needed if you create a action in the github website action tab.
#### First, you want to create a folder called `.github\workflows` in the root of the repository.  
<p align="left">
  <img src="https://github.com/user-attachments/assets/3cebdd23-bcb2-43c8-b9d6-3528e73a351e" width="300">
</p>

***
#### Go to github where you repo is located, and click the actions tab  


<p align="left">
  <img src="https://github.com/user-attachments/assets/ecc7e787-2faf-4064-9e88-d4553352f218" width="700">
</p>

***
#### if no workflows exist, you should see this screen where you can use pre made workflow templates or create your own.  
<p align="left">
  <img src="https://github.com/user-attachments/assets/da1e1f48-8764-4d2e-a484-8ac54ea5cf8d" width="700">
</p>

***

#### Click on "set up a workflow yourself"  
Copy and paste this code to try out your first workflow  
```
name: Basic Workflow

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  greetings-job:
    name: Greeting People
    runs-on: ubuntu-latest
    steps:
      - name: Print a greeting
        env:
          MY_VAR: Hi there! My name is 
          FIRST_NAME: Ben
          LAST_NAME: Jerry
        run: |
          echo $MY_VAR $FIRST_NAME and $LAST_NAME.
```
***
After you copy and paste it, it should look like this:  
<p align="left">
  <img src="https://github.com/user-attachments/assets/2c9f1340-807f-4c6c-adcb-83cc2a7df148" width="600">
</p>

***
Press commit changes, and then either pull or push to the github you added this work flow on.  
After you pushed or pull, go back to the github actions tab and it should look something like this:  
<p align="left">
  <img src="https://github.com/user-attachments/assets/4f0c5775-e8f1-4693-b8d8-ff8ff38ddef4" width="600">
</p>

***
text    
<p align="left">
  <img src="https://github.com/user-attachments/assets/030b2c72-13b2-43e7-a148-a8eaeb46750b" width="600">
</p>

***
text    
<p align="left">
  <img src="https://github.com/user-attachments/assets/a95c2645-cdb4-44ca-8c1a-43738296b7a4" width="600">
</p>

***
**And you just made you first workflow!**

## Self Hosted Runners
**Self-hosted runners:** A self-hosted runner is a system that you deploy and manage to execute jobs from GitHub Actions on GitHub.  

### Add runner
Follow steps [here](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners#adding-a-self-hosted-runner-to-a-repository). Github already made good insturctions here.

Once you complete those steps, you should set a screen something like this
![image](https://github.com/user-attachments/assets/1a8419d7-ac54-48f6-839b-9c3cf57364a8)

Follow the steps on the download section on your github repo screen similar to the example above. Once completed your github self hosted runner will run and works flows will be able to use `runs-on: self-hosted`.
