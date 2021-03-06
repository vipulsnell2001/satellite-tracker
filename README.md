# CASIS Satellite Tracker

*Read this in other languages: [한국어](README-ko.md), [Español](README-es.md).*

In this Code Pattern, we will build a satellite tracker using  Node-RED and IBM Watson.
A flow will be created for connecting a Watson Assistant Chatbot with a node-red-contrib-satellites node, as well as a web UI and worldmap node.

When the reader has completed this Code Pattern, they will understand how to:

* Build a complex flow and web UI using simple Node-RED tools.
* Implement a chatbot with Watson Assistant and embed it on a web page with Node-RED.
* Get satellite information for the International Space Station (ISS) and use it in a web app.

![](doc/source/images/architecture.png)

## Flow

1. User interacts with Web UI to query the chat bot "Where is the ISS?".
2. Web UI communicates with Node-RED running on IBM Cloud.
3. Node-RED app running on the cloud processes info and performs HTTP requests.
4. The Node-RED app communicates with Watson Assistant to extract intents and entities.
5. Satellites orbiting the earth send position info which is streamed to Node-RED module.

## Included components

* [Watson Assistant](https://www.ibm.com/watson/developercloud/conversation.html): Create a chatbot with a program that conducts a conversation via auditory or textual methods.

## Featured technologies

* [Node-RED](https://nodered.org/): Node-RED is a programming tool for wiring together hardware devices, APIs and online services in new and interesting ways.
* [Artificial Intelligence](https://medium.com/ibm-data-science-experience): Artificial intelligence can be applied to disparate solution spaces to deliver disruptive technologies.
* [Node.js](https://nodejs.org/): An open-source JavaScript run-time environment for executing server-side JavaScript code.

<!--
# Watch the Video
-->

# Steps

1. [Clone the repo](#1-clone-the-repo)
1. [Create Watson services with IBM Cloud](#2-create-watson-services-with-ibm-cloud)
1. [Import the Watson Assistant workspace](#3-import-the-watson-assistant-workspace)
1. [Get the Watson Assistant credentials](#4-get-the-watson-assistant-credentials)
1. [Create a Node-RED Workspace](#5-create-a-node-red-workspace)
1. [Get a LocationIQ API key](#6-get-a-locationiq-api-key)
1. [Install additional nodes and Perform either 7a or 7b](#7-install-additional-nodes)

    7a. [Build the Node-RED flow manually](doc/CreateFlowManually.md)

    7b. [Import the completed flow](#7b-import-the-completed-flow)
1. [Configure credentials](#8-configure-credentials)

### 1. Clone the repo

Clone the `casis-satellite-tracker` locally. In a terminal, run:

```
$ git clone https://github.com/IBM/casis-satellite-tracker
```

### 2. Create Watson services with IBM Cloud

Create the [*Watson Assistant*](https://cloud.ibm.com/catalog/services/conversation) service by providing a name of your choice and clicking `Create`.

Once created, you'll see either the credentials for *username* and *password* or an IAM *apikey*, either of which you should copy down to be used later. (Click `Show` to expose them).


![](https://github.com/IBM/pattern-images/blob/master/watson-assistant/WatsonAssistantCredentials.png)

![](https://github.com/IBM/pattern-images/blob/master/watson-assistant/watson_assistant_api_key.png)

### 3. Import the Watson Assistant workspace

Once you have created your instance of Watson Assistant, click `Launch Tool`, then click the `Workspaces` tab. Import the workspace by clicking the upload icon:

![](https://github.com/IBM/pattern-images/blob/master/watson-assistant/UploadWorkspaceJson.png)

Click `Choose a file` and navigate to [`data/AssistantWorkspace/sat-tracker-workspace.json`](data/AssistantWorkspace/sat-tracker-workspace.json) in this repo. Click `Import`.

Get the Workspace ID by clicking the 3 vertical dots on the `Workspaces` tab. Save this for later.

<p align="center">
  <img width="200" height="300" src="https://github.com/IBM/pattern-utils/blob/master/watson-assistant/GetAssistantDetails.png">
</p>

### 4. Get the Watson Assistant credentials

The credentials for IBM Cloud Watson Assistant service can be found
by selecting the ``Service Credentials`` option for the service. You
saved these in [step #2](#2-create-watson-services-with-ibm-cloud).

The `WORKSPACE_ID` for the Watson Assistant workspace was saved in
[step #3](#3-import-the-watson-assistant-workspace).

### 5. Create a Node-RED Workspace

From the the [IBM Cloud Catalog](https://cloud.ibm.com/catalog/) navigate to `Platform` -> `Boilerplates` and choose [Node-RED Starter](https://cloud.ibm.com/catalog/starters/node-red-starter). Choose a name and click `Create`.

Once the App has deployed, click on `Visit App URL`

![](https://github.com/IBM/pattern-images/blob/master/node-red/NodeRedVisitAppURL.png)

Follow the instructions to `Secure your Node-RED editor` and `Browse available IBM Cloud nodes`. Click `Finish` and then click `Go to your Node-RED flow editor`.

### 6. Get a LocationIQ API key

You will need an API key from [LocationIQ](https://locationiq.com/) for the reverse geocoding function in this app.

* Visit the [LocationIQ website](https://locationiq.com/) and scroll down to `Excited?! Get a developer token!`. Input your name and email and follow the instructions to get an API token. Save this for later, when you configure the `credentials` node.

### 7. Install additional nodes

You will need to install the following additional nodes:

* [node-red-contrib-credentials](https://flows.nodered.org/node/node-red-contrib-credentials)
* [node-red-contrib-web-worldmap](https://flows.nodered.org/node/node-red-contrib-web-worldmap)
* [node-red-contrib-satellites](https://flows.nodered.org/node/node-red-contrib-satellites)

Click the menu icon in the upper right and then `Manage palette`.

![](https://github.com/IBM/pattern-images/blob/master/node-red/NodeREDmanagePallete.png)

Click the `Install tab` and enter the name of the node you wish to install into the search bar, then click `install`.

![](https://github.com/IBM/pattern-images/blob/master/node-red/NodeREDInstallSearch.png)

### 7.a Build the Node-RED flow manually

Follow these instructions to [Build the Node-RED flow manually](doc/CreateFlowManually.md).

### 7.b Import the completed Flow

We will walk through the steps to build the Node-RED flow, but you can import the completed Flow. Copy the flow to your machine's clipboard by navigating to `data/NodeRED/`.

A flow can be moved to a Mac OS clipboard with:
```
$ pbcopy < flow.json
```

On Windows use:
```
$ cat flow.json | clip
```

On Linux use:
```
$ cat flow.json | xclip
```

Once the `flow.json` is on your clipboard, Click the upper-right menu icon and choose `Import` -> `Clipboard`. Paste the contents of your clipboard and click `Import`.

![](https://github.com/IBM/pattern-images/blob/master/node-red/ImportNodeREDflowToClipboard.png)

### 8. Configure credentials

* Click on the `ISS Assistant` node and fill in either the `username` and `password` or the `API Key`, depending on which was part of your Watson Assistant credentials from [Get the Watson Assistant credentials](#4-get-the-watson-assistant-credentials).

* Click on the `Credentials` node and put the locationIQ API key from [Get a LocationIQ API key](#6-get-a-locationiq-api-key) in the field `private`.

> NOTE: After any changes, you will need to click the `Deploy` button to make them live.

# Sample output

Use the running app by going to `<Node-RED_URL>/bot`

![](doc/source/images/satTrackSampleOut.png)

# Troubleshooting

# Links

* [Node-RED satellite module](https://flows.nodered.org/node/node-red-contrib-satellites)
* [Node-RED World Map](https://flows.nodered.org/node/node-red-contrib-web-worldmap)
* [IBM Bot Asset Exchange](https://developer.ibm.com/code/exchanges/bots/)

# Learn more

* **Artificial Intelligence Code Patterns**: Enjoyed this Code Pattern? Check out our other [AI Code Patterns](https://developer.ibm.com/technologies/artificial-intelligence/).
* **AI and Data Code Pattern Playlist**: Bookmark our [playlist](https://www.youtube.com/playlist?list=PLzUbsvIyrNfknNewObx5N7uGZ5FKH0Fde) with all of our Code Pattern videos
* **With Watson**: Want to take your Watson app to the next level? Looking to utilize Watson Brand assets? [Join the With Watson program](https://www.ibm.com/watson/with-watson/) to leverage exclusive brand, marketing, and tech resources to amplify and accelerate your Watson embedded commercial solution.

# License
This code pattern is licensed under the Apache Software License, Version 2.  Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
