---
layout: post
title:  "Beyond git-flow"
date:   2015-03-23 13:41:00
category: process
author: gotoplanb
---

note: I'm trying not to include too much deployment logic other than minimal necessary for the branching and merging pattern to have context. I should redraw the full branch diagram after this article is solid in order to reflect the position of patch branches and omission of hotfix branches.

## git-flow is awesome but not a standard.

Mobiquity follows the git-flow pattern described by [Vincent Driessen back in 2010](http://nvie.com/posts/a-successful-git-branching-model/). This pattern is widely used though naming details vary between organizations. I'm not going to enumerate the different naming schemes others use successfully. Instead, I  want to describe how Mobiquity names branches and merges.

Open the [original git-flow article](http://nvie.com/posts/a-successful-git-branching-model/) so that you have the branching diagram for reference. Perhaps we should redraw branching diagram based on our internal conventions, but that is a task for a different day.

## Feature branches

All new work is done in a **feature** branch created from `develop`. There are multiple types of **features** branches, and naming convention changes slightly.

### A new feature

#### Creating the branch

Every new feature should be developed in a **feature** branch. We  name the branch to include the creator's JIRA username and the JIRA ticket ID that covers this work (e.g. `feature/dstanton/MOB-123`). You should immediately publish your branch to GitHub, so that the team knows work is in progress. Publishing also provide an event JIRA can use to automatically assign the ticket to you and move the ticket to in-progress status. There should be a separate **feature** branch for each JIRA user story. 

If a story is big, and multiple developers are contributing pieces, we should keep a single feature branch for that user story, and all developers should be pulling frequently. If a user story is so big that more than two developers are working, and pulling before push is inadequate to keep from stepping on each other, the team probably should split the large user story into multiple smaller user stories.

If the developer needs to start the next story before a dependency has been merged from another pull request, it is fine to create a new feature branch using the dependent feature as the base.

#### Merging the branch

When the ticket is complete, the developer should pull `develop` to get any new commits, then `git rebase develop` from the **feature** branch before pushing the completed code to the feature branch. Rebasing helps us minimize merge conflicts.

Go to GitHub and open a pull request from your branch into `develop`. You should add an animated GIF to the pull request comments that shows the feature working as expected. We use [LICEcap](http://www.cockos.com/licecap/) to create animated GIFs from a simulator. You also should attach this animated GIF to the appropriate JIRA ticket. In the future, we'll automate the JIRA attachment, but for now this is double duty.

The pull request name should include a brief description of the work done along with the JIRA ticket number (e.g. "Closes MOB-123. Add translations for coach marks."). Including the "Closes MOB-123" give JIRA a hook to move the ticket to dev-complete status upon successful merge. We state the ticket number before the description in case the description needs to be longer than GitHub likes in the pull-request name field. This gives us a nice commit history that shows all the ticket numbers with additional description instead of some commits with the relevant ticket number buried in the expanded description for that commit.

Assign the pull-request to the designated reviewer for the sprint. Some teams prefer to have the most senior developer merge all pull requests. Either manner is fine, as long as there is a specific developer that knows they are responsible for code reviews and merging pull requests during the current sprint. 

#### Code review the pull request

Every pull request should include: 

* an animated GIF showcasing functionality matching the ticket's acceptance criteria
* unit tests

There are exceptions. Not all work involves the UI layer, so there may be nothing to directly showcase. However, you could include a GIF showing the request/response cycle or some other evidence that the feature is working. Likewise, there are some areas of the app that may not have test coverage. 

Strive to always include GIFs and unit tests. If either of these do not exist, the developer should have commented on the PR, so that the reviewer knows why these items are missing. If you have to close a pull request without merging, you must leave a comment of why you did not merge. Otherwise, it is nebulous if the PR was closed intentionally or accidentally. The comment should include an at-mention to the developer that opened the branch.

#### Delete branch after merge

After completing code review, and follow-up review of any pull request modifications made by the developer, the reviewer merges the pull request into `develop` and deletes the feature branch. A feature branch should only exist at GitHub if there is work underway or merges pending. **Do not leave feature branches intact after merging**. Keeping the published branches clean gives the team better insight into in-flight work. Developers can keep all of their local branches as they like. 

### Bug branch

A **bug** branch lives in the same part of branching diagram as a **feature** branch and uses the same naming convention (e.g. `bug/dstanton/MOB-321`). We use the **bug** prefixing so that reviewer spends a bit of extra time on checking for regressions in related functionality. Otherwise, there isn't much difference between a **bug** branch and a **feature** branch.

### Patch branch

**Patch** branches live between `develop` and **release** branches. A **patch** branch is used when we have to make a fix against a **release** branch. For example, we may have a `release/3.6` branch that needs a fix. A ticket in JIRA exists that specifies the fix version as 3.6 and hence the developer creates a **patch** branch from the stated **release** branch instead using `develop` as the base branch. The naming convention stays the same (e.g. `patch/dstanton/MOB-444`).

When dev is complete, two pull requests are created:

1. From the **patch** branch into the relevant **release** branch
2. From the **patch** branch into `develop`

The PR into `develop` should receive a comment like "Blocked by #44" to indicate this PR should not be merged until PR #44 is closed. This association allows the reviewer to have a link back to the other PR that included the GIF evidence of fix. This convention allows for small teams where one reviewer is merging into both **release** and `develop` branches as well as larger teams that may have a Support team fixing releases while a Construction team is building new features and managing merges into `develop`.

Frequently there will be patches that should not be merged into `develop` because the code lines have diverged so far as to make back porting the patch irrelevant. The back porting PR (from **patch** into `develop`) always should be opened. If the patch is irrelevant, the developer reviewing PRs into `develop` will comment regarding irrelevancy and close this PR without merging.

## Develop

We expect that `develop` represents a functioning state of the application that can be used for QA.

### On-commit builds

Commits to `develop` trigger a Jenkins build job that should:

* run unit tests
* send code for static analysis
* build the application
* run automated tests
* deliver QA binaries or deploy to staging server

There is a bunch of stuff happening during this build phase, but the only information related to git is that `develop` should build successfully. A failing build should be treated as a showstopper bug -- all developer attention is redirected to getting builds passing again. 

Commits to `develop` should be shortly available either as QA-ready binaries delivered through Apperian EASE (for iOS and Android) or deployed to a staging server (JavaScript and Java).

### On-demand builds

For projects that have minimal integration testing for whatever reason, we typically have another Jenkins job that builds on-demand from `develop`. We use this alternative approach with the construction team needs to smoke-test `develop` before sending to QA.

## Release branches

Upon sprint completion, the tech lead will create a **release** branch from `develop`. We like to follow [semantic versioning](http://semver.org/) and limit our release branches to specifying the minor version (e.g. `release/1.0`).

We should create release branches after every sprint to follow the agile philosophy that the result of sprint work is a stable, shippable application. We may not always ship every release, but our process should be geared to allow frequently releases. Jenkins needs to build from a branch, so we cannot rely on tags for building releases.

### UAT builds

We create a user-acceptance testing (UAT) Jenkins job with a configurable parameter for branch. Typically this should be the most current **release** branch. Allowing the parameter to be manually set allows for some weird scenarios where we have to build an old release for some reason.

## No hotfixes

If a problem is found in a release, we do not create a traditional hotfix. Instead, we create a **patch** branch (see full description above) based on the **release** branch. The reasoning is to ensure every commit at least goes to a staging server or minimal acceptance-testing phase first. We ship/deploy based on release branches, so we treat the **release** branch as  the basis of truth.

The urgency and speed for patching should not supersede the need to move code through our integration systems. 

## Master

Once a **release** branch has completed acceptance testing, we prepare for production deployment. A pull request should be opened from the **release** branch into the `master` branch.

### Build for production

We should run the code through one more set of integration tests to ensure any patches to a **release** branch go through integration testing at least once. Upon merge into `master`, a Jenkins **production** job runs. This job will:

* run unit tests
* send code for static analysis
* build the application
* run automated tests
* **tag master with full version**
* deliver production binaries or deploy to production server
 
### Tagging and documenting

Once `master` is deemed ready for production deployment, we tag master using the full semantic versioning pattern. For example, we are merging `release/1.0` into `master`. The tag will be `version/1.0.0` and made against `master` instead of the **release** branch itself.
  
##  Dependencies

Many projects have multiple code repositories that need simultaneous deployment to ensure server and client can communicate nicely. None of these dependencies should be baked into the branching workflow. Modify the deployment scripts as appropriate.

## Comments?

Add your comments to [this ticket](https://github.com/Mobiquity/team-stanton/issues/8).

 

