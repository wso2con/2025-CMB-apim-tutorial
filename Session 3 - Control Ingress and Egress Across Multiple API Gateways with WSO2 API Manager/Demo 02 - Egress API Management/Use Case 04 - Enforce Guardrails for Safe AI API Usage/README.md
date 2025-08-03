# Use Case 03 - Model Failover

Failover routing enhances reliability by automatically switching to an alternate AI model if the primary model is throttled out or becomes unresponsive. This strategy ensures continuous service availability without manual intervention.

## Step 1: Add a New Endpoint

1. Log in to the API Publisher at: `https://<hostname>:9443/publisher`.
2. Select the SmartChat AI API you created previously under [Use Case 01](../Use%20Case%2001%20-%20AI%20API%20Design%20and%20Invocation/README.md).
3. Navigate to **API Configurations**, and click **Endpoints**.
4. Click **Add New Endpoint**.
5. Fill out the form using the below provided details.

    <table>
        <tr>
            <th>Field</td>
            <th>Sample Value</td>
        </tr>
        <tr>
            <td>Endpoint Type</td>
            <td>Production</td>
        </tr>
        <tr>
            <td>Endpoint Name</td>
            <td>Prod</th>
        </tr>
        <tr>
            <td>Endpoint URL</td>
            <td>https://api.openai.com/v1</td>
        </tr>
        <tr>
            <td>API Key</td>
            <td>Provide a throttled out OpenAI API Key to test out the failover scenario.</td>
        </tr>
    </table>

6. Click on **Create**.
7. Set the **Prod** endpoint as the primary endpoint by clicking on the **Set as Primary** button.
8. Navigate to **Deployments** and deploy the API.
9. Optionally, try to invoke the API and verify that the invocation is failing with a throttled out message.

## Step 2: Configure Failover

1. Navigate to **API Configurations**, and click **Policies**.
2. Look for the policy named **Model Failover** listed under the Common Policies section within the policy list. Let's, drag and drop the **Model Failover** policy to the **Request** flow of `/chat/completions` POST operation.
3. Configure the policy using the following details and click **Save**.

    - Production:
        - Target Model: gpt-4o, Prod
        - Fallback Models:
            - Model: gpt-4o-mini, Default Production Endpoint
    - Sandbox: disabled
    - Request Timeout: 30
    - Suspend Duration: 30

4. Scroll to the bottom of the page and click on **Save and deploy** to save the API and deploy to the Gateway.

## Step 3: Try Out Failover

1. Navigate to the Developer Portal.
2. Select the SmartChat AI API.
1. Navigate to **Try Out** → **API Console**.
2. Obtain a **Production** access token for invocation since we configured the failover policy only for production.
3. Select `/chat/completions` resource and click on **Try it out**.
4. Replace the request body with the following payload:

    ```json
    {
        "model": "gpt-4o",
        "messages": [
            {
                    "role": "user",
                    "content": "Say this is a test!"
            }
        ]
    }
    ```

5. Click on **Execute**. Notice how the invocation will now succeed as the failover model is being used for invocations.
