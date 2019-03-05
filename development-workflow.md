## 💡 DEVELOPMENT WORKFLOW

### Stories

1. Always **create** a story for things you work on. If it is worth spending time on, it is worth creating a story since that enables other people to learn and help. You can always edit the description or close it when the problem changed to something different or was solved. _(TODO: link to Pivotal projects)_
1. Not every solution will solve the problem at hand. Keep discussions focused by **defining the problem first**. Also, don't rush to the first solution it comes to mind, unless the solution is obvious, try to propose multiple options and ask advice from business and product team-mates if needed.
1. **Double link** stories with related conversations. E.g. if there’s an Instabug ticket that causes you to open a story on Pivotal Tracker, make sure to document the story link in the Instabug issue and vice versa.
1. If two stories are related, **crosslink** them (a link from each story to the other one). When a story blocks another story, use the blockers feature of Pivotal Tracker.
1. After a discussion about a feature **update the story description** with the consensus or final conclusions. This makes it much easier to see the current state of a story for everyone involved in the implementation and prevents confusion and discussion later on.
1. Submit the **smallest** item of work that makes sense. When creating a story describe the smallest fix possible, put suggestions for enhancements in separate stories and link them.
1. Do not leave stories open for a long time, stories should be actionable and realistic. If you are assigned to a story but don't have time to work on it, assign it to someone else or move it to the _icebox_.
1. Make a conscious effort to prioritize your work. The priority of items depends on multiple factors: Is someone waiting for the answer? What is the impact if you delay it? How many people does it affect, etc.? This is detailed in Engineering Workflow. _(TODO: link to engineering workflow)_
1. Pick stories from the top of the backlog.
1. Assign a story to yourself as soon as you start to work on it, but not before that time. If you complete part of a story and need someone else to take the next step, **re-assign** the story to that person.
1. When re-assigning a story, make sure that the story description contains the latest information. The story description should be the **single source of truth**.
1. Do not close a story until it is **done**.
1. When closing a story leave a comment explaining why you are closing it.
1. If a fix/implementation gets merged to master, add a comment with the expected release version(s) that will include it.

#### Stories Guidelines

##### Story Types

* **features**: stories that provide verifiable value to the user. They have a story point estimate.
* **chores**: stories that are necessary but provide no direct, obvious value to the user. They don’t require extra validation when they’re finished, so the states for chores are just _unstarted_, _started_, and _accepted_.
* **bugs**: unintended behavior that can be related to features. Bugs have the same states as _features_.
* **releases**: milestone markers that allow your team to track progress toward concrete goals. Releases only have unstarted and accepted states.

##### Story States

Features and bugs have the following states:

  * _Unscheduled_: this are stories in the Icebox, there's no plan to fix them soon.
  * _Unstarted_: development on this story hasn't started yet.
  * _Started_: development has started.
  * _Finished_: a fix is ready, a Pull Request is awaiting review.
  * _Delivered_: Pull Request merged and deployed.
  * _Accepted_: the fix has solved the issue, the story is closed.
  * _Rejected_: doesn't meet the acceptance criteria according to the user or product owner, a new fix is needed.

##### Story Points

* **0 points** – it's super simple and I'm sure I can do it in minutes.
* **1 point** – I already know how to complete this story. It looks easy and it should be quick
* **2 points** – I'm not sure. There might be something I don't know how to do and/or unforeseen events
* **3 points** – it's complex and I need to figure out what to do. But it won't take more than 1 day full time. If it takes longer break down the story into smaller stories.

### Pull Requests

1. Start the related story.
1. Create a new feature branch, the name should be meaningful, e.g: `12345-fix-payment-button` where `12345` is the Pivotal story ID.
1. Make your changes in the branch and commit using descriptive messages (see below for guidelines on commit messages). Prepend the commit message with the Story # you're working on. Example: `[#12345] Fixes payment button`.
1. Push the branch: `git push -u origin 12345-fix-payment-button`.
1. Create a pull request from your branch.
1. Prepend the pull request title with the Story #. Examples: `[#12345] Fixes payment button`.
1. If the Pull Request is still a work in progress, create a [draft pull request](https://github.blog/2019-02-14-introducing-draft-pull-requests/).
1. Add at least one human reviewer to your story.
1. Press Finish on the story on Pivotal Tracker.
1. Wait for at least one of the human reviewer to review the change.
1. Update the master branch within your local repository with `git checkout master` and `git pull`. If you have other branches that need to be updated, then run also: `git checkout other-branch` and `git pull --rebase . master`. Note that subsequent pushes for these branches will require a `-f`
since history has been rewritten by rebasing.
1. Deploy, or wait for someone to deploy the newly written code.
1. Once deployed press `Deliver` on the story on Pivotal Tracker and add a comment with the release version that includes the PR.
1. Based on how it goes the story will be accepted or rejected.

### Code Reviews

1. Every developer should spend from 30min to 1h each day on code reviews.
1. Each PR should be as small as possible: the general guideline is to create pull requests ranging from 200 to 400 lines of code, not more.
1. Include explanation comments in the PR: this can help the reviewers in making the review faster. At the same time try to be succinct, not verbose, comments should have a high informational density.
1. Ask for feedback early by creating [draft pull requests](https://github.blog/2019-02-14-introducing-draft-pull-requests/). This can shorten dramatically approval and review time.

### Instabug

1. When a user opens a new bug via Instabug, a _Bug_ story gets automatically created on Pivotal in the _Icebox_.
1. If you feel the bug is critical, move the story to the _Backlog_ and (possibly) assign it to yourself.
1. When a fix gets released, go to Instabug and reply to the user that opened the bug telling him a fix is coming soon, mention the iOS/Android release version that is expected to fix the issue.
1. When the story gets accepted, mark the Instabug issue as closed.
