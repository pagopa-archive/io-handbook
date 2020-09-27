## 💡 DEVELOPMENT WORKFLOW

### Design

To thoroughly analyze and discuss non-trivial decisions or features, we rely
on collaborative document editing, taking several iterations of proposals and
feedbacks before the actual planning and implementation.

#### RFCs

Macro changes that impact the software and system architecture such as logging,
authentication, integration between components and 3rd party systems, format and
structure of data etc. must be initially outlined in a document (_RFC_) that is
shared and discussed with the rest of the team. RFCs are also useful for describing
high-level activities not necessarily related to software development - for example:
topics related to the legal aspects of the project or the interaction with public subjects.

For hystorical reasons old RFCs are called CDMs (_Cittadinanza Digitale Memos_)
and are stored in a publicly accessible folder named [CDM](https://drive.google.com/drive/folders/1nmPZqEzH9aN_5qFJbd36vS3v7leUcJx0)
in the Digital Team Google Drive.

All team members are invited to write new RFCs and comment the existing ones.

##### Creating a new RFC

New RFC documents must be named `IO-RFC-{n} - {title}` where `n` is an incremental
number (one plus the highest memo in the RFC folder) and `title` is a short title
describing the proposal (when in doubt try to be consistent with the current RFCs
or ask advice in the `#dev_io` channel).

When writing an RFC, assume that the reader knows almost nothing about the context,
the issues and the reasons that motivate any eventual implementation proposal.

We currently write RFCs in the Italian language
(but if you feel like writing in English, you are welcome to do so).

The RFC should be roughly structured as following:

* A title (same as the name of the document)
* Author
* Current state of the document (i.e. work in progress, waiting for review, planned, implemented)
* A _Context_ (_Contesto_) section where we explain why the document has been written (i.e. the starting point, the need, the current state of things)
* A _Proposal_ (_Proposta_) section where we explain what the RFC is proposing to implement or change, 
how this proposal solves a certain need or problem and (if applicable) what are the effects on other systems.

##### Asking for feedback

Once the first draft has been completed, the author shares the document with the
team in the `#dev_io` Slack channel, asking for feedback and/or tagging
some key stakeholder (ie. the CTO) using Google Docs comments feature.

There may be several iterations of feedbacks/comments and updates.

##### Implementation

Once a RFC is finalized and approved for implementation, it must be translated
into development stories that get added to the backlog of each affected component.

#### Architecture Decision Records

For registering architectural decisions we rely on [Architecture Decision Records](https://github.com/joelparkerhenderson/architecture_decision_record#architecture-decision-record-adr) (ADR):

See the [ADRs repository](https://github.com/teamdigitale/digital-citizenship/tree/master/architecture/decisions).

### Writing stories

1. Always **create** a story for things you work on. If it is worth spending time on, it is worth creating a story since that enables other people to learn and help. You can always edit the description or close it when the problem changed to something different or was solved. _(TODO: link to Pivotal projects)_
1. Always start with the "**why**" not with the "how": not every solution will solve the problem at hand. Keep discussions focused by **defining the problem first**. Also, don't rush to the first solution it comes to mind, unless the solution is obvious, try to propose multiple options and ask advice from business and product team-mates if needed.
1. **Double link** stories with related conversations. E.g. if there’s an Instabug ticket that causes you to open a story on Pivotal Tracker, make sure to document the story link in the Instabug issue and vice versa. The same states for notable slack conversations.
1. If two stories are related, **crosslink** them (a link from each story to the other one). When a story blocks another story, use the blockers feature of Pivotal Tracker.
1. After a discussion about a feature **update the story description** with the consensus or final conclusions. This makes it much easier to see the current state of a story for everyone involved in the implementation and prevents confusion and discussion later on.
1. Submit the **smallest** item of work that makes sense. When creating a story describe the smallest fix possible, put suggestions for enhancements in separate stories and link them.
1. Do not leave stories open for a long time, stories should be actionable and realistic. If you are assigned to a story but don't have time to work on it, assign it to someone else or move it to the _icebox_. Consider removing idle stories from the "Started" state to not compromise velocity auto-computation.
1. Make a conscious effort to prioritize your work. The priority of items depends on multiple factors: Is someone waiting for the answer? What is the impact if you delay it? How many people does it affect, etc.? This is detailed in Engineering Workflow. _(TODO: link to engineering workflow)_
1. Pick stories from the top of the backlog.
1. Assign a story to yourself as soon as you start to work on it, but not before that time. If you complete part of a story and need someone else to take the next step, **re-assign** the story to that person.
1. When re-assigning a story, make sure that the story description contains the latest information. The story description should be the **single source of truth**.
1. Do not Accept stories about changes not yet deployed into production. Do not close a story until it is really **done**.
1. When closing a "wont-fix" story (use the label!) leave a comment explaining why you are closing it.

#### Stories Guidelines

##### Story Types

* **features**: stories that provide verifiable value to the user. They have a story point estimate. Use the form `As <type of user> I want to <some goal> so that <some reason>` for features stories.
* **chores**: stories that are necessary but provide no direct, obvious value to the user. They don’t require extra validation when they’re finished, so the states for chores are just _unstarted_, _started_, and _accepted_.
* **bugs**: unintended behavior that can be related to features. Bugs have the same states as _features_.
* **releases**: milestone markers that allow your team to track progress toward concrete goals. Releases only have unstarted and accepted states.

Titles / descriptions must be:

*	**Understandable**	the story or its related description must provide all the necessary information in order to be understood by everyone working on the project with minimal information about the context
*	**Friendly** put implementation details into the story description

Stories must be:

* **Independent** (can be built separately to other stories)
* **Negotiable** (requirements can be adapted)
* **Valuable** (provides benefit to the end-user)
* **Estimable** (to reasonable accuracy)
* **Small** (can be built within one iteration)
* **Testable** (can be verified by QA)

see https://medium.com/tribalscale/writing-technical-user-stories-434bf96f1dd5

##### Stories examples

Bad: _Implement login screen_  
Good: _As an Author I want to be able to login in order to start edit the content_

Bad: _Create a modal to show errors_  
Good: _As an Editor I want to be notified in case some error occurs in order to plan a retry_

...

##### Story States

Features and bugs have the following states:

  * _Unscheduled_: this are stories in the Icebox, there's no plan to fix them soon.
  * _Unstarted_: development on this story hasn't started yet.
  * _Started_: development has started.
  * _Finished_: the relative Pull Request is merged into master.
  * _Delivered_: Pull Request is deployed into production.
  * _Accepted_: the fix has solved the issue, the story is closed.
  * _Rejected_: doesn't meet the acceptance criteria according to the user or product owner, a new fix is needed.

##### Story Points

* **0 points** – it's super simple and I'm sure I can do it in minutes.
* **1 point** – I already know how to complete this story. It looks easy and it should be quick
* **2 points** – I'm not sure. There might be something I don't know how to do and/or unforeseen events
* **3 points** – it's complex and I need to figure out what to do.

### Pull Requests

1. Start the related story.
1. Create a new feature branch, the name should be meaningful, e.g: `12345-fix-payment-button` where `12345` is the Pivotal story ID.
1. Make your changes in the branch and commit using descriptive messages (see below for guidelines on commit messages). Prepend the commit message with the Story # you're working on. Example: `[#12345] Fixes payment button`.
1. Push the branch: `git push -u origin 12345-fix-payment-button`.
1. Create a pull request from your branch.
1. Prepend the pull request title with the Story #. Examples: `[#12345] Fixes payment button`.
1. If the Pull Request is still a work in progress, create a [draft pull request](https://github.blog/2019-02-14-introducing-draft-pull-requests/).
1. Add at least one human reviewer to your story.
1. Attach the Pull Request to the story by clicking on the _Attach from Github_ link in the story page and pasting the PR URL.
1. Press Finish on the story on Pivotal Tracker.
1. Wait for at least one of the human reviewer to review the change.
1. Update the master branch within your local repository with `git checkout master` and `git pull`. If you have other branches that need to be updated, then run also: `git checkout other-branch` and `git pull --rebase . master`. Note that subsequent pushes for these branches will require a `-f`
since history has been rewritten by rebasing.
1. Create a new relase of the packages using `yarn release-it <patch|minor|major>`
1. Deploy, or wait for someone to deploy the newly written code.
1. Once deployed press `Deliver` on the story on Pivotal Tracker.
1. Based on how it goes the story will be accepted or rejected.

### Code Reviews

1. Every developer should spend from 30min to 1h each day on code reviews.
1. Each PR should be as small as possible: the general guideline is to create pull requests ranging from 200 to 400 lines of code, not more.
1. Include explanation comments in the PR: this can help the reviewers in making the review faster. At the same time try to be succinct, not verbose, comments should have a high informational density.
1. Ask for feedback early by creating [draft pull requests](https://github.blog/2019-02-14-introducing-draft-pull-requests/). This can shorten dramatically approval and review time.

### Templates

When you need to create a new project repository, use a [repository template](https://github.blog/2019-06-06-generate-new-repositories-with-repository-templates/). Here are the available templates:

* [io-template-typescript](https://github.com/teamdigitale/io-template-typescript): For nodejs/Typescript projects
