---
title: 'Quickstart: Use Azure OpenAI Service with the JavaScript SDK'
titleSuffix: Azure OpenAI
description: Walkthrough on how to get started with Azure OpenAI and make your first chat completions call with the JavaScript SDK. 
#services: cognitive-services
manager: nitinme
ms.service: azure-ai-openai
ms.topic: include
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/22
---

[Source code](https://github.com/openai/openai-node) | [Package (npm)](https://www.npmjs.com/package/openai) | [Samples](https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/openai/openai/samples)

> [!NOTE]
> This guide uses the [latest OpenAI npm package](https://www.npmjs.com/package/openai) which now fully supports Azure OpenAI. If you're looking for code examples for the legacy Azure OpenAI JavaScript SDK, they're currently still [available in this repo](https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/openai/openai/samples/v2-beta/javascript).

## Prerequisites

- An Azure subscription - [Create one for free](https://azure.microsoft.com/free/cognitive-services?azure-portal=true)
- [LTS versions of Node.js](https://github.com/nodejs/release#release-schedule)
- [Azure CLI](/cli/azure/install-azure-cli) used for passwordless authentication in a local development environment, create the necessary context by signing in with the Azure CLI.
- An Azure OpenAI Service resource with either a `gpt-35-turbo` or `gpt-4` series models deployed. For more information about model deployment, see the [resource deployment guide](../how-to/create-resource.md).

### Microsoft Entra ID prerequisites

For the recommended keyless authentication with Microsoft Entra ID, you need to:
- Install the [Azure CLI](/cli/azure/install-azure-cli) used for keyless authentication with Microsoft Entra ID.
- Assign the `Cognitive Services User` role to your user account. You can assign roles in the Azure portal under **Access control (IAM)** > **Add role assignment**.

## Retrieve resource information

[!INCLUDE [resource authentication](resource-authentication.md)]

> [!CAUTION]
> To use the recommended keyless authentication with the SDK, make sure that the `AZURE_OPENAI_API_KEY` environment variable isn't set. 


## Create a Node application

In a console window (such as cmd, PowerShell, or Bash), create a new directory for your app, and navigate to it. 

## Install the client library

Install the required packages for JavaScript with npm from within the context of your new directory:

```console
npm install openai @azure/identity
```

Your app's _package.json_ file is updated with the dependencies.

## Create a sample application

Open a command prompt where you want the new project, and create a new file named ChatCompletion.js. Copy the following code into the ChatCompletion.js file.


## [Microsoft Entra ID](#tab/keyless)

```javascript
const { AzureOpenAI } = require("openai");
const { 
  DefaultAzureCredential, 
  getBearerTokenProvider 
} = require("@azure/identity");

// You will need to set these environment variables or edit the following values
const endpoint = process.env["AZURE_OPENAI_ENDPOINT"] || "<endpoint>";
const apiVersion = "2024-05-01-preview";
const deployment = "gpt-4o"; //This must match your deployment name.


// keyless authentication    
const credential = new DefaultAzureCredential();
const scope = "https://cognitiveservices.azure.com/.default";
const azureADTokenProvider = getBearerTokenProvider(credential, scope);

async function main() {

  const client = new AzureOpenAI({ endpoint, apiKey, azureADTokenProvider, deployment });
  const result = await client.chat.completions.create({
    messages: [
    { role: "system", content: "You are a helpful assistant." },
    { role: "user", content: "Does Azure OpenAI support customer managed keys?" },
    { role: "assistant", content: "Yes, customer managed keys are supported by Azure OpenAI?" },
    { role: "user", content: "Do other Azure AI services support this too?" },
    ],
    model: "",
  });

  for (const choice of result.choices) {
    console.log(choice.message);
  }
}

main().catch((err) => {
  console.error("The sample encountered an error:", err);
});

module.exports = { main };
```

Run the script with the following command:

```cmd
node.exe ChatCompletion.js
```


## [API key](#tab/api-key)

```javascript
const { AzureOpenAI } = require("openai");

// You will need to set these environment variables or edit the following values
const endpoint = process.env["AZURE_OPENAI_ENDPOINT"] || "<endpoint>";
const apiKey = process.env["AZURE_OPENAI_API_KEY"] || "<api key>";
const apiVersion = "2024-05-01-preview";
const deployment = "gpt-4o"; //This must match your deployment name.

async function main() {

  const client = new AzureOpenAI({ endpoint, apiKey, apiVersion, deployment });
  const result = await client.chat.completions.create({
    messages: [
    { role: "system", content: "You are a helpful assistant." },
    { role: "user", content: "Does Azure OpenAI support customer managed keys?" },
    { role: "assistant", content: "Yes, customer managed keys are supported by Azure OpenAI?" },
    { role: "user", content: "Do other Azure AI services support this too?" },
    ],
    model: "",
  });

  for (const choice of result.choices) {
    console.log(choice.message);
  }
}

main().catch((err) => {
  console.error("The sample encountered an error:", err);
});

module.exports = { main };
```

Run the script with the following command:

```cmd
node.exe ChatCompletion.js
```

---

## Output

```output
== Chat Completions Sample ==
{
  content: 'Yes, several other Azure AI services also support customer managed keys for enhanced security and control over encryption keys.',
  role: 'assistant'
}
```

---

> [!NOTE]
> If your receive the error: *Error occurred: OpenAIError: The `apiKey` and `azureADTokenProvider` arguments are mutually exclusive; only one can be passed at a time.* You might need to remove a preexisting environment variable for the API key from your system. Even though the Microsoft Entra ID code sample isn't explicitly referencing the API key environment variable, if one is present on the system executing this sample, this error is still generated.


## Clean up resources

If you want to clean up and remove an Azure OpenAI resource, you can delete the resource. Before deleting the resource, you must first delete any deployed models.

- [Azure portal](../../multi-service-resource.md?pivots=azportal#clean-up-resources)
- [Azure CLI](../../multi-service-resource.md?pivots=azcli#clean-up-resources)

## Next steps

* [Azure OpenAI Overview](../overview.md)
* [Get started with the chat using your own data sample for JavaScript](/azure/developer/javascript/ai/get-started-app-chat-template?toc=/azure/ai-services/openai/toc.json&bc=/azure/ai-services/openai/breadcrumb/toc.json&tabs=github-codespaces)
* For more examples, check out the [Azure OpenAI Samples GitHub repository](https://github.com/Azure-Samples/openai)
