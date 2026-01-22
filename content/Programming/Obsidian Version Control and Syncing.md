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