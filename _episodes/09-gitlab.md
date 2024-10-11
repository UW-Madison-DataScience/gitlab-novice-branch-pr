---
title: Remotes in GitLab
teaching: 45
exercises: 0
questions:
- "How do I share my changes with others on the web?"
objectives:
- "Explain what remote repositories are and why they are useful."
- "Push to or pull from a remote repository."
keypoints:
- "A local Git repository can be connected to one or more remote repositories."
- "`git push` copies changes from a local repository to a remote repository."
- "`git pull` copies changes from a remote repository to a local repository."
---

Version control really comes into its own when we begin to collaborate with
other people.  We already have most of the machinery we need to do this; the
only thing missing is to copy changes from one repository to another.

Systems like Git allow us to move work between any two repositories.  In
practice, though, it's easiest to use one copy as a central hub, and to keep it
on the web rather than on someone's laptop.  Most programmers use hosting
services like [GitHub](https://github.com), [Bitbucket](https://bitbucket.org) or
[GitLab](https://gitlab.com/) to hold those main copies; we'll explore the pros
and cons of this in a later episode.

Let's start by sharing the changes we've made to our current project with the
world. To this end we are going to create a *remote* repository that will be linked to our *local* repository.

## 1. Create a remote repository/project
First we are going to create a placeholder repository, on GitLab these are called "projects",
that we will eventually connect with our local repository.
Log in to [UW GitLab Instance](https://git.doit.wisc.edu/), then click on the "New project"
button in the upper-right corner.

<!---  replace this image with gitLab 
![Creating a Repository on GitHub (Step 1)](../fig/github-create-repo-01.png)
-->

On the "Create new Project" page, click the option to "Create blank project".
Give your project a project name, it can be "planets" or something different if you prefer.
You can then change the url for your project to be different if you would like.

For visiablity, this is up to you. 
Private will mean you have to give access to each individual or group explicitly to see the project.
Internal makes the project visiable to anyone that is a part of the UW GitLab instance.
Public makes the project publically visiable on the web.

Since we already have a repository that we are creating this project for, we also need to
uncheck the Project Configuration option to "Initialize repository with a README".

Then click "Create project".

<!--- replace this image for GitLab
![Creating a Repository on GitHub (Step 2)](../fig/github-create-repo-02.png)
-->

As soon as the repository is created, GitLab displays a page with a URL and some
information on how possible options for next steps.

<!--- replace this image for GitLab
![Creating a Repository on GitHub (Step 3)](../fig/github-create-repo-03.png)
-->

Creating a project on GitLab effectively does the following on the GitLab server:

~~~
$ mkdir planets
$ cd planets
$ git init
~~~
{: .language-bash}

If you remember back to the earlier [episode](../04-changes/) where we added and
committed our earlier work on `mars.txt`, we had a diagram of the local repository
which looked like this:

![The Local Repository with Git Staging Area](../fig/git-staging-area.svg)

Now that we have two repositories, we need a diagram like this:

![Freshly-Made GitHub Repository](../fig/git-freshly-made-github-repo.svg)

Note that our local repository still contains our earlier work on `mars.txt`, but the
remote project on GitLab appears empty as it doesn't contain any files yet.

## 2. Connect local to remote project
Now we connect the two repositories.  We do this by making the
GitLab repository/project a [remote]({{ page.root}}{% link reference.md %}#remote) for the local repository.
The home page of the project on GitLab includes the ssh address we need to
identify it:

<!--- replace this image for GitLab
![Where to Find Repository URL on GitHub](../fig/github-find-repo-string.png)
-->

Click on the "Code" blue button and click the little clickboard icon next to the 
"Clone with SSH option. This will copy the ssh address to your computer's clipboard.

> ## HTTPS vs. SSH
>
> We use SSH here because, while it requires some additional configuration, it is a 
> security protocol widely used by many applications. Recall that we set up SSH
> authentication in the setup episode. 
{: .callout}
  
<!--- replace this image for GitLab
![Changing the Repository URL on GitHub](../fig/github-change-repo-string.png)
-->

Now that we have copied the address from the browser, 
go into the local `planets` repository, and run this command:

**the picture is incorrectly labeled. It reads `github.com` but it should be `git.doit.wisc.edu`**

~~~
$ git remote add origin git@git.doit.wisc.edu:VLAD.DRACULA/planets.git
~~~
{: .language-bash}

Make sure to use the address for your repository rather than Vlad's: the only
difference should be your username instead of `VLAD.DRACULA`, unless you changed
the project name or address.

`origin` is a local name used to refer to the remote repository. It could be called
anything, but `origin` is a convention that is often used by default in git
and GitLab, so it's helpful to stick with this unless there's a reason not to.

We can check that the command has worked by running `git remote -v`:

~~~
$ git remote -v
~~~
{: .language-bash}

~~~
origin   git@git.doit.wisc.edu:VLAD.DRACULA/planets.git (fetch)
origin   git@git.doit.wisc.edu:VLAD.DRACULA/planets.git (push)
~~~
{: .output}

We'll discuss remotes in more detail in the next episode, while
talking about how they might be used for collaboration.

## 3. Push local changes to a remote

This command will push the changes from
our local repository to the repository on GitLab:

~~~
$ git push origin main
~~~
{: .language-bash}

Since Dracula set up a passphrase, it will prompt him for it.  If you did not add a password
when you setup your SSH keys, it will not prompt for a passphrase. 

~~~
Enumerating objects: 16, done.
Counting objects: 100% (16/16), done.
Delta compression using up to 8 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (16/16), 1.45 KiB | 372.00 KiB/s, done.
Total 16 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), done.
To git@git.doit.wisc.edu:VLAD.DRACULA/planets.git
 * [new branch]      main -> main
~~~
{: .output}

> ## Proxy
>
> If the network you are connected to uses a proxy, there is a chance that your
> last command failed with "Could not resolve hostname" as the error message. To
> solve this issue, you need to tell Git about the proxy:
>
> ~~~
> $ git config --global http.proxy http://user:password@proxy.url
> $ git config --global https.proxy https://user:password@proxy.url
> ~~~
> {: .language-bash}
>
> When you connect to another network that doesn't use a proxy, you will need to
> tell Git to disable the proxy using:
>
> ~~~
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~
> {: .language-bash}
{: .callout}

> ## Password Managers
>
> If your operating system has a password manager configured, `git push` will
> try to use it when it needs your username and password.  For example, this
> is the default behavior for Git Bash on Windows. If you want to type your
> username and password at the terminal instead of using a password manager,
> type:
>
> ~~~
> $ unset SSH_ASKPASS
> ~~~
> {: .language-bash}
>
> in the terminal, before you run `git push`.  Despite the name, [Git uses
> `SSH_ASKPASS` for all credential
> entry](https://git-scm.com/docs/gitcredentials#_requesting_credentials), so
> you may want to unset `SSH_ASKPASS` whether you are using Git via SSH or
> https.
>
> You may also want to add `unset SSH_ASKPASS` at the end of your `~/.bashrc`
> to make Git default to using the terminal for usernames and passwords.
{: .callout}

Our local and remote repositories are now in this state:

<!--- replace this image for GitLab
![GitHub Repository After First Push](../fig/github-repo-after-first-push.svg)
-->

> ## The '-u' Flag
>
> You may see a `-u` option used with `git push` in some documentation.  This
> option is synonymous with the `--set-upstream-to` option for the `git branch`
> command, and is used to associate the current branch with a remote branch so
> that the `git pull` command can be used without any arguments. To do this,
> simply use `git push -u origin main` once the remote has been set up.
{: .callout}

We can pull changes from the remote repository to the local one as well:

~~~
$ git pull origin main
~~~
{: .language-bash}

~~~
From git@git.doit.wisc.edu:VLAD.DRACULA/planets.git
 * branch            main     -> FETCH_HEAD
Already up-to-date.
~~~
{: .output}

Pulling has no effect in this case because the two repositories are already
synchronized.  If someone else had pushed some changes to the repository on
GitLab, though, this command would download them to our local repository.

## Practicing syncing with a pretend 2nd laptop

Let's imagine a senario where we have two computers, the one we've been working on now
and a personal laptop. Some days we work on campus with our work computer and others days
we work from home on our personal laptop. Let's learn how to use GitLab to sync between
these two computers.

First we will make and switch to a folder that represents our pretend personal laptop, 
we will pretend it is a separate computer.

~~~
$ cd # to get back to our home directory
$ mkdir laptop2
$ cd laptop2
~~~
{: .language-bash}

Now back in GitLab we need to copy the SSH address again, we can click the blue "Code"
button again and click the keyboard icon to copy it to our clipboard again.

~~~
$ git clone git@git.doit.wisc.edu:VLAD.DRACULA/planets.git
~~~
{: .language-bash}

~~~
Cloning into 'planets'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
~~~
{: .output}

This action creates a folder for the repo, initializes it as a repo, sets up the remote
(with the remote name 'origin'), and pulls down the history and files.

Let's explore the repo on our personal laptop. Change to the planets repo,
and try listing the files, the contents of `mars.txt`, and the history of commits.
~~~
$ cd planets
$ ls
$ cat mars.txt
$ git log --oneline
~~~
{: .language-bash}

The repository is in the same state as the one on our work laptop.
Let's make a change and practice syncing to GitLab and our work computer.

~~~
$ nano venus.txt
~~~
{: .language-bash}

~~~
$ cat venus.txt
~~~
{: .language-bash}

~~~
We should explore venus next
~~~
{: .output}


Now we need to do our usual git cycle and add and commit the new file.

~~~
$ git add venus.txt
$ git commit -m "new notes on exploring venus"
~~~
{: .language-bash}

<!--- needs output block -->

Note this change is still only on our personal computer.  It is not in GitLab or on our work laptop.
To sych this with GitLab we can `push` our changes.

~~~
$ git push origin main
~~~
{: .language-bash}

<!--- needs output block -->

Now when we refresh the page on GitLab we can see the new file.
What happens if we go back to our work laptop? Will it be updated to the version on GitLab?
Let's try it and find out

First, we need to switch back to our desktop folder. Then we can list the file to see if venus is there. 
We can also check the history to see if our commit is listed
~~~
$ cd Desktop/planets
$ ls
$ git log --oneline
~~~
{: .language-bash}

<!--- needs output block -->

There is still no `venus.txt` file and the last commit is missing.  To sync with GitLab, we need to `pull` the latest changes down.
~~~
$ git pull origin main
$ ls
~~~
{: .language-bash}

<!--- needs output block -->

Now all our copies of this repo are in sycn with one another.
If you are working on several computers, best practice is to make sure to `commit` and `push` any changes at the end of each
work session on each computer and to `pull` the latest changes before you start working on a new computer.


> ## Push vs. Commit
>
> In this episode, we introduced the "git push" command.
> How is "git push" different from "git commit"?
>
> > ## Solution
> > When we push changes, we're interacting with a remote repository to update it with the changes 
> > we've made locally (often this corresponds to sharing the changes we've made with others). 
> > Commit only updates your local repository.
> {: .solution}
{: .challenge}

> ## GitLab License and README files
>
> In this episode we learned about creating a remote repository on GitLab, but when you initialized 
> your GitLab project, you didn't add a README.md or a license file. If you had, what do you think 
> would have happened when you tried to link your local and remote repositories?
>
> > ## Solution
> > In this case, we'd see a merge conflict due to unrelated histories. When GitLab creates a 
> > README.md file, it performs a commit in the remote repository. When you try to pull the remote 
> > repository to your local repository, Git detects that they have histories that do not share a 
> > common origin and refuses to merge.
> > ~~~
> > $ git pull origin main
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > warning: no common commits
> > remote: Enumerating objects: 3, done.
> > remote: Counting objects: 100% (3/3), done.
> > remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
> > Unpacking objects: 100% (3/3), done.
> > From git@git.doit.wisc.edu:VLAD.DRACULA/planets.git
> >  * branch            main     -> FETCH_HEAD
> >  * [new branch]      main     -> origin/main
> > fatal: refusing to merge unrelated histories
> > ~~~
> > {: .output}
> >
> > You can force git to merge the two repositories with the option `--allow-unrelated-histories`. 
> > Be careful when you use this option and carefully examine the contents of local and remote 
> > repositories before merging.
> > ~~~
> > $ git pull --allow-unrelated-histories origin main
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > From git@git.doit.wisc.edu:VLAD.DRACULA/planets.git
> >  * branch            main     -> FETCH_HEAD
> > Merge made by the 'recursive' strategy.
> > README.md | 1 +
> > 1 file changed, 1 insertion(+)
> > create mode 100644 README.md
> > ~~~
> > {: .output}
> >
> {: .solution}
{: .challenge}
