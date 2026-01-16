# GitHub Deploy Keys Setup Guide

## 🚀 What Are Deploy Keys?

Deploy keys are SSH keys used to grant a **single repository** read or
read/write access **without using a personal account**.\
They are commonly used for:

-   CI/CD pipelines
-   automated deployments
-   servers pulling private repos
-   Docker builds

------------------------------------------------------------------------

## ✅ Step 1 --- Generate Deploy Key

On the machine or server that needs access:

``` bash
ssh-keygen -t ed25519 -C "deploy-key" -f ~/.ssh/deploy_key
```
List all files in .ssh
``` bash
ls -l ~/.ssh
```

This creates:

    ~/.ssh/deploy_key
    ~/.ssh/deploy_key.pub

------------------------------------------------------------------------

## ✅ Step 2 --- Add Deploy Key to GitHub Repository

1.  Go to your GitHub repo\
    `Settings → Deploy Keys`
2.  Click **Add deploy key**
3.  Title it (ex: `server-deploy`)
4.  Paste the contents of:

``` bash
cat ~/.ssh/deploy_key.pub
```

5.  If needed, check **Allow write access**
6.  Save

------------------------------------------------------------------------

## ✅ Step 3 --- Add Private Key to SSH Agent

``` bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/deploy_key
```

------------------------------------------------------------------------

## ✅ Step 4 --- Test Connection

``` bash
ssh -T git@github.com
```

Expected output:

    Hi! You've successfully authenticated, but GitHub does not provide shell access.

------------------------------------------------------------------------

## ✅ Step 5 --- Clone Using Deploy Key

``` bash
git clone git@github.com:owner/repository.git
```

Example:

``` bash
git clone git@github.com:kodevite/ad-insights.git
```

------------------------------------------------------------------------

## 🔐 Deploy Keys vs Personal Access Tokens

  Feature      Deploy Key    Personal Access Token
  ------------ ------------- -----------------------
  Repo scope   Single repo   All repos
  Tied to      Machine       User
  Use case     automation    CLI pushes
  Security     High          Medium

------------------------------------------------------------------------

## 🎯 Best Practices

-   use separate keys per server
-   keep **read-only** unless necessary
-   never commit private keys
-   restrict server access

------------------------------------------------------------------------

## 🧹 Remove Deploy Key

GitHub:

`Repo → Settings → Deploy Keys → Delete`

Or remove locally:

``` bash
ssh-add -d ~/.ssh/deploy_key
rm ~/.ssh/deploy_key ~/.ssh/deploy_key.pub
```

------------------------------------------------------------------------

## 🎉 You now understand Deploy Keys!

Deploy keys help secure automated deployments without exposing user
accounts or passwords.
