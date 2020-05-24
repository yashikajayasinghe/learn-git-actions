# Using Actions

## Use Actions from the Marketplace

steps:

1. open the workflow yml file on github web editor
2. search for the actions required for in the 'Marketplace'
3. Copy the snippet and paste it under 'Steps' in the workflow file
4. commit and push. Check in the Actions tab for the workflow result  

## Use actions from a repository

e.g. From docker hub

```bash
steps:
    - uses: "docker://python:3.9"
```

## Passing arguments to an action

- by using 'with' parameter

e.g. scenario: create a user in apache tomcat container

```bash
on: push
jobs:
    build:
        runs-on: ubuntu-latest
    steps:
    - name: checkout Apache version
      uses: actions/checkout@v2
      with:
        repository: apache/tomcat
        ref: master
        path: ./tomcat

```

## Using env variables

- when actions need additional information provided to them at runtime
- Dynamic key:value pairs injected at runtime
- ref: <https://help.github.com/en/actions/configuring-and-managing-workflows/using-environment-variables>
- visibility of the env variables are dependent on the level they are defined
  - env vars defined in workflow level are visible to all jobs,steps,actions,and commands
  - env vars defined at jobs level only visible to all steps, actions, and commands with the job

```bash
${{env.VARIABLE_NAME}}

```

## Using secrets

synatx for yaml format

```bash
${{secret.SECRET_NAME}}
```

- eg. aws secrets
- secrets can be stored in github account->settings->secrets and they can be accessed using the above syntax
- why we access secrets thru arguments-> we don't want to store our secrets derectly in a repository with public accessibility 

```bash
steps:
    name: Configure aws credentials
    uses: aws-actions/configure-aws-credentials@v1
    with:
        aws-secret-key-id: ${{secret.SECRET_NAME_AS_STORED_IN_SETTINGS}}
```

## Using artifacts

- what are artifacts:  Test results, Log files etc
- can only be uploaded by w workflow
  - actions/upload-artifacts
- can only be downloaded by the uploading workflow
  - actions/download-artifact
- manual downloads are also possible

## Manage pull requests

Example criteria:

- run automation tests to check the code in the pull-request
- check username and if it is in the allowed(pre defined) list of users
- automatically approve and merge the pull-request
- delete the pull-request
