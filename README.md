 
# learn-git-actions   ![](https://github.com/yashikajayasinghe/learn-git-actions/workflows/first/badge.svg)

## Git Actions
 
+ ref: list of events that can be added to workflows  <https://help.github.com/en/actions/reference/events-that-trigger-workflows>

1. one event
    - On: eventName
2. multiple events enclosed by brackets
    - On: [event1Name, event2Name]

- workflow must have at least one job
- Each job must have a unique identifier
- Job identifier must start with a letter or underscore
- jobs run in prallel
- dependencies can be added to make it sequential

Dependencies between actions
Example scenario: Results of the first job needs to be used in the second job, if they were run prallel, second job will fail.
but to pass this scenario, it can be achieved by adding the ```needs``` attribute to the second job

- needs: Identifies one or more jobs that must be completed successfully before a job will run (job Id)

```
jobs:
    Job1:
    Job2:
    Job3:
        needs:[Job1, Job2]
```

Add Conditions to a workflow
    - i.e. When we need to limit activities to specific branches

```
    on:
    push:
        branches:
            - develop
            - master
```
- i.e. job to run when a pull request is added to master branch

```
on:
    pull_request:
        branches:
            - master
```

i.e. for workflows triggered by two or more events (i.e push and pull_request).
Workflows only have ```on``` attribute incleded once, it is needed to create to blocks for each type of trigger.

Steps: Include each trigger in 'on' attribute, then each trigger to have 'branches' conditional and each 'branches' to have specific branches defined.

```
on:
    push:
        branches:
            - master
            - developer
    
    pull_request:
        branches:
            -master

```

Other conditionals:
- branches-ignore: Along with include branches, ignore branches, but can't keep both in one workflow: 
- tags:
- tags-ignore:
- 'feature/login.feature': Special chars (regEx)  e.g. Feature files in the feature folder, 'release/*' , 'm?ster'

Note: there are limitations to workflows, i.e. number of actions that can be triggered per day, job concurrency limits according to the GitHub plan
