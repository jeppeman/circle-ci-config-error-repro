### CircleCI CLI error repro

To reproduce, run the following: 

```bash
cd .circleci
circleci config pack config > .packed
circleci config process .packed
```

Notice how the workflows section in the following output is missing the job that has a branch filter with the `only`-key:

```yml
version: 2
jobs:
  Test with only branch filter:
    docker:
    - image: ubuntu-latest
    steps:
    - run:
        command: echo Hello
  Test with ignore branch filter:
    docker:
    - image: ubuntu-latest
    steps:
    - run:
        command: echo Hello
  Test without branch filter:
    docker:
    - image: ubuntu-latest
    steps:
    - run:
        command: echo Hello
workflows:
  dummy_workflow:
    jobs:
    - Test with ignore branch filter:
        filters:
          branches:
            ignore:
            - master
    - Test without branch filter
  version: 2

# Original config.yml file:
# executors:
#     dummy:
#         docker:
#             - image: ubuntu-latest
# jobs:
#     dummy_job:
#         executor: dummy
#         steps:
#             - run: echo Hello
# version: 2.1
# workflows:
#     dummy_workflow:
#         jobs:
#             - dummy_job:
#                 filters:
#                     branches:
#                         only:
#                             - master
#                 name: Test with only branch filter
#             - dummy_job:
#                 filters:
#                     branches:
#                         ignore:
#                             - master
#                 name: Test with ignore branch filter
#             - dummy_job:
#                 name: Test without branch filter
#     version: 2
```
