---
tags:
  - clblogs
---

>[!summary]
>> I use Git(Hub) for my version control. I use the [obsidian-git](https://github.com/Vinzent03/obsidian-git) plugin on my desktop to push to my remote repository. I treat the remote repo `main` branch as the ground truth. I can also just use git in terminal if I want to.
>>
>> I use [Obsidian Sync](https://obsidian.md/sync) to sync my obsidian vaults across devices. It provides versioning but I don't use that feature. 

# What is Version Control and Syncing
Version control and syncing are two important and distinct processes for making sure your Obsidian vault is properly maintained across time (version control) and different devices (syncing). 

>[!tip]
>> I’d always recommend some form of version control, while syncing is only needed if you want to access your vault across multiple devices

# Version Control 

*Version control* is how we keep track of the history of changes made to the vault. With version control, we maintain snapshots of what the vault looked like over time, and even “jump back in time” to the past, or see the history of changes that led to the current vault.  I treat version control as keeping track of  the “ground truth” state and history of my vault. 

>[!example]
>> I use Git(Hub) to do my version control. I frequently make pushes to my repository, and loosely review the diffs before pushing. I can do this lazily because if I push something incorrectly, I can always jump back to an older state. 

>[!warning]
>> If you have sensitive information in your vault, the safest way to maintain is to keep it on your local machine and just use Git (instead of GitHub).

>[!tip]
>> I recommend having a `.gitignore` file for anything you don't want to push. You can see the one I use for my vault [here](https://github.com/eric-rosen/erosen_obsidian/blob/main/.gitignore).

# Syncing

*Syncing* is how we ensure that the vault has the same content across devices. It allows us to distribute our “ground truth” version of the vault to other devices that have my vault. 

>[!example]
>> I use Obsidian Sync to sync my vault content across my devices (laptop, desktop and phone). 

# Why use both GitHub and Obsidian Sync?

>[!summary]
>> I started with using GitHub for both version control and syncing, then only added in sync later to make phone integration easier. 


You can use either GitHub or Obsidian sync to do both version control and syncing. So why use both?

I started with just GitHub for version control because
1.  I am very familiar with it (I use it all the time for all my work and personal coding projects)
2. I absolutely love my notes rendering in GitHub in a similar way to my website. You can see one of my math blog posts here: without doing anything besides using GitHub I can effectively host my content there too. Makes it super easy to be anywhere, even without obsidian app, to see my past content and commit history: https://github.com/continuallylearning/continuallylearning.github.io/blob/v4/content/Math/Reparameterization%20trick.md
3. I love all the features of GitHub (GitHub actions, .gitignore, branches, rebasing, and merging). I haven’t checked how fully featured sync is compared to GitHub.
4. While I hope Obsidian Sync is around forever, if it goes down, I don’t want to lose my history. I am confident that GitHub will be around for a while. I am a big fan of file over app philosophy / having complete control over my content and version history, and I have my repository local so I always own my versioning. 
5. I haven’t checked Obsidian sync, but I love having control over what files I commit, and seeing diffs all the time for me to control manually. 

Pulling from GitHub across devices was actually my original syncing method. It worked fine for my desktop and laptop, but for iPhone, it’s more annoying to set up and use GitHub. So since obsidian sync makes it seamless on my phone + I like supporting the developers, I decided to use sync just for that.