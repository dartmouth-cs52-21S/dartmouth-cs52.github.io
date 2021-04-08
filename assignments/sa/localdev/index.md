---
layout: page
title: Local Dev Environment — Short Assignment
published: true
comment_term: sa-localdev
---

First things first.  You need a local development environment.  

*Why is this important you ask.  Couldn't I just copy code over to a server every time I make changes?  You could.  But imagine debugging,  every change you make you spend a few seconds maybe copying code over, or pushing git.  This time adds up.  Save yourself hours of your life, get your laptop set up for coding now.*


## Tools! ##

🚀Install [VSCode](https://code.visualstudio.com/download). VSCode is a code editor. VSCode is an open source, made by Microsoft
web technology written in html+css+javascript using a platform called [Electron](https://electronjs.org/). Using Electron to create a desktop app is an option for the final project.  We'll be using VSCode for coding and utilizing a lot of its plugins. After installing and starting up VSCode, run: *cmd + shift + p -> Shell Command: Install 'code' command in PATH* to be able to run `code` from the command line. *You are welcome to use another editor like [Atom](http://atom.io) instead, but we'll be adding various plugins and you'll need to make sure the equivalent functionality such as eslinting is available.*

🚀If you don't have it, install [Google Chrome](https://www.google.com/chrome/browser/desktop/).  Why?
Because we'll be using [Chrome Developer Tools](https://developers.google.com/web/tools/chrome-devtools/) for debugging — more on that later.  Also because all the other browsers support slightly different standards, and Chrome simply displays pages more correctly and sticks to the actual standards so we can all be on the same page.  To re-iterate: you are required to use Chrome for this class.

🚀 If you don't have git set up on your machine: Install [GitHub Desktop](http://desktop.github.com).  We will be mostly using `git` from the command line but installing the App gives you a nice tree visualization and setups up your local git environment.


### OS X ###

🚀Install [Homebrew](http://brew.sh/).  This is a package manager for all things open source.
We will use `brew` extensively to setup your dev environment.

### Various Linux ###

Most of the directions through the class will be very similar on the various flavors of linux.  Mostly just replace any `brew` command with the package manager for your system (`apt` on Ubuntu or `yum` on Fedora).

### Windows ###

Windows is a trickier dev environment.  We'll try to provide directions but these may need revision.

In general though whenever we refer to 💻 Terminal/iTerm you can just use ***Git Shell*** (which comes bundled with the GitHub app).  You may want to check out [Chocolatey](http://chocolatey.org) which is a package manager for Windows.

You can also try out the newish [ubuntu in windows](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) if you have Windows 10 — no guarantees there.

You could also run a [linux virtual machine](https://www.lifewire.com/run-ubuntu-within-windows-virtualbox-2202098) if you don't have windows 10 / the above didn't work out for you.  This runs a self-contained linux OS on your windows box.

*see the `#windows` Slack channel for support*

## Command Line ##

We'll be doing a lot of commandline stuff.  We'll introduce stuff as we go, so do not fear, commandline is best.

If you are interested here's a tutorial that gives you more than enough to be a master:
[learn just enough commandline to be dangerous](https://www.learnenough.com/command-line-tutorial)

Before you even think about opening *Terminal*, you should download [iTerm2](https://www.iterm2.com/downloads.html). It is open-source, and vastly superior to the default terminal: panes, tabs, colors, unlimited scrollback, etc.

You can also download a git command prompt to make life easier when we work with git later on. You can find a cool one [here](https://github.com/jimeh/git-aware-prompt). Follow the instructions on the README at that link to install it!

Here's some commandline basics:

* `ls -la`  will list files in current directory
* `pwd`  will show current directory
* `cd  somedirectory`  will change directory to *somedirectory*
* `cp source target` will copy files
* `mv source target` will move
* `rm somefile` will remove that file (permanently, bypassing trash)
* `mkdir somedirectory` will create a folder (directory)

On OSX:

* `open somefile` will open the file in the default app associated with it
* `open somedirectory` will open that folder in Finder


## Register a Domain!

Lets start off the term right, with a domain of your own that you'll use throughout the course.

We'll use NameCheap as our registrar.  Namecheap is a good [net citizen](https://www.netneutrality.com/) and have free domains for students.

🚀go to: [https://nc.me/](https://nc.me/)
and lets register you a domain!

![](img/namecheap.jpg){: .fancy }

🚀I'm going to grab one for cs52 now too!

![](img//buy.png){: .fancy .small }

🚀When prompted choose Github Pages Setup

![](img//choose_github_pages.png){: .fancy .small }

🚀Now, the automatic Github Setup in the next screen may do some of the steps below for you. It has a tendency to fail for a variety of reasons (don't worry if that happens to you) so the below are the full directions in case.

<!-- ![](img//setup.jpg){: .fancy .small } -->

## Create a GitHub Repo

🚀 go to [github.com](http://github.com) if you don't have an account, set one up! Use your .edu account and get the [GitHub Student Developer Pack](https://education.github.com/pack)

🚀 create a new repo for your domain, name it with *your* domain name. 

*Note: Keep this in your own personal github — later we'll use repos under the course organization but for this one your own is fine.*

![](img//create_repo.png){: .fancy .small }

*Note: Do not select "Initialize this repository with a README", we'll create one manually later.*

Grab the URL for the repository (either SSH or HTTP depending on how you have git setup.  If SSH gives you trouble just try HTTP.)

**Note:**  the following are transcripts of MY terminal session while setting up **cs52.me** with my github URL.  You should replace anything with cs52.me with your repo name and your github URL instead.

### Clone Repo

```bash
💻 git clone git@github.com:dartmouth-cs52/cs52.me.git
Cloning into 'cs52.me'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
💻 cd cs52.me/
💻 pwd  #checking current working directory
/Users/tim/Sandbox/cs52.me
```
{: .example}

### Create some HTML

```bash
💻 code index.html
```
🚀basically just start VSCode and create a file called `index.html` in your cloned workspace.

![](img//index_html.png){: .fancy .small }


### Test Locally

```bash
💻 python -m SimpleHTTPServer 9000
Serving HTTP on 0.0.0.0 port 9000 ...
127.0.0.1 - - [09/Jun/2016 18:25:18] "GET / HTTP/1.1" 200 -
```
*If you are running python3 try: `python -m http.server 9000` instead.*

❓does anybody remember what `127.0.0.1` is the address for?

Lets check out what we have made: [http://localhost:9000](http://localhost:9000)

![](img//localhost.png){: .fancy .small }

Note: to quit the python server type: ctrl+c
(this is a default for many commandline processes).

### Git Add

```bash
💻 git status  #check and see what the story is
On branch main

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  index.html

nothing added to commit but untracked files present (use "git add" to track)
💻 git add index.html  #lets track index.html
💻 git status  #check again -- I'm OCD with this
On branch main

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

  new file:   index.html

💻 git commit -am "its hideous"   #please make yours prettier 😃
[main (root-commit) 93a5c69] its hideous
 1 file changed, 10 insertions(+)
 create mode 100644 index.html
```
{: .example}

### Git Branch

Ok, now we have a local commit!  However before we push our changes.. GitHub Pages requires that the git branch we use for their hosting is named `gh-pages`.

So lets rename our branch!

```bash
💻 git branch
 * main
💻 git branch -m main gh-pages
💻 git branch
 * gh-pages
```
{: .example}

### Git Push

```bash
💻 git push -u origin gh-pages
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 289 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:dartmouth-cs52/cs52.me.git
 * [new branch]      gh-pages -> gh-pages
```
{: .example}

*Note: On GitHub you might want to double check your branches. You could either change settings to use main branch, or use gh-pages branch, but make sure that you don't have both.*

### Default GitHub Pages Hosting

By default GitHub Pages, when it sees a branch named `gh-pages`, will publish it at `http://<github username>.github.io/<repository name>`.

*You can also go into settings and turn on github-pages for a differently named branch.*

You can make sure of this by going to Settings for your repository.

![settings](img//settings.png){: .fancy .small }

![default hosting](img//default_hosting.png){: .fancy .small }


### CNAME hosting

But we want our domain!

Ok. Add another file to your local workspace named `CNAME`.  Inside this file put in just one line with your domain name.

![cname](img//cname.png){: .fancy .small }

🚀 Now you have to `git add` and `git commit` and `git push` this new file.  See [Git Add](#git-add) section (skip branching as that is a one time thing).

### DONE!

As soon as you `git push` your site should become available at your URL!  *Note: actually it might take up to 5 minutes for it to start working with your domain name as github pushes your content to various servers.*

![](img//done.png){: .fancy .small }

Well, almost, it might not quite be working yet.

### What About DNS?!?!

You didn't actually have to do any DNS setup,  what happened?

NameCheap should have set up the right Domain Name Service Records automatically for you. In NameCheap you can inspect what it did:

![](img/a-records.jpg){: .fancy .large }

Take a look at your settings in your Namecheap Dashboard.

Note the several IP addresses in the `A Record` fields,  those are github servers ([apex domain setup](https://help.github.com/articles/setting-up-an-apex-domain/#configuring-a-records-with-your-dns-provider))

⚠️__WARNING__⚠️
If you have a *URL Redirect Record / CNAME Record* then the github setup failed. You should delete both the *URL Redirect Record* and the *CNAME Record* in NameCheap and add in the *A Record* per the [github apex domain setup](https://help.github.com/articles/setting-up-an-apex-domain/#configuring-a-records-with-your-dns-provider). Here's [more on adding in *A Records*.](https://www.namecheap.com/support/knowledgebase/article.aspx/319/2237/how-can-i-set-up-an-a-address-record-for-my-domain). Basically just make it match the above screenshot, you may have to wait a little while for the changes to work.  

🚀 Try doing a reverse DNS lookup (IP address to name):

```bash
💻 host 185.199.108.153
Host 153.108.199.185.in-addr.arpa. not found: 3(NXDOMAIN)
```

Wait, so it doesn't even have a DNS entry? But how does github know what page to show? Does that machine serve lots of addresses?

Yes, those IPs point to load balancers that direct requests based on the transport packet which happens to have the domain name requested in it.  This is where that magical `CNAME` file comes in.  That tells GitHub Pages that your repository should be served when requests come in for that domain. :cool:


### To Turn In

1. GitHub repository URL
1. your working domain name URL
