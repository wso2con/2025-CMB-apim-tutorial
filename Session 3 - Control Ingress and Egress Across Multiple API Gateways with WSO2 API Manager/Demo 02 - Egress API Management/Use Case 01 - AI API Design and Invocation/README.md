# Use Case 01 - AI API Design and Invocation

## Step 1: Create an AI API

1. Log in to the Publisher Portal at: `https://<hostname>:9443/publisher`.
2. Create an **AI/LLM API** by clicking on **Create AI/LLM API**.
3. Select **OpenAI** as the provider and select version **1.0.0**. Then, click **Next**.
4. Fill in the following API details:

    | Field | Value |
    |-------------|---------------|
    | API Name   | SmartChat        |
    | Context  | smartchat          |
    | API Version  | 1.0.0          |
    | Gateway Type | Universal Gateway |

5. Click **Create**.

## Step 2: Obtain API Key from AI Vendor

1. Log in to OpenAI and go to [OpenAI Dashboard](https://platform.openai.com/api-keys).
2. Navigate to **API keys** section from the left menu. Then, click on **Create new secret key**. Provide a name for the key and click on **Create secret key**.

## Step 3: Configure Endpoint Security

1. Navigate to **API Configurations** → **Endpoints**
2. Edit `Default Production Endpoint` and add the API key obtained from [step 2](#step-2-obtain-api-key-from-ai-vendor). Then, click on Update.

## Step 4: Deploy and Publish API

1. Navigate to **Deployments** and click **Deploy** to deploy the API.
2. Go to **Lifecycle** tab and publish the API.


## Step 5: Create a New Application for AI API Subscription

1. Log in to the Developer Portal at `https://<hostname>:9443/devportal`.
2. In the Developer Portal, navigate to the **Applications** tab.
3. Click on **Add New Application**.
4. Provide the following details:
   - **Application Name**: SmartChatApp
   - **Tier Selection**: Choose an appropriate throttling tier.
5. Click **Save** to save the application.
6. Generate API Keys:
   - Under the **Production Keys** section.
   - Click **Generate Key**.

## Step 6: Subscribe to AI API

1. In the Developer Portal, navigate to the **APIs** tab.
2. Select the **SmartChat** API.
3. Go to the **Subscriptions** tab.
4. Select the **SmartChatApp** application created earlier.
5. Select the **subscription plan** (e.g., Bronze: 1000 requests/min, Gold: 5000 requests/min).
6. Click **Subscribe**.

## Step 7: Invoke AI API

1. Navigate to **Try Out** → **API Console**.
2. Click **GET TEST KEY**.
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
5. Click on **Execute** and notice the response from OpenAI.
