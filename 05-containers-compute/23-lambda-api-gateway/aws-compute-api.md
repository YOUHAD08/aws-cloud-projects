<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# APIs with Lambda + API Gateway

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-api)

**Author:** YOUHAD AYOUB  
**Email:** yo_ayoub@etu.enset-media.ac.ma

---

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-api_c9d0e1f2)

---

## Introducing Today's Project!

In this project, I will  :

 1- Create a storage space in S3 for a website's files.

 2- Set up CloudFront to distribute the website globally.

 3- Manage permissions for both S3 and CloudFront.

 4- Compare different methods for hosting the website and analyze their performance.

### Tools and concepts

In this project, I learned key AWS services like Lambda, and API Gateway I also learned important concepts such as serverless architecture, API methods and resources, stages and deployment, invoke URLs, and publishing API documentation.

### Project reflection

This project took me approximately 60 min

I chose to do this project today because I wanted to learn how to build a serverless API with Lambda and API Gateway. Something that would make learning with NextWork even better is more step-by-step examples connecting APIs to databases like DynamoDB.

---

## Lambda functions

AWS Lambda is a serverless compute service that allows you to run code without provisioning or managing servers. AWS handles all the infrastructure, including scaling, patching, and availability.

Your code runs only in response to events, so you are charged only for the compute time you actually use — no idle costs.

Lambda automatically scales from a few requests per day to thousands per second. You simply upload your code in one of the supported programming languages, and Lambda takes care of the rest.

This code creates an AWS Lambda function that interacts with a DynamoDB table to retrieve user information.

It searches the table using a specific userId and returns the corresponding data. If the requested user does not exist or an error occurs during the query, the function responds with an appropriate error message.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-api_a1b2c3d5)

---

## API Gateway

An API (Application Programming Interface) is a set of rules that allows different software applications to communicate with each other.

It defines how requests are made, how data is sent, and how responses are returned. In simple terms, an API acts like a messenger between a client (such as a web or mobile app) and a server, enabling them to exchange information in a structured and secure way.

Amazon API Gateway is a fully managed AWS service that enables developers to build, publish, maintain, monitor, and secure APIs at any scale.

It acts as a front door for applications by handling incoming requests, routing them to the appropriate backend services (such as Lambda functions or other AWS services), and enforcing security measures to ensure that only authorized requests are processed.

API Gateway serves as the entry point to our Lambda function. It receives client requests and forwards them to the appropriate Lambda function for processing.

Once the Lambda function processes the request, it sends the response back through API Gateway, which then returns it to the user.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-api_m3n4o5p6)

---

## API Resources and Methods

API resources are distinct endpoints within an API that represent specific parts of its functionality.

API methods specify the type of operation that can be performed on a particular resource.

They are based on standard HTTP methods, which define how clients interact with data over the web. For example:

GET – retrieve data

POST – create or add new data

PUT – update existing data

DELETE – remove data

Each method represents a specific action that can be performed on a resource.

The method set up is the GET method on the /users resource.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-api_c9d0e1f2)

---

## API Deployment

In API Gateway, a stage represents a deployed version of your API that is accessible to users.

Each stage corresponds to a specific deployment, allowing you to manage multiple versions of the same API. For example, you might have separate stages for development, testing, and production. This setup helps you control when changes go live and who can access each version of your API.

I visited my API by opening the invoke URL in a browser. The API displayed an error because the DynamoDB table hasn’t been created yet.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-api_3ethryj2)

---

## API Documentation

For my project's extension, I am writing API documentation because is crucial for developers to understand how to use the API correctly and efficiently. You can do this by Navigating to the Documentation Section

I published my documentation by creating a documentation part in API Gateway, then selecting the prod stage, entering version 1, and clicking Publish. This made my API description and base URL available in a standardized OpenAPI format.

In my downloaded documentation, I saw a Swagger/OpenAPI file describing my API. It included the API’s title, version, base URL, endpoint paths (/users), HTTP method (GET), response details, and Lambda integration info.

![Image](http://learn.nextwork.org/radiant_cyan_daring_clementine/uploads/aws-compute-api_z9a0b1c2)

---

---
