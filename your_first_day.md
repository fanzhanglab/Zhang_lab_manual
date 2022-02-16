
# Your First Day #

Author: Fan Zhang

Date: June 22, 2021

Last update: Feb 16, 2022


Congratulations, and welcome! This guide is written by people who've been where you are and want to help you to have a productive and successful experience here.

First, ask Dr. Zhang to add you to the lab [Trello](https://trello.com/en-US) space and [Github team](https://github.com/fanzhanglab). We use Trello to manage project tasks between lab members, and use Github to perform data analysis for each project-driven repository.

Second, you should first read through this document and configure your computer with necessary software. Understanding the contents in this document will help you establish a good habit on conducting reproducible computational experiments.There are a few things you need to do to get started on data analysis: 

```
We do 1) large-scale processing at Boulder Summit (will be Alpine for future), 
2) then grab the processed data to our local box (e.g. Jupyter notbook), and 
3) shared code and key results at Github team and 
4) archive key results data and figure at Boulder Summit storage space.
```


## Request your account on our server `Summit`
1. Boulder Summit is our primary linux server for computation including read mapping, large-scale gennomics data computation, running jobs. Boulder Summit supports HPC computing. First, you need to obtain RMASS Access to Summit by getting an XSEDE account (for Anschutz campus users) following [these steps](https://curc.readthedocs.io/en/latest/index.html).

2. Read the [details](https://curc.readthedocs.io/en/latest/index.html) (e.g. "Running Jobs"), and installing Python and R with Anaconda following the steps [here](https://curc.readthedocs.io/en/latest/software/python.html).

3. Learn the structure of the [Filesystems](https://curc.readthedocs.io/en/latest/compute/filesystems.html).

A terminal application allows you to connect to our server, check the directories and files on it, and perform computations. 

Once you've set up your working environment, then you'll need to start learning about the software, systems and culture for the lab. 


## Set Up Your Jupyterlab Working Environment locally
We perform downstream analysis using Jupyterlab. So far the Summit doesn't allow XSEDE users to access Jupyter notebook given the differences between Boulder and Anschutz in authentication. Please install R version 4.1.2 and Python 3.9.10 locall, and jupyterlab. For the details of how to install Jupyterlab, please check our lab Trello board.

[Here](./reference/linux_tutorial.pdf) is a recommended unix tutorial and tips. You can skip the first part and start reading from Page 6.

Writing good bash scripts can be challenging. Use this [guide](http://www.tldp.org/LDP/abs/html/) to get the basics. And use this [guide](http://www.davidpashley.com/articles/writing-robust-shell-scripts.html) to make your code more robust.



## Sharing code and results through Github
We manage our code repository for each project at https://github.com/fanzhanglab. We strongly encourage every lab member to share code and key results at Github for the purpose of tracking changes, sharing experience, and enhancing collaborations.

Please learn version Control by Git:

We use a version control system git in the lab extensively. The system has a tree structure that bears all the work in the shared drive. Git allows many people to work on the same project at the same time, and avoids the confusion caused by keeping track of the updated versions of the same file. The following paragraph describes the steps for your to complete a job using Git.

At the beginning of your work, you should **clone** a copy of the repository to your local drive and this is your own branch of the central repository. You can edit the files under the copied repository. All work should be saved under the copy of the repository in the end. Here you may **stage (add)** your changes to save it on your own branch, **commit** your changes to make recorded changes to your own branch, and **push** your changes to make recorded changes to the main repository. Also, you would be able to **fetch** from the main repository to see what others have done to the same experiment and **pull** the changes if you feel like merging others' work with yours. All the changes made to the main repository is recorded so that the activities at any time spot can always be undone. 

If there's any collision where you and your teammate have done the same changes to the repository, only the first one who pushes would be able to push and the second push would be denied. Whenever this happens, the second person should first pull from the first person and push again to make other changes. To keep the collision to a minimum, you and your teammate should keep the communication open and distribute the work properly to make sure that you are working accordingly and efficiently on the project. 

The GIT Graphic User Interface (GUI) is very useful and easy to start with software for version control purposes in the lab. The detailed instructions of the commands will be introduced in later parts of this document. 

**Windows:** 

You can either download [Git for Windows](http://msysgit.github.com/) or [SourceTree for Windows](http://www.sourcetreeapp.com). 

The configuration steps below are for SourceTree. After downloading it, install all as default.

Click Clone/New -> clone repository ->Source: `ssh://username@xxx/home/fzhang/zhanglab/git/projectfoldername.git`

You need to enter your password for this.

Target: should be somewhere safe.

Clone Type: Standard.

**Mac:** 

<http://www.sourcetreeapp.com>.

Run Source Tree -> Clone Repository ->
Source Path:`ssh://username@xxx/home/fzhang/zhanglab/git/projectfoldername.git`

Destination Path: should be somewhere safe. 

You need to enter your redwood password for this.

**Linux:**

However most of the work in the lab is done over command line in linux. A good intro is available [here](http://pcottle.github.io/learnGitBranching/). And here is a good [cheat sheet](http://zrusin.blogspot.com/2007/09/git-cheat-sheet.html) for Git commands.

![Alt text](./figures/git_cheat_sheet.png)

###### Commonly Used GIT Commands ##

- `$ git status`: Show the working tree status.
- `$ git clone`: Clone the existing project. 
- `$ git pull`: Fetch from and merge with another repository or a local branch.
- `$ git add`: Add file contents to the index.
- `$ git commit`: Record changes to the repository. You can use `git commit -m` and a quoted sentence to describe what you have done.
- `$ git push`: Update remote refs along with associated objects. Only pushed files can be seen by others. Committed files won't be seen by others.

###### Checking what branches are available ##

* `$ git ls-remote` shows all of the branches that are available remotes regardless of whether you're tracking them locally or not.
* `$ git branch` shows all of the _local_ branches 
* `$ git branch -r` shows all of the _remote_ branches being tracked

###### Switching branches ##

Often we want to switch to a different branch that's available on the server.  

1. `$ git remote` Shows the names of the remote server nicknames that you can pull from. Call this `<repository>`.
	
2. `$ git ls-remote <repository>` Shows the branches that are available on that repository that you can pull from. It's best to pull a branch that is also a head. You can isolate those using `$ git ls-remote --heads <repository>`.  
	
3. `$ git pull <repository> <branch>` Pulls a current branch from the repository. Make sure you don't have any uncommitted changes; if so use the stash. 

##### More Advanced Commands



###### Git References ##
* [Git in 5 Minutes](http://scottr.org/presentations/git-in-5-minutes/)
* [Version Control with Git - Software Carpentry](http://software-carpentry.org/v5/novice/git/index.html)

###### Some Common Git Problems & Solutions ##

1. Sometimes a user will be denied access to a shared repository on redwood. When cloning the repository, include your username in the ssh link as shown below:

	`$ git clone ssh://yourusername@redwood.wpi.edu/research/fzhang/zhanglab/git/common.git `

2. The difference between clone and pull.
	
	Clone: copy the directory to your local drive/create your own branch (only do once for each project).

	Pull: copy the changes to the repository to your local drive (done multiple times when developing the project, manually controlled by the project manager).

3. Great git Manual Page:
<http://www.kernel.org/pub/software/scm/git/docs/>

###### GIT Flow in Zhang Lab ##

You might have read about the git repository structure of a game design company. However the structure can be different according to the types of work we do and the Zhang Lab has its own GIT flow. 

![Alt text](./figures/GIT_flow.jpg)

According to the picture, we can see that we open a new repository for each project and create a master repository for it. Under that we have the develop branch where most of the work are kept. Under the develop branch we may branch out for various features/experiments of the project, and even smaller ones for individual work. After a good amount and quality of code is written, we may push it back to the develop branch. And when the project is developed enough to write a paper, we would push it to release with all the paper writing components. 


## Archive key results data and figure
To deliver the results, we will eventually publish our code together with key metadata files. For example, we will need to save a copy of the identified cluster identity for each cell barcode. Once we finalize the results, we will transfer the key results data and figure to our Boulder Summit storage space under `cd /projects/fanzhanglab@xsede.org` where we have 250 GB/user space.

