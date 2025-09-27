---
title: "Jujutsu (jj) Conflicts"
date: 2025-09-27T15:24:15+02:00
summary: Jujutsu is a Git-compatible version control system that treats conflicts as first
  class citizens. In this post I explore how to work with and resolve such conflicts.
tags:
  - software-development
draft: true
---

I want to explore a bit more what a conflict is, how to interpret the conflict marker output and give practical tips on how to resolve conflicts.

Let's say we have a repository with a single file fruit.txt

```bash
jj git init --colocate jj-demo
cd jj-demo
echo "apple
grape
orange
" >> fruit.txt
jj commit -m "initial commit"
```

Now we make conflicting changes to it:

```bash

```

When we try to merge those changes:

```
jj new
```

We end up with a conflict.

One nice thing about jj is that this does not stop us from doing work. For example I could add another change on top of the conflicting one without any issues.

```
echo "chair
table
couch" >> furniture.txt
jj commit -m "add furniture.txt"
```

Of course our options of getting work done are still somewhat limited. The file fruit.txt contains some strange content. For example if this was code in a programming language then our program would not compile right now.

We could, however, get some work done somewhere else and the conflict would just remain behind waiting to be fixed.

```
echo "cucumber
mushroom
carrot" >> vegetables.txt
jj commit -m "add vegetables.txt"
```

Now we could for example rebase the conflicting change on top of the new change we just created. Neat, right?

Something else we can do is create yet another change to fruit.txt and merge that in, what would happen?

```
echo "lemon" >> fruit.txt
jj commit -m "add lemon to fruit.txt"
```

Now merge that into the conflicting change.

```bash
jj new asdf asdf
```

So even though the change is in conflict, we can still work with it.

Let's go back to our first the conflict.

What does jj store internally here? Multiple tree objects.

Those conflict markers we see in the output of fruit.txt are not stored like that. They are only added when necessary e.g. when the working copy includes a file that contains conflicts. This is called materializing the conflict.

How do we interpret the materialized conflict output?

`>>>>` and `<<<<` indicate the start and end of a conflict.

`%%%%%%%` indicates the start of a snapshot

`#####` indicates the start of a diff.

The numbers e.g. `#1`, `#2` tell us who contributed which version to the conflict.

This format seems very strange and unintuitive at first (but it's not!). Let's first look at another format that may seem some convenient at first glance.

Only snapshots.


We can also show the format as as Git 3 side diff.

Let' go back to the default format.

What does it all mean:
* Snapshot tells you what that side of the conflict wanted the final result to look like
* Diff tells you what that side of the conflict wanted to change

Resolving the conflict involves:
* Taking the snapshot
* Applying the diff to the snapshot in a way that makes sense. Of course you can't just literally apply the diff like a machine, otherwise jj would already have done it for you, but you have to conceptually consider how that this diff integrate with the final state the that other side of the conflict wanted?

In this case we edit the fruit.txt file and do this:
* Append FRUIT to GRAPE

Now we squash the merge conflict fix into the parent (the one who had the conflict).

You may note that the change XYZ is now automatically no longer in a conflicted state.

The other change still is. Let's resolve that one as well.

Again we have a snapshot etc.

What if we have more than two sides?

Let's go back to an old version of the repository.

Let's look at the diff.

And resolve the conflict.

## Rebase conflicts

asdf


## Choosing a side of the conflict

Sometimes you want to resolve conflicts by choosing one of the sides instead of merging them.

For example let's say we have images and want to keep the green logo.

```
jj resolve --tool :ours logo.jpg
```

`:ours` always refers to `#1` and `:theirs` to `#2`. Keep that in mind for rebases.

For example:

```
jj resolve --tool :ours logo.jpg
```

Another way of doing this is the following (note `restore` not `resolve`!):

```
jj restore --from ASDF logo.jpg
jj squash
```


## jj resolve TUI (terminal UI)

How to interpret the output.


## Larger example with code

Code example with a rebase and merge conflict.




Which side of the conflict is the snapshot and which the diffs? I _think_ jj tries to optimize for making the diffs as small as possible and chooses the snapshot accordingly. In any case, just be aware that for a conflict any of the sides could be the snapshot so you should NOT assume something like #1 always is the snapshot.

lib/src/conflicts.rs

We can look a bit under the hood of what jj is doing by looking at the Git information jj stores under the hood. Note that there is also some additional information under .jj/... which I don't know how to easily view.

```
git log -1 --format=raw 23a3a3fc  # 3 tree objects, potentially multiple parents
git show 2ea3a3fc -- .jjconflict-*
```

https://jj-vcs.github.io/jj/latest/config/#3-way-merge-tools-for-conflict-resolution


vscode can deal with 2 sided conflicts "Resolve in Merge Editor"

Current is #1 and Incoming is #2

it can't deal with more than 2.

When rebasing two changes, each introducing their own conflict (e.g. pear->apple, to upper, grape->grapefruit) then the materialized conflict contains two bases.

also, then it has 3 sides, enumerated in the order of the rebase.


The built in 'jj resolve' TUI seems to be really primitive. Could also not find documentation.
