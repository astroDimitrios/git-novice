---
title: Remotes in GitHub
teaching: 45
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain what remote repositories are and why they are useful.
- Push to or pull from a remote repository.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I share my changes with others on the web?

::::::::::::::::::::::::::::::::::::::::::::::::::

Version control really comes into its own when we begin to collaborate with
other people.  We already have most of the machinery we need to do this; the
only thing missing is to copy changes from one repository to another.

Systems like Git allow us to move work between any two repositories.  In
practice, though, it's easiest to use one copy as a central hub, and to keep it
on the web rather than on someone's laptop.  Most programmers use hosting
services like [GitHub](https://github.com), [Bitbucket](https://bitbucket.org) or
[GitLab](https://gitlab.com/) to hold those main copies; we'll explore the pros
and cons of this in a [later episode](13-hosting.md).

Let's now share the changes we've made to our current project with the
world. To this end we are going to create a *remote* repository that will be linked to our *local* repository.

## 1\. Create a remote repository

Log in to [GitHub](https://github.com), then click on the icon in the top right corner to
create a new repository called `weather`:

![](fig/github-create-repo-01.png){alt='The first step in creating a repository on GitHub: clicking the "create new" button'}

Name your repository "weather" and then click "Create Repository".

Note: Since this repository will be connected to a local repository, it needs to be empty. Leave
"Initialize this repository with a README" unchecked, and keep "None" as options for both "Add
.gitignore" and "Add a license." See the "GitHub License and README files" exercise below for a full
explanation of why the repository needs to be empty.

![](fig/github-create-repo-02.png){alt='The second step in creating a repository on GitHub: filling out the new repository form to provide the repository name, and specify that neither a readme nor a license should be created'}

::: spoiler

### Repository Visibility

Here we have chosen to make our repository public.
The visibility of your repository depends on which option you choose:

- Private: only you
- Internal (organisations only): read permissions to anyone in the organisation
- Public: read permissions to anyone

Some organisations will restrict the creation of public repositories
so you may find their default is internal.
If your project deals with sensitive material then create a private repository.

:::

As soon as the repository is created, GitHub displays a page with a URL and some
information on how to configure your local repository.
Ignore the suggested commands for now as we will run these later.

![](fig/github-create-repo-03.png){alt='The summary page displayed by GitHub after a new repository has been created. It contains instructions for configuring the new GitHub repository as a Git remote'}

This effectively does the following on GitHub's servers:

```bash
$ mkdir weather
$ cd weather
$ git init
```

If you remember back to the earlier [episode](04-changes.md) where we added and
committed our earlier work on `forecast.md`, we had a diagram of the local repository
which looked like this:

![](fig/git-staging-area.svg){alt='A diagram showing how "git add" registers changes in the staging area, while "git commit" moves changes from the staging area to the repository'}

Now that we have two repositories, we need a diagram like this:

![](fig/git-freshly-made-github-repo.svg){alt='A diagram illustrating how the GitHub "weather" repository is also a Git repository like our local repository, but that it is currently empty'}

Note that our local repository still contains our earlier work on `forecast.md`, but the
remote repository on GitHub appears empty as it doesn't contain any files yet.

## 2\. Connect local to remote repository

Now we connect the two repositories.  We do this by making the
GitHub repository a [remote](../learners/reference.md#remote) for the local repository.
The home page of the repository on GitHub includes the URL string we need to
identify it:

![](fig/github-change-repo-string.png){alt='A screenshot showing that clicking on "SSH" will make GitHub provide the SSH URL for a repository instead of the HTTPS URL'}

Click on the 'SSH' link to change the [protocol](../learners/reference.md#protocol) from HTTPS to SSH.

:::::::::::::::::::::::::::::::::::::::::  callout

## HTTPS vs. SSH

We use SSH here because, while it requires some additional configuration, it is a
security protocol widely used by many applications.  The steps below describe SSH at a
minimum level for GitHub.

::::::::::::::::::::::::::::::::::::::::::::::::::

::: spoiler

### Note for Personal Access Token Users

If you use a Personal Access Token (PAT)
to connect to GitHub you should select
'HTTPS' **not** 'SSH'.

We recommend you move to using SSH Keys
instead of a PAT.
Part [3](./07-github.md#3-ssh-background-and-setup)
of this episode shows you how to set up an SSH key.

If you choose to continue using your PAT,
in the rest of the material any SSH links such as:

```bash
git@github.com:mo-eormerod/weather.git
```

should be converted to the HTTPS form like this:

```bash
https://github.com/mo-eormerod/weather.git
```

:::

Copy that URL from the browser, go into the local `weather` repository, and run
this command:

```bash
$ git remote add origin git@github.com:mo-eormerod/weather.git
```

Make sure to use the URL for your repository: the only
difference should be your username instead of `mo-eormerod`.

`origin` is a local name used to refer to the remote repository. It could be called
anything, but `origin` is a convention that is often used by default in Git
and GitHub, so it's helpful to stick with this unless there's a reason not to.

We can check that the command has worked by running `git remote -v`:

```bash
$ git remote -v
```

```output
origin   git@github.com:mo-eormerod/weather.git (fetch)
origin   git@github.com:mo-eormerod/weather.git (push)
```

We'll discuss remotes in more detail in the next episode, while
talking about how they might be used for collaboration.

## 3\. SSH Background and Setup

::: instructor

Some learners may have set up an ssh key already.
If they have a key with this name: `~/.ssh/id_ed25519_github`
and can successfully authenticate with `ssh -T git@github.com`
they can skip this section.

If their ssh key name does not match the one above we **strongly**
recommend they follow the instructions below.

Those who are skipping this section can revisit the challenges
in earlier episodes (yellow callouts with a lightning bolt)
or read through the [discussion notes](../learners/discuss.md).

:::

Before you can connect to a remote repository, you need to set up a way for your computer to authenticate with GitHub so it knows it's you trying to connect to the remote repository.

We are going to set up the method that is commonly used by many different services to authenticate access on the command line.  This method is called Secure Shell Protocol (SSH). SSH is a cryptographic network protocol that allows secure communication between computers using an otherwise insecure network.

SSH uses what is called a key pair. This is two keys that work together to validate access. One key is publicly known and called the public key, and the other key called the private key is kept private. Very descriptive names.

You can think of the public key as a padlock, and only you have the key (the private key) to open it. You use the public key where you want a secure method of communication, such as your GitHub account.  You give this padlock, or public key, to GitHub and say "lock the communications to my account with this so that only computers that have my private key can unlock communications and send git commands as my GitHub account."

What we will do now is the minimum required to set up the SSH keys and add the public key to a GitHub account.
The first thing we are going to do is check if this has already been done on the computer you're on.
Because generally speaking, this setup only needs to happen once and then you can forget about it.

:::::::::::::::::::::::::::::::::::::::::  callout

## Keeping your keys secure

You shouldn't really forget about your SSH keys, since they keep your account secure. It's good
practice to audit your secure shell keys every so often. Especially if you are using multiple
computers to access your account.


::::::::::::::::::::::::::::::::::::::::::::::::::

We will run the list command to check what key pairs already exist on your computer.

```bash
ls -al ~/.ssh
```

Your output is going to look a little different depending on whether or not SSH has ever been set up on the computer you are using.

If you have not set up SSH on your computer, you will see

```output
ls: cannot access '~/.ssh': No such file or directory
```

If SSH has been set up on the computer you're using, the public and private key pairs will be listed. The file names are either `id_ed25519`/`id_ed25519.pub` or `id_rsa`/`id_rsa.pub` depending on how the key pairs were set up.

### 3\.1 Create an SSH key pair

To create an SSH key pair use the following command, 
where the `-t` option specifies which type of algorithm to use:

```bash
$ ssh-keygen -t ed25519 -C "e.ormerod@mo-weather.uk"
```

The `-C` flag attaches a comment to the key.
The comment has no effect on your key, you may place anything here to help
you remember what the key is for. It makes no difference whether you use
a public email or your no-reply private GitHub email in the comment.

If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
`$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

```output
Generating public/private ed25519 key pair.
Enter file in which to save the key (~/.ssh/id_ed25519):
```

We want to store our key in the file `~/.ssh/id_ed25519_github`.

```output
Created directory '~/.ssh'.
Enter passphrase (empty for no passphrase):
```

Now you will be prompted for a passphrase.
If the computer you work on is shared between multiple users you should 
set a passphrase. 
Be sure to use something memorable or save your passphrase somewhere,
as there is no "reset my password" option.
If you do not share your computer there is no need 
to set a passphrase so just press <kbd>Enter</kbd>.

Note that, when typing a passphrase on a terminal, there won't be any visual feedback of your typing.
This is normal: your passphrase will be recorded even if you see nothing changing on your screen.

```output
Enter same passphrase again:
```

After entering the same passphrase a second time, we receive the confirmation

```output
Your identification has been saved in ~/.ssh/id_ed25519_github
Your public key has been saved in ~/.ssh/id_ed25519_github.pub
The key fingerprint is:
SHA256:SMSPIStNyA00KPxuYu94KpZgRAYjgt9g4BA4kFy3g1o e.ormerod@mo-weather.uk
The key's randomart image is:
+--[ED25519 256]--+
|^B== o.          |
|%*=.*.+          |
|+=.E =.+         |
| .=.+.o..        |
|....  . S        |
|.+ o             |
|+ =              |
|.o.o             |
|oo+.             |
+----[SHA256]-----+
```

The "identification" is actually the private key. You should never share it.
The public key is appropriately named.
The "key fingerprint" is a shorter version of a public key.

Now that we have generated the SSH keys, we will find the SSH files when we check.

```bash
ls -al ~/.ssh
```

```output
drwxr-xr-x 1 Eleanor   197121   0 Jul 16 14:48 ./
drwxr-xr-x 1 Eleanor   197121   0 Jul 16 14:48 ../
-rw-r--r-- 1 Eleanor   197121 419 Jul 16 14:48 id_ed25519_github
-rw-r--r-- 1 Eleanor   197121 106 Jul 16 14:48 id_ed25519_github.pub
```

### 3\.2 Copy the public key to GitHub

Now we have a SSH key pair and we can run this command to check if GitHub can read our authentication.

```bash
ssh -T git@github.com
```

```output
The authenticity of host 'github.com (192.30.255.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
```

Right, we forgot that we need to give GitHub our public key!

::: spoiler

### Checking the GitHub RSA Key

Ideally before connecting to a new host, like `github.com` in the output above,
you would check the RSA key fingerprint matches the expected value.
GitHub publishes their public [SSH key fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)
for you to check against.

:::

First, we need to copy the public key.  Be sure to include the `.pub` at the end, otherwise you're looking at the private key.

```bash
cat ~/.ssh/id_ed25519_github.pub
```

```output
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDmRA3d51X0uu9wXek559gfn6UFNF69yZjChyBIU2qKI e.ormerod@mo-weather.uk
```

Now, going to GitHub.com, click on your profile icon in the top right corner to get the drop-down menu.  Click "Settings", then on the
settings page, click "SSH and GPG keys", on the left side "Access" menu. Click the "New SSH key" button on the right side. Now,
you can add the title (normally an ID for the computer storing the keys such as "Work Linux"),
paste your SSH key into the field, and click the "Add SSH key" to complete the setup.

::: callout

## Single sign-on (SSO)

If you are part of an organisation that requires single
sign-on (SSO) to access their GitHub organisation you
will need to authorise the key for use in the organisation.

Next to the newly created SSH key in the GitHub settings
click on "Configure SSO".
Find the organisation in the list and click on "Authorise".

:::

Now that we've set that up, let's check our authentication again from the command line.

```bash
$ ssh -T git@github.com
```

```output
Hi Eleanor! You've successfully authenticated, but GitHub does not provide shell access.
```

Good! This output confirms that the SSH key works as intended. We are now ready to push our work to the remote repository.

::: spoiler

### Troubleshooting SSH Setup

If your new key failed to connect you may need to alter your ssh config.

1. Create the `~/.ssh/config` file if it doesn't exist
2. Add the following to the file:

```ssh-config
Host github.com
  IdentityFile ~/.ssh/id_ed25519_github
```

This explicitly states which key to use for `github.com`
and is needed if you have many SSH keys already for other hosts.

:::

## 4\. Push local changes to a remote

Now that authentication is setup, we can return to the local repository.
Ensure you are on the `main` branch:

```bash
$ git switch main
```

This command will push our main branch on
our local repository to the repository on GitHub:

```bash
$ git push
```

```output
fatal: The current branch main has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin main

```

Git is telling us it doesn't know what branch we want to push
our local `main` branch to on GitHub.
We can tell Git this by setting the upstream `origin` branch
to also be named `main`.

If you entered a passphrase when creating an shh key you will be prompted for it.

```output
Enumerating objects: 16, done.
Counting objects: 100% (16/16), done.
Delta compression using up to 4 threads
Compressing objects: 100% (13/13), done.
Writing objects: 100% (16/16), 1.69 KiB | 216.00 KiB/s, done.
Total 16 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), done.
To github.com:mo-eormerod/weather.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

::: callout

### Automatically set the upstream branch

You can get git to automatically set the upstream branch.
This avoids having to run git push with the `--set-upstream`
flag the first time you push from a new branch.

Change your git config using:

```bash
$ git config --global push.autoSetupRemote true
```

:::

:::::::::::::::::::::::::::::::::::::::::  callout

## Proxy

If the network you are connected to uses a proxy, there is a chance that your
last command failed with "Could not resolve hostname" as the error message. To
solve this issue, you need to tell Git about the proxy:

```bash
$ git config --global http.proxy http://user:password@proxy.url
$ git config --global https.proxy https://user:password@proxy.url
```

When you connect to another network that doesn't use a proxy, you will need to
tell Git to disable the proxy using:

```bash
$ git config --global --unset http.proxy
$ git config --global --unset https.proxy
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Password Managers

If your operating system has a password manager configured, `git push` will
try to use it when it needs your username and password.  For example, this
is the default behavior for Git Bash on Windows. If you want to type your
username and password at the terminal instead of using a password manager,
type:

```bash
$ unset SSH_ASKPASS
```

in the terminal, before you run `git push`.  Despite the name, [Git uses
`SSH_ASKPASS` for all credential
entry](https://git-scm.com/docs/gitcredentials#_requesting_credentials), so
you may want to unset `SSH_ASKPASS` whether you are using Git via SSH or
https.

You may also want to add `unset SSH_ASKPASS` at the end of your `~/.bashrc`
to make Git default to using the terminal for usernames and passwords.


::::::::::::::::::::::::::::::::::::::::::::::::::

Our local and remote repositories are now in this state:

![](fig/github-repo-after-first-push.svg){alt='A diagram showing how "git push origin" will push changes from the local repository to the remote, making the remote repository an exact copy of the local repository.'}

:::::::::::::::::::::::::::::::::::::::::  callout

## The '-u' Flag

You may see a `-u` option used with `git push` in some documentation.  This
option is synonymous with the `--set-upstream-to` option for the `git branch`
command, and is used to associate the current branch with a remote branch so
that the `git push` command can be used without any arguments. To do this,
simply use `git push -u origin main` once the remote has been set up.

Here, we are telling Git to push the branch
to the origin (GitHub) repositories `main` branch.

::::::::::::::::::::::::::::::::::::::::::::::::::

We can pull changes from the remote repository to the local one as well:

```bash
$ git pull
```

```output
Already up-to-date.
```

Pulling has no effect in this case because the two repositories are already
synchronized.  If someone else had pushed some changes to the repository on
GitHub, though, this command would download them to our local repository.

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub GUI

Browse to your `weather` repository on GitHub.
Under the Code tab, find and click on the text that says "XX commits" (where "XX" is some number).
Hover over, and click on, the three buttons to the right of each commit.
What information can you gather/explore from these buttons?
How would you get that same information in the shell?

:::::::::::::::  solution

## Solution

The left-most button (with the picture of a clipboard) copies the full identifier of the commit
to the clipboard. In the shell, `git log` will show you the full commit identifier for each
commit.

When you click on the middle button, you'll see all of the changes that were made in that
particular commit. Green shaded lines indicate additions and red ones removals. In the shell we
can do the same thing with `git diff`. In particular, `git diff ID1..ID2` where ID1 and
ID2 are commit identifiers (e.g. `git diff a3bf1e5..041e637`) will show the differences
between those two commits.

The right-most button lets you view all of the files in the repository at the time of that
commit. To do this in the shell, we'd need to checkout the repository at that particular time.
We can do this with `git checkout ID` where ID is the identifier of the commit we want to
look at. If we do this, we need to remember to put the repository back to the right state
afterwards!



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Uploading files directly in GitHub browser

Github also allows you to skip the command line and upload files directly to
your repository without having to leave the browser. There are two options.
First you can click the "Upload files" button in the toolbar at the top of the
file tree. Or, you can drag and drop files from your desktop onto the file
tree. You can read more about this [on this GitHub page](https://help.github.com/articles/adding-a-file-to-a-repository/).


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub Timestamp

Create a remote repository on GitHub. Push the contents of your local
repository to the remote. Make changes to your local repository and push these
changes. Go to the repo you just created on GitHub and check the
[timestamps](../learners/reference.md#timestamp) of the files. How does GitHub
record times, and why?

:::::::::::::::  solution

## Solution

GitHub displays timestamps in a human readable relative format (i.e. "22 hours ago" or "three
weeks ago"). However, if you hover over the timestamp, you can see the exact time at which the
last change to the file occurred.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Push vs. Commit

In this episode, we introduced the "git push" command.
How is "git push" different from "git commit"?

:::::::::::::::  solution

## Solution

When we push changes, we're interacting with a remote repository to update it with the changes
we've made locally (often this corresponds to sharing the changes we've made with others).
Commit only updates your local repository.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub License and README files

In this episode we learned about creating a remote repository on GitHub, but when you initialized
your GitHub repo, you didn't add a README.md or a license file. If you had, what do you think
would have happened when you tried to link your local and remote repositories?

:::::::::::::::  solution

## Solution

In this case, we'd see a merge conflict due to unrelated histories. When GitHub creates a
README.md file, it performs a commit in the remote repository. When you try to pull the remote
repository to your local repository, Git detects that they have histories that do not share a
common origin and refuses to merge.

```bash
$ git pull origin main
```

```output
warning: no common commits
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/mo-eormerod/weather
 * branch            main     -> FETCH_HEAD
 * [new branch]      main     -> origin/main
fatal: refusing to merge unrelated histories
```

You can force git to merge the two repositories with the option `--allow-unrelated-histories`.
Be careful when you use this option and carefully examine the contents of local and remote
repositories before merging.

```bash
$ git pull --allow-unrelated-histories origin main
```

```output
From https://github.com/mo-eormerod/weather
 * branch            main     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
README.md | 1 +
1 file changed, 1 insertion(+)
create mode 100644 README.md
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- A local Git repository can be connected to one or more remote repositories.
- Use the SSH protocol to connect to remote repositories.
- `git push` copies changes from a local repository to a remote repository.
- `git pull` copies changes from a remote repository to a local repository.

::::::::::::::::::::::::::::::::::::::::::::::::::
