---
title: Git FUN!damentals
subtitle: Advanced miscellaneous
minutes:
---



## Rebasing

What happens when your feature branch diverges from the upstream master? This might happen if somebody else's changes get merged into master.

One way to deal with this is to `rebase` -- you'd essentially rewrite the git tree history so that the new commits on the master branch come *before* the ones you've made on your feature branch. However, you should only rebase your own branches and must generally not rebase any branch which you collaborate on with someone else.

The process would look something like this:

~~~{.input}
$ git checkout master
$ git pull upstream master
$ git checkout FEATURE-BRANCH
$ git rebase master
~~~

If any conflicts occur, fix the according files (by hand, unfortunately) and continue:

~~~{.input}
$ git add conflict-file1 conflict-file2
$ git rebase --continue
~~~

Finally, you must push your rebased branch:

~~~{.input}
$ git push --force origin FEATURE-BRANCH
~~~

You can also use `rebase` to go back and split up an old commit into multiple commits, to change commit messages or meta-data, to reorder commits, etc.  There are some [good examples in Pro Git](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History).

If you're curious, here's [an article on the dangers of rebasing](http://article.gmane.org/gmane.comp.video.dri.devel/34744). Be careful when you rewrite history!


## Stashing

I don't personally use `git stash` much in my workflow, but many people seem to love it.

From [Pro Git]():

>Often, when you’ve been working on part of your project, things are in a messy state and you want to switch branches for a bit to work on something else. The problem is, you don’t want to do a commit of half-done work just so you can get back to this point later. The answer to this issue is the git stash command.

> Stashing takes the dirty state of your working directory – that is, your modified tracked files and staged changes – and saves it on a stack of unfinished changes that you can reapply at any time.

~~~{.input}
$ mate fruit_list.txt
$ git add newfile
$ git checkout new_fruits
~~~

~~~{.output}
error: Your local changes to the following files would be overwritten by checkout:
	fruit_list.txt
Please commit your changes or stash them before you switch branches.
Aborting
~~~

~~~{.input}
$ git stash
~~~

~~~{.output}
Saved working directory and index state WIP on master: 0b6f7d7 PREVIOUS COMMIT MESSAGE
HEAD is now at 0b6f7d7 PREVIOUS COMMIT MESSAGE
~~~

Now, you can cleanly switch branches and develop on the feature branch. When you switch back to master and want to unstash your prior uncommitted changes, you can `pop` the last stash you did. This reapplies the changes to the index and working directory, and removes the stash from the stack.

~~~{.input}
$ git checkout master
$ git stash pop
$ git status
~~~


## Aliasing

You may want to create shortcuts for commands you use commonly, or add more advanced things to your git repertoire. Aliasing allows you to do that. Just like you'd add custom UNIX shortcuts in a bashrc file for the terminal, you can add aliases in your git config.

You probably type `git checkout` a lot, so let's shorten it to two letters:

~~~{.input}
$ git config --global alias.co checkout
~~~

Now `git co` does the same thing as `git checkout`.

I have a few cool ones set up in my config. `git log` will print the commit history, but you can actually customize *how* it prints the commit history. This alias tells git to print it like a graph and to use colors to highlight different pieces of information.

~~~{.input}
$ git config --global alias.lg=log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)[%an]%Creset' --abbrev-commit --date=relative
~~~

`git diff` will show you which lines have changed between the last committed version of a file and the edited version in your working directory. This alias will highlight the *words* that have changed.

~~~{.input}
$ git config --global alias.wdiff=diff --color-words
~~~
