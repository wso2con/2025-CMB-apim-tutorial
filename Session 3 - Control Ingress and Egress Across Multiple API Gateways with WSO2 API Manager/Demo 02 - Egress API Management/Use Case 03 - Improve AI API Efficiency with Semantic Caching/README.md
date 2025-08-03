# Use Case 03 – Improve AI API Efficiency with Semantic Caching

This guide demonstrates how to create an **Egress API** in **Bijira** that integrates with **Azure OpenAI**, and how to improve its performance and reduce cost by enabling **semantic caching**.

You will:

- Create an Egress API to Azure OpenAI (e.g., `https://<resource>.openai.azure.com`)
- Configure the necessary authentication headers
- Enable semantic caching to reuse similar AI responses and optimize latency

---

## Prerequisites

Before starting, make sure you have the following:

- An **LLM model deployed** via **Azure AI Foundry** (e.g., `gpt-4o-mini`)
- The **endpoint URL** for the deployed model (e.g., `https://<your-resource>.openai.azure.com`)
- The corresponding **API key** provided by Azure OpenAI (Azure AI Foundry)
- A **Redis database** instance to act as the **vector store** for caching embeddings
- A **text embedding model** (e.g., `text-embedding-ada-002`) to generate vector representations of prompts for semantic similarity evaluation. This model can be **pre-deployed via Azure AI Foundry**, in the same way the LLM model is deployed. Bijira currently supports embedding models from **Mistral** and **Azure**.

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

5. Click **Save**.

### Step 4: Enable Semantic Caching Policy

1. Go to the **Develop > Policy** section.
2. In the graphical flow editor, locate the **/chat/completions** resource.
3. Find the **Policy Configuration** button in the middle of the UI—between the Service Contract Endpoint and the API Proxy Contract Endpoint.
4. Click the button to open the **Edit Mediation Policies** panel for the resource.
5. Under the **Request Policy** section, click **Attach Mediation Policy**.
6. From the policy list, choose **Semantic Caching Policy**.
7. You will be prompted to enter the following configuration:

#### Embedding Details

- **Provider**: Select `Azure OpenAI` from the dropdown.
- **Auth Header Name**: e.g., `api-key`
- **API Key**: The API key used for the embedding provider
- **Embedding Model Name**: The name of the deployed embedding model (e.g., `text-embedding-ada-002`)
- **Embedding Upstream URL**: Endpoint URL for the embedding model

#### Vector Store Details

- **Provider**: Select `Redis`
- **Host & Port**: Redis instance hostname and port
- **Dimension**: Set to `1536` (required for Azure OpenAI embeddings)
- **Threshold**: Set to `0.01` (for semantic similarity matching)
- **Username**: Redis username
- **Password**: Redis password
- **Database ID**: Redis database ID

8. Click **Save** to apply the policy.
9. Click **Save** again on the **Edit Mediation Policies** pane.
10. Finally, click **Edit** on the main **API Policy** UI to confirm and complete configuration.

### Step 5: Deploy the API to Development

1. Go to the **Deploy** section from the left-hand menu.
2. Under the **Build Area**, click the **Deploy** button.
3. This will deploy your updated API with semantic caching configuration to the **development environment**.

### Step 6: Test Semantic Caching Behavior

1. Go to the **Test > Console** section.
2. Expand the **POST /chat/completions** resource.
3. Use a prompt that takes a bit of time to process—this helps demonstrate the benefit of caching. Example:

```json
{
  "model": "gpt-4o-mini",
  "messages": [
    {
      "role": "user",
      "content": "Explain in detail how a black hole forms from a collapsing star and how event horizons work."
    }
  ],
  "max_tokens": 200
}
```

4. Click **Execute** to send the first request. This initial response will take a few seconds.
5. Execute the same request again.
6. The second response should be **near-instant**, confirming that semantic caching is working correctly.

### Step 7: Promote to Production

1. Go to the **Deploy** section.
2. Under **Development Environment**, click **Promote**.
3. Choose **Use Development Endpoint Configuration** and proceed.

### Step 8: Publish to Developer Portal

1. Navigate to **Develop > Lifecycle**.
2. Click **Publish** to make the API available in the Developer Portal.
