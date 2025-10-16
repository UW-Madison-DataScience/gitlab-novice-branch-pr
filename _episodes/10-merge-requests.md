---
title: Merge Requests
teaching: 60
exercises: 15
questions:
- "What are merge requests for?"
- "How can I make a merge request?"
objectives:
- "Define the terms fork, clone, origin, remote, upstream"
- "Understand how to make a merge request and what they are useful for"
keypoints:
- "Merge requests suggest changes to repos where you don't have privileges or want the changes to be reviewed"
---


Merge requests are a great way to collaborate with others using GitLab.
Instead of making changes directly to a repository/project you can suggest changes to a repository/project.
This can be useful if you don't have permission to modify a project directly or
you want someone else to review your changes before they are merged.

For this lesson we will be working on the `countries` repository together.
Open the GitLab link for the `countries` repo provided by the instructor 
in your browser window.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_upstream_repo.png)


> ## Project owners
>
> You may have noticed that the `countries` repo in this lesson's pictures 
> is owned by the 'McMahonLab' organization and this doesn't match
> the address you were given.
> This is to be expected because this will differ depending on 
> what organization your instructor used to setup the `countries` repo.
> 
> You will also see your username where the 'sstevens2' is in the pictures.
> 
{: .callout}
-->

Once at the `countries` project, click the **Fork** button which can be found
in the upper right hand corner of the window. 
Forking the project makes us each our own copy of the repo in our GitLab 
account which we can edit.
On the "Fork project" page, you can change the project name if you'd like (but we will leave it the same),
and you can change the project URL to be at your namespace in the dropdown menu.
If you belong to other groups in GitLab you may see those as options as well.
You can leave all the other options as the defaults and then click the "Fork project" button at the bottom of the page.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_upstream_forking.png)
-->

Next we need to get this repo on our local computer and
setup connections from our computer to both our forked version 
and the authoritative version we forked it from.

First we will **clone** the repo from our forked version.
The clone command does two things:

1. Copies the repo to your local computer
2. Sets up a remote called 'origin' between your computer and the GitLab repo

Copy the SSH address for your forked version of repo 
(click the "Code" button and then use the clipboard to copy the SSH address to your clipboard).

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_cloneOrigin.png)
-->

In terminal or Gitbash, navigate to a folder you'd like to hold this repo,
we will place it on our `Desktop`.
Once there you can use the `clone` command with the link you copied as the first argument.

~~~
$ cd ~/Desktop
$ git clone git@git.doit.wisc.edu:USERNAME/countries.git
~~~
{: .bash}

> ## Why does the command above say 'USERNAME'?
>
> So that we can't copy the command above and accidentally clone someone else's
> version of countries to our computer, the command above uses the placeholder
> 'USERNAME' where you should put your own username if your copied from above
> instead of copying the link from your browser and pasting it into the command.
> 
> 
{: .callout}

~~~
Cloning into 'countries'...
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 0), reused 6 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
~~~
{: .output}

Next we will set up a connection or **remote** to the authoritative repository 
(the original version given to you by your instructor).
In your browser, you can go back this repo by clicking on the link that says 'forked from'
in the upper left hand corner, under your username and repo name.

Copy the SSH address for this repo. To find it you can go to the original link or GitLab
should have a link to that says "Forked from INSTRUCTOR-GIVEN / countries". From there you can click the
"Code" button and copy the SSH address.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_upstream_repo.png)
-->

Then back in your terminal, navigate into the cloned repo and add the remote 
connection to this repository.
For this command we must give the remote a different nickname, 
where our original remote is 'origin'
this new remote will be called 'upstream'.
You could give it a different nickname but 'upstream' is a common nickname for
the authoritative repository.

~~~
$ cd countries
$ git remote add upstream git@git.doit.wisc.edu:INSTRUCTOR-GIVEN/countries.git
~~~
{: .bash}

> ## If you tried copying the command above...
>
> You will have to replace 'INSTRUCTOR-GIVEN' with the site your instructor
> indicated at the beginning of this lesson. This will vary depending
> on how your instructor set up for this lesson.
> 
> 
{: .callout}

At anytime you can see the remote connections your repo has using the following command:

~~~
$ git remote -v
~~~
{: .bash}

~~~
origin	git@git.doit.wisc.edu:USERNAME/countries.git (fetch)
origin	git@git.doit.wisc.edu:USERNAME/countries.git (push)
upstream	git@git.doit.wisc.edu:INSTRUCTOR-GIVEN/countries.git (fetch)
upstream	git@git.doit.wisc.edu:INSTRUCTOR-GIVEN/countries.git (push)
~~~
{: .output}

Now that we have this setup done we will be able to suggest 
changes to this repo using a merge request.
Each person will add a new file with info about a new country in it.

**The instructor's helper will now add a single file to the upstream repository containing 
information about the the United States.**
<!--- this should be an instructor note if we ever convert to the workbench -->

Next, we will update our local version of the repo to include the new file.
We will use `pull` to bring these changes to our local repository.
We must specify the remote and branch we want to pull from, in this case the 
`upstream` remote's `main` branch.

~~~
$ git pull upstream main
~~~
{: .bash}

Now your local version of the repo is updated but our forked version of the 
project/repo is not yet up to date.
You can reload your fork in GitLab and see it does not contain the new 
`united_states.txt` file.
Now we need to update our forked version.
To do so we can `push` the changes in our local version to the main branch of our fork, 
called 'origin'.

~~~
$ git push origin main
~~~
{: .bash}

Now that we are all in sync with the latests version of upstream,
let's each add a new country to the repository.
First let's make a new branch to work on.  This will keep our 'main' version
in sync with the upstream version of the repository.
We can name our branch descriptively after the country we will be adding.
Mine will be `addFrance` since I'll be working with France.
Please pick a different country and shout it out (or add it to the etherpad) 
so no one else chooses the same one.
We will create the branch and switch into in one step 
as we learned earlier in the branching lesson.

~~~
$ git checkout -b addFrance
~~~
{: .bash}

~~~
Switched to a new branch 'addFrance'
~~~
{: .output}

Finally before we proceed to adding the new file, we will double 
check that we are on the right branch.

~~~
$ git branch
~~~
{: .bash}

~~~
* addFrance
  main
~~~
{: .output}

Next we will copy `united_states.txt` and change the name to the name of our chosen country.
Then we can use nano to edit the contents to reflect the info of your chosen country.  
Hint: You may need to do some internet searching to fill in the information.

~~~
$ cp united_states.txt france.txt
$ nano france.txt
$ cat france.txt
~~~
{: .bash}

~~~
Population: 66,991,000
Capital: Paris
~~~
{: .output}

Next let's add and commit the changes to the repo.

~~~
$ git add france.txt
$ git commit -m "Added file on france"
~~~
{: .bash}

~~~
[addFrance 79a312a] Added file on france
 1 file changed, 2 insertions(+), 2 deletions(-)
~~~
{: .output}

In some cases we may not have permission to push changes directly to the 
upstream repo or we might like our changes to be reviewed regardless 
of permissions, so we'll create a `merge request`. 
A `merge request` is a **request** for a member of the upstream repository to **merge** 
our changes into the upstream repository from a `fork`, allowing them to request further 
changes/improvements and make comments on the changes before doing so. 
In order to create a `merge request`, we must push our new branch containing the
changes we'd like to submit to the remote linked to our fork, `origin`, on GitLab.

~~~
$ git push origin addFrance
~~~
{: .bash}

~~~
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 783 bytes | 0 bytes/s, done.
Total 4 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
To git@git.doit.wisc.edu:USERNAME/countries.git
   2037539..79a312a  addFrance -> addFrance
~~~
{: .output}

Next go to your forked GitLab version of the repo and reload the page.
You won't see the new file added in the list of files but you will see 
that you recently pushed a new branch to the repository.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_origin_master_addFrance.png)
-->

If you wish to view your new branch you can click on the 'Main' drop down menu
and select the new country branch.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_switch_github_branch.png)
-->

Then you should be able to view the files and commits in that branch.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_origin_master_addFrance.png)
-->

GitLab already suspects that we are going to want to make a merge request so we can click
the 'Create merge request' button to start a new merge request.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_makingPR1.png)
-->

The "from should be the new branch of our personal fork and the "into" should be the 
upstream/authoritative version's main branch.
You can edit the title or add more information into the comment section if there is anything you'd like 
to add for the person who reviews your suggestion.
You can also mark it as draft if you'd like to make more commits to it before someone reviews it.
There are other options for picking a reviewer or showing you as the author/assignee, we will keep the defaults for the rest.
Then you can click the 'create merge request' button to submit the merge request.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_makingPR2.png)
-->


Now someone with privileges to the upstream repo can review it, give comments and
suggestions, and merge it into the upstream version.
In our merge request they can see any messages we left or click and look at the commits that were made and see the files changed.

Our collaborator reviewing the merge request noticed that 
we forgot to add the largest city so let's add it and update our merge request.
<!--- this could be instructor note when we move to workbench -->

~~~
$ nano france.txt
$ cat france.txt
~~~
{: .bash}

~~~
Population: 66,991,000
Capital: Paris
Largest City: Paris
~~~
{: .output}

Next we will add and commit these changes.
Then we can push them to our fork of the repo.

~~~
$ git add france.txt
$ git commit -m "Added largest city to france file"
$ git push origin addFrance
~~~
{: .bash}

~~~
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 387 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@git.doit.wisc.edu:USERNAME/countries.git
   31aa2e3..609acfe  addFrance -> addFrance
~~~
{: .output}

If we reload the merge request, we'll see that the new commit was added to the 
merge request and the changes have been automatically updated.
New commits pushed to the same branch are included in the previously created merge request.
If you want to suggest changes separately you need to make separate branches but 
if you want the changes to be considered together you should put them in the same branch.

<!--- needs to be replaced for GitLab
![](../fig/github_screenshot_after_new_commit.png)
-->

Now the owner/administrator/manager of the upstream repo can review our merge requests and decide to incorporate them.


> ## Add new country file and make additional MR
>
> - Starting in the main branch make a new branch
> - Copy other country file into a new country
> - Edit the file to include info on the new country
> - Add and commit this new file
> - Push the new changes to GitLab
>
> > ## Solution
> >
> > ~~~
> > $ git checkout main
> > $ git checkout -b addItaly
> > $ cp united_states.txt italy.txt
> > $ nano italy.txt #Add the right info into the file
> > $ git add italy.txt
> > $ git commit -m "Added file on Italy"
> > $ git push origin addItaly
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}



