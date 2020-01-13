---
author: Olcay Bayram
layout: post
categories: Azure
published: true
title: Wildcard For Azure Devops Branch Policies
image: /img/AzureDevopsWildcardBranchCover.png
tags:
  - cloud
  - azure
  - devops
  - git
---
We use [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/) as our branching model on Azure Devops. When we create a new Release branch, we want it to be protected by default. By protection I mean, preventing unreviewed code changes.

This is not like setting a branch policy for an existing branch, it would be a wildcard for future branches. You need to do one extra step to access the correct page for a wildcard and this is not documented yet.

<!--more-->

1. Go to Branches page under Repos on Azure Devops.
1. If you already have a folder in your repo, you can skip to step 5.
1. Click __New branch__ and fil in the name like _release/tempBranch_.
1. Click __Create__.
1. Click __⋮ (vertical ellipsis)__ next to a folder.

    ![Azure Devops Wildcard Branch](/img/AzureDevopsWildcardBranch.png)

1. Select __Branch policies__ in the menu.
1. It will redirect to the branch policies page with the title _Branch policies for {folderName}/*_.
1. Here you can set the policies.
1. After setting the policies in place you can remove the _tempBranch_ if you created one.

![Azure Devops Wildcard Branch Policies](/img/AzureDevopsWildcardBranchPolicies.png)

The existing or future branches which matches the wildcard will be using the policies.

Cover photo by [Jan Huber](https://unsplash.com/@jan_huber?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/tree?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
