# Use Case 02 - Integrating Moesif and Auth0

## **Prerequisites**

Before proceeding, ensure the following prerequisites are met:

1. Login to [Bijira Console](https://console.bijira.dev/) with your credentials.
   - Go to Bijira Console and sign in using your Google, GitHub, or Microsoft account.
   - Enter a unique organization name.
   - Read and accept the privacy policy and terms of use.
   - Click Create.
   - This creates the organization and opens the organization home page.
2. Create a new Project named `Grand Union Bank Projects`
   - Click on **Create**.
   - Enter the project name as `Grand Union Bank Projects`.
   - Click **Create**.

## Step 1: Create an MCP Server

1. On the project home page, click Import API Contract under My APIs (Ingress). 
2. Click URL for API Contract, enter the following URL, and then click Next:
3. Select `Import Open API Contract`.
   - Enter the OpenAPI definition URL: 
     ```
     https://raw.githubusercontent.com/wso2/bijira-samples/refs/heads/main/services/card-promotion-service-nodejs/openapi.yaml
     ```
4. Click `Next`.     
5. The metadata has been populated with the following API details extracted from the OpenAPI definition.

   | Field              | Value                                                                                                                                   |
   |--------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
   | API Name           | Card Promotions API                                                                                                                     |
   | Identifier         | card-promotions-api                                                                                                                     |
   | API Version        | 1.0                                                                                                                                     |
   | BasePath           | /grand-union-bank-unity/card-promotions-api/v1.0                                                                                        |
   | Description        | API for retrieving and filtering credit/debit card promotional offers                                                                   |
   | Network Visibility | External                                                                                                                                |
   | Target             | https://ecad9e6b-e034-43ac-a3a0-46553e637d00-prod.e1-us-east-azure.choreoapis.dev/grand-union-bank-unity/card-promotion-service-no/v1.0 |

6. Click **Create**.

## Step 2: Navigate to the Policy section under the Develop menu
1. Click on the **Policies** tab.
2. Verify how the actual backend rest API is being mapped with MCP Server tools.


## Step 2: Navigate to the MCP Playground under the Test menu
1. Click on the **MCP Playground** tab. 
2. Select Development from the environment drop-down list (If you have deployed the API to other environments, you can select the respective options as well). 
3. Click on Get Test Key if the test key is not populated 
4. Click on Connect to connect with your deployed MCP Server. 
5. You can select and call individual tools by providing the parameters if necessary.

## Step 3: Promote the MCP server to the Production

1. Navigate to **Deploy**
2. In the Development card, click Promote.
3. In the Configuration Types pane, select the option Use Development endpoint configuration and click Next.
4. The Production card indicates the Deployment Status as Active when the MCP Server is successfully deployed to production.

## Step 4: Publish the MCP Server
1. Navigate to **Lifecycle**.
2. Click on the **Publish** button to publish the MCP Server.
3. The MCP Server is now published and available for use in the Developer Portal.

## Step 5: Navigate to devportal

## Step 6: Go to the MCP Server listing page using the left navigation menu and select your MCP Server
1. Discover the available tools under MCP landing page
2. Copy the configuration for setting up the MCP Server with an MCP Client.

## Step 7: Subscribe to the MCP Server and generate token
1. Create or select an application from the Developer Portal.
2. Subscribe to the MCP Server through that application.
3. generate a valid OAuth2 token. (refer to the "Generate Keys" section in the Developer Portal)

## Step 8: Configure MCP Client
1. Open VS Code with the Copilot Agent extension.
2. Copy the MCP Server Configuration from the overview tab. 
3. Add it as a new server configuration in mcp.json within your VS Code environment. 
4. Replace the token placeholder with the token you generated in Step 7.

## Step 9: Connect and Interact
1. Start the server via VS Code.
2. If everything is configured correctly, the client will:
   - Connect to the MCP Server.
   - Automatically discover and list the available MCP Tools.

## Step 10: Use the Tools
Open the chat option in Copilot to start trying out the MCP tools!




