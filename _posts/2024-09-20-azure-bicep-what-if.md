---
author: Olcay Bayram
layout: post
categories: Software
published: true
title: Haste is a tree whose fruit is regret
image: /img/oracle.jpeg
tags:
  - cloud
  - bicep
  - azure
  - IaC
---

# What If

Life is full of regrets. We sometimes wish we had made different decisions. However, when you use Bicep as your ARM template language, you don’t need to worry about your actions. You can simply ask Azure CLI for the changes, even before you apply them.

- Will I break something?
- How will this deployment affect existing resources?
- Am I going to delete the shared service bus that we use throughout the whole company, causing every flow in production to break, and then everyone will find out I did it, and I’ll be immediately fired, and I won’t find another job in this economy, and I won’t be able to support a marriage, and my spouse will divorce me, and I’ll have to move back in with my parents after 20 years, and they won’t be able to look me in the eye but will talk about me in bed and wonder why anyone would deploy without checking the output from the `what-if` operation?

The `what-if` operation helps you anticipate the consequences of a new deployment. It does not make any changes to existing resources. The operation only predicts what will change if the specified template is deployed at a resource group and subscription level, providing a clear understanding of what is going to happen.

It compares the current state model to the desired state model. However, it is not perfect yet, so sometimes it may predict incorrectly, but the team is aware of this limitation.

## Usage

For example, assume you are changing the storage type in a template that deploys a single storage account to an existing environment.

```terminal
az deployment group what-if --resource-group PincushionAppTstRg --template-file $templateFile --result-format FullResourcePayloads
```

The preceding command produces the following results:

```text
Resource and property changes are indicated with this symbol:
 ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/.../resourceGroups/PincushionAppTstRg

 ~ Microsoft.Storage/storageAccounts/... [2019-06-01]
  ~ sku.name: "Standard_LRS" => "Standard_GRS"

Resource changes: 1 to modify.
```
{: file='Output'}

The six types of changes: Create, Delete, Ignore, NoChange, Modify, Deploy

## Other cloud platforms

As a cloud agnostic developer, I would like to share the commands to achive the same operation on other platforms.

| Amazon Web Services (AWS)    | `aws cloudformation create-change-set`                   |
| Google Cloud Platform (GCP)  | `gcloud deployment-manager deployments update --preview` |
| Terraform (Multi-Cloud)      | `terraform plan`                                         |

"Haste is a tree whose fruit is regret." is a Turkish proverb.