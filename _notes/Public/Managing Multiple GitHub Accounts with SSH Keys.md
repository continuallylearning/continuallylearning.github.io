---
title: Managing Multiple GitHub Accounts with SSH Keys
feed: show
date: 2025-12-27
---
I maintain multiple GitHub accounts on the same machine:

- `eric-rosen`
- `continuallylearning`

The cleanest and most reliable way to work with multiple GitHub accounts on a single computer is to use **separate SSH keys** and map each one to a custom SSH host. This avoids constantly switching credentials or accidentally pushing to the wrong account.

The setup steps involve:

- [Create an SSH key for each GitHub account](#Create%20an%20SSH%20key%20for%20each%20GitHub%20account)
- [Assign a hostname to each SSH key](#Assign%20a%20hostname%20to%20each%20SSH%20key)
- [Configure Git remotes to use the correct host](#Configure%20Git%20remotes%20to%20use%20the%20correct%20host)
- [Set git config user name and email](#Set%20git%20config%20user%20name%20and%20email)

# Create an SSH key for each GitHub account

Each GitHub account should have its own SSH key pair (public + private). Follow GitHub’s official documentation for each GitHub account, making sure to give custom-names for each SSH key. You can name these hosts and key files however you like—just keep them consistent:

- Generate a new SSH key and add it to the SSH agent:  
	[GitHub Docs – Generating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- Add the public key to your GitHub account:  
	[GitHub Docs – Adding a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

After this step, you should have multiple private and public keys on your machine, one per GitHub account. For example, when I look at the files in the `~/.ssh` folder, `ear_ed25519` and `ear_ed25519.pub` are the private and public keys associated with the `eric-rosen` GitHub account, whereas `cl_ed25519` and `cl_ed25519.pub` are the private and public keys associated with the `continuallylearning` account:

```bash
eric@ericbot:~$ ls -lh ~/.ssh
    total 52K
    -rw------- 1 eric eric  432 Dec 27 17:25 cl_ed25519
    -rw-r--r-- 1 eric eric  118 Dec 27 17:25 cl_ed25519.pub
    -rw-rw-r-- 1 eric eric  250 Dec 27 17:31 config
    -rw------- 1 eric eric  419 Dec 27 17:30 ear_ed25519
    -rw-r--r-- 1 eric eric  109 Dec 27 17:30 ear_ed25519.pub
```

The GitHub instructions tell you to add the ssh keys to the SSH agent via `ssh-add /PATH/TO/PRIVATE_KEY`, make sure you've done this by checking that all the keys are loaded. For example:

```bash
eric@ericbot:~$ ssh-add -l
256 SHA256:rVtbltX9gB1gA+oJ1tdsftbdnXduNdwzedqDKeIMlNE eric.andrew.rosen@gmail.com (ED25519)
256 SHA256:s19ObVbe6dheJtIPeuhRKJceoi6kV3oJzpdU6Rv/GVk continuallylearninggithub@gmail.com (ED25519)
```

# Assign a hostname to each SSH key

Next, edit your SSH config file:

```bash
~/.ssh/config
```

Define a separate host entry for each GitHub account:

```bash
# GitHub account alias for eric-rosen
Host github-eric
    HostName github.com
    User git
    IdentityFile ~/.ssh/ear_ed25519
    IdentitiesOnly yes

# GitHub account alias for continuallylearning
Host github-cl
    HostName github.com
    User git
    IdentityFile ~/.ssh/cl_ed25519
    IdentitiesOnly yes
```

Here:

- `IdentityFile` points to the private SSH key for that account
- `Host` is a custom alias you’ll use instead of `github.com`
- `IdentitiesOnly yes` tells SSH to use only the key(s) specified by `IdentityFile` for this host, and ignore any other keys loaded in the SSH agent.

## Verify SSH access for each account

Test that each SSH key is correctly associated with its GitHub account:

```bash
ssh -T git@github-eric
ssh -T git@github-cl
```

Each command should authenticate successfully and indicate which GitHub account you’re logged in as.

# Configure Git remotes to use the correct host

To push and pull using the correct account, your repository must:

- Use SSH (not HTTPS)
- Reference the correct SSH host

Go into directory of the repository, and check your current remote:

```bash
git remote -v
```

Example output:

```bash
origin  git@github.com:eric-rosen/eric-rosen.github.io.git (fetch)
origin  git@github.com:eric-rosen/eric-rosen.github.io.git (push)
```

If the host or username is incorrect, update it using the following format:

```bash
git@{host-name}:{repo_username}/{repo_name}.git
```

For example:

```bash
git remote set-url origin git@github-eric:eric-rosen/eric-rosen.github.io.git
```


# Set git config user name and email
[Highly relevant resource about where git looks for this information and in what order](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)
At this point, you can push commits to your repository correctly. However, if you'd like to make sure that the shown authors of the commits represents your account, run this command to see the author name and account to be used:

```bash
git config --show-origin --get user.name
git config --show-origin --get user.email
```

If this matches the account you want, great! If not, it may be grabbing that info from a git config (it'll print it out). Otherwise, you can set the name and email directly, for example:

```bash
# for eric-rosen
git config user.name "Eric Rosen"
git config user.email "eric.andrew.rosen@gmail.com"
# for continuallylearning
git config user.name "continuallylearning"
git config user.email "continuallylearninggithub@gmail.com"
```

# Final checklist

- `ssh -T` works for each custom host.
- `git remote -v` shows the correct SSH host and username.
- Pushes and pulls go to the intended GitHub account.
- Commits are being shown to be authored by desired account.

If both the SSH test and the remote configuration are correct, you’re all set.