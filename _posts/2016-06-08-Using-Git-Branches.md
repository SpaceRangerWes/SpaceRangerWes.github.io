---
layout: post
title: Using Git Branches w/ Github
---

## Preface ##
This is an article I wrote for Caliber Contracting's internal development so that students and Github newbies have a clear instruction set on what to do. Of course these are specific principles that I follow and aren't strict rules. Merging vs. Rebasing is a valid argument and these are just the guidelines I follow.



Working with branches is an integral part of using version systems like git or svn. A branch like in a tree is a separation from the main "trunk" or in git's lingo, the master branch.

Branches should be created each time you work on something new or "major." These things include milestone cards, features, major bug fixes, and improvements. Ask the question, "Am I changing more than 3 lines of code?"

## Creating a Branch ##
I am going to assume that you know how github works in some form or fashion and that you know the difference between remote and local.
<pre><code>
git checkout -b [branch_name]
git push --set-upstream origin [branch_name]
</code></pre>
The above terminal snippet is saying to create a local branch of 'branch_name' and then simultaneously create a remote (upstream) branch of the same name and push to it. Think of it this way, when you create a local branch, github has no idea you have done so. Therefore, if you try to say <code>git push [branch_name]</code>, github doesn't know about this new fancy branch named [branch_name]. <code>git push --set-upstream origin [branch_name]</code> is creating a branch on github with the same name as your local and telling github that when you say git push from within that local branch to link it to the same-named remote branch on github's servers.

## Working in your Branch ##
This will be exactly the same as when you work from master or just work locally. You will <code>git add, commit, push, etc</code> the same way as in the past. However, there is one caveat. You need to stay updated with the changes in master. Other developers will be changing their branches, pushing to master, changing master, merging master to their branches, and more. My method is to make sure my code is updated with master every time I make a commit. This brings us to our next topic...

## Keeping Up with Master ##
As said in the previous section, we need to stay updated with master if only for the sole purpose of not having monstrous merge conflicts (covered in the next section) at the end of our branch work. Massive merge conflicts are a whole project in-and-of themselves so we want to make the job easier on you so that you get paid for being smart and not for grunt work that slows the river of shit to a halt. So how do we keep up with the master branch? Easy.
<pre><code>
git fetch
git merge origin/master
</code></pre>
<code>git fetch</code> updates your remote branches, there usually is no need to have a local copy of a branch when your are not planning to work on this branch. It is very likely you will have merge conflicts, but it is better to fix these smaller ones now than a massive list of them later.

## Finishing and Deleting Branch ##
When you are finished with the work you've done on your branch and you've tested it, your peers have tested it, and the team lead has given you the go to merge it to master, then you are almost finished with adding your sweet, savory new code into master where it will become apart of Caliber's legacy and help grow our legion of followers and worshipers. Anywho, so you've gotten all of the approval and rubber stamps...let's continue by doing a final
<pre><code>
git fetch
git merge origin/master
git push origin [branch_name]
</code></pre>
and resolve any differences. Now do the following..
<pre><code>
git checkout master
git merge [branch_name]
</code></pre>
Now because you had been so diligent about staying up to date with master over the course of your branch work, there should be few to none conflicts. And of course fix them if there are. Once the merge conflicts are no like in conflict and in unity with the universe..
<pre><code>
git push origin master
git branch -d [branch_name]
git push origin :[branch_name]
</code></pre>
Let's go through this line by line.

1. We are pushing master to the remote master on github so no coworkers can get there grubby little code changes in there before you push. Otherwise, guess what happens? Yep, more conflicts.
2. The second command deletes the branch locally and only locally. This is considered a safe operation at this point.
- git may want you to use <code>-D</code> instead of <code>-d</code>. The reasoning is because there is still some code you haven't merged/committed either on purpose or accident.
3. Lastly, this removes the branch from the remote repository on github. Only do this if you are certain that, after merging master, the code compiles and runs as expected.

That's it!
