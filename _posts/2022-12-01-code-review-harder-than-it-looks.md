---
layout: post
title: "A good code review is harder than it looks"
category: ["team collaboration"]
tags: [GitHub, "pull request"]
---

Opening easy to review pull requests, giving relevant feedback, or responding to feedback is hard.
It requires a good mix of soft and hard skills, which are often overlooked.

Like any activity, practice makes perfect, and I believe anyone can become better at it.
<!--more-->

In this blog post, I summarized all the best practices I learned along the years.
## One does not open a pull request in one click!

It is an everyday situation to many developers: after a long and draining coding session during which hundreds of lines of codes have been written, all the static analysis tool lights are green and the feature you've been working on for the past few days is finally ready.

It is then tempting to open a new pull request, give it a title, immediately click on the "Create pull request" button and ask for a review so that you can move to something else.

However, one crucial step is missing... filling in the description field of the pull request.

It's an ideal place to explain the motivation behind introduced changes, depict implementation choices, and ease the process for the person who is going to review the code. Broadly used in open source software, where the reviewer does not necessarily personally know the reviewee, it is too often overlooked in the "Proprietary" world.

A comprehensive pull request description should, at least, contain the following information.

### Context, context and more context!

The best way to start a pull request description is to explain what triggered the need for changes.

Could it be a user story, an issue escalated by the Support team, a refactor opportunity discovered during a pair-programming session, the desire to test a new tool, performances issues, or anything else.

👉 _It is the “why ?” of the pull request_

### A single and unambiguous goal

Si le but de la pull request ne peut être résumé en une courte phrase, c’est souvent un symptôme indiquant que la PR est trop grosse et introduit trop de changements. -->
What are we achieving merging this pull request ?

When the pull request goal cannot be easily described in one short sentence, it is often a symptom signaling that the pull request is too big and introduces too many changes.

👉 _It's the “what ?” of the pull request_

### Details about implementation strategy

Documenting implementation in a pull request description can only be beneficial.

For instance, one can explain why design pattern X was used, how modules are bounded, why Y naming convention was chosen, or with which constraints was the code written.

The reasoning behind the chosen implementation is really important.
It will influence the reviewer mindset and give him/her all the intangible information needed to perform a relevant code review.

👉 _It is the "how" of the pull request_

### We want results!

When applicable, giving pointers on how to test or visualize the introduced changes will greatly ease the review process.

With web development, many changes are visual.
If the setup allows it, sharing a link to a staging or review app to let the reviewer test the changes himself/herself is a must.
Even if this isn't the QA stage yet, being able, as a reviewer, to first test and visualize the changes implemented will improve the quality of the review.

---

To standardize pull requests descriptions, GitHub let us define [pull request templates][pull_request_template].

Here is an example of a template I've been using with different teams

{% gist 87f7677c187e8e6112b9954667648106 %}

And some real life examples:

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/pr_template_example_1.png" alt="Pull Request Template example 1" %}

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/pr_template_example_2.png" alt="Pull Request Template example 2" %}

Every project can use a different pull request template. I find it always interesting to browse around some open source project for inspiration.
When looking at [VueJS][vuejs_pr_template], [Atom][atom_pr_template], [ESLint][eslint_pr_template] or [Webpack][webpack_pr_template] templates, we can quickly see what matters the most for each team.

> When I write a pull request description, I keep in mind that the person who is going to do the review does not know anything about the changes I implemented. I do it even when I know it's not the case, it helps me take a step back.

Also, it is so pleasing, when doing a `git blame` to come across a well documented pull request written a few months ago. One can effortlessly dig into it.

## Pull request best practices

On top of having a solid description, here are some best practices when opening a pull request.

### Small PR = easy to read

The bigger the pull request, the more complex the review process is.

When reviewing a pull request, the attention level of a reviewer will inherently decrease after about ten minutes. Leading to bugs hitting production while they could have easily been discovered during the code review.

Even on big projects or feature, it is often easy to break up changes in multiple small pull requests.

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/pr_big_vs_small.png" caption="For a reviewer, the second pull request is more appealing than the first one" %}

### The first reviewer of every pull request should be its author.

Before sending an important email, we all take the time to proofread it, chasing after spelling mistakes and making sure no important information is missing.

It is a good habit to so the same before hitting the create button for a new pull request. A quick scan over the git diff allows to easily spot mistakenly committed files, unadvisedly commented out lines of codes or typos.

Reviewers can notice obvious hints giving away that the author didn't proofread his/her pull request.
Could it be a local file being versioned by mistake, or a comment intended to be removed before the pull request (ex: "TODO: extract logic into private method").

Not the best way to start a review, and so much credibility lost for the author of the PR.

### Commit messages... a story in itself

It is common while coding to do many iterations, to fumble around, to go backward. We then end up with a commit history like the one below.

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/commits_history_mess.png" alt="Commit History Mess" %}

Before opening a new pull request, it is wise to take some time to rework on the commit history. Thanks to [GIT interactive rebase][interactive_rebase], we can easily move, squash, reword or edit commits.

Ideally, a commit history should tell a story. In some cases, it is also advised to do a code review commit by commit.

### Avoid inopportune code linting

While working on a feature, and opening an old file, it is always tempting to correct all the offenses found by the static analysis tool, to reorder methods or to rename some random variables.

It is however counterproductive as it will introduce a lot of noise for the reviewer.

Ideally, we should open a first pull request dedicated to code linting. Or at least, do all the code linting in a single commit and facilitate the review commit by commit.

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/linting_in_pr_diff.png" alt="Linting in PR diff" caption="It is hard to spot the actual changes among all the code linting" %}

## Why go to all that trouble?

The hidden goal here is to reduce average lead time between pull requests creation and completion. It will improve an overall codebase quality without impacting the team velocity.

We can break down the lead time in the following way:

* the elapsed time between a pull request being opened and a team member actually starting to review
* the time it takes for the reviewer to actually carry out the review
* the implementation time of potential changes triggered by the code review

The idea is to make the life of the person who is going to review the pull request easier. It will impact the actual review time, but also the delay between pull requests openings and beginning of code reviews. As a reviewer, if I foresee that a review won't be too much time-consuming, I'll tend to prioritize it.

## By the way, how can we give good feedback while reviewing code ?

Now that we have seen some tips on how to write a good pull request, the reviewer may come into play.

Reviewing code can be tricky and time-consuming.
Here are some best practices to keep in mind while reviewing code.

### “Ask, don’t tell”

Asking open-ended questions nudge to open discussions instead of creating frustration.

> J’ai remarqué que les logiques entre X et Y sont très proches. Est ce que tu penses qu’il y a moyen de factoriser le code ? -->

For instance, if during a code review, I spot that some logic has been duplicated. I won't write

> Code duplication !

but instead

> I noticed that the logic between X and Y is fairly similar. Do you think there is an opportunity for code factorization here ?

### Ask for clarifications

As a reviewer I am not omniscient, I often ask for clarifications while reviewing code. It can be a sign that the code is not explicit enough, or I just might have missed something important.

> I'm not sure to fully understand this validation. Does it ensure two students cannot ask two questions at once ?

> What is this method doing ? Based on the name I'd think it does X but when I look at the code it looks like it does Y

### Cognitive bias can affect your writing

Without voice intonations or facial expressions, it is harder to convey an intention in a written exchange than in a verbal one.

A neutral question such as:

> Did you take the time to test this method before opening this pull request ?

...will be perceived negatively when being read.

Similarly, sarcasm is very hard to spot when written.

Luckily, there is no shortage of emojis 😀😁😃😄😅😆😉😊😋😎

### Explicit > implicit

It can seem obvious, but, the more explicit we are with feedback, the better it is.

When the author of a pull request answer the reviewer feedback asking for some clarification, it often means that the feedback given wasn't good enough.

Instead of writing

> We can use `map` here

One could write

> I noticed this pattern where you loop over an array, do some computing on each element, and save the result in another previously instanced array. For such logic, the `map` method of the `Enumerable` module can be used. You can find some good examples here: https://...

### One single problem, hundreds of solutions

As a reviewer, for a given problem, it is important to make the distinction between a valid implementation different from the one he/she would have used from an invalid implementation.

There are almost always several possible paths to reach the same goal.

Taking a step back could also let the reviewer discover new paths. The review is not one-sided.

### It's tool late to challenge architectural or design choices

> Why don't we create the concept of "credit" in our codebase to deal with discounts ?

> Should we use Redis instead of PostgreSQL for this feature ?

While valid points, this kind of questions, challenging the whole implementation strategy of a new feature during a code review, are often a symptom of poor technical specifications or lack of communication.

Having dedicated instances with its team to come up with implementation strategies before writing a single line of code will avoid such frustrating situations.

### Highlighting best practices

Code reviews feedback is not always negative. It is also a great way to give positive feedback to teammates or express gratitude when leaning something new when reading someone else code.

> ❤️ the way you handled the corner case for users with temporary passwords, very clever :)

> I wasn't aware of the `zero?` method on number. So useful ! I'll start using it from now on.

### Offer help

When I give feedback that implies substantial or very specific rework, I like to offer my help to the pull request author to implement the changes.

> I deeply feel that the Composite pattern would be a great fit for this implementation. What do you think about it ? Do you want to do some pair-programming together over it ?

### Don't bother with code styling

Code reviews are not the right place to open debate on code styling

> I'd like us to use spaces instead of tabulations.

> Why didn't you use CamelCase in your variables names ?

This kind of feedback do not add value to the code review. There are many statistical analysis tools available. Robots are excellent at this job.
Best practice is to have a style guide already defined for and by the team.

### Contemplate peer code review

There are cases where one does not really know how to begin a code review, or feel overwhelmed by the task ahead of him/her. In such cases, why don't go ask the pull request author to do the code review directly with him/her.

It is usually very efficient

### Response time to code review is important

If a reviewer waits four days and three follow-ups to start reviewing a pull request, chances are he/she will be bypassed next time.

Dedicating some time during the day for reviewing code is a must. It is not something a developer does on top of its current workload, it is work.

When evaluating developer performances, we tend to focus too much on its velocity and the quality of code he/she produces.

I like to build a strong and healthy code review culture in teams I work with and praise good code reviewing skills.

## How to respond to feedback

### Don't let your ego get in the way

It is most likely the hardest part of a code review. Personally, it took me some time. when I first started coding professionally, to understand that code reviews are about code, not people.

After realizing it, it was easier to be in the right mindset to reply to feedbacks given on my pull requests. Not needing to be on the defensive and instead welcoming it as a way to improve my code.

However, it doesn't mean that we have to accept 100% of a code review feedbacks. After all, we are all opinionated.

### Don't let yourself being overwhelmed

Quite often, good ideas are born during code reviews.

When they are out of scope from the original pull request, a good habit is to take not and extract it in another ticket or an issue.

That way, it can be prioritized and addressed in a following pull request or at a later stage and do not impede the current pull request to hit production as soon as ready.

### Reply to all feedbacks

If not, how will the reviewer know if the pull request author actually took into account its feedback ?

{% include image.html url="/assets/image/posts/2019-06-07-code-review-harder-than-it-looks/github_emojis_reaction.png" alt="GitHub emoji reaction" caption="GitHub allows using emojis to quickly respond to feedback" %}

### Take it offline when needed

When there is a lot of back and forth in a code review, lengthy messages, and no clear convergence point, a good idea is to discuss directly and verbally the subject with the reviewer.
I'm always pleasantly surprised at the efficiency of a short 15 minutes talk on a topic compared to hours of written exchanges.

Giving and receiving relevant feedback in code reviews is not an innate ability. But with some practice it easily becomes a reflex.

_Note: this post was originally published in French on [LiveMentor's Tech team Medium][original_post]_

---

## Sources

[https://github.com/blog/1943-how-to-write-the-perfect-pull-request](https://github.com/blog/1943-how-to-write-the-perfect-pull-request)

[https://github.com/thoughtbot/guides/tree/master/code-review](https://github.com/thoughtbot/guides/tree/master/code-review)

[http://kevinlondon.com/2015/05/05/code-review-best-practices.html](http://kevinlondon.com/2015/05/05/code-review-best-practices.html)

[https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message)

[pull_request_template]: https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository
[vuejs_pr_template]: https://github.com/vuejs/vue/blob/dev/.github/PULL_REQUEST_TEMPLATE.md
[atom_pr_template]: https://github.com/atom/atom/blob/master/.github/PULL_REQUEST_TEMPLATE/feature_change.md
[eslint_pr_template]: https://github.com/eslint/eslint/blob/master/.github/PULL_REQUEST_TEMPLATE.md
[webpack_pr_template]: https://github.com/webpack/webpack/blob/master/.github/PULL_REQUEST_TEMPLATE.md
[interactive_rebase]: https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History
[original_post]: https://medium.com/livementor-product/la-code-review-un-exercice-plus-difficile-quil-n-y-parait-7959c872fb2
