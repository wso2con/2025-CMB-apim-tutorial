# Creating an Egress API in Bijira for Twilio REST API

This guide walks you through the process of creating an **Egress API** in **Bijira**, using the **Twilio REST API** as the target third-party service.

You will:

- Create the Egress API directly within **Bijira**.
- Configure it to call Twilio's API using sandbox credentials.
- Use credentials managed by **Bijira** for secure authentication.
- Test the integration in both development and production environments.
- Publish the API to the developer portal for external access.

---

## Prerequisites

Before creating the Egress API in Bijira, ensure the following setup is complete:

1. **Create a Twilio Account**\
   Sign up at [https://www.twilio.com/](https://www.twilio.com/) and verify your email and phone number.

2. **Activate the Messaging Sandbox and Gather Parameters**\
   Use [Twilio's test credentials documentation](https://www.twilio.com/docs/iam/test-credentials#test-sms-messages) to obtain the credentials and required message parameters for sending a test message:

   - `ACCOUNT SID`
   - `AUTH TOKEN`
   - `From`: Twilio-provided sandbox number
   - `To`: Verified recipient phone number
   - `Body`: Message content (e.g., "Hello from Twilio via Bijira!")

---

## Step-by-Step Instructions

### Step 1: Create the API

1. Visit [https://console.bijira.dev](https://console.bijira.dev) and log in.
2. Select your project and click **Create**.
3. Choose **API Proxy** > **Browse APIs** under **Third-Party Ingress**.
4. Select **Twilio REST API** from the catalog.
5. Review the auto-filled form and click **Create**.

### Step 2: Configure Endpoint Credential

1. Go to **Develop > Policy**.
2. Under **Service Contract**, open **Endpoint Configuration**.
3. Click **Configure** near the **Endpoint** field.
4. Enter:
   - **Header**: `Authorization`
   - **Value**: `Basic <base64-encoded ACCOUNT_SID:AUTH_TOKEN>`
5. Click **Save**.

### Step 3: Deploy to Development Environment

1. Go to **Deploy** in the left menu.
2. Under the **Build Area** card, click **Deploy**.
3. Wait for the build to finish.

### Step 4: Test the API

1. Go to **Test > Console**.
2. Select:
   ```
   POST /2010-04-01/Accounts/{AccountSID}/Messages.json
   ```
3. Enter values for `AccountSID`, `From`, `To`, and `Body`.
4. Uncheck **Send Empty Value** for unused fields.
5. Click **Execute** to test the development endpoint.

### Step 5: Promote to Production

1. Return to **Deploy**.
2. Under **Development Environment**, click **Promote**.
3. Choose **Use Development Endpoint Configuration**.
4. Click **Next** to promote.

### Step 6: Publish to Developer Console

1. Go to **Develop > Lifecycle** from the left navigation pane.
2. Click **Publish**.
3. The API will now be published to the **Developer Portal**.
4. Developers can consume this API just like any other published API from the portal.

---
