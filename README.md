# Serverless E-Commerce Platform

A production-grade three-tier serverless architecture demonstrating how modern organisations build cost-efficient, infinitely scalable web applications without managing servers.

## Overview

Traditional e-commerce platforms require significant infrastructure management—provisioning servers, configuring load balancers, and planning for traffic spikes. This project eliminates that operational overhead by implementing a fully serverless architecture where AWS handles all scaling automatically, and costs scale linearly with actual usage rather than anticipated capacity.

The platform separates concerns across three distinct tiers: a React single-page application delivered globally via CloudFront CDN, a RESTful API layer powered by Lambda functions behind API Gateway, and a DynamoDB NoSQL database for persistent storage. Each tier scales independently, meaning a traffic spike hitting the API doesn't affect frontend delivery, and database read capacity automatically adjusts to demand.

All infrastructure is codified in Terraform using a modular structure that mirrors the application architecture. This approach enables environment promotion (dev → staging → prod) through variable changes rather than manual configuration, while maintaining state in S3 for team collaboration.

## Architecture

The request flow follows a clear path through the stack: users access the application via a CloudFront distribution that caches and serves the React build from S3. When the frontend needs product data, it calls the API Gateway endpoint which routes requests to the appropriate Lambda function based on HTTP method and path.

Each Lambda function handles a single CRUD operation—this single-responsibility pattern keeps functions small, fast to cold-start, and independently deployable. The functions authenticate to DynamoDB using IAM roles with least-privilege policies scoped to specific table operations.

The Terraform structure reflects this separation with dedicated modules for `database`, `backend`, and `frontend`. The root module orchestrates these components, passing outputs between modules (e.g., DynamoDB table ARN to the backend module for IAM policy creation). State is stored remotely in S3 to enable collaborative infrastructure management.

## Tech Stack

**Infrastructure**: AWS Lambda, API Gateway (REST), DynamoDB, S3, CloudFront, IAM

**IaC**: Terraform with modular structure, S3 remote state

**Frontend**: React 18, Vite, React Router, localforage for cart persistence

**Backend**: Python 3.9 Lambda functions with boto3

**Deployment**: PowerShell automation for S3 sync and CloudFront invalidation

## Key Decisions

- **DynamoDB with PAY_PER_REQUEST billing**: Chose on-demand capacity mode to avoid provisioning decisions and enable true pay-per-use pricing—ideal for variable traffic patterns common in e-commerce.

- **Single-purpose Lambda functions over monolithic handler**: Each CRUD operation gets its own function, enabling independent scaling, isolated deployments, and faster cold starts compared to a single function routing internally.

- **CloudFront with SPA error handling**: Configured 403/404 responses to return index.html with 200 status, enabling client-side routing without server-side route configuration.

- **Modular Terraform with explicit dependencies**: Structured infrastructure as composable modules with outputs passed between them, making the architecture self-documenting and enabling selective applies during development.

## Screenshots

![Application frontend](screenshots/frontend-products.png)

![API Gateway endpoints](screenshots/api-gateway-console.png)

![Lambda functions](screenshots/lambda-functions.png)

![DynamoDB table](screenshots/dynamodb-table.png)

![CloudFront distribution](screenshots/cloudfront-distribution.png)

![S3 bucket hosting](screenshots/s3-bucket.png)

![Terraform deployment](screenshots/terraform-apply.png)

![Git workflow](screenshots/feature-branches.png)

## Author

**Noah Frost**

- Website: [noahfrost.co.uk](https://noahfrost.co.uk)
- GitHub: [github.com/nfroze](https://github.com/nfroze)
- LinkedIn: [linkedin.com/in/nfroze](https://linkedin.com/in/nfroze)