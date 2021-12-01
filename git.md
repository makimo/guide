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
  with hyphens (not underscores).

* Most common branch types are `feature`, `improvement`, `fix`, `docs`,
  `refactor`, `tests`, `style`, `chore` (updating config files, build tasks
  and the like). The name should be human-readable, and briefly but clearly
  describe the work the branch contains. Example:

  ```git
  # Good, specific name.
  git checkout -b fix/rabbitmq-dist-config

  # Bad, too vague.
  git checkout -b fix/logging

  # Bad, too specific. It's better to use more human-readable
  # name and put issue reference in the title/description instead.
  git checkout -b feature/PROJ-1234
  ```

* When feature branch is removed from the remote (usually origin) it is
  a good idea to also remove local branches to keep repository clean and
  fast. To remove stale branches from local repository first we need to
  prune it (in other words: remove upstream references to branches that
  have been deleted on the `origin`):

```git
git fetch --prune <remote_name>
```

  This will fetch all the refs from `<remote_name>` and prune the stale
  references. There is also possibility to prune references without
  fetching:

```git
git remote prune <remote_name>
```

  It is important to remember that pruning will not remove local
  branches. To do this, checkout one of the main branches (`master` or
  `develop`) and run the following script:

```bash
git fetch --prune <remote_name> && git branch -vv \
    | grep ': gone]' \
    | awk '{print $1}' \
    | xargs git branch -d
```

> NOTE: this will only work on systems on which `grep`, `xargs` and
> `awk` are available.

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
# not "Fixed a bug." or "fixed a bug".

<description>
# More detailed explanatory text, if necessary. Usually about few sentences,
# but may require more explainig (e.g. if solving intricate bug or
# describing new feature). Further paragraphs come after blank lines.

# Bullet points are okay too, examples:
#
# - A fix to a quite embarassing issue where raw_copy_to_user() was
#   implemented with asm_copy_from_user() (and vice versa).
#
# - Improvements to our makefile to allow flat binaries to be generated.
#
# NOTE: Description, including bullet points, consists of normal sentences.
# Therefore, usual English grammar applies.

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
Fix nasty little bug ISSUE-51 #resolve #time 2h30m
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
  
* When merging multiple small commits into a single change, make sure to
  keep high locality of the changes. In another words, if you're changing,
  let's say, some elements of the UI, group changes
  that occur in small vicinity of each other.

* When merging multiple changes into one, make sure to _do not exceed
  limit of 10 changes in one commit_. Additionally, always include
  ticket/issue numbers in the commit log. For example:

  ```git
  Improve user-experience in subscription views PROJ-10

  This batch includes:
  * Tweak padding between buttons PROJ-13
  * Ensure proper scrolling of the list-view PROJ-22
  * Fix alpha layer on the toolbar PROJ-19
  ```
  
  where `PROJ-10` is a super-ticket for all subsequent issues.

### 4. Rewriting history

#### 4.1 Rewording last commit

Perhaps the most straightforward history rewrite is changing the commit
message. If the commit in question is the *last* one, the task is trivial:

```git
git commit --amend
```

This command will open an editor (configured with `git config core.editor`)
with last commit for you to modify. After the changes are saved, git will
update your *local repository*.

---
Please note that this will only work if you didn't push your changes to the
remote. In other cases, you will have to use push --force which is invasive
and dangerous operation. Before pushing your changes do your best to make sure
they are ready to be published to avoid the necessity to rewrite the history.

#### 4.2 Adding/removing changes to/from last commit

If you forgot to include a change in the last commit, or if you want to remove
something, you can do it with the following commands:

```git
git add [<file>,]
git rm  [<file>,]
git commit --amend --no-edit
```

---
As was the case in the previous paragraph, in this situation too you have to
be sure that commit was not pushed to the remote.

#### 4.3 Rebasing

##### 4.3.1 Usage

In order to change commits further back in the history, we will have to use
more advanced tools. Enter `rebase`. `git rebase` re-applies commits,
one by one, from your current branch onto the HEAD they were originally based
on. When used in the interactive mode, `rebase` works as follows:

1. You specify how far into history you want to go. You can either
a specify single commit or a relative count from the `HEAD` (`i` stands for
_interactive_):

```git
git rebase -i d8b9bd # Rewrite from the specified commit to HEAD
git rebase -i HEAD~3 # Rewrite three last commits
```

2. In the next step, `git` opens up an editor with list of commits which looks
something like this (for `git rebase -i HEAD~3`):

```git
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file

# Rebase 710f0f8..a5f4a0d onto 710f0f8
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Here you can tell git how you want to proceed with your rewrite. Think of it
as a script. In this script you can:

1. `pick` - To "pick" a commit means to include it in the rewrite. If you were
to remove the line `pick a5f4a0d added cat-file` from the script, git would
remove this commit from the history.

2. `edit` - This tells the script to stop at the relevent commit and let you
apply changes you make to make to it. When git stops the script, you will see
a message similar to the following:

```git
$ git rebase -i HEAD~3
Stopped at f7f3f6d... changed my name a bit
You can amend the commit now, with

       git commit --amend

Once you’re satisfied with your changes, run

       git rebase --continue
```

At which point you can whatever you want with the commit and all changes
will be included in the rebase.

3. `reword` - This is a shorthand for `edit` which, instead of giving you
full control, asks only for the new message for the commit.

4. `squash` - Squashing a commit means "melding" it with the previous one.
When you squash two commits, git will combine the changes and the messages
and ask you for the new one. With `squash` it is crucial to understand the
order of operations. If you're unsure which commit gets squashed with which
commit, please be sure to read section 4.4.

5. `fixup` - Version on `squash` which completely discards message of the
commit being squashed. This is handy when you made a change, realised you
forgot about something and added the fix in the next commit. In such case
you want to discard "fixed typo from the previous commit"-type message.

6. `exec` - This option allows to run a command *between* two commits in the
rebase script. As such, it does not tag any specific commit - it is inserted
between the two. Let's say we want to run `echo "Woah, this is cool"` after
some commit. Then, we would write:

```git
edit 310154e updated README formatting and added blame
exec echo "Woah, this is cool"
squash a5f4a0d added cat-file
```

`exec` can also be given the option to `rebase` with `--exec`. In such case,
specified command will be run after _every_ step in the rebase script.

---

Additionally, by re-ordering commits in the rebase script, you can re-order
commits in the history (if applicable).

##### 4.3.2 Order of operations

It is important to keep in mind that commits in the rebase script are listed
in the reverse order (with respect to `git log`):

```git
git log --pretty=format:"%h %s" HEAD~3..HEAD
a5f4a0d added cat-file
310154e updated README formatting and added blame
f7f3f6d changed my name a bit
```

Notice the reverse order of the rebase script in the previous paragrah.
The interactive rebase generates a script which is going to run starting
with the commit at the up and ending with the commit at the bottom. Therefore,
for options such as `squash` and `fixup`, previous commit it the commit above.

##### 4.3.3 Precautions

Because `git rebase` changes hashes of the commits being rewritten, it is only
safe when working with your local repository.

Generally, you should not be rewriting remote branches. However, if you're sure
that no one is using the branch (e.g. you're working on a private feature
branch) you can apply rebase this with `push --force` (!).

##### 4.3.4 Further reading

1. [Git Tools - Rewriting History](
https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

2. [Git Interactive Rebase, Squash, Amend and Other Ways of Rewriting History](
https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history)

3. [git-rebase man pag](https://git-scm.com/docs/git-rebase)

### 5. Work-In-Progress

Sometimes during work on a large feature, there is a necessity to commit
partially finished job (for example: because the workday is ending and
you want to back up your work in case of an outage). In such cases, the
commits should be marked with `WIP:` prefix in the title. It would be
a good idea to include the short summary for finished parts of the task
in the commit description.

```
WIP: Add some long, mundane feature

So far the I have refactored the `MainView` and `SecondaryView` to get
them ready for this awesome new feature.
```

Such commits can be **only** pushed to the private feature branches and
**must** be removed (with rebase or amending commits) before the pull
request is created. **The existence of `WIP:` commits in the history of
revised changes may be a reason of declining the pull request.**

Although the existence of `WIP:` commits on the private feature branches
is acceptable, you are strongly encouraged to keep the parts of your job
as simple and short as possible to avoid the need to commit partially
finished job.

### 6. Tags

#### Adding a tag

```git
git tag <tag-name>
```
#### Pushing a tag to remote

```git
git push --tags
```

#### Deleting a tag locally

```git
git tag -d <tag-name>
```

#### Deleting a tag on remote

```git
git push --delete <origin> <tag-name>
```

### 7. Misc.

* **Always** test before you push. Do not push something that may break.

* Don't push work half-done. If you're working on a private branch and want
  to back-up your data, clean-up before opening pull-request.

* Always check where are you pushing.
