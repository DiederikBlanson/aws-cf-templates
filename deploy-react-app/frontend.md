# CI/CD Pipeline for React Web App Deployment to S3 Hosting using AWS CloudFormation

This script is a CloudFormation template that provisions a CodePipeline that builds and deploys a static website hosted on an S3 bucket. The pipeline has three stages: Source, Build, and Deploy. The Source stage retrieves the website's source code from a CodeStar Connection to a source code repository, while the Build stage uses CodeBuild to compile the website's source code and store the output in an S3 bucket. The Deploy stage copies the built website to another S3 bucket that is configured for website hosting and sets up a CloudFront distribution that proxies requests to the S3 bucket.

The resources created by the template include the CodePipeline, CodeBuild project, two S3 buckets (one for storing pipeline artifacts and another for website hosting), two IAM roles (one for CodePipeline and one for CodeBuild), a CloudFront origin access identity, a CloudFront distribution, an ACM certificate, and a Route53 record set. The website URL is exposed as an output parameter of the CloudFormation stack.

### Parameters
The following parameters are used throughout the template:
- `AppName`: A string representing the name of the application.
- `Url`: A string representing the URL for the deployed web app. 
- `ServerUrl`: A string representing the URL of the server hosting the app's APIs. 
- `AcmArn`: A string representing the Amazon Resource Name (ARN) of the AWS Certificate Manager (ACM) certificate used for HTTPS/TLS.
- `HostZoneId`: A string representing the hosted zone ID of the Amazon Route 53 DNS record for the deployed web app. 
- `Stage`: A string representing the deployment stage (e.g., "production" or "staging"). The default value is production.
- `SourceCodeArn`: A string representing the ARN of the AWS CodeStar connection used to connect to the source code repository. 
- `SourceCodeRepo`: A string representing the name of the source code repository for the app.
- `SourceCodeBranch`: A string representing the name of the branch in the source code repository to deploy.

### TODO
There are still some improvements to be made.
- Invalidate CloudFront-cache upon every new deployment such that the users always sees the latest version.
- At the moment, the user has to manually create a SSL-certificate for the frontend (value `AcmArn`) which is not ideal.
- In case the user wants to delete the CloudFormation deployment, the S3 buckets should be manually deleted as this is not possible to do via CloudFormation. A potential solution includes triggering a Lambda-function that calls the AWS S3 api that is able to delete buckets programmatically.