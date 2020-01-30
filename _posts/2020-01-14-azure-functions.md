---
author: Olcay Bayram
layout: post
categories: Azure
published: true
title: Azure Functions
subtitle: Architect serverless solutions in Azure
image: /img/AzureFunctionCover.png
ogimage: /img/AzureFunctionOgImage.png
tags:
  - cloud
  - azure
  - function
serieName: serverless
---

Serverless, as the name suggests, is computing without managing a server. You would not care about any server specification to process your data. The server should already be provided by a cloud platform as a PaaS (platform as a service) or as a FaaS (function as a service). The two common approaches on Azure are Logic Apps and Functions.

## Is serverless computing right for my business needs?

No, it is not. You need to create clear requirements for your business needs then develop your application. You can answer this question only by analyzing your telemetry results from production. The serverless computing is a technical solution for a better infrastructure allocation and event driven design. There is not any engineer who can calculate necessacities without any data.

For example, functions have a timeout of 10 minutes maximum. If it is triggered by an HTTP request then it lowers to 2.5 minutes. Even if it responds in seconds, there is still a probability that it is executed continuously. In this case, it would be more expensive than having an app service or a VM.

## How serverless can help me?

You will be working only on your business logic code in the technology stack of your choice (.NET Core, Node.js, Python, Java and PowerShell Core). No more hassle about infrastucture, scaling up or down. Most importantly, you are charged based on what is used.

<!--more-->

## Bindings

You can declare bindings to connect to other services so you do not need to maange connections in your code. We want to work only on our business logic which is special for our needs.

Go to Azure documents for an up-to-date [list of triggers and bindings](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings) to connect to various storage and messaging services.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

A sample _function.json_ file to configure our function to be triggered when a message is added to __myqueue-items__ queue and then the return value is written to the __outTable__ table in Azure Table Storage.

## Triggers

A trigger is a special type of input binding that has the additional capability of initiating execution. Functions can be triggered by those services below;

- Blob storage
- Azure Cosmos DB
- Event Grid
- HTTP
- Microsoft Graph Events
- Queue storage
- Service Bus 
- Timer

## Create a function app

A function app is a way to organize and collectively manage your functions.

1. Sign into the [Azure portal](https://portal.azure.com/)
1. From the portal menu, select __Create a resource__.
1. Select __Compute > Function App__.
1. Select a resource group.
1. Give a globally unique function app name. This will serve as the base URL of your service.
1. Select __Node.js__ as runtime stack.
1. By default selections; __Windows__ as OS, __Consumption Plan__ as plan type, Azure Application Insights is __ON__ and a new storage account will be created to store your code, queue or tables.
1. Click __Create__. Deployment may take several minutes. When it is complete, you will be notified, then you can continue to create a function.

## Create a function

1. Go to __All resources__ page and select your function with the lightning bolt Function icon.
1. Select __➕ (Create new)__ next to __Functions__.

  ![Azure Function Create new](/img/AzureFunctionCreateNew.png)

1. On the quickstart page, select __In-portal__ and then __Continue__ below.
1. On the __CREATE A FUNCTION__ step, select __More templates...__ and then click __Finish and view templates__.
1. Select __HTTP Trigger__.
1. Enter a name for your trigger and click __Create__. Leave the Authorization level as _Function_, we will come to this later.
1. When it is complete, the _index.js_ file will be opened in an online editor. The code in the file has a simple logic which expects a name parameter as part of query string or request body, then responds with a message like _Hello {name}_.
1. If you select __View files__ tab on the right hand side, you seen _function.json_ file there. You can also go to __Integrate__ page of that function to see the bindings in a better UI.

```javascript
// Default template for a function triggered by an HTTP request
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```

## Test a function

We can use __Test__ tab on the right hand side menu but let's test the function on a separate window.

1. On the function editor page, select __</> Get function URL__.

  ![Azure Function Get function URL](/img/AzureFunctionGetURL.png)

1. This will create a URL with default key. We left the Authorization level as _Function_ so the default is funtion-specific API key. Copy the _URL_.
1. Open a new browser and paste the URL.
1. Add name parameter at the end of the URL `&name=olcay` so the URL is like `https://{functionAppName}.azurewebsites.net/api/{functionName}?code={APIKey}&name=olcay`
1. The result will be `Hello olcay`

> The API key can also be provided as a HTTP header named `x-functions-key` instead of `code` parameter in the query string. These keys can be managed on __Manage__ page.

## Monitor a function

If you go __Monitor__ page of a function on the left hand side menu, you can see the history of function executions. Since we had a log statement in our code as below, it would also be visible in the details of executions.

```javascript
context.log('JavaScript HTTP trigger function processed a request.');
```

Cover photo by [Michael Rogers](https://unsplash.com/@alienaperture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
