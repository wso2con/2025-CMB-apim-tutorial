# Use Case 02 - Model Load Balancing

Load balancing ensures that AI API requests are efficiently distributed across multiple models within the same AI vendor, preventing overloading of any single model. WSO2 API Manager supports the following load balancing methods:

- Round Robin
- Weighted Round Robin

Let's configure **Weighted Round Robin**.

## Step 1: Configure Weighted Round Robin Routing

1. Log in to the API Publisher at: `https://<hostname>:9443/publisher`.
2. Select the SmartChat AI API you created previously under [Use Case 01](../Use%20Case%2001%20-%20AI%20API%20Design%20and%20Invocation/README.md).
3. Navigate to **API Configurations**, and click **Policies**.
4. Look for the policy named **Model Weighted Round Robin** listed under the Common Policies section within the policy list. Let's, drag and drop the **Model Weighted Round Robin** policy to the **Request** flow of `/chat/completions` POST operation.
5. Configure the policy using the following details and click **Save**.

    - Production:
        - Model: gpt-4o, Default Production Endpoint, 50
        - Model: gpt-4o-mini, Default Production Endpoint, 50
    - Sandbox: disabled
    - Suspend Duration: 30

6. Scroll to the bottom of the page and click on **Save and deploy** to save the API and deploy to the Gateway.

## Step 2: Try Out Weighted Round Robin Routing

1. Navigate to the Developer Portal.
2. Select the SmartChat AI API.
1. Navigate to **Try Out** → **API Console**.
2. Obtain a **Production** access token for invocation since we configured the weighted round robin policy only for production.
3. Select `/chat/completions` resource and click on **Try it out**.
4. Replace the request body with the following payload:

    ```json
    {
        "model": "o3-mini",
        "messages": [
            {
                    "role": "user",
                    "content": "Say this is a test!"
            }
        ]
    }
    ```

5. Click on **Execute** multiple times and notice how the load is distributed among `gpt-4o` and `gpt-4o-mini`.
