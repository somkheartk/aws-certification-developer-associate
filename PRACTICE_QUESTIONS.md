# AWS Developer Associate - Practice Questions

## Domain 1: Development with AWS Services

### Question 1
A developer is building a serverless application using AWS Lambda and Amazon DynamoDB. The Lambda function occasionally experiences timeouts when accessing DynamoDB. What should the developer do to resolve this issue?

A. Increase the Lambda function's memory allocation  
B. Enable DynamoDB auto-scaling  
C. Implement connection pooling for DynamoDB  
D. Increase the Lambda function's timeout setting  

**Answer:** C
**Explanation:** Lambda functions should reuse DynamoDB connections across invocations. Connection pooling helps reduce the overhead of creating new connections, which can cause timeouts.

---

### Question 2
A company wants to process uploaded images stored in S3. The processing should happen automatically whenever a new image is uploaded. Which solution requires the LEAST operational overhead?

A. Configure S3 event notifications to trigger a Lambda function  
B. Use CloudWatch Events to poll S3 every minute and trigger Lambda  
C. Create an EC2 instance that polls S3 for new files  
D. Use Step Functions to check S3 periodically  

**Answer:** A
**Explanation:** S3 event notifications with Lambda is the most direct and serverless approach with minimal operational overhead. It triggers automatically on upload without polling.

---

### Question 3
An application uses API Gateway with Lambda integration. Users report that API calls occasionally take more than 30 seconds to complete. What is the BEST solution?

A. Increase API Gateway timeout to 60 seconds  
B. Increase Lambda timeout to 60 seconds  
C. Redesign the application to use asynchronous processing  
D. Use Step Functions to handle long-running tasks  

**Answer:** C
**Explanation:** API Gateway has a hard limit of 29 seconds. For operations taking longer, you should use asynchronous processing (e.g., Lambda → SQS → Lambda) and return immediately to the client.

---

### Question 4
A developer needs to store session data for a web application. The data must be accessible with sub-millisecond latency and support automatic expiration. Which service should be used?

A. Amazon DynamoDB with TTL  
B. Amazon ElastiCache for Redis  
C. Amazon RDS  
D. Amazon S3  

**Answer:** B
**Explanation:** ElastiCache for Redis provides sub-millisecond latency and built-in TTL support for session management. While DynamoDB with TTL is good, ElastiCache provides better latency for session data.

---

### Question 5
A developer is building a REST API that needs to return large result sets. What is the BEST practice for pagination in API Gateway?

A. Return all results in a single response  
B. Use query string parameters for page number and page size  
C. Store results in S3 and return a pre-signed URL  
D. Use POST requests to send pagination parameters  

**Answer:** B
**Explanation:** Query string parameters (e.g., ?page=1&limit=50) are the standard REST approach for pagination and work well with API Gateway.

---

### Question 6
An application processes messages from an SQS queue. Sometimes message processing fails. The developer wants failed messages to be reprocessed after all successful messages. What should be configured?

A. Increase visibility timeout  
B. Configure a Dead Letter Queue  
C. Use FIFO queue instead of Standard queue  
D. Enable long polling  

**Answer:** B
**Explanation:** A Dead Letter Queue captures messages that fail processing after a specified number of attempts, allowing them to be analyzed and reprocessed separately.

---

### Question 7
A Lambda function needs to access data from multiple AWS services (S3, DynamoDB, SQS). What is the MOST secure way to grant permissions?

A. Embed AWS access keys in environment variables  
B. Create an IAM role with required permissions and attach to Lambda  
C. Use the root account credentials  
D. Store credentials in AWS Secrets Manager  

**Answer:** B
**Explanation:** IAM roles are the recommended and most secure way to grant permissions to Lambda functions. They provide temporary credentials automatically.

---

### Question 8
A developer needs to ensure that DynamoDB queries are efficient. The table has a partition key (userId) and sort key (timestamp). Which query is MOST efficient?

A. Scan the entire table  
B. Query with only the partition key  
C. Query with partition key and sort key condition  
D. Use a filter expression on other attributes  

**Answer:** C
**Explanation:** Querying with both partition key and sort key condition is most efficient as it uses the table's primary key structure to locate data quickly.

---

### Question 9
An application needs to send notifications to multiple subscribers (email, SMS, mobile push). Which AWS service is BEST suited for this?

A. Amazon SQS  
B. Amazon SNS  
C. Amazon EventBridge  
D. AWS Lambda  

**Answer:** B
**Explanation:** Amazon SNS is designed for pub/sub messaging and can send notifications to multiple endpoints of different types (email, SMS, HTTP, Lambda, etc.).

---

### Question 10
A developer needs to trace requests through a distributed application to identify performance bottlenecks. Which service should be used?

A. Amazon CloudWatch Logs  
B. AWS CloudTrail  
C. AWS X-Ray  
D. Amazon CloudWatch Metrics  

**Answer:** C
**Explanation:** AWS X-Ray provides distributed tracing, allowing developers to analyze and debug distributed applications and identify performance bottlenecks.

---

## Domain 2: Security

### Question 11
A Lambda function needs to access a database password. What is the MOST secure way to handle this?

A. Store the password in an environment variable  
B. Hardcode the password in the Lambda code  
C. Store the password in AWS Secrets Manager and retrieve it in the function  
D. Store the password in an S3 bucket  

**Answer:** C
**Explanation:** AWS Secrets Manager is designed for storing and rotating secrets. It provides encryption, access control, and automatic rotation capabilities.

---

### Question 12
An application needs to encrypt data before storing it in S3. The encryption keys must be rotated automatically every year. Which solution meets this requirement?

A. Client-side encryption with custom keys  
B. S3 server-side encryption with S3-managed keys (SSE-S3)  
C. S3 server-side encryption with KMS-managed keys (SSE-KMS) and automatic rotation  
D. S3 server-side encryption with customer-provided keys (SSE-C)  

**Answer:** C
**Explanation:** SSE-KMS with automatic key rotation provides encryption with AWS-managed automatic key rotation annually.

---

### Question 13
A developer needs to allow a Lambda function in Account A to access an S3 bucket in Account B. What is required?

A. Create an IAM user in Account B with access to the bucket  
B. Create an IAM role in Account B that trusts Account A, and configure Lambda to assume the role  
C. Make the S3 bucket public  
D. Share the root credentials between accounts  

**Answer:** B
**Explanation:** Cross-account access is best achieved using IAM roles with trust relationships. The Lambda execution role in Account A can assume a role in Account B.

---

### Question 14
An API needs to authenticate users. The solution should support social identity providers and custom authentication logic. Which service should be used?

A. AWS IAM  
B. Amazon Cognito User Pools  
C. AWS Secrets Manager  
D. AWS Directory Service  

**Answer:** B
**Explanation:** Amazon Cognito User Pools provides user authentication with support for social identity providers (Google, Facebook) and custom authentication flows.

---

### Question 15
A developer needs to ensure that all API calls to AWS services are logged for compliance. Which service should be enabled?

A. Amazon CloudWatch Logs  
B. AWS CloudTrail  
C. AWS X-Ray  
D. AWS Config  

**Answer:** B
**Explanation:** AWS CloudTrail logs all API calls made to AWS services, providing a complete audit trail for compliance and security analysis.

---

## Domain 3: Deployment

### Question 16
A developer is using AWS SAM to deploy a serverless application. Which file defines the application resources?

A. buildspec.yml  
B. appspec.yml  
C. template.yaml  
D. package.json  

**Answer:** C
**Explanation:** AWS SAM uses template.yaml (or template.json) to define serverless application resources. It's an extension of CloudFormation templates.

---

### Question 17
An Elastic Beanstalk application needs zero-downtime deployments and the ability to quickly roll back. Which deployment policy should be used?

A. All at once  
B. Rolling  
C. Rolling with additional batch  
D. Immutable  

**Answer:** D
**Explanation:** Immutable deployments create a new set of instances with the new version, ensuring zero downtime and quick rollback by terminating new instances if deployment fails.

---

### Question 18
A CodePipeline needs to wait for manual approval before deploying to production. What should be added to the pipeline?

A. A Lambda function that sends approval requests  
B. A manual approval action in the pipeline  
C. An SNS topic for notifications  
D. A CloudWatch alarm  

**Answer:** B
**Explanation:** CodePipeline supports manual approval actions that pause the pipeline and wait for approval before proceeding to the next stage.

---

### Question 19
A developer needs to deploy different configurations to development, staging, and production environments using CloudFormation. What feature should be used?

A. Multiple templates  
B. Template parameters  
C. Nested stacks  
D. Stack policies  

**Answer:** B
**Explanation:** CloudFormation parameters allow you to customize template values at runtime, enabling the same template to be used for different environments.

---

### Question 20
A Lambda function deployment package is 60 MB zipped. The developer cannot upload it directly. What should be done?

A. Reduce the package size below 50 MB  
B. Upload to S3 and reference in Lambda  
C. Split into multiple functions  
D. Use Lambda layers  

**Answer:** B
**Explanation:** Lambda deployment packages over 50 MB (zipped) must be uploaded to S3 and referenced. The unzipped size limit is 250 MB.

---

## Domain 4: Troubleshooting and Optimization

### Question 21
A Lambda function is experiencing high latency during cold starts. What can be done to reduce cold start time?

A. Increase memory allocation  
B. Use provisioned concurrency  
C. Reduce deployment package size  
D. All of the above  

**Answer:** D
**Explanation:** All these approaches help reduce cold starts: more memory provides more CPU, provisioned concurrency keeps functions warm, and smaller packages load faster.

---

### Question 22
DynamoDB requests are being throttled. The application has unpredictable traffic patterns. What is the BEST solution?

A. Increase provisioned read/write capacity  
B. Switch to on-demand capacity mode  
C. Implement exponential backoff and retry  
D. Add more partition keys  

**Answer:** B
**Explanation:** For unpredictable traffic patterns, on-demand capacity mode automatically scales to handle varying workloads without throttling.

---

### Question 23
A developer needs to identify which Lambda function is causing high costs. Which tool should be used?

A. AWS Cost Explorer with Lambda cost allocation tags  
B. CloudWatch Logs  
C. X-Ray  
D. CloudTrail  

**Answer:** A
**Explanation:** AWS Cost Explorer allows you to analyze costs by service and tags, making it easy to identify which Lambda functions are generating the most cost.

---

### Question 24
An application's API response times have increased. The developer needs to identify slow database queries. What should be enabled?

A. CloudWatch Logs  
B. CloudTrail  
C. RDS Performance Insights  
D. X-Ray  

**Answer:** C (or D depending on context)
**Explanation:** For RDS, Performance Insights provides detailed query performance data. X-Ray would help trace the entire request flow and identify bottlenecks.

---

### Question 25
A Lambda function needs to write custom metrics to CloudWatch. What should be used?

A. CloudWatch Logs  
B. PutMetricData API  
C. X-Ray  
D. CloudTrail  

**Answer:** B
**Explanation:** The CloudWatch PutMetricData API allows applications to publish custom metrics to CloudWatch for monitoring and alarming.

---

## Answer Key Summary

| Q# | Answer | Domain |
|----|--------|--------|
| 1  | C | Development |
| 2  | A | Development |
| 3  | C | Development |
| 4  | B | Development |
| 5  | B | Development |
| 6  | B | Development |
| 7  | B | Security |
| 8  | C | Development |
| 9  | B | Development |
| 10 | C | Development |
| 11 | C | Security |
| 12 | C | Security |
| 13 | B | Security |
| 14 | B | Security |
| 15 | B | Security |
| 16 | C | Deployment |
| 17 | D | Deployment |
| 18 | B | Deployment |
| 19 | B | Deployment |
| 20 | B | Deployment |
| 21 | D | Troubleshooting |
| 22 | B | Troubleshooting |
| 23 | A | Troubleshooting |
| 24 | C/D | Troubleshooting |
| 25 | B | Troubleshooting |

---

## Tips for Practice Questions

1. **Read carefully** - Look for keywords like "MOST secure", "LEAST operational overhead", "BEST practice"
2. **Eliminate obviously wrong answers** - Narrow down to 2 choices
3. **Consider AWS best practices** - AWS prefers managed services and serverless solutions
4. **Think about cost and scalability** - Often these are deciding factors
5. **Don't overthink** - The answer is usually straightforward

---

**Keep practicing and good luck! 🎯**
