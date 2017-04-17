---
layout: post
title: "Import Existing Git Repository to Another"
date: 2017-04-17 16:44:53 +0700
comments: true
categories: [git]
---

{% img left /images/logo/git.png %}

While working with git, we may need to import source code from an existing git repository to our working copy. Merging it and pushing to origin master.
The scenario that I had was :

<br/>
<ul>
<li> I have a project template that I store on a git repository. Let say the url is <code>http://server/git/template.git</code>
   {% img center /images/post/2017-04-17-template.png %}
</li>
<li>
I have another git repository with url <code>http://server/git/project1.git</code>
   {% img center /images/post/2017-04-17-project1.png %}
</li>
</ul>
What I need from the two repositories are importing all contents (libraries, sources, etc) from <code>template</code> repository into my <code>project1</code> working copy. Since I don't want to coding from zero. To get into what I need, here are the steps.

``` bash
# git clone project1
$ git clone http://server/git/project1.git
$ cd project1

# add remote url named REMOTE.TEMPLATE
$ git remote add REMOTE.TEMPLATE http://server/git/template.git

# fetch from REMOTE.TEMPLATE remote url
$ git fetch REMOTE.TEMPLATE

# checkout REMOTE.TEMPLATE/master and create a new branch called TEMPLATE
$ git checkout -b TEMPLATE REMOTE.TEMPLATE/master

#switch back to master branch
$ git checkout master

# merge TEMPLATE brach to master branch
$ git merge TEMPLATE

# commit changes
$ git commit

```

The next step is checking the merge result on our working copy of master branch.
If what we have been imported already there, now we can remove the remote URL of REMOTE.TEMPLATE
and TEMPLATE branch to get rid of the extra branch before pushing.

``` bash
# remove REMOTE.TEMPLATE remote address
$ git remote rm REMOTE.TEMPLATE

# remove template branch. It is useful to get rid of the extra branch before pushing
$ git branch -d TEMPLATE

# push to remote origin/master
$ git push

```

## References
1. http://stackoverflow.com/questions/1683531/how-to-import-existing-git-repository-into-another
