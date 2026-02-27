<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Fetch Data with AWS Lambda

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-lambda)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

![Image](https://learn.nextwork.org/projects/static/aws-compute-lambda/architecture-complete.png)

---

## Fetch Data with AWS Lambda

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-lambda_p9thryj2)

---

## Introducing Today's Project!

In this project, I will :

1- Create a database table to store user data.

2- Create a serverless function to retrieve user data.

3- Write tests to validate if the function can fetch data from DynamoDB.

4- Secure serverless function with proper permissions.

5- Secure database with an inline policy .

### Tools and concepts

In this project, I learned how to use AWS Lambda and DynamoDB together, including creating tables with partition keys, writing Lambda functions with the AWS SDK for JavaScript, and handling errors. I also learned about IAM permission policies, narrowing access with AmazonDynamoDBReadOnlyAccess, and updating policies for specific tables to follow the principle of least privilege. Finally, I practiced testing Lambda functions and validating that they can securely retrieve data from DynamoDB.

### Project reflection

This project took me approximately 60 min

I chose to do this project today to gain hands-on experience with AWS Lambda and DynamoDB. Something that would make learning with NextWork even better is more guided, real-world exercises like this.

---

## Project Setup

To set up my project, I created a database using Amazon DynamoDB. I configured the table with a partition key set to userId.

The partition key is the primary key attribute that uniquely identifies each item in the table and determines how the data is distributed across storage partitions.

This means that:

1- Each record in the table must have a unique userId.

2- DynamoDB uses the userId value to store and retrieve data efficiently.

In my DynamoDB table, I created an item with userId as the partition key and two attributes: email and name. DynamoDB is schemaless, which means I can create different items with completely different attributes.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-lambda_a112c3d5)

### AWS Lambda

AWS Lambda is a service that run code without needing to manage any computers/servers - Lambda will manage them .

---

## AWS Lambda Function

My Lambda function has an execution role, which is an IAM role for Lambda function. It defines what the function is allowed to do, By default, the role grants permissions for writing logs to CloudWatch.

My Lambda function uses the AWS SDK for JavaScript to communicate with DynamoDB. It receives a userId as input, retrieves the corresponding record from the UserData table, and returns the result. The function also includes error handling to manage potential issues during the database operation, providing a clear and customized error message if something goes wrong.

The code uses the AWS SDK, which is a collection of tools and libraries that enable developers to build applications that interact with AWS services.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-lambda_a1b2c3d5)

---

## Function Testing

To test whether my Lambda function works, I switched to the Test tab. The test event is written in JSON format. If the test is successful, I should see the requested item in the response. If not, I would receive an error message.

The test displayed a "success" status because the function itself executed successfully. However, the function’s response contained an error because we had not given the Lambda function explicit permission to access the DynamoDB table.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-lambda_u1v2w3x4)

---

## Function Permissions

To resolve the AccessDenied error, I reviewed the error message to identify which permissions the Lambda function was missing.

There were four DynamoDB-related IAM policies I could choose from, but I did not select AWSLambdaDynamoDBExecutionRole or AWSLambdaInvocation-DynamoDB because they do not include the GetItem permission.

I also did not choose AmazonDynamoDBFullAccess because it grants more permissions than necessary, such as deleting and modifying resources, which could create security risks. Instead, AmazonDynamoDBReadOnlyAccess was the appropriate choice because it provides only the specific permissions required, following the principle of least privilege.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-lambda_3ethryj2)

---

## Final Testing and Reflection

To validate my updated permission settings, I ran the tests again. The results were successful, as expected, because the Lambda function now has the correct permissions to retrieve data from DynamoDB.

Web apps are a common use case for Lambda and DynamoDB. For example, I could use them to fetch user profiles and related content for a social media project, or to retrieve product information and descriptions for an e-commerce platform.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-lambda_p9thryj2)

---

## Enahancing Security

For my project extension, I challenged myself to narrow the read-only policy so that it applies only to the specific DynamoDB table I created, instead of granting high-level access to all DynamoDB tables. This will enhance the security of my application, reduce the risk of accidental data exposure or modification, and follow the principle of least privilege.

To create the permission policy, I used JSON because I know some basics of how to write policies, and I wanted to develop my skills by doing this exercise.

When updating a Lambda function’s permission policies, there is a risk of breaking its access. I validated that my Lambda function still works by re-running the test and confirming that the expected item was successfully retrieved.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-lambda_1qthryj2)

---

---
