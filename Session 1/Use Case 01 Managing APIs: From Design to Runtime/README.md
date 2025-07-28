# Use Case 01 - API Design and Invocation

## **Prerequisites**

Before proceeding, ensure the following prerequisites are met:

1. Create a tenant named `retail.grandub.com` in WSO2 API Manager and add the users mentioned in the below table.
   - Log in to the WSO2 API Manager carbon console at `https://<hostname>:9443/carbon`.
   - Navigate to **Tenants** and create a new tenant named `retail.grandub.com`.
   - 
     | Users | Role                 |
     |-------|----------------------|
     | Larry | internal/creator     |
     | Emily | internal/publisher   |
     | David | internal/subscriber  |

## Step 1: Create an API

1. Log in to the Publisher Portal as user Larry at`https://<hostname>:9443/publisher`
2. Create an **API** by clicking on **Create API**.
3. Select `Import Open API`.
   - Enter the OpenAPI definition URL: 
     ```
     https://raw.githubusercontent.com/wso2/bijira-samples/refs/heads/main/services/accounts-service-nodejs/openapi.yaml
     ```
4. Click `Next`.
5. The metadata has been populated with the following API details extracted from the OpenAPI definition.

    | Field        | Value             |
    |--------------|-------------------|
    | API Name     | Customer Accounts API |
    | Context      | customeraccountsapi         |
    | API Version  | 1.0.0             |
    | Endpoint     | https://ecad9e6b-e034-43ac-a3a0-46553e637d00-prod.e1-us-east-azure.choreoapis.dev/grand-union-bank-unity/accounts-service-nodejs/v1.0 |
    | Gateway Type | Universal Gateway |

6. Click **Create**.

## Step 2: Deploy and Publish API

1. Log in to the Publisher Portal as the user Emily at `https://<hostname>:9443/publisher`
2. Navigate to **Deployments** and click **Deploy** to deploy the API.
2. Go to **Lifecycle** tab and publish the API.


## Step 3: Create a New Application for API Subscription

1. Log in to the Developer Portal as user David at `https://<hostname>:9443/devportal`.
2. In the Developer Portal, navigate to the **Applications** tab.
3. Click on **Add New Application** 
4. Provide the following details:
   - **Application Name**: CustomerAccountsApp
   - **Tier Selection**: Choose an appropriate throttling tier.
5. Click **Save** to save the application.
6. Generate API Keys:
   - Under the **Production Keys** section.
   - Click **Generate Key**.

## Step 7: Invoke the API

5. Click on **Execute** and notice the response from backend account service.
