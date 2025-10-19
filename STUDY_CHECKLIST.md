# AWS Developer Associate - Study Checklist

Use this checklist to track your progress as you prepare for the exam.

## 📚 Domain 1: Development with AWS Services (32%)

### AWS Lambda
- [ ] Understand Lambda function lifecycle
- [ ] Know how to configure Lambda functions (memory, timeout, environment variables)
- [ ] Understand Lambda concurrency and throttling
- [ ] Know Lambda limits (15 min timeout, 10 GB memory)
- [ ] Understand Lambda layers
- [ ] Practice creating Lambda functions with different runtimes
- [ ] Understand event source mappings
- [ ] Know how to handle errors and retries

### Amazon API Gateway
- [ ] Understand REST vs HTTP vs WebSocket APIs
- [ ] Know how to configure API stages and deployments
- [ ] Understand API Gateway caching
- [ ] Know throttling and rate limiting
- [ ] Understand request/response transformations
- [ ] Practice creating APIs with Lambda integrations
- [ ] Know the 29-second timeout limit
- [ ] Understand CORS configuration

### Amazon DynamoDB
- [ ] Understand partition keys and sort keys
- [ ] Know when to use GSI vs LSI
- [ ] Understand provisioned vs on-demand capacity
- [ ] Know DynamoDB Streams
- [ ] Understand DAX (DynamoDB Accelerator)
- [ ] Practice query and scan operations
- [ ] Know item size limit (400 KB)
- [ ] Understand transactions and batch operations
- [ ] Know TTL (Time To Live) functionality

### Amazon S3
- [ ] Understand different storage classes
- [ ] Know versioning and lifecycle policies
- [ ] Understand S3 event notifications
- [ ] Know multipart upload for large files
- [ ] Understand pre-signed URLs
- [ ] Practice CORS configuration
- [ ] Know S3 Transfer Acceleration
- [ ] Understand object size limits (5 TB max)

### Amazon SQS
- [ ] Understand Standard vs FIFO queues
- [ ] Know visibility timeout
- [ ] Understand Dead Letter Queues (DLQ)
- [ ] Know long polling vs short polling
- [ ] Understand message retention (1 min - 14 days)
- [ ] Know message size limit (256 KB)
- [ ] Practice send and receive messages

### Amazon SNS
- [ ] Understand topics and subscriptions
- [ ] Know message filtering
- [ ] Understand fan-out pattern with SQS
- [ ] Know supported endpoints (SMS, Email, HTTP/HTTPS)
- [ ] Practice creating topics and subscriptions

### Amazon Kinesis
- [ ] Understand Kinesis Data Streams
- [ ] Know Kinesis Data Firehose
- [ ] Understand shards and partition keys
- [ ] Know data retention (1-365 days)
- [ ] Understand when to use Kinesis vs SQS

### AWS Step Functions
- [ ] Understand state machines
- [ ] Know different state types (Task, Choice, Parallel, etc.)
- [ ] Understand error handling and retry logic
- [ ] Practice creating workflows

### Amazon ElastiCache
- [ ] Understand Redis vs Memcached
- [ ] Know cache strategies (lazy loading, write-through)
- [ ] Understand when to use caching

### AWS X-Ray
- [ ] Understand distributed tracing
- [ ] Know segments and subsegments
- [ ] Understand annotations and metadata
- [ ] Practice enabling X-Ray for Lambda

---

## 🔒 Domain 2: Security (26%)

### AWS IAM
- [ ] Understand users, groups, and roles
- [ ] Know policy types (identity-based, resource-based)
- [ ] Practice writing IAM policies
- [ ] Understand permission boundaries
- [ ] Know when to use IAM roles vs users
- [ ] Understand AssumeRole
- [ ] Know principle of least privilege

### AWS KMS
- [ ] Understand Customer Master Keys (CMK)
- [ ] Know envelope encryption
- [ ] Understand key rotation
- [ ] Know KMS integration with other services

### AWS Secrets Manager
- [ ] Understand secret storage and rotation
- [ ] Know integration with RDS
- [ ] Practice retrieving secrets in code
- [ ] Understand automatic rotation

### AWS Systems Manager Parameter Store
- [ ] Understand parameter types (String, SecureString)
- [ ] Know hierarchical parameters
- [ ] Understand when to use vs Secrets Manager

### Amazon Cognito
- [ ] Understand User Pools vs Identity Pools
- [ ] Know authentication flow
- [ ] Understand JWT tokens
- [ ] Know social identity provider integration

### Security Best Practices
- [ ] Understand encryption at rest and in transit
- [ ] Know how to secure API endpoints
- [ ] Understand VPC security groups and NACLs
- [ ] Know MFA best practices

---

## 🚀 Domain 3: Deployment (24%)

### AWS Elastic Beanstalk
- [ ] Understand deployment policies (All at once, Rolling, Immutable, Blue/Green)
- [ ] Know .ebextensions configuration
- [ ] Understand environment tiers (Web server, Worker)
- [ ] Practice deploying applications

### AWS CloudFormation
- [ ] Understand template structure (Resources, Parameters, Outputs)
- [ ] Know intrinsic functions (Ref, GetAtt, Join, Sub)
- [ ] Practice writing templates in YAML
- [ ] Understand change sets
- [ ] Know nested stacks
- [ ] Understand stack policies

### AWS SAM
- [ ] Understand SAM template syntax
- [ ] Know SAM CLI commands (build, deploy, local)
- [ ] Practice local testing with sam local
- [ ] Understand Transform directive

### AWS CodeCommit
- [ ] Understand Git-based repository
- [ ] Know triggers and notifications
- [ ] Understand integration with CodePipeline

### AWS CodeBuild
- [ ] Understand buildspec.yml structure
- [ ] Know build phases (install, pre_build, build, post_build)
- [ ] Practice creating build projects
- [ ] Understand artifacts and caching

### AWS CodeDeploy
- [ ] Understand deployment types (in-place, blue/green)
- [ ] Know appspec.yml configuration
- [ ] Understand deployment configurations
- [ ] Know rollback mechanisms

### AWS CodePipeline
- [ ] Understand pipeline stages and actions
- [ ] Know source, build, and deploy stages
- [ ] Practice creating pipelines
- [ ] Understand manual approval actions

### Amazon ECS/EKS
- [ ] Understand task definitions and services
- [ ] Know Fargate vs EC2 launch types
- [ ] Understand service discovery
- [ ] Know auto-scaling configuration

### Lambda Deployment
- [ ] Understand versions and aliases
- [ ] Know deployment preferences (Linear, Canary, All-at-once)
- [ ] Practice using Lambda layers
- [ ] Understand container image support

---

## 🔧 Domain 4: Troubleshooting and Optimization (18%)

### Amazon CloudWatch
- [ ] Understand metrics, alarms, and dashboards
- [ ] Know CloudWatch Logs and log groups
- [ ] Practice CloudWatch Logs Insights
- [ ] Understand custom metrics
- [ ] Know log retention policies
- [ ] Understand EventBridge/CloudWatch Events

### AWS CloudTrail
- [ ] Understand API call logging
- [ ] Know management events vs data events
- [ ] Understand multi-region trails
- [ ] Know log file integrity validation

### Performance Optimization
- [ ] Know Lambda optimization techniques
- [ ] Understand DynamoDB optimization strategies
- [ ] Know API Gateway caching best practices
- [ ] Understand connection pooling
- [ ] Know when to use async processing

### Cost Optimization
- [ ] Understand reserved capacity
- [ ] Know auto-scaling strategies
- [ ] Understand appropriate storage classes
- [ ] Know how to use Cost Explorer
- [ ] Understand billing alarms

### Common Issues
- [ ] Know how to troubleshoot Lambda timeouts
- [ ] Understand DynamoDB throttling solutions
- [ ] Know how to debug API Gateway errors
- [ ] Practice reading CloudWatch Logs
- [ ] Understand exponential backoff

---

## 🎯 Practice and Preparation

### Hands-on Labs
- [ ] Complete AWS Free Tier labs
- [ ] Try AWS Workshops
- [ ] Complete Well-Architected Labs
- [ ] Practice on Qwiklabs

### Practice Exams
- [ ] Take AWS official practice exam
- [ ] Complete practice exams from A Cloud Guru
- [ ] Take Udemy practice tests
- [ ] Review incorrect answers

### Documentation Review
- [ ] Read AWS Developer Guide for key services
- [ ] Review AWS Best Practices whitepapers
- [ ] Study AWS Well-Architected Framework
- [ ] Read service FAQs

### Final Preparation
- [ ] Review all quick reference cards
- [ ] Memorize key limits and quotas
- [ ] Practice time management
- [ ] Review common scenarios
- [ ] Get good rest before exam

---

## 📊 Progress Tracking

Track your overall progress by domain:

- Domain 1 (Development): ____%
- Domain 2 (Security): ____%
- Domain 3 (Deployment): ____%
- Domain 4 (Troubleshooting): ____%

**Overall Progress:** ____%

**Target Exam Date:** __________

**Notes:**
