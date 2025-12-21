# SSH Setup Guide

## Step 1 --- Check if you already have SSH keys

Run:

``` bash
ls -l ~/.ssh
```

If you see files like:

-   id_rsa + id_rsa.pub
-   id_ed25519 + id_ed25519.pub

Then you already have SSH keys.\
If not, continue to the next step.

------------------------------------------------------------------------

## Step 2 --- Generate a new SSH key

Recommended:

``` bash
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/habib_git
```

This creates:

    ~/.ssh/habib_git
    ~/.ssh/habib_git.pub

------------------------------------------------------------------------

## Step 3 --- Start the SSH agent

``` bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/habib_git
```

------------------------------------------------------------------------

## Step 4 --- Add your public key to GitHub

Show key:

``` bash
cat ~/.ssh/habib_git.pub
```

Copy the output and add to:

GitHub → Settings → SSH and GPG Keys → New SSH Key

------------------------------------------------------------------------

## Step 5 --- Clone using SSH

``` bash
git clone git@github.com:username/repository.git
```

------------------------------------------------------------------------

You are now set up and authenticated with SSH!
