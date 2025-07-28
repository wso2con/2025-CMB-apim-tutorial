# Use Case 02 - Integrating Moesif and Auth0

## **Prerequisites**

Before proceeding, ensure the following prerequisites are met:

1. Create a tenant named `digital.grandub.com` in WSO2 API Manager and add the users mentioned in the below table.
   - Log in to the WSO2 API Manager carbon console at `https://<hostname>:9443/carbon`.
   - Navigate to **Tenants** and create a new tenant named `digital.grandub.com`.
   - 
     | Users | Role                 |
     |-------|----------------------|
     | Larry | internal/creator     |
     | Emily | internal/publisher   |
     | David | internal/subscriber  |
   
2. Add Auth0 as Key Manager
   - Configure Auth0 as Key Manager [Configure Auth0](https://apim.docs.wso2.com/en/latest/administer/key-managers/configure-auth0-connector/#step-1-configure-auth0)
   - Add Auth0 as Key Manager
     - Log in to the WSO2 API Manager admin console at `https://<hostname>:9443/admin` using user `admin@digital.grandub.com`
     - Navigate to **Key Managers** and click on **Add Key Manager**. [[Configure WSO2 API Manager](https://apim.docs.wso2.com/en/latest/administer/key-managers/configure-auth0-connector/#step-2-configure-wso2-api-manager)

3. Setup Moesif
    - Configure Moesif account at [Moesif Guide](https://apim.docs.wso2.com/en/latest/monitoring/api-analytics/moesif-analytics/moesif-integration-guide/#step-1-set-up-your-moesif-account) and obtain the Application ID.
    - [Configure Moesif at WSO2 APIManager](https://apim.docs.wso2.com/en/latest/monitoring/api-analytics/moesif-analytics/moesif-integration-guide/#step-2-configure-wso2-api-manager)

## Step 1: Create an API

1. Log in to the Publisher Portal as user Larry at`https://<hostname>:9443/publisher`
2. Create an **API** by clicking on **Create API**.
3. Select `Import Open API`.
   - Enter the OpenAPI definition URL: 
     ```
     https://raw.githubusercontent.com/wso2/bijira-samples/refs/heads/main/services/merchant-onboarding-service-nodejs/openapi.yaml
     ```
4. Click `Next`.     
5. The metadata has been populated with the following API details extracted from the OpenAPI definition.

   | Field        | Value                                                                                                                                 |
       |--------------|---------------------------------------------------------------------------------------------------------------------------------------|
   | API Name     | MerchantOnboardingAPI                                                                                                                 |
   | Context      | merchantonboardingapi                                                                                                                 |
   | API Version  | 1.2.0                                                                                                                                 |
   | Endpoint     | https://ecad9e6b-e034-43ac-a3a0-46553e637d00-prod.e1-us-east-azure.choreoapis.dev/grand-union-bank-unity/merchant-onboarding-servi/v1.0 |
   | Gateway Type | Universal Gateway      

6. Click **Create**.

## Step 2: Configure Auth0 as KeyManager of the `MerchantOnboardingAPI`
1. Navigate to **API Configurations** → **Application Level Security**.
2. Select Auth0 as the Key Manager under **Key Manager Configuration**
3. Save the API.

## Step 3: Deploy and Publish API

1. Log in to the Publisher Portal as the user Emily at `https://<hostname>:9443/publisher`
2. Navigate to **Deployments** and click **Deploy** to deploy the API.
2. Go to **Lifecycle** tab and publish the API.


## Step 3: Create a New Application for API Subscription

1. Log in to the Developer Portal as user David at `https://<hostname>:9443/devportal`.
2. In the Developer Portal, navigate to the **Applications** tab.
3. Click on **Add New Application** 
4. Provide the following details:
   - **Application Name**: MerchantOnboardingApp
   - **Tier Selection**: Choose an appropriate throttling tier.
5. Click **Save** to save the application.
6. Generate Auth2 tokens from `Auth0` KeyManager:
   - Under the **Production Keys** section.
   - Click **Generate Key**.

## Step 7: Invoke the API

5. Click on **Execute** and notice the response from backend account service.

## Step 8: Verify Moesif Analytics
