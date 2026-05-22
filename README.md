Project Overview:

The Cloud Fun Facts Generator is a serverless cloud-native application built to demonstrate how multiple AWS services work together in a real-world workflow. Instead of learning AWS services individually through isolated examples, this project focuses on integrating them into a complete application architecture.

The application allows users to generate random cloud computing facts through a web interface while leveraging a fully serverless backend. Behind the scenes, the project combines AWS Lambda, Amazon API Gateway, DynamoDB, Amazon Bedrock, and AWS Amplify to create a scalable and interactive cloud solution.

The frontend is hosted using AWS Amplify, which communicates with a REST API exposed through Amazon API Gateway. API Gateway triggers an AWS Lambda function that processes incoming requests, retrieves cloud facts from DynamoDB, and enhances responses using Amazon Bedrock for AI-generated output.

This project provided practical experience in designing and deploying serverless architectures, integrating cloud services, managing APIs, handling NoSQL databases, and working with generative AI services within AWS.

The primary goal of this project was to move beyond basic tutorials and build a portfolio-ready application that demonstrates practical cloud engineering skills and real AWS service integration.

Services Used:

AWS Lambda: Serverless backend to generate cloud fun facts.
Amazon API Gateway: Exposes the Lambda as a REST API endpoint.
Amazon DynamoDB: Database for storing facts.
Amazon Bedrock: Generative AI to make facts witty.
AWS Amplify: Hosting service for the React frontend.
AWS IAM: Identity and Access Management for secure permissions.

Architectural Diagram:

<img width="1147" height="621" alt="ba68b9bbca714ba386bf118d7078c8b4" src="https://github.com/user-attachments/assets/e12b44e2-c3bc-4c68-82f3-2261a288840c" />

Final Result:

<img width="1280" height="619" alt="27795a6e54624873b0b67eb1d16722ae" src="https://github.com/user-attachments/assets/e30bd3a2-22f7-40b9-a18a-e6ca66fd56f2" />



