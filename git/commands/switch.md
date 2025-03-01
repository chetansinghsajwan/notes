# Git `switch` command

This command is used to switches branches.

## Synopsis

```shell
git switch [<options>] [--no-guess] <branch>
git switch [<options>] --detach [<start-point>]
git switch [<options>] (-c|-C) <new-branch> [<start-point>]
git switch [<options>] --orphan <new-branch>
```

---
```shell
git switch [<options>] (-c|-C) <new-branch> [<start-point>]
```

This form is used to create a new branch wit name `<new-branch>`. 

---

## OPTIONS

`<branch>`: Branch to switch to.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt-ltnew-branchgt)<new-branch>

Name for the new branch.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt-ltstart-pointgt)<start-point>

The starting point for the new branch. Specifying a `<start-point>` allows you to create a branch based on some other point in history than where HEAD currently points. (Or, in the case of `--detach`, allows you to inspect and detach from some other point.)

You can use the `@{-N}` syntax to refer to the N-th last branch/commit switched to using "git switch" or "git checkout" operation. You may also specify `-` which is synonymous to `@{-1}`. This is often used to switch quickly between two branches, or to undo a branch switch by mistake.

As a special case, you may use `A...B` as a shortcut for the merge base of `A` and `B` if there is exactly one merge base. You can leave out at most one of `A` and `B`, in which case it defaults to `HEAD`.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt--cltnew-branchgt)-c <new-branch>

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---createltnew-branchgt)--create <new-branch>

Create a new branch named `<new-branch>` starting at `<start-point>` before switching to the branch. This is the transactional equivalent of

$ git branch <new-branch>
$ git switch <new-branch>

that is to say, the branch is not reset/created unless "git switch" is successful (e.g., when the branch is in use in another worktree, not just the current branch stays the same, but the branch is not reset to the start-point, either).

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt--Cltnew-branchgt)-C <new-branch>

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---force-createltnew-branchgt)--force-create <new-branch>

Similar to `--create` except that if `<new-branch>` already exists, it will be reset to `<start-point>`. This is a convenient shortcut for:

$ git branch -f <new-branch>
$ git switch <new-branch>

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt--d)-d

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---detach)--detach

Switch to a commit for inspection and discardable experiments. See the "DETACHED HEAD" section in [git-checkout[1]](https://git-scm.com/docs/git-checkout) for details.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---guess)--guess

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---no-guess)--no-guess

If `<branch>` is not found but there does exist a tracking branch in exactly one remote (call it `<remote>`) with a matching name, treat as equivalent to

$ git switch -c <branch> --track <remote>/<branch>

If the branch exists in multiple remotes and one of them is named by the `checkout.defaultRemote` configuration variable, we’ll use that one for the purposes of disambiguation, even if the `<branch>` isn’t unique across all remotes. Set it to e.g. `checkout.defaultRemote=origin` to always checkout remote branches from there if `<branch>` is ambiguous but exists on the _origin_ remote. See also `checkout.defaultRemote` in [git-config[1]](https://git-scm.com/docs/git-config).

`--guess` is the default behavior. Use `--no-guess` to disable it.

The default behavior can be set via the `checkout.guess` configuration variable.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt--f)-f

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---force)--force

An alias for `--discard-changes`.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---discard-changes)--discard-changes

Proceed even if the index or the working tree differs from `HEAD`. Both the index and working tree are restored to match the switching target. If `--recurse-submodules` is specified, submodule content is also restored to match the switching target. This is used to throw away local changes.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt--m)-m

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---merge)--merge

If you have local modifications to one or more files that are different between the current branch and the branch to which you are switching, the command refuses to switch branches in order to preserve your modifications in context. However, with this option, a three-way merge between the current branch, your working tree contents, and the new branch is done, and you will be on the new branch.

When a merge conflict happens, the index entries for conflicting paths are left unmerged, and you need to resolve the conflicts and mark the resolved paths with `git add` (or `git rm` if the merge should result in deletion of the path).

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---conflictltstylegt)--conflict=<style>

The same as `--merge` option above, but changes the way the conflicting hunks are presented, overriding the `merge.conflictStyle` configuration variable. Possible values are "merge" (default), "diff3", and "zdiff3".

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt--q)-q

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---quiet)--quiet

Quiet, suppress feedback messages.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---progress)--progress

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---no-progress)--no-progress

Progress status is reported on the standard error stream by default when it is attached to a terminal, unless `--quiet` is specified. This flag enables progress reporting even if not attached to a terminal, regardless of `--quiet`.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt--t)-t

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---trackdirectinherit)--track [direct|inherit]

When creating a new branch, set up "upstream" configuration. `-c` is implied. See `--track` in [git-branch[1]](https://git-scm.com/docs/git-branch) for details.

If no `-c` option is given, the name of the new branch will be derived from the remote-tracking branch, by looking at the local part of the refspec configured for the corresponding remote, and then stripping the initial part up to the "*". This would tell us to use `hack` as the local branch when branching off of `origin/hack` (or `remotes/origin/hack`, or even `refs/remotes/origin/hack`). If the given name has no slash, or the above guessing results in an empty name, the guessing is aborted. You can explicitly give a name with `-c` in such a case.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---no-track)--no-track

Do not set up "upstream" configuration, even if the `branch.autoSetupMerge` configuration variable is true.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---orphanltnew-branchgt)--orphan <new-branch>

Create a new unborn branch, named `<new-branch>`. All tracked files are removed.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---ignore-other-worktrees)--ignore-other-worktrees

`git switch` refuses when the wanted ref is already checked out by another worktree. This option makes it check the ref out anyway. In other words, the ref can be held by more than one worktree.

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---recurse-submodules)--recurse-submodules

[](https://git-scm.com/docs/git-switch#Documentation/git-switch.txt---no-recurse-submodules)--no-recurse-submodules

Using `--recurse-submodules` will update the content of all active submodules according to the commit recorded in the superproject. If nothing (or `--no-recurse-submodules`) is used, submodules working trees will not be updated. Just like [git-submodule[1]](https://git-scm.com/docs/git-submodule), this will detach `HEAD` of the submodules.
## References

- https://git-scm.com/docs/git-switch
