# Repository Template

[![Build Status](https://travis-ci.org/cs130-w21/template.svg?branch=master)](https://travis-ci.org/cs130-w21/template)
[![Release](https://img.shields.io/github/v/release/cs130-w21/template?label=release)](https://github.com/cs130-w21/template/releases/latest)

This repo serves as a template for repositories in this organization. The following information describes how the native features/workflows of Github can be customized to work in a scrum development process.

## Issues

An issue is a unit of tracking work. Issues can be classified into different classes using `labels`. This can be used to classify issues in the scrum process as follows.

### Epic

An epic is an issue with the label `epic`. It represents a large story that can be broken into stories, which can be addressed over multiple sprints. An epic issue references its story issues as a task list in its description. A Github action has been added to automatically check/uncheck the story task items when they get closed/reopened.

### Story

A story is an issue with the label `story`. It may represents a new feature, or an enhancement to an existing feature. A story issue can be broken into sub tasks, which are added as a task list in the description of the story issue. These sub task items can be checked manually by the developer to indicate completion.

### Bug

A bug is an issue with the label `bug`. It represents a problem with the existing code that needs to be fixed.

### Question

A question is an issue with the label `question`. It represents a question raised by any one and that may get converted into other types of issues.

## Labels

In addition to the standard labels above, you can add new labels to issues to classify them into different classes like `documentation`, `frontend`, etc, or to add metadata like `duplicate`, `invalid` etc.

## Milestones

A milestone groups issues that are expected to be delivered at some point in time. It also allows ordering (prioritizing) theses issues and tracking their progress (percentage of issues completed so far). In the scrum context, a milestone can be used as a sprint. So, you can create your sprints and give them names like Sprint1, Sprint2, etc. and set their due dates respectively.

## Projects

A project is a kanban-style board that can aggregate a set of issues for any purpose. In the scrum context, we can create one project called `Scrum Board` and choose its templaste as `Automated kanban with reviews`. (This will create a set of initial notes that you can delete).

## Branches

The `master` branch is the main branch used for releases. Other branches can be created. For example, a branch called `gh-pages` is often used to create a website for ther repository (for more information check this [link](https://pages.github.com/)). Other branches can be created to address the issues of the repository, one branch per issue (called an `issue` branch). Such branches can then be used to create pull requests, where they get peer reviewed and eventually merged into the `master` branch.

## Pull Requests

A pull request is a request to merge commits in one branch to another branch. This is typically used to merge commits from an `issue` branch into the `master` branch. A pull request is where the process of peer review is carried. Reviewers can comment on the code changes to show approvals or request changes (which will need to be addressed by additional commits to the `issues` branch). When a CI pipeline is configured on a repository, it will run on `issue` brnach and its result will be shown in the pull request status. When the peer review process has concluded, one of the repository committers can merge it into master. The recommended merge option is the `Squash and merge`, (i.e., squash all commits into a single commit), since it makes the repository simple and linear.

## Tags

Tags can be used to mark release points in a repository's commit history. Typically, after some code milestone has been achieved, with a set of commits, a release is made by pushing a tag (typically a version number like 1.0.0, 1.0.1, etc.) to the respository. This results in the tag showing up in the repository's tags page.

## Workflows

### Creating issues

An issue can be created from the `issues` tab of a repository. An issue type (bug, story, epic, question) is first chosen then its corresponding template needs to be sufficiently filled.

### Triaging issues

The product owner goes frequently to the Scrum Board project and clicks on the `Add Cards` link to triage existing issues in the repository to the board's `To do` column, which acts as a product backlog. Product owner can also added unbaked ideas to the `To do` column as notes, which are placeholders that can later be converted into issues). Issues and notes can then be ordered in the `To do` column to show their priority.

### Planning sprints

The scrum master creates a new milestone and gives it a suitable name (e.g., Sprint1) and a due date. Then, in the Scrum Board, issues from the top of the `To do` column (assuming they have been ordered based on priority) can be assigned to that milestone and to the developers who will work on them.

### Working on issues

Developers go to the Scrum Board and filter it for the issues assigned to them in a given milestone. They can pick the ones to work on by moving them to the `In progress` column.

### Reviewing progress

In the daily standup, scrum master can review progress by going to the Scrum Board and filtering it by the current milestone (sprint). Developers can then reference issues in the various columns when they answer the usual standup questions, e.g., isses they current work on (`In progress`), finsihed (`Done`) or yet to work on (`To do`).

### Working with issue branches

Before developers work on an issue, they need to checkout and pull the master branch to ensure they have all the latest commits. Then, they should create a new local `issue` branch and name it `issue-[number]` (replacing `[number]` by the issue number). Several `issue` branches can be created concurrently, one for each issue, but it is important to make them independent from each other by checking out the `master` branch before creating each of them. This allows them to be pushed and merged independently from each other.

Each `issue` branch can accumulate commits to address the issue. When ready, it can then be pushed to a corresponding remote branch that can then be used to create a pull request into the `master` branch. The pull request template needs to be filled at this point. One created, the pull request can be reviewed by peer reviewers who may request changes. These changes can be made using new commits in the local `issue` branch that can subsequently be pushed to the corresponding remote `issue` branch. When all peer review has concluded, the pull request can be `squash merged` into the `master` branch, and the issue branch can be deleted. If the pull request description includes `fixes #[number]` (where `[number]` is an issue number), the  issue with that number will automatically be closed.

> it is recommeded to not push commits to the master branch directly but to always go through a peer review process using an `issue` branch.

### Creating releases

It is recommended to create periodic releases from the repository, at least at the end of each sprint but can be more frequent. These releases should be working versions of the component being developed. To create such releases, a new tag representing a version number (e.g., 1.0.0) is added to the local `master` branch and pushed to the remote `master` branch. Then, in the tags pages in Github, such tag can be used to create a new release.

### Using a CI/CD pipeline

Every repository needs to have a way to build its artifacts headlessly. It is a good idea to run test cases as part of such build. Instructions on how to build a repository needs to be documented in the repository's README.md file.

A reposirory can also be setup to build continuously when a commit is pushed to its master branch by setting up a Travis CI script (.travis.yml file) in its root folder. Such script will setup the build environment on a virtual machine and invokes the build script on the master branch. If the script fails for some reason, the committer will be notified. It is a good practice to add a build badge to the README.md file to visibly indicate the status of the last Tracis CI build. 

The Travis CI script will also be run when a new pull request is created or when more commits are pushed to its linked `issue` branch. Such build gives peer reviewers an idea about the status of building the `master` branch whe the propsoed changes are merged into it. A successful CI build can be a prerequisute for peer reviewers looking at the changes.

When a tag is pushed on the master branch, in the release process, the CI script will additionally deliver and/or deploy the built artifact. The script can be also be configured to create a Github release based on the tag.
