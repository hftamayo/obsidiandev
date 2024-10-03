
==finalidad de las Lambdas: ==

- Event Driven Processing: auto scale with incoming events, pay only for execution times
- serverless ops


EJERCICIOS:
=

https://www.linkedin.com/feed/update/urn:li:activity:7217354498736807936/?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7217354498736807936%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29


https://www.linkedin.com/feed/update/urn:li:activity:7224859036968529920?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7224859036968529920%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29


ANOTACIONES:
=


==formas de invocar una lambda: sync invoc, async invoc, poll-based invoc

![[Pasted image 20240704114846.png]]

### CONFIGURANDO AWS CLI CON UN ACCESS KEY PREVIAMENTE GENERADO EN IAM:

==Este es el key de devopsadmin:

![[Pasted image 20240929195121.png]]

### CREANDO UN ROLE PARA EJECUCION DE LOS LAMBDAS:

Sure, let's break it down step-by-step:

### 1. Obtain Your AWS Account ID

You can obtain your AWS Account ID using the AWS CLI:

```sh
aws sts get-caller-identity --query Account --output text
```

This command will return your AWS Account ID.

### 2. Understand "Your Lambda Execution Role"

An AWS Lambda execution role is an IAM role that grants your Lambda function permissions to access AWS services and resources. This role is necessary for your Lambda function to interact with other AWS services, such as reading from an S3 bucket or writing logs to CloudWatch.

#### Create a Lambda Execution Role

1. **Go to the IAM Console**:
   - Open the [IAM Management Console](https://console.aws.amazon.com/iam/).

2. **Create a New Role**:
   - Click on "Roles" in the left sidebar.
   - Click the "Create role" button.

3. **Select Trusted Entity**:
   - Choose "AWS service".
   - Select "Lambda" as the service that will use this role.
   - Click "Next: Permissions".

4. **Attach Permissions Policies**:
   - Attach the necessary policies to your role. For basic Lambda execution, you can attach the `AWSLambdaBasicExecutionRole` policy.
   - Optionally, attach other policies based on what your Lambda function needs to access (e.g., S3, DynamoDB).

5. **Review and Create Role**:
   - Give your role a name (e.g., `MyLambdaExecutionRole`).
   - Review the settings and click "Create role".

6. **Get the Role ARN**:
   - After creating the role, click on the role name to view its details.
   - Copy the Role ARN (Amazon Resource Name), which looks like this: `arn:aws:iam::YOUR_ACCOUNT_ID:role/MyLambdaExecutionRole`.

### Putting It All Together

When creating your Lambda function using the AWS CLI, you will use the Role ARN you obtained:

```sh
aws lambda create-function --function-name myLambdaFunction \
--zip-file fileb://function.zip --handler index.handler --runtime nodejs14.x \
--role arn:aws:iam::YOUR_ACCOUNT_ID:role

/My

LambdaExecutionRole
```

Replace `YOUR_ACCOUNT_ID` with your actual AWS Account ID and `MyLambdaExecutionRole` with the name of the role you created.

This setup ensures that your Lambda function has the necessary permissions to execute and interact with other AWS services.


### DANDO LOS PERMISOS NECESARIOS AL ROLE Y AL USUARIO:

The error indicates that the user `devopsadmin` does not have the necessary permissions to create a Lambda function. You need to attach a policy to the `devopsadmin` user that grants the `lambda:CreateFunction` permission.

### Steps to Attach the Necessary Policy

1. **Go to the IAM Console**:
   - Open the [IAM Management Console](https://console.aws.amazon.com/iam/).

2. **Select the User**:
   - In the left sidebar, click on "Users".
   - Click on the `devopsadmin` user.

3. **Attach a Policy**:
   - Click on the "Add permissions" button.
   - Choose "Attach policies directly".
   - Search for the `AWSLambda_FullAccess` policy or create a custom policy with the necessary permissions.
   - Select the policy and click "Next: Review".
   - Click "Add permissions".

### Example of a Custom Policy

If you prefer to create a custom policy, you can use the following JSON to grant the necessary permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "lambda:CreateFunction",
        "lambda:UpdateFunctionCode",
        "lambda:UpdateFunctionConfiguration",
        "lambda:InvokeFunction",
        "lambda:DeleteFunction"
      ],
      "Resource": "arn:aws:lambda:us-west-2:482619384845:function:*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": "arn:aws:iam::482619384845:role/lambda101"
    }
  ]
}
```

### Attach the Custom Policy

1. **Create the Policy**:
   - In the IAM console, click on "Policies" in the left sidebar.
   - Click the "Create policy" button.
   - Select the "JSON" tab and paste the custom policy JSON.
   - Click "Next: Tags", then "Next: Review".
   - Give the policy a name (e.g., `LambdaCreateFunctionPolicy`) and click "Create policy".

2. **Attach the Custom Policy to the User**:
   - Go back to the `devopsadmin` user.
   - Click on the "Add permissions" button.
   - Choose "Attach policies directly".
   - Search for the `LambdaCreateFunctionPolicy` you just created.
   - Select the policy and click "Next: Review".
   - Click "Add permissions".

After attaching the necessary policy, try running the `aws lambda create-function` command again. This should resolve the `AccessDeniedException` error.



### CREANDO EL LAMBDAS:

```
aws lambda create-function --function-name lambdashello --zip-file fileb://lambdashello.zip --handler index.handler --runtime nodejs20.x --role arn:aws:iam::482619384845:role/lambda101

aws lambda get-function --function-name lambdashello

aws lambda list-functions

aws lambda invoke --function-name lambdashello output.txt

```

![[Pasted image 20240929202652.png]]

![[Pasted image 20240929202935.png]]

![[Pasted image 20240929203208.png]]

### VERIFICANDO LA CONSOLA DE AWS: (mucho ojo con la region seleccionada)

![[Pasted image 20240929203917.png]]

![[Pasted image 20240929203937.png]]

### BILLING DE LOS LAMBDAS

AWS Lambda billing is based on the number of requests and the duration of your function's execution. If your Lambda function is not being invoked, you won't incur any charges. However, it's always a good practice to clean up resources that you no longer need to avoid any unexpected charges.

### If You Want to Keep the Lambda Function

If you plan to use the Lambda function in the near future, you can leave it active. Since AWS Lambda charges are based on usage, you won't be billed if the function is not invoked.

### If You Want to Delete the Lambda Function

If you no longer need the Lambda function and want to ensure there are no charges, you can delete it. Here’s how to delete the Lambda function using the AWS CLI:

```sh
aws lambda delete-function --function-name lambdashello
```

Or, you can delete it from the AWS Management Console:

1. **Navigate to the Lambda Console**:
   - Go to the AWS Management Console and open the Lambda service.

2. **Select Your Lambda Function**:
   - Find and select the `lambdashello` function.

3. **Delete the Function**:
   - Click on the "Actions" dropdown and select "Delete function".
   - Confirm the deletion.

By deleting the Lambda function, you ensure that there will be no charges associated with it.

### Monitoring and Budgeting

To avoid unexpected charges, you can also set up AWS Budgets and billing alerts:

1. **Set Up AWS Budgets**:
   - Go to the [AWS Budgets](https://console.aws.amazon.com/billing/home#/budgets) service in the AWS Management Console.
   - Create a budget to monitor your AWS spending and set up alerts.

2. **Enable Billing Alerts**:
   - Go to the [Billing and Cost Management Dashboard](https://console.aws.amazon.com/billing/home#/).
   - Enable billing alerts to receive notifications when your usage exceeds a certain threshold.

These steps will help you keep track of your AWS spending and avoid any unexpected charges.

## ==Steps to update my lambda

Sure, here are the steps you need to follow to update and deploy your Lambda function whenever you make changes to your TypeScript code:

### 1. **Compile TypeScript to JavaScript**

Ensure your TypeScript files are compiled to JavaScript and placed in the [`dist`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Fnodelambdas101%2Fdist%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%226172f99b-0d58-4f7f-9610-ec4c78513c62%22%5D "/home/hftamayo/sourcecode/nodejs/myprojects/nodelambdas101/dist") directory.

```sh
npm run build
```

### 2. **Zip the Compiled Files**

Navigate to the [`dist`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Fnodelambdas101%2Fdist%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%226172f99b-0d58-4f7f-9610-ec4c78513c62%22%5D "/home/hftamayo/sourcecode/nodejs/myprojects/nodelambdas101/dist") directory and zip the contents.

```sh
cd dist
zip -r ../lambdashello.zip .
cd ..
```

### 3. **Update the Lambda Function Code**

Use the AWS CLI to update the Lambda function with the new zip file.

```sh
aws lambda update-function-code --function-name lambdashello --zip-file fileb://lambdashello.zip
```

### 4. **Verify the Deployment**

Invoke the Lambda function to ensure it works correctly.

#### Using AWS CLI

1. **Create a JSON File for the Event Data**:
   - Create a file named `event.json` with the event data you want to pass to your Lambda function. For example:
     ```json
     {
       "key1": "value1",
       "key2": "value2",
       "key3": "value3"
     }
     ```

2. **Invoke the Lambda Function**:
   - Use the following command to invoke your Lambda function:
     ```sh
     aws lambda invoke --function-name lambdashello --payload file://event.json output.json
     ```

3. **Check the Output**:
   - The response from the Lambda function will be saved in `output.json`. You can check this file to see the result of the invocation.

### 5. **Check Logs for Errors**

Use CloudWatch Logs to check for any errors or issues during the invocation.

1. **View Logs in CloudWatch**:
   - Go to the CloudWatch service in the AWS Management Console.
   - Navigate to "Logs" and find the log group for your Lambda function (`/aws/lambda/lambdashello`).

2. **Check Logs**:
   - View the logs to see the output and any errors generated by your Lambda function.

### Summary of Steps

1. **Compile TypeScript**:
   ```sh
   npm run build
   ```

2. **Zip Compiled Files**:
   ```sh
   cd dist
   zip -r ../lambdashello.zip .
   cd ..
   ```

3. **Update Lambda Function Code**:
   ```sh
   aws lambda update-function-code --function-name lambdashello --zip-file fileb://lambdashello.zip
   ```

4. **Invoke Lambda Function**:
   ```sh
   aws lambda invoke --function-name lambdashello --payload file://event.json output.json
   ```

5. **Check Logs in CloudWatch**.

By following these steps, you can ensure that your Lambda function is updated and deployed correctly after every change.


## Verificacion de la ejecucion de la lambda

### Desde la AWS Console:

![[Pasted image 20240930133501.png]]

### Probando desde mi terminal:

```
aws lambda invoke --function-name lambdashello --payload file://event.json output.json

```

![[Pasted image 20240930133905.png]]

### Verificando CloudWatch:

![[Pasted image 20240930134224.png]]

![[Pasted image 20240930134315.png]]

![[Pasted image 20240930134350.png]]

### ==Que pasa si dejo encendida mi lambda ?

Yes, if your Lambda function is publicly accessible and someone creates an infinite loop of requests, it could lead to significant charges. To prevent this, you can implement several strategies to control and monitor usage:

### 1. **Set Up AWS Budgets and Alerts**

Create a budget in AWS to monitor your spending and set up alerts to notify you when your usage exceeds a certain threshold.

1. **Go to AWS Budgets**:
   - Open the [AWS Budgets Console](https://console.aws.amazon.com/billing/home#/budgets).

2. **Create a Budget**:
   - Click on "Create budget".
   - Follow the steps to set up a budget for your Lambda usage and configure alerts.

### 2. **Use API Gateway with Throttling**

If your Lambda function is exposed via API Gateway, you can set up throttling to limit the number of requests.

1. **Navigate to API Gateway**:
   - Go to the API Gateway service in the AWS Management Console.

2. **Set Up Throttling**:
   - Select your API and go to the "Stages" section.
   - Click on the stage you want to configure.
   - Under "Settings", set the "Throttle" limits for requests per second (RPS) and burst limits.

### 3. **Implement Rate Limiting in Your Lambda Function**

You can implement rate limiting within your Lambda function to reject requests that exceed a certain rate.

### 4. **Use AWS WAF (Web Application Firewall)**

AWS WAF can help protect your API Gateway by filtering and monitoring HTTP requests.

1. **Navigate to AWS WAF**:
   - Go to the AWS WAF service in the AWS Management Console.

2. **Create a Web ACL**:
   - Create a Web ACL and associate it with your API Gateway.
   - Set up rules to block or rate-limit requests based on IP addresses, request patterns, etc.

### 5. **Monitor and Log Usage**

Use CloudWatch to monitor and log usage of your Lambda function.

1. **Set Up CloudWatch Alarms**:
   - Go to the CloudWatch service in the AWS Management Console.
   - Create alarms to monitor metrics such as the number of invocations, errors, and duration.

### Example: Setting Up a CloudWatch Alarm

1. **Navigate to CloudWatch**:
   - Open the CloudWatch Console.

2. **Create an Alarm**:
   - Go to "Alarms" and click "Create Alarm".
   - Select a metric (e.g., Lambda > By Function Name > Invocations).
   - Set the threshold and notification settings.

### Summary

By implementing these strategies, you can control and monitor the usage of your Lambda function to prevent unexpected charges:

1. **Set Up AWS Budgets and Alerts**.
2. **Use API Gateway with Throttling**.
3. **Implement Rate Limiting in Your Lambda Function**.
4. **Use AWS WAF**.
5. **Monitor and Log Usage with CloudWatch**.

These measures will help you manage and mitigate the risk of excessive usage and associated costs.
### ==Borrando mi Lambda

Sure, you can delete your Lambda function using the AWS CLI. Here’s the command to delete your Lambda function named `lambdashello`:

```sh
aws lambda delete-function --function-name lambdashello
```

This command will remove the Lambda function from your AWS account, ensuring that it no longer incurs any charges.

### Steps to Delete the Lambda Function

1. **Open Terminal**:
   - Open your terminal or command prompt.

2. **Run the Delete Command**:
   - Execute the following command:
     ```sh
     aws lambda delete-function --function-name lambdashello
     ```

3. **Verify Deletion**:
   - You can verify that the function has been deleted by listing your Lambda functions:
     ```sh
     aws lambda list-functions
     ```

   - Ensure that `lambdashello` is no longer in the list.

By following these steps, you can delete your Lambda function and ensure that it no longer incurs any charges.

![[Pasted image 20240930140042.png]]

