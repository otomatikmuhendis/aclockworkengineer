---
author: Olcay Bayram
layout: post
categories: Software
published: true
title: Fetch Azure Function Settings for Local Development
image: /img/AzureConfigToLocalSettings.png
tags:
  - programming
  - chatgpt
  - azure
---
To run Azure Functions locally on your development machine, you need to use a local development environment such as Visual Studio Code or the Azure Functions Core Tools. This local development environment allows you to simulate the Azure Functions runtime on your machine.

The `local.settings.json` file is used to store configuration settings for your Azure Functions when running locally. These settings include things like connection strings, app settings, and other configuration values that are used by your function code.

When you create an Azure Function project in Visual Studio Code or with the Azure Functions Core Tools, a default local.settings.json file is created for you. You can edit this file to add or modify configuration settings as needed.

<!--more-->

When you run your Azure Functions locally, the `local.settings.json` file is read by the Azure Functions runtime to get the configuration settings that your function code needs to run. Without this file, your function code may not be able to access the resources it needs to function correctly, such as a database or a storage account.

## Risks of committing the settings to Git

It is generally considered risky to commit your `local.settings.json` file to Git or any other version control system for a few reasons:

**Security**: The local.settings.json file contains sensitive information such as connection strings, API keys, and other configuration values that are specific to your local development environment. By committing this file to Git, you are potentially exposing this sensitive information to anyone who has access to the Git repository. This could include other members of your development team or even attackers who gain access to your repository.

**Compatibility**: The local.settings.json file is specific to your local development environment and may contain settings that are not compatible with other environments. For example, you may have a connection string that points to a local database that is not available in other environments. By committing this file to Git, you may cause compatibility issues when other developers try to run your code on their own machines.

**Overwriting**: If you have multiple developers working on the same codebase and they all commit their own local.settings.json files, it can be difficult to reconcile the differences between them. This can lead to overwriting of configuration settings and other issues that can be difficult to diagnose and fix.

To mitigate these risks, it is generally recommended that you exclude the local.settings.json file from your Git repository by adding it to the .gitignore file. Instead, you can provide default configuration values in your code or use a separate configuration system such as Azure App Configuration or Azure Key Vault to store sensitive information. This way, each developer can configure their own development environment without exposing sensitive information to others, and compatibility issues can be avoided.

## Creating the settings file manually

If the local.settings.json file is missing from a cloned project, you can create a new one by following these steps:

1. Open your code editor or IDE and navigate to the root folder of the project.

1. Create a new file named "local.settings.json" in the root folder.

1. Copy the following template into the file:

    ```json
    {
      "IsEncrypted": false,
      "Values": {
        "AzureWebJobsStorage": "",
        "FUNCTIONS_WORKER_RUNTIME": "dotnet",
        // add your configuration values here
      }
    }
    ```

1. Replace the // add your configuration values here comment with your own configuration values. These values should include any connection strings, app settings, or other configuration values that your function code needs to run.

1. Save the file.

Once you've created the local.settings.json file with the appropriate configuration values, you should be able to run the Azure Function locally on your development machine. However, note that if the missing file contained sensitive information such as API keys or passwords, you may need to obtain that information from a secure source before you can populate the new file.

## Fetching the settings using Azure Function Core Tools

After logging in to Azure Account and setting our desired subscription on Azure Function Core Tools, we can fetch our app settings by running the command below;

```powershell
func azure functionapp fetch-app-settings '<function-name>' --output-file local.settings.json
```

Then decrypt the values with the command below;

```powershell
func settings decrypt
```

## Fetching the settings using Azure Portal

To fetch an Azure Function setting from the Azure Portal, you can follow these steps:

1. Log in to the [Azure Portal](https://portal.azure.com/).
1. Navigate to your Azure Function App.
1. Click on the "Functions" option in the left-hand navigation menu.
1. Select the Function that you want to fetch the settings for.
1. Click on the "Configuration" option in the left-hand navigation menu.
1. You should now see a list of settings for your Azure Function, including any Application Settings and Connection Strings that have been configured. These settings can be updated or modified as needed.
1. Click `Advanced edit` button copy the content to the first textarea below.
1. Click `Convert to local.settings.json format` button to convert the content `local.settings.json` file format.

<textarea id="inputText" style="height: 326px;width: 100%;">
[
  {
    "name": "clientid",
    "value": "...",
    "slotSetting": false
  },
  {
    "name": "AzureWebJobsStorage",
    "value": "UseDevelopmentStorage=true",
    "slotSetting": false
  }
]
</textarea>

<button type="button" onclick="copyText()">&#8659; Convert to local.settings.json format &#8659;</button>

<textarea id="outputText" style="height: 326px;width: 100%;">
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "clientid": "..."
  }
}
</textarea>

<script>
    function copyText() {
      var inputText = document.getElementById("inputText");
      var outputText = document.getElementById("outputText");

      try {
        const inputObject = JSON.parse(inputText.value);

        const outputObject = inputObject.reduce((acc, curr) => {
        return Object.assign(acc, {
          Values: {
            ...acc.Values,
            [curr.name]: curr.value
          }
        });
      }, { IsEncrypted: false, Values: {} });

        outputText.value = JSON.stringify(outputObject, null, 2);
      } catch (error) {
        outputText.value = 'Invalid JSON string';
      }
    }
</script>

*PS: This blog post is written by an AI using ChatGPT including the Javascript code to convert values.*
