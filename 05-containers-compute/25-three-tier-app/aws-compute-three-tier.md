<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Three-Tier Web App

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-threetier)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

## ![Image](https://learn.nextwork.org/projects/static/aws-compute-threetier/architecture-complete.png)

## Build a Three-Tier Web App

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-threetier_2b3c4d5e)

---

## Introducing Today's Project!

In this project, I will:

🪣 Create an S3 bucket to store and host the website files using Amazon S3.

🌎 Deliver the content globally with Amazon CloudFront to ensure fast performance.

⚙️ Build the application logic using serverless functions with AWS Lambda.

🚪 Create a secure API to handle user requests through Amazon API Gateway.

💾 Store and retrieve user data using Amazon DynamoDB.

🔗 Integrate all these AWS services together to build a fully functional, scalable cloud application.

### Tools and concepts

In this project, I learned how to store website files with S3, distribute them globally using CloudFront, run serverless code with Lambda, create and manage APIs with API Gateway, store and retrieve data with DynamoDB, and connect all these services together in a three-tier web application.

### Project reflection

This project took me approximately 60 min.

I chose to do this project today because I wanted hands-on experience building a full three-tier web application on AWS. Something that would make learning with NextWork even better is adding more step-by-step troubleshooting tips for common errors.

---

## Presentation tier

For the presentation tier, I will:

🪣 Create an S3 bucket to store the website’s files using Amazon S3.

📄 Upload a simple index.html file to the bucket to serve as the main webpage.

🌍 Configure Amazon CloudFront to distribute the website’s content globally for better performance and low latency.

I accessed my delivered website by using distribution domain name given by CloudFront distribution.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-threetier_3a4b5c6d)

---

## Logic tier

For the logic tier, I will:

1- Create an AWS Lambda function to retrieve data from a DynamoDB table.

2- Write and configure the Lambda function code.

3- Set up an API Gateway REST API to expose the Lambda function.

4- Create a resource and a GET method to handle incoming requests.

5- Deploy the API so it becomes publicly accessible.

This setup connects the backend logic to the database and makes it available to the frontend

The Lambda function retrieves data using the AWS SDK for JavaScript by searching for a user based on the partition key called userId.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-threetier_6a7b8c9d)

---

## Data tier

For the data tier, I will :

1- Create a DynamoDB table.

2- Add user data into your table.

DynamoDB is used to store user information in a flexible way, using a partition key that enables fast data retrieval.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-threetier_u1v2w3x4)

---

## Logic and Data tier

In this step, we will connect all the components by updating our index.html file to send a request to the API Gateway endpoint and display the returned data.

We will also update the script.js file with JavaScript code to make the API request and verify that the data is correctly displayed on the website.

To test my API copied the Invoke URL of the my Production API followed by /users?userId=1 and as result i got the user i reqyuested whci is a validation of the cnection between the logiclayer and data layer

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-threetier_a112c3d5)

---

## Console Errors

The error on my distributed site occurred because there is no actual endpoint called "[YOUR-PROD-API-URL]/users?userId=${userId}". I need to replace [YOUR-PROD-API-URL] with the real Production API URL.

To resolve the error, I updated script.js by replacing YOUR-PROD-API-URL with the actual value, which is the Invoke URL of the Production API. I then reuploaded the updated file to S3.

I ran into a second error after updating script.js. This was a CORS (Cross-Origin Resource Sharing) error because the browser blocked my CloudFront site from accessing resources on another origin (API Gateway) since the server hadn’t given permission for my site to use them.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-threetier_a1b2c3d5)

---

## Resolving CORS Errors

To resolve the CORS error, I first enabled CORS on the /users resource in API Gateway and added the CloudFront distribution domain as the Access-Control-Allow-Origin value. This allows requests from my CloudFront site to access the API.

I also updated my Lambda function because API Gateway expects Lambda to return the full HTTP response, including CORS headers. I modified it to include the Access-Control-Allow-Origin header in the response.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-threetier_1qthryj2)

---

## Fixed Solution

I verified the fixed connection between API Gateway and CloudFront by visiting the CloudFront distribution domain and testing a request for a user, receiving the expected response.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-threetier_2b3c4d5e)

---
