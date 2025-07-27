# **AWS Gateway Integration with WSO2 API Manager**  

## **Prerequisites**  

Before proceeding, ensure the following prerequisites are met:  

1. Ensure you have access to a **AWS API Gateway**.  
2. Follow the [Deploy API on AWS API Gateway](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/federated-gateways/deploy-on-aws-api-gateway) guide.  
3. Start the **Mock Backend Server** by following the setup instructions in the [README](../resources/backends/rest-mock-backend/README.md).

## **Steps to Configure AWS Gateway Integration**  

### **1. Log in to the Publisher Portal**  

- Navigate to **WSO2 API Manager Publisher**.  
- Use the following credentials:  

  ```plaintext
  Username: admin
  Password: admin
  ```  

### **2. Create a New API**  

1. Navigate to **APIs** in the WSO2 API Manager dashboard.  
2. Click **Create API** → **Import OpenAPI**.  
3. Enter the OpenAPI definition URL:  

   ```plaintext
   https://raw.githubusercontent.com/wso2/samples-apim/refs/heads/demo_2025/apim-tutorial/resources/apis/FinancialDataAPI/FinancialDataAPI.yaml
   ```  
4. Select **AWS Gateway** as the **Gateway Type**.  

### **3. Configure Security for the API**  

1. Navigate to **Policies** → **API Level Policies**.  
2. Add **Set AWS OAuth** policy.  
3. Fill in the following details:  

   ```plaintext
   Lambda ARN: <ARN of the AWS Lambda function>  
   AWS Lambda Invoke Role ARN: <User role ARN to access the AWS Lambda function>  
   ```  
4. Click **Save** to apply the policy.  

### **4. Deploy and Publish the API**  

1. Navigate to the **Deployment** section and deploy the API.  
2. Go to **AWS API Gateway** and verify that the API is deployed and visible in the AWS API Gateway dashboard.  
3. Navigate to **Lifecycle** and click **Publish** to make the API available in the Developer portal.  

### **5. Log in to the Developer Portal**  

- Navigate to **WSO2 API Manager Devportal**.  
- Use the following credentials:  

  ```plaintext
  Username: admin
  Password: admin
  ```  

### **6. Create an Application in the Developer Portal**  

1. Navigate to **Applications** and create a new application.  
2. Open the newly created application and select **OAuth2 Tokens**.  
3. Click **Generate Keys**.  
4. Click **Generate Access Token** and copy the generated token.  

### **7. Test the API**  

1. Navigate to **APIs** and select **FinancialDataAPI**.  
2. Open the **API Console**.  
3. Paste the copied access token into the **Authorization** section.  
4. Select the `/query` resource and invoke the API.  

### **8. Verify API Functionality**  

- Check the API response to confirm it is functioning correctly.  
- If any issues arise, verify the deployment and configuration in both **WSO2 API Manager** and **AWS API Gateway**.  
