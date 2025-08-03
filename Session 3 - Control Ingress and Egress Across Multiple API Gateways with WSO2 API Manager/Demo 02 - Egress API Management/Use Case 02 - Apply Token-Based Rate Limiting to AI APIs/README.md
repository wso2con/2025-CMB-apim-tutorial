# Use Case 02 – Apply Token-Based Rate Limiting to AI APIs

This guide demonstrates how to create an **Egress API** in **Bijira** that integrates with **Azure OpenAI**, and how to apply **token-based rate limiting** using **Bijira**.

You will:

- Create an Egress API to Azure OpenAI (e.g., `https://<resource>.openai.azure.com`)
- Configure the necessary authentication headers
- Apply a token-based rate limiting policy via Bijira
- Test the rate limit enforcement in the integrated console

---

## Prerequisites

Before starting, make sure you have the following:

- An **LLM model deployed** via **Azure AI Foundry** (e.g., `gpt-35-turbo` or `gpt-4`)
- The **endpoint URL** for the deployed model (e.g., `https://<your-resource>.openai.azure.com`)
- The corresponding **API key** provided by Azure OpenAI (Azure AI Foundry)

---

## Step-by-Step Instructions

### Step 1: Create the AI Egress API

1. Log in to [https://console.bijira.dev](https://console.bijira.dev).
2. Select your target project from the dashboard.
3. Click the **Create** button.
4. Under the **Third-Party Egress API** section, choose **AI API**.

### Step 2: Select Azure OpenAI API and Fill Details

1. In the **AI API Catalog**, select **Azure OpenAI Service API**.
2. A form will appear with fields such as:
   - **Name**
   - **Base Path**
   - **Description**
   - **Target Endpoint**
3. Enter the **Target Endpoint** you received from Azure AI Foundry (e.g., `https://<your-resource>.openai.azure.com`).
4. Fill out the other details and click **Create**.

### Step 3: Configure Endpoint Credential

1. Go to the **Develop > Policy** section.

2. Under **Service Contract**, click **Endpoint Configuration**.

3. Click **Configure** near the **Endpoint** field.

4. Provide authentication credentials using one of the following options, based on the method supported by Azure OpenAI:

- **Header**: `Authorization`
- **Value**: `Bearer <AZURE_OPENAI_API_KEY>`

Or alternatively:

- **Header**: `api-key`
- **Value**: `<AZURE_OPENAI_API_KEY>`

Refer to the **Azure AI Foundry Model Deployment** section to obtain the API key.

6. Click **Save**.

### Step 4: Apply Token-Based Rate Limiting

1. Navigate to the **Develop > Policy** section.
2. Under the **API Proxy Contract**, click the **Policy** button in the top-right corner.
3. In the **API Policies Configuration** pane (right side), click **Attach Mediation Policy**.
4. From the list, select **Token-Based Rate Limiting Policy**.
5. Configure the policy with the following values:
   - **Rate Limit Token Count**: `200`
   - **Max Completion Token Count**: `200`
   - **Max Total Token Count**: `400`
   - **Time Unit**: `per minute`
6. Click **Add** to attach the policy.
7. Save the **API Policies Configuration**, then save the overall **API Policy** page.

### Step 5: Deploy the API to Development

1. Go to the **Deploy** section in the left-hand menu.
2. In the **Build Area** card, click the **Deploy** button.
3. This will deploy the API to the **development environment** with the new rate limiting and endpoint configuration settings.
4. Wait a few seconds for the deployment to complete before testing.

### Step 6: Test the API in the Integrated Test Console

1. Go to the **Test** section in the left navigation.
2. Click on **Console** to open the integrated test console.
3. Locate the deployed API endpoint and note the security key provided by Bijira.
4. Open the **chat/completions** endpoint resource.
5. Use the following payload in the request body:

```json
{
  "model": "gpt-4o-mini",
  "messages": [
    {
      "role": "user",
      "content": "What is the minimum age limit to open a bank account in Sri Lanka?"
    }
  ],
  "max_tokens": 150
}
```

6. Click **Execute** to send the request.
7. You should receive a valid response from Azure OpenAI if the configuration is correctly applied.

### Step 7: Verify Rate Limiting Behavior

1. Execute the same request multiple times in quick succession.
2. Once the token limits are exceeded, you should receive a `429 Too Many Requests` response.
3. This confirms that Bijira is actively enforcing the token-based rate limits on your endpoint.

---
