# SAP Product Copilot

Azure OpenAI-powered Copilot for SAP users, enabling:

- Choosing the right product.
- Assisting in ordering a product (wip).
- Providing details of a product.
- Update an existing product (only the price in this demo).
- Enter a new product into the system.

Unlike traditional bots, where users are constrained to rigid, predefined pathways, this AI assistant provides the flexibility to interact in natural language. It mimics a conversational experience like interacting with a human, making it intuitive and user-friendly. Users can freely express their queries and commands in everyday language, eliminating the need to understand complex code or specific command language.

No more clicking through endless menus or decoding tech jargon. Just type what you need in your natural language. It’s as easy as chatting with a friend - quick, straightforward, and no hassle!

**Greetings in Teams**

![Greetings](images/greetings.jpg)


**Architecture**

![High Level Process](images/architecture.jpg)


## Prerequesites

1. Power Platform Access
    - Power Virtual Agent (PVA), to create the copilot https://web.powerva.microsoft.com/
    - Power Automate Flows: https://make.powerautomate.com/
    - Recommended for productive use is separated DEV and PROD environments
    - Trial license: https://learn.microsoft.com/en-us/power-apps/maker/signup-for-powerapps
1. Azure Subscription
    - Azure Trial: https://azure.microsoft.com/en-us/free
1. Azure OpenAI Service
    - Deployment Guide: https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource?pivots=web-portal
    - Request Access to OpenAI as described.
    - gpt-4-32k is recommended for this use case due to larger context with longer discussions.
    - Therefore pay attention during the request for regions that can provide gpt-4-32k (e.g. Several in the US, France, Canada, Sweden)
1. SAP System
    - For learning and demo purpose you can use the SAP Gateway Demo System ES5
    - Signup here: https://developers.sap.com/tutorials/gateway-demo-signup.html
    
# Setup

## Azure OpenAI Service

- Follow this documentation to deploy the Azure OpenAI Service: https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource?pivots=web-portal <br> 
- Once the OpenAI Service is deployed go to the OpenAI Studio, enter Deployments and deploy the gpt-4-32k model. <br>

    ![Deploy the gpt-4-32k model](images/deploy-model.jpg)

- Retrieve your endpoint URL and Key in "Chat Playground" -> "View code".

    ![Get URI and Key with "View Code"](images/view-code.jpg)

## Import the Solution

This step will import the 2 flows and the Copilot into your environment.

- Login to https://make.powerapps.com/
- Go to Solutions
- Chose "Import solutions"
- Browse files and select the 
    SAPProductCopilot_[version number].zip


- Publish
- Open in Teams


## Setup the Power Automate Flow (Flow)

- Login to: https://make.powerautomate.com/ 
- Download the Power Automate Flow as zip file: [Download the Power Automate Flow](https://github.com/mimergel/Azure_OpenAI_powered_SAP-Self-Services/raw/main/flow/SAPSelfService_1_0_0_3.zip)
- Go to Solutions and import the flows including its required components:

    ![Import the Flow](images/import-flow.jpg) <br>

- During the subsequent import steps you'll need to create the missing connection(s): <br>

    ![Create Odata Connection](images/create-connection1.jpg) <br>

    Values for SAP ES5 Demo System are:
    - OData Base URI: https://sapes5.sapdevcenter.com/sap/opu/odata/iwbep/GWSAMPLE_BASIC
    - Username e.g.: S001234567
    - Password: xxxxxxxxxx (Note: during preview only basic authentication is available)
    
    Then press "import" 

    ![Create Odata Connection](images/create-connection2.jpg) <br>


- Save the Flow
- "Turn on" the flow.
- In case you're insterested to understand each step in the flow: [Description of the Flow](flow/README.md)

## Adapt the HTTP Connector in the Flow

- Enter your Azure OpenAI URL and API-Key from the Azure Open AI Service Chat playground.

    ![Get URI and KEY](images/http-connector.jpg) <br>

- For now you can enter this directly in clear text. For later production use it's recommended to configure these as secrets and put references to the secrets. <br>

## Setup the Power Virtual Agent (PVA)

- Login to Copilot Studio: https://web.powerva.microsoft.com/ and create a new copilot. <br>

    ![Create Copilot Step 1](images/create-copilot1.jpg) <br>

    You may want to turn off the leasson topics under "edit advanced options" as we don't need them. <br>

    ![Create Copilot Step 1](images/create-copilot2.jpg) <br>

    ![Create Copilot Step 1](images/create-copilot3.jpg) <br>
    <br>

- In the fallback topic paste the code from the provided fallback.yaml file. <br>
- Download the fallback topic code here: [fallback.yaml](pva/fallback.yaml) <br>
- Open the fallback topic in your bot   


    ![Fallback Topic](images/fallback.jpg) <br>

- Open the code editor and enter the code via copy/paste. <br> 

    ![Fallback Topic](images/fallback2.jpg) <br>

- The last step is to configure your personal bot greeting message into the following 2 topics: 
    - Greetings
    - Reset Conversation <br>

    Example: 
    > I’m an AI assistant that helps you with SAP activities.
    >
    > I can:
    >
    > A.) Check if your user account exists, determine the roles assigned, and verify if it is locked.
    >
    > B.) Check the status of an SAP job.
    >
    > C.) Communicate planned maintenance schedules.
    >
    > What would you like me to do?



## Test & Publish the Bot

- Test the Bot: <br>

    ![Test](images/bot-test.jpg) <br>

- When OK publish the bot: <br>

    ![Publish](images/publish.jpg) <br>

- Open the Bot in Teams and test: <br>

    ![Publish](images/bot-test-teams.jpg) <br>


## Troubleshooting

- In case of errors most likely the flow failed.   <br>
    - Check here: https://make.powerautomate.com/ in <br>
        - "My Flows"   <br>
        - "28-days run history"  <br>
- The logs of the flow run provide helpful error messages.  <br>

    ![Troubleshooting 1](images/troubleshooting1.jpg) <br>

    ![Troubleshooting 2](images/troubleshooting2.jpg) <br>