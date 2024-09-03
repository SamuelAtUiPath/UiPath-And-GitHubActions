# UiPath-And-GitHubActions

This repository shows how to manage Pipelines using GitHub Actions from commit code in Dev branch up to publish it to the UiPath Orchestrator

## What is a CICD Pipeline?
Per Chat GPT...
> A CI/CD pipeline, which stands for Continuous Integration/Continuous Deployment, is a series of steps that must be performed in order to deliver a new version of software. The aim is to automate the software delivery process, enabling swift and reliable updates.
> 
> Continuous Integration refers to the practice of merging all developers' working copies of code to a shared mainline several times a day. This aims to prevent integration problems, which are common when developers work on their own versions of a project's source code separately.
> 
> Continuous Deployment is related, but it covers the stages that occur after code is integrated, particularly testing and releasing the software. Automated testing and deployment processes mean that the software is always in a release-ready state.

In the UiPath enviroment, the new versions of the software must be added to the Orchestrator. The Pipelines can do that by using the UiPath CLI

## UiPath CLI
> UiPath® offers a command-line interface (CLI) that allows you to execute certain pre-defined tasks for RPA package management and testing. The purpose of the UiPath CLI is to easily integrate those capabilities with third-party tools like GitLab, Jenkins, and many others, without a plugin. 
> Reference: [UiPath docs - UiPath CLI](https://docs.uipath.com/automation-ops/automation-cloud/latest/user-guide/uipath-command-line-interface)

The CLI is the piece of software that enables pipelines to Build, Analyse, Deploy and other activities within the code stored in the repository.

For more details on the possible actions the CLI enables to do, check out this doc: [UiPath Docs - Executing Tasks](https://docs.uipath.com/automation-ops/automation-cloud/latest/user-guide/executing-tasks-cli#analyzing-a-project)

## GitHub Actions pipeline
The GitHub Action will always run in a Virtual machine created only for this purpose. It is possible to create agents to make the actions run in a know environment, but here I won't describe this method.

If you have never used a pipeline before, I really recommend reading this [GitHub Docs - Understanding GitHub Actions](https://docs.github.com/en/actions/writing-workflows/quickstart) 

Creating one Pipeline to build and deploy a package to Orchestrator depends on the following steps:
- Define when to run the pipeline
- Checkout the code from the repository
- Install the UiPath CLI
- Build the package
- Deploy the package to the desired Tenant.

This operations work by using commands in the installed uipath cli executable file downloaded.

Take a look at the job details at [this Pipeline](https://github.com/SamuelAtUiPath/Actions-Pipelines/blob/main/.github/workflows/called-pipeline.yml)

## Avoiding duplicate code
A good practice in pipelines is to have Central pipelines with its steps. Pipelines will have similar steps that generally will be different only by a few arguments.
In this case we can make pipeline branched base to use the Central pipeline by passing its arguments to run.

## Central Pipeline - Called
There is a [Called Pipeline](https://github.com/SamuelAtUiPath/Actions-Pipelines/blob/main/.github/workflows/called-pipeline.yml) that holds the steps needed whenever a package should be added to UiPath Orchestrator.

## Branched Pipeline - Caller
There is a [Caller Pipeline](https://github.com/SamuelAtUiPath/UiPath-And-GitHubActions/blob/test/.github/workflows/caller-pipeline.yml) that shows how to call the Called pipeline.
