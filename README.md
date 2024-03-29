# dare's `/git(hub)?/g` convention

## Table Of Contents

- [dare's `/git(hub)?/g` convention](#dares-githubg-convention)
  - [Table Of Contents](#table-of-contents)
  - [commiting guide: `atomic commits`](#commiting-guide-atomic-commits)
  - [commit message convention: `hrcc`](#commit-message-convention-hrcc)
    - [message](#message)
    - [body](#body)
    - [footer](#footer)
    - [breaking changes](#breaking-changes)
  - [branch naming scheme: `hrbc`](#branch-naming-scheme-hrbc)
  - [pr naming scheme: `hrpr`](#pr-naming-scheme-hrpr)
  - [github flow description](#github-flow-description)
    - [main branch](#main-branch)
    - [dev branch](#dev-branch)
    - [feature branches](#feature-branches)
    - [hotfix branches](#hotfix-branches)
    - [why squash for feature -\> dev?](#why-squash-for-feature---dev)
    - [why merge for dev -\> main?](#why-merge-for-dev---main)
  - [how to prs guide](#how-to-prs-guide)
  - [how to issues guide](#how-to-issues-guide)
  - [why you should use the `github issue notebooks` vscode extension](#why-you-should-use-the-github-issue-notebooks-vscode-extension)
  - [gh cli guide](#gh-cli-guide)

## commiting guide: `atomic commits`

this section is easier explained using a few articles online.
here are the articles:

- [make atomic git commits](https://www.aleksandrhovhannisyan.com/blog/atomic-git-commits/)
- [atomic commits will help you git legit](https://dev.to/paulinevos/atomic-commits-will-help-you-git-legit-35i7)

---
---

## commit message convention: `hrcc`

`hrcc` is a commit message convention made by me, dare, in order to make a commit convention with the readability of putting whatever you want but the automatationability of the angular commit convention.

in this git convention, it will mostly be used for squash commits from a feature branch to the dev branch.

the syntax is as follows:

### message

- [type] message {scope}

for example:

- [add] hrcc explanation to {readme}.md

type can be anything you want, as long as any automation software you use understands it. same thing for scope.

### body

there are two types of bodies depending on whether you are doing a squash commit or a normal commit.

normal:

```diff
explanation

(footer)
```

squash:

```diff
+ -> commit messages from squash
+ -> commit 2
+ -> commit 3

explanation

(footer)
```

### footer

there is only 3 footers:

- breaking changes (explained next): `!: message`
- reviewers: `reviewers: @kyoline, @kyoline2`
- notifications: `cc @kyoline @kyoline2`

### breaking changes

breaking changes in a commit are signified in two ways:

`<!>` at the beginning of the commit message.

`!:` in the footers to tell you what broke.

example commit:

```diff
<!> [add] new breaking feature to {hrcc} > #2
+ -> new breaking feature

this adds a new breaking feature or something idfk

!: this breaks bc i changed the commit convention
```

---
---

## branch naming scheme: `hrbc`

`<issue number>/message-in-kebab-case`

where:

- `<issue number>` is the issue number.
- if you don't have a `<issue number>`, either remove it (`message-in-kebab-case`) or make it 0 (`0/message`).

example:

```text
42/add-branch-naming-scheme
```

- 42 is issue number.
- message is `add-branch-naming-scheme`.

## pr naming scheme: `hrpr`

the pr naming scheme will essentially just be a version of the normal commit scheme, as a pr is basically just a massive commit.

the scheme is as follows:

```diff
[type] message {scope} > #prnumber
```

example, breaking changes with a squash body:

```diff
<!> [add] thing to {readme} > #3
+ -> readme thing

description

!: breaks stuff

```

---
---

## github flow description

Github Flow is a lightweight, branch-based git workflow.

The main concepts I will be adressing here are these:

- the main (prod) branch
- the dev (develop) branch
- feature branches
- hotfix branches
- why we use squash when we do feature -> dev prs
- why we use merge when we use dev -> main prs

---

### main branch

the main branch in a github flow repository is `main`. it could also be called `master`, `trunk`, etc.

this branch is best described as the *production* branch.

whenever you make a pr from dev (main development branch) to this branch, you are essentially releasing a new version of your software.

what a version means is up to you.

whenever you release a new version, you tag it (`git tag` or github's web ui). this axn be automated using a github workflow.

tagging also allows for easy CD.

---

### dev branch

the dev branch in a github flow repository is usually `dev`. it could also be called `develop`, `development`, `devel`, among other things.

this branch is best described as *the latest development version*.

all feature branches eventually end up squashed into here.

it is also the "default" branch in github.

this is the only branch besides `main` who has `hrcc` commit histories.

this is also the only branch allowed to be merged into `main`.

---

### feature branches

these branches are the *pr branches*.

basically, these branches are the `from` in `from -> to`, where `to` is always `dev`.

the commits on these branches do not have to follow the `hrcc`, because when these are squash-commited, it will provide `hrcc` commits to the actual branches that have changelogs. it is recommended to follow it though.

these branches are only allowed to be squashed, and only into `dev`.

---

### hotfix branches

these branches are the branches that are used to fix important issues in the `main` branch.

essentially, if you accidentally push a very severe bug to `main`, you would use a hotfix branch in order to fix it.

a small explanation of a hotfix workflow:

- created off latest `main`
- only commits allowed on hotfix are about issue
- no feature enhancements or chores (`feat` or `chore` in *`hrcc`*)
- merges into both master & develop branches when its finished
- deleted after merge

---

### why squash for feature -> dev?

we use squash when we do feature -> dev because it allows for the commits inside of the pr's `feature` branch to be anything (the developer doesn't have to worry about commit conventions), and then when the pr is finished it gets squashed into dev as a `hrcc` commit.

if we didn't do this, it would require every developer to follow commit conventions and have to worry about all of that stuff. it's better for both ends.

instead of every commit having to be formatted correctly, they just make sure the pr is named correctly according to `hrcc` and then they're done.

for example, look at pr #1.

---

### why merge for dev -> main?

we use merge for dev -> main because of the fact that we only use squashes so the developers don't have to worry and it makes sense in the context of github flow.

we use merges, because of instead of squashing all of the new developer commits into one commit for main, we instead make the `main` branch be a snapshot of the latest production-ready (new-version-ready) `dev` branch, and tag the main branch for both CD purposes and to mark previous version releases.

---
---

## how to prs guide

[this guide](https://www.atlassian.com/blog/git/written-unwritten-guide-pull-requests) is a quite good guide to pull requests.

pull requests usually rely on [issues](#how-to-issues-guide), so it is important to get those correct as well.

pull requests usually also have to do with [projects](https://www.youtube.com/watch?v=lzpcyYIbHqE&list=PLiO7XHcmTsldZR93nkTFmmWbCEVF_8F5H), and are very imporant to the github ecosystem.

don't forget to also check out [hrpr](#pr-naming-scheme-hrpr).

## how to issues guide

[this video](https://www.youtube.com/watch?v=MvyGcLg6AvI) is a great guide on how issues work.

issues are meant to be a "what should we do" sort of building block of github.

you make an issue, you make a pr to solve said issue, you close the issue.

or, it can be used as a roadmap, a task checklist to finally close when it's done.

a lot of things can be done with issues, and if you want some examples, look at various github repos.

## why you should use the `github issue notebooks` vscode extension

[github issue notebooks] is a vscode extension that allows for jupyter-notebook
like querying of github repos.

here's an example query:

```text
is:closed repo:kyoline/git-convention
```

if you put that in a github issues notebook, and then run it, it will provide you a list of closed prs or issues that are in the repo kyoline/git-convention:

![github issues example](github-issues-1.png)

this extension can be used to do lots of different things, and i think you have already started seeing the possibilities.

want to know all of the closed prs in repo kyoline/git-convention that are assigned to the currently signed in account?

```text
is:closed repo:kyoline/git-convention is:pr assignee:@me
```

that is why you should start using github issue notebooks in your repositories.

## gh cli guide

the github cli is too big of a program for me to easily explain here.

here's a [great tutorial on it.](https://www.youtube.com/watch?v=BRAG1Kj4-Ss)

[github issue notebooks]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-github-issue-notebooks
