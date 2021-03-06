Bug Triage Release Team Role
==============================

Your job for the release is to make sure that bugs (represented by GitHub issues PRs) which affect the release are dealt with in a timely fashion.  In general, you will be polling bugs, checking status, sending reminders and questions to contributors and SIG leads, and publishing summary reports.  The role has been interchangeably called "Bug Triage" and "Issue Triage", but the general term "issue" can be conflated with the specific GitHub usage of `is:issue` artifact type.  Especially as the requirement in kubernetes is no longer in place that PRs must have an associated issue, it is important to think of the role as broadly covering bug triage and covering both `is:issue` and `is:pr` GitHub artifacts.

Secondarily, you will be helping improve automation around artifact management and release tagging.

How this works depends on where you are in the release cycle.  There are five relevant periods where your workload changes:

1. Early Release: between Feature Freeze and a week before Code Slush.
2. Pre-Code-Slush: a week to 10 days before Code Slush
3. Code Slush: from Code Slush to Code Freeze, usually 3-4 days.
4. Code Freeze: from Code Freeze through Betas, until Release Burndown
5. Release Burndown: Less than 1 week until final release.

## How You Do Your Job

As Bug Triage lead, it is not your job to fix, label, sort, or gatekeep issues and PRs.  It is your job to get the SIGs, the issue or PR owners, and the key contributors to do it.  So whenever you find a bug that needs to be "fixed" or kicked out of the release, you go through the following escalation:

1. Leaving a comment on the GitHub issue or PR, e.g. "This issue hasn't been updated in 3 months.  Are you still expecting to complete it for 1.11?".  It's helpful here to @ mention individuals or SIG ```-bugs``` or ```-pr-reviews``` aliases, e.g. "@kubernetes/sig-node-bugs" or "@kubernetes/sig-network-pr-reviews".
2. Sending a message to the SIG channel or mailing list about the problem.  It's helpful here to condense multiple issues into a list, e.g. "Hey, these three issues haven't seen any attention, are they still valid for 1.11?"
3. Messaging individual owners and reviewers via Slack and/or direct email (GitHub notification emails must have been filtered or missed if you're past step #1).  Individual's email addresses may be harder to find than GitHub ID, but ure usually in the git commit history.  Slack handles are often harder yet to find.  There is no master list mapping human names to GitHub ID, email or Slack ID.  If you can't find contact info, asking in the appropriate SIG's Slack channel will usually get you pointed in the right direction.
4. Escalating to the Release Team Lead with suggestions on what to do with non-responsive issues.

In practice, you should fix anything simple that saves folks time and doesn't usurp the decision-making of the SIGs.  For example, adding/modifying kind and priority labels, or making PR labels match issue labeling.  However, you should never decide whether something is in or out of a milestone; the SIG or the Release Team Lead needs to do that.

## Early Release

You have no critical work during this cycle.

Instead, use the time to familiarize yourself with the major features and fixes planned by each SIG for this release, so that you build context in advance of when you will need to identify incoming bugs as being associated with a work focus in the current release.  It is a good time to interact with the Features Lead and CI Signal Lead to understand any early concerns they might have, as the release team's risk management comes as much from this proactive collaboration more as from the Bug Triage lead reacting to incoming issues and PRs.

This is also a good time to do any work on automation that you plan to do.  You can also get started early on the pre-code-slush work.

### Sample Searches

* Open Feature v1.11 Issues:
  * `is:issue is:open milestone:v1.11 repo:kubernetes/features` [k/features repo milestone features](https://github.com/kubernetes/features/issues?q=is%3Aopen+is%3Aissue+milestone%3Av1.11)
  * `is:issue is:open milestone:v1.11 label:kind/feature repo:kubernetes/kubernetes` [k/k repo milestone features](https://github.com/kubernetes/kubernetes/issues?q=is%3Aopen+is%3Aissue+milestone%3Av1.11+label%3Akind%2Ffeature)
  * `is:issue is:open milestone:v1.11 label:kind/feature org:kubernetes` [k org-wide milestone features](https://github.com/search?q=org%3Akubernetes+is%3Aissue+is%3Aopen+milestone%3Av1.11+label%3Akind%2Ffeature)
* Issues in the v1.11 milestone which haven't been updated in a while: `is:open is:issue milestone:v1.11 updated:<2018-05-01 repo:kubernetes/kubernetes` [k/k old issues in milestone](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+milestone%3Av1.11+updated%3A%3C2018-05-01+repo%3Akubernetes%2Fkubernetes)
* Issues in the milestone with no SIG or Kind: **code/automation needed**
* Feature Issues with no PR: **code/automation needed**

### Reports

No reports are required during this period, although you might consider setting up a red/yellow/green report template.

## Pre-Code-Slush

During this period your job is to make sure that all issues and PRs which are related to features for the upcoming release have reasonable labels.  While you may have been doing that some earlier, now you have a deadline.  From the beginning of Code Slush every feature issue or PR should be linked to the milestone.  Specifically:

* each Feature issue should have a milestone, kind, and sig.
* PRs linked to these feature issues should have the same labels (and milestone)
* priority should be important-soon or critical-urgent

If issues do not match the above, you should comment and urge the SIGs to fix them, or even fix some labels yourself (particularly kind and sig).

The second thing you need to do is to start filtering issues which have been assigned to the milestone, specifically getting SIGs to change milestones on issues which are not making progress.  For example, if there's an issue with no consensus on an approach to fix it, and no PRs, you should comment and suggest that that issue be taken out of the milestone.

The third thing you should do, time permitting, is to scan through the newly opened issue and PR lists for bugs to see if they're failures against the upcoming release features, especially critical ones.  For ones which are, you should ensure that they have all the correct labelling to be tracked and updated.  Depending on volume and time, you may wish to exclude ```label:"kind/feature"``` and ```label:"priority/failing-test"``` from your searches, as those are covered by the Features and CI Signal leads respectively.

### Sample Searches
* Open v1.11 Issues
   * `is:open is:issue milestone:v1.11 -label:"kind/feature" -label:"priority/failing-test"` [milestone open issues, excluding kind/feature and priority/failing-test](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+milestone%3Av1.11+-label%3A%22kind%2Ffeature%22+-label%3A%22priority%2Ffailing-test%22+)
* Open v1.11 PRs
   * `is:open is:issue milestone:v1.11 -label:"kind/feature" -label:"priority/failing-test"` [milestone open PRs, excluding kind/feature and priority/failing-test](https://github.com/kubernetes/kubernetes/pulls?utf8=%E2%9C%93&q=is%3Aopen+is%3Apr+milestone%3Av1.11+-label%3A%22kind%2Ffeature%22+-label%3A%22priority%2Ffailing-test%22+)

### Reports

No reports are required during this period, although you should be maintaining a red/yellow/green report template starting now.

## Code Slush

At the beginning of Code Slush, all issues to stay in the milestone need to have the following characteristics.  PRs for the milestone should share the same labels.

* kind, sig, and milestone labels
* status/approved-for-milestone

All bugs should also show progress towards resolution and that they're getting attention between Code Slush and Code Freeze.  If they're not, you need to get the attention of the SIG(s) on those specific bugs, and find out if they're going to fix them or target the fix for a future milestone instead.  Also, SIGs need to make a decision on "approved-for-milestone" during Code Slush; you need to remind them to do so on each bug.

Even when bugs have PRs resolving them, these PRs can get stuck in the approval process. This means it's your job to remind SIG leads of any stuck PRs until they get approved and merged.

Checking newly-reported test failures is now more urgent; you should assume that any new failure is related to the upcoming release, bring to the attention of the CI Signal lead, and assist in getting follow-up from the appropriate SIG.  Generally the CI Signal lead will track these failures, but newly opened issues may not initially have sufficient labelling to catch their attention.

Issues and PRs missing correct labels will begin to see reminders from the bot.  While the bot gives clear instructions, you may still need to help owners understand what they need to do.

### Sample Searches

* Open v1.11 Issues
  * `is:open is:issue milestone:v1.11 -label:"kind/feature" -label:"priority/failing-test"` [milestone open issues, excluding kind/feature and priority/failing-test](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+milestone%3Av1.11+-label%3A%22kind%2Ffeature%22+-label%3A%22priority%2Ffailing-test%22+)
  * `is:open is:issue milestone:v1.11` [milestone open issues, no exclusions](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+milestone%3Av1.11+)
* Open v1.11 PRs
  * `is:open is:pr milestone:v1.11 -label:"kind/feature" -label:"priority/failing-test"` [milestone open PRs, excluding kind/feature and priority/failing-test](https://github.com/kubernetes/kubernetes/pulls?utf8=%E2%9C%93&q=is%3Aopen+is%3Apr+milestone%3Av1.11+-label%3A%22kind%2Ffeature%22+-label%3A%22priority%2Ffailing-test%22+)
  * `is:open is:pr milestone:v1.11"` [milestone open PRs, no exclusions](https://github.com/kubernetes/kubernetes/pulls?utf8=%E2%9C%93&q=is%3Aopen+is%3Apr+milestone%3Av1.11+)

### Reports

Starting with Code Slush, you should report on issue status at least three times per week.  This report will break down into red/yellow/green issues and PRs.  As you get closer to release, the specific definitions of red/yellow/green will change, but the basic idea won't:

* red: issues/PRs which don't seem to be headed towards any resolution on deadline, and represent either major breakage or features still in the release
* yellow: issues/PRs which are in progress but need to be watched
* green: issues/PRs which still show up on the reports, but are on their way to being resolved very soon

In Code Slush, these three levels are:

* red: major breakage issues which are both old and getting no attention at all, and feature issues without a PR.
* yellow: major breakage issues with a PR in progress, minor breakage issues, and feature issues with a PR which is in progress but still needs work.
* green: minor breakage issues with a PR and feature issues with a PR that looks to be approved soon.

You should also identify to the release team a "leader board" of those areas which appear to need major attention (e.g. upgrade testing, some specific feature).

The Release Lead will include a template for this information in the Monday/Wednesday/Friday release team meeting notes.

## Code Freeze

Once Code Freeze starts, all issues still in the upcoming release **must** have labels of `approved-for-milestone` and `priority/critical-urgent`.  This means that on the morning before Code Freeze begins you need to go through the open issues (and PRs) which are approved-for-milestone but not marked critical-urgent and poke the owners reminding them that in the next day Code Freeze will mean any issues not marked both approved-for-milestome and critical-urgent will be kicked out of the milestone by the bot.

After this, you need to monitor all of these issues to make sure that daily progress is made on them.  Theoretically, issue owners/SIGs are supposed to make a daily status comment in the issue, but this is seldom followed. You also will need to send daily reminders/queries about stuck PRs.

New test failures will also show up during Code Freeze and you need to make sure that these are labeled properly, get attention of the CI Signal lead, and get attention from the appropriate SIGs.

As Code Freeze progresses, you should get increasingly aggressive about getting SIGs to kick out any issue which doesn't represent substantial breakage in the new release.  Particularly, new features which aren't making rapid progress need to either jump to the next release, or reduce their scope.  You can remove issues from the release by downgrading their priority, but it's really better if the SIGs do it.  Start watching for issues with the release version string in them, eg: "1.11.0-beta".

### Sample Searches

* Open v1.11 Issues
  * `is:open is:issue milestone:v1.11 -label:"kind/feature" -label:"priority/failing-test"` [milestone open issues, excluding kind/feature and priority/failing-test](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+milestone%3Av1.11+-label%3A%22kind%2Ffeature%22+-label%3A%22priority%2Ffailing-test%22+)
  * `is:open is:issue milestone:v1.11` [milestone open issues, no exclusions](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+milestone%3Av1.11+)
  * `is:open is:issue created:>2018-06-14` [open issues, no exclusions, created since you last looked](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aopen++created%3A%3E2018-06-14)
  * `is:open is:issue modified:>2018-06-14` [open issues, no exclusions, modified since you last looked](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=is%3Aopen++modified%3A%3E2018-06-14)
* Open v1.11 PRs
  * `is:open is:pr milestone:v1.11 -label:"kind/feature" -label:"priority/failing-test"` [milestone open PRs, excluding kind/feature and priority/failing-test](https://github.com/kubernetes/kubernetes/pulls?utf8=%E2%9C%93&q=is%3Aopen+is%3Apr+milestone%3Av1.11+-label%3A%22kind%2Ffeature%22+-label%3A%22priority%2Ffailing-test%22+)
  * `is:open is:pr milestone:v1.11"` [milestone open PRs, no exclusions](https://github.com/kubernetes/kubernetes/pulls?utf8=%E2%9C%93&q=is%3Aopen+is%3Apr+milestone%3Av1.11+)
  * `is:open is:pr created:>2018-06-14` [open PRs, no exclusions, created since you last looked](https://github.com/kubernetes/kubernetes/pulls?utf8=%E2%9C%93&q=is%3Aopen++created%3A%3E2018-06-14)
  * `is:open is:pr modified:>2018-06-14` [open PRs, no exclusions, modified since you last looked](https://github.com/kubernetes/kubernetes/pulls?utf8=%E2%9C%93&q=is%3Aopen++modified%3A%3E2018-06-14)
* Reports against the beta(s):
  * `is:issue "1.11.0-beta"' [issues mentioning beta version](https://github.com/kubernetes/kubernetes/issues?utf8=%E2%9C%93&q=%221.11.0-beta%22)
  * `is:pr "1.11.0-beta"' [PRs mentioning beta version](https://github.com/kubernetes/kubernetes/pulls?utf8=%E2%9C%93&q=%221.11.0-beta%22)

### Reports

We're still doing the red/yellow/green reports:

* red: major breakage issues which are not making daily progress, and feature issues without a PR which looks to be approved soon.
* yellow: major breakage issues with a PR in good shape, minor breakage issues, and feature issues with a PR in good shape.
* green: minor breakage issues with a good PR and feature issues with a PR that is just waiting on merge.

There will also be several features which will have requested exceptions from the normal release timeline/requirements.  You'll want to track the exceptions specifically and report on them.  It is useful to be tracking in a spreadsheet the open issues/PRs which have caught your attention, because GitHub queries are only point in time.  It's easy for an issue you want to follow to stop showing up in a query and thus fall off your radar if you're only going by the queries.

The Release Lead will include a template for this information in the Monday/Wednesday/Friday release team meeting notes.

## Release Burndown

Starting a few working days before the first Release Candidate, we go into Sudden Death Overtime.  At this point, you need to make sure that three things happen:

1. major breakage bugs get fixed immediately
2. any pending PRs get approved and merged
3. anything else gets kicked out of the release

During this period, it's reasonable to expect issue owners and SIG leads to get back to you within hours (check their time zones, though).  In cases where SIG Leads are unavailable, you may need to appeal to Kubernetes project leaders to deal with stuck PRs.

### Sample Searches

Same as in code freeze.  Here it becomes critical to have recorded the results of prior queries to compare what issues/PRs have come into or left from the query results compared to the last time you ran a query.

### Reports

During Release Burndown, you need to report on issue status at least once per day.

* red: major breakage issues or feature issues without a PR which looks to be approved very soon.  Minor breakage issues with no PR.
* yellow: major breakage issues with an nearly-approved PR, minor breakage issues with a PR, and feature issues with a nearly-approved PR.
* green: issues and PRs which are just waiting on being taken out of the milestone.

The Release Lead will include a template for this information in the daily Monday through Friday release team meeting notes.

During this short period it may also be necessary to check in on and report out status changes on the weekend.  This should be an exception versus the norm because all the prior months' work by the release team and community has led to well managed and understood risks, but surprises do happen.
