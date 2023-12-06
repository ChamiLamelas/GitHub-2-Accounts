# How to Push to a 2nd GitHub Account Anonymously

## Description 

In this document, I explain how to push to a 2nd GitHub account anonymously. I assume this is being done on a Unix system (OSX, Linux).

## Introduction 

Suppose that you already have set up your machine to work with 1 GitHub account. That is, when every you add, commit, and push changes are made with your name and email and then the SSH validation for your push is done with a single SSH key on your machine. Suppose now you wish to push using a second account (with SSH) anonymously. 

## Instructions

### Set up new SSH key 

Create a new SSH key with: 

```bash
ssh-keygen
```

Follow the prompts but make sure you do not overwrite your existing SSH keys. By default, the SSH keys are put into `.ssh/id_rsa`. When you make a second SSH key, you could put them in say `.ssh/id_rsa2`. 

### Add Key to GitHub 

Add the newly generated key above to the 2nd GitHub account online. Add the key as normal under SSH keys under Settings. 

Note, it is crucial you use the new key. If you use the same key you use with any other GitHub account, then adding the new key will fail and GitHub will give an error saying that the key is already in use.

### Modify the SSH config 

Back on your machine, modify the file `.ssh/config` as follows. Suppose your second account is named `my-second-account` and that your SSH keys are in `~/.ssh/id_rsa2` and `~/.ssh/id_rsa2.pub`.

```text
Host github.com-my-second-account
  Hostname github.com
  IdentityFile ~/.ssh/id_rsa2
```

### Clone the Repository of interest 

Suppose the repository you want to work on is stored at `https://github.com/my-second-account/myrepo`. 

Regularly, you would clone this with: 

```bash
git clone git@github.com:my-second-account/myrepo.git
```

Instead, run this command: 

```bash
git clone git@github.com-my-second-account:my-second-account/myrepo.git
```

### Set the remote

Once you have cloned, `cd` into `myrepo` and run: 

```bash 
git remote set-url origin git@github.com-my-second-account:my-second-account/myrepo.git
```

### Set your commits to be anonymous 

Now, set your commits to be anonymous (no name or email). Do the following (still inside `myrepo`):

```bash
git config user.name 'Anonymous'
git config user.email '<>'
```

### Add, Commit, and Push

Now you can go ahead and make changes, `add`, `commit`, and `push` them as normal. 

If you want to confirm that your commits are anonymous, you can run `git log` before pushing. 

Pushing will now be done using the key stored in `id_rsa2` which is the one on GitHub. 

## Acknowledgements 

* [This article](https://theboreddev.com/use-multiple-ssh-keys-different-git-accounts/) on pushing to multiple github accounts.
* [This SO answer](https://stackoverflow.com/a/48458633) on how to commit anonymously.
