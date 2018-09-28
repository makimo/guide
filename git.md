## Git conventions

### Overview

This document gives a concise account of the essential Git practices
used at Makimo. Before reading this document, ensure you've read the following:

1. [*Gitflow Workflow*](
https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

Git-flow is a standard practice at Makimo. As such, its solid understanding is
required. Supplementary reading:

1. [*How to Get Your Change Into the Linux
Kernel*](https://kernel.org/doc/html/latest/process/submitting-patches.html)
2. [git man pages](http://git-scm.com/doc)
3. [*Using Smart Commits*](
https://confluence.atlassian.com/fisheye/using-smart-commits-298976812.html)

### 1. Branches

* Whether implementing features or fixing bugs - always use a new branch.
  Whenever branching, choose _short_ and _descriptive_ names. Branch names
  are of the form `<type>/<name>`, where the name is comprised of words separated
  with hyphens (not underscores). Example:

```git
# Good, specific name.
git checkout -b fix/rabbitmq-dist-config

# Bad, too vague.
git checkout -b fix/logging
```

* Most common branch types are `feature`, `improvement`, `fix`, `docs`,
  `refactor`, `tests`, `style`, `chore` (updating config files, build tasks
  and the like). When branching to fix a bug or resolve an issue, include it
  in the name:

```git
git checkout -b fix/slug-56
```

where `slug` is the project slug from the issue tracker and `56` is the issue
number.

### 2. Commits

* Commit represents atomic change (on a conceptual level) made to the repository.
  It goes without saying that single commit should record a single, self-contained
  change. Readability of the git history crucially depends on that. Therefore,
  commits such as these ones should be avoided:

```git
3676983 Various fixes
b6d9a02 Add email template; add dist config
e62d25a Tests, tests, tests
```

And these:

```git
e62d25a Ups, forgot about print statement
b6d9a02 Add that feature
```

* If you've made a mistake or forgot about something and pushed to the
  origin, you can rewrite published history but *only* if you're working
  on a private branch and are positive that no one is using it.

* Generally speaking, you should avoid commiting removing forgotten prints
  and the like. You can either clean-up *before* publishing or made tweaks in
  batches.

There are two accepted commit formats: short and long.

#### 2.1 Long format

This format is used when commit needs explanation and/or is important.
Anatomy of the full commit is as follows:

```git
<title>
# Commit title is a *title* - capitalized, short (70 chars or less) summary
# without the dot at the end, written in imperative present tense: "Fix bug",
# not "Fixed a bug.".

<description>
# More detailed explanatory text, if necessary. Usually about few sentences,
# but may require more explainig (e.g. if solving intricate bug or
# describing new feature). Further paragraphs come after blank lines.

# - Bullet points are okay, too
# - Second line

<meta-information>
# This section is reserved for additional information, such as who signed-off
# or reviewed the change:
# Reviewed-by: <Franz Kafka fkafka@email.de>

# It is in good taste to include related issues, e.g.:
# Fixes: SLUG-77

# Most basic tags are "Fixes", "Resolves", "Reviewed-by".
```

Example:

```git
Ensure we return the error if someone kills a waiting layoutget

If someone interrupts a wait on one or more outstanding layoutgets in
pnfs_update_layout() then return the ERESTARTSYS/EINTR error.

Reviewed-by: Franz Kafka <fkafka@email.de>
Fixes: PNFS-191
```

The blank line separating the summary from the body is critical; tools like
rebase can get confused if you run the two together.

For other examples of excellent commit messages see Linux kernel git history.

#### 2.2 Short format

In case of short commit, used for small patches and tweaks, the format is as
follows:

```git
<title> [<issue>,] [<command>,]
```

where `<command>` is a smart commit command (see 3. in supplementary reading
above). For example:

```git
Some commit title ISSUE-51 #resolve #time 2h30m
```

### 3. Merging

* **Do not rewrite published history**. If it happens that someone already
  pulled a copy of the history being rewritten, conflicts emerge. To save
  everyone a headache, stick by this rule.

* However, you *can* rewrite private branches *before* publishing it to
  `develop` or `master`. It's easy to make mistakes - if you're 100% positive
  that your branch is not used by anyone on the team, feel free to tweak it.

* Never rewrite special branches, e.g. `master` (or any other used by CI).

* Keep the history *clean* and *simple*. Before pushing any branch to the
  public always ensure that it conforms to the style-guide. If it doesn't:
  squash, rebase, reword commits as necessary.

### 4. Misc.

* **Always** test before you push. Do not push something that may break.

* Don't push work half-done. If you're working on a private branch and want
  to back-up your data, clean-up before opening pull-request.

* Always check where are you pushing.
