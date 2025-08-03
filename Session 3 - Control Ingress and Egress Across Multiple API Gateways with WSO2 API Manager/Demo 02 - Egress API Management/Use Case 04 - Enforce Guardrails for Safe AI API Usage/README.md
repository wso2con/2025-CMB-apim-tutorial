# Use Case 04 – Enforce Guardrails for Safe AI API Usage

This guide demonstrates how to create an **Egress API** in **Bijira** that integrates with **Azure OpenAI**, and how to enforce safety guardrails by applying runtime validation and content control policies.

You will:

- Create an Egress API to Azure OpenAI (e.g., `https://<resource>.openai.azure.com`)
- Configure the necessary authentication headers
- Apply guardrail policies such as input filtering, output moderation, and prompt restrictions

---

## Prerequisites

Before starting, make sure you have the following:

- An **LLM model deployed** via **Azure AI Foundry** (e.g., `gpt-4o-mini`)
- The **endpoint URL** for the deployed model (e.g., `https://<your-resource>.openai.azure.com`)
- The corresponding **API key** provided by Azure OpenAI (Azure AI Foundry)
- An **AI Safety Control Endpoint** (used for runtime content moderation)
  - This endpoint is typically provided as part of an OpenAI  deployment
  - Ensure the service is active and provides moderation for categories like hate speech, violence, sexual content, etc.

You will use this safety endpoint in Bijira to enforce guardrail policies on AI-generated input/output.

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

### Step 4: Attach Guardrail Mediation Policies

1. Go to the **Develop > Policy** section.
2. Under the **Resource-Specific Policy** UI, locate the **/chat/completions** resource.
3. Click the **Policy Configuration** button between the Service Contract Endpoint and API Proxy Contract.
4. In the **Request** row, click **Attach Mediation Policy**.

#### Add PII Masking Guardrail

5. From the list, select **PII Masking Guardrail AI Mediation Policy**.
6. Set the **Guardrail Name** to `PII Guardrail`.
7. Under **PII Entries**, select the following from the dropdown:
   - `Credit Card`
   - `Email Address`
   - `Phone Number`
8. Click **Add**.

#### Add Azure Content Safety Guardrail

9. Click **Attach Mediation Policy** again.
10. Select **Azure Content Safety Content Moderation Policy**.
11. Set the **Guardrail Name** to `Azure Guardrail`.
12. Provide the following:

- **Content Safety Endpoint**
- **Content Safety API Key**

13. Under **Security Level**, set values for the following categories:

- `Hate`: Level 1
- `Self-Harm`: Level 1
- `Sexual`: Level 1
- `Violence`: Level 1

14. Enable the **Show Guardrail Assessment** checkbox to include detailed results in case of policy failure.

15. Click **Add**.

16. Click **Save** in the **Edit Mediation Policies** panel.

17. Finally, click **Save** in the overall **Policy** page.

### Step 5: Deploy the API to Development

1. Go to the **Deploy** section from the left-hand menu.
2. Under the **Build Area**, click the **Deploy** button.
3. This will deploy your updated API with guardrail configurations to the **development environment**.

### Step 6: Test PII Guardrail Behavior

1. Go to the **Test > Console** section.
2. Open the **POST /chat/completions** resource.
3. First, test the **PII Guardrail** by submitting a prompt that includes sensitive data such as an email address, credit card number, or phone number.
4. Click **Execute** and verify that the PII is masked in the response.

Example prompt:

```json
{
  "model": "gpt-4o-mini",
  "messages": [
    {
      "role": "user",
      "content": "Create an email that I can send to jothanthan@forbiztrust.com so that I need to activate my credit card(4024-0071-5334-5046). Also include my phone number (0716782222)"
    }
  ]
}
```

Sample masked response:

```
Subject: Request for Credit Card Activation

Dear [Customer Service Team/Specific Contact Name],

I hope this message finds you well. I am writing to request activation for my credit card, [CREDIT_CARD_576C_0002]. 

To assist with the activation process, you can contact me at my phone number: [PHONE_NUMBER_0F2F_0001]. Additionally, I can be reached at [PHONE_NUMBER_D25C_0003] if needed.

Thank you for your prompt attention to this matter. I look forward to your response.

Best regards,
[Your Full Name]  
[Your Address, if required]  
[Your Email, if necessary]  
[PHONE_NUMBER_0F2F_0001]
```

This shows that PII values are masked before the prompt reaches the LLM, and the model responds using these masked tokens.

### Step 7: Test Azure Content Safety Guardrail

1. In the same **Test > Console** section, use a prompt that includes potentially unsafe content (e.g., hate speech or violent language).
2. Click **Execute** to send the request.
3. If content safety rules are triggered, you will receive a response like the one below:

```json
{
  "code": 900514,
  "message": {
    "action": "GUARDRAIL_INTERVENED",
    "actionReason": "Violation of Azure content safety content moderation detected.",
    "assessments": {
      "categories": [
        {
          "category": "Hate",
          "result": "FAIL",
          "severity": 2,
          "threshold": 1
        },
        {
          "category": "Sexual",
          "result": "PASS",
          "severity": 0,
          "threshold": 1
        },
        {
          "category": "SelfHarm",
          "result": "PASS",
          "severity": 0,
          "threshold": 1
        },
        {
          "category": "Violence",
          "result": "PASS",
          "severity": 0,
          "threshold": 1
        }
      ],
      "inspectedContent": "{ ... your original request ... }"
    },
    "direction": "REQUEST",
    "interveningGuardrail": "Azure Guardrail"
  },
  "type": "AZURE_CONTENT_SAFETY_CONTENT_MODERATION"
}
```

4. This confirms that the Azure Content Safety Guardrail has correctly identified and blocked unsafe content based on your configured thresholds.

### Step 8: Promote to Production and Publish

1. Go to the **Deploy** section.
2. Under the **Development Environment** card, click **Promote**.
3. Choose to use the existing development endpoint configuration or configure a new one for production.
4. Click **Next** and complete the promotion process.
5. Once promoted, navigate to **Develop > Lifecycle**.
6. Click **Publish** to publish the API to the **Developer Portal**.
7. Your API is now accessible to consumers through the developer portal with guardrails enforced.
