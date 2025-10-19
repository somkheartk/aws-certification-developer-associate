# AWS Certified Developer - Associate Tutorial (Tutorial สำหรับสอบ AWS Developer Associate)

คู่มือเตรียมสอบ AWS Certified Developer - Associate อย่างครบถ้วน

## 📋 Table of Contents
- [ภาพรวมการสอบ (Exam Overview)](#exam-overview)
- [โดเมนการสอบ (Exam Domains)](#exam-domains)
- [1. Development with AWS Services (32%)](#1-development-with-aws-services)
- [2. Security (26%)](#2-security)
- [3. Deployment (24%)](#3-deployment)
- [4. Troubleshooting and Optimization (18%)](#4-troubleshooting-and-optimization)
- [แหล่งข้อมูลเพิ่มเติม (Additional Resources)](#additional-resources)
- [เคล็ดลับการสอบ (Exam Tips)](#exam-tips)

## 📚 Additional Study Materials
- **[Study Checklist](STUDY_CHECKLIST.md)** - Track your learning progress
- **[Quick Reference Guide](QUICK_REFERENCE.md)** - Service limits, CLI commands, and decision trees
- **[Practice Questions](PRACTICE_QUESTIONS.md)** - 25+ practice questions with explanations
- **[Resources](RESOURCES.md)** - Comprehensive list of courses, books, and study materials

---

## Exam Overview

### ข้อมูลการสอบ (Exam Information)
- **ระยะเวลา (Duration):** 130 นาที
- **รูปแบบ (Format):** คำถามแบบเลือกตอบ (Multiple choice และ Multiple response)
- **จำนวนข้อ (Number of Questions):** 65 ข้อ
- **คะแนนผ่าน (Passing Score):** 720/1000
- **ราคา (Cost):** $150 USD
- **ภาษา (Languages):** อังกฤษ, ญี่ปุ่น, เกาหลี, จีนกลาง (ตัวย่อ)

### ความรู้ที่ต้องมี (Prerequisites)
- ประสบการณ์การพัฒนาและบำรุงรักษาแอปพลิเคชันบน AWS อย่างน้อย 1 ปี
- ความรู้เกี่ยวกับ AWS service อย่างน้อย 1 รายการ
- ความสามารถในการเขียนโค้ดด้วยภาษาระดับสูง (High-level programming language)
- ความเข้าใจในแนวคิดหลักของ AWS และการใช้ AWS CLI, SDKs, และ APIs

---

## Exam Domains

การสอบแบ่งออกเป็น 4 โดเมนหลัก:

1. **Development with AWS Services** - 32%
2. **Security** - 26%
3. **Deployment** - 24%
4. **Troubleshooting and Optimization** - 18%

---

## 1. Development with AWS Services

### 1.1 AWS Lambda
**สิ่งที่ต้องรู้:**
- Lambda function lifecycle และ execution context
- Event source mappings และ triggers
- Lambda layers และ environment variables
- Concurrency และ throttling
- Cold starts และ warm starts
- ขีดจำกัดของ Lambda (Timeout: 15 นาที, Memory: 128 MB - 10 GB)

**แนวทางการใช้งาน:**
```python
import json

def lambda_handler(event, context):
    # ประมวลผล event
    body = json.loads(event['body'])
    
    # Business logic
    result = process_data(body)
    
    return {
        'statusCode': 200,
        'body': json.dumps(result)
    }
```

### 1.2 Amazon API Gateway
**สิ่งที่ต้องรู้:**
- REST APIs vs HTTP APIs vs WebSocket APIs
- API Gateway stages และ deployment
- Request/Response transformations
- API throttling และ rate limiting
- API Gateway caching
- Integration types: Lambda, HTTP, AWS Service, Mock

**Best Practices:**
- ใช้ API keys สำหรับ authentication
- Enable caching เพื่อลด latency
- Configure throttling เพื่อป้องกัน abuse
- ใช้ stages สำหรับ development, staging, production

### 1.3 Amazon DynamoDB
**สิ่งที่ต้องรู้:**
- Partition key และ Sort key
- Read/Write capacity modes (Provisioned vs On-demand)
- Global Secondary Index (GSI) และ Local Secondary Index (LSI)
- DynamoDB Streams
- DAX (DynamoDB Accelerator) สำหรับ caching
- Transactions และ Batch operations
- TTL (Time To Live)

**ตัวอย่างการใช้งาน:**
```python
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Users')

# Put item
table.put_item(
    Item={
        'userId': '123',
        'name': 'John Doe',
        'email': 'john@example.com'
    }
)

# Query with partition key
response = table.query(
    KeyConditionExpression='userId = :uid',
    ExpressionAttributeValues={':uid': '123'}
)
```

### 1.4 Amazon S3
**สิ่งที่ต้องรู้:**
- S3 storage classes (Standard, IA, Glacier, etc.)
- Versioning และ Lifecycle policies
- S3 Event Notifications
- S3 Transfer Acceleration
- Multipart upload สำหรับไฟล์ขนาดใหญ่
- Pre-signed URLs
- CORS configuration

**Storage Classes:**
- **S3 Standard:** ข้อมูลที่เข้าถึงบ่อย
- **S3 Standard-IA:** ข้อมูลที่เข้าถึงไม่บ่อย
- **S3 One Zone-IA:** ข้อมูลที่ไม่สำคัญ, AZ เดียว
- **S3 Glacier:** Archive ระยะยาว
- **S3 Glacier Deep Archive:** Archive ที่เข้าถึงน้อยมาก

### 1.5 Amazon SQS (Simple Queue Service)
**สิ่งที่ต้องรู้:**
- Standard Queue vs FIFO Queue
- Message visibility timeout
- Dead Letter Queue (DLQ)
- Long polling vs Short polling
- Message retention period (1 นาที - 14 วัน)
- Maximum message size: 256 KB

**การใช้งาน:**
```python
import boto3

sqs = boto3.client('sqs')

# Send message
response = sqs.send_message(
    QueueUrl='queue-url',
    MessageBody='Hello World'
)

# Receive message
messages = sqs.receive_message(
    QueueUrl='queue-url',
    MaxNumberOfMessages=10,
    WaitTimeSeconds=20  # Long polling
)
```

### 1.6 Amazon SNS (Simple Notification Service)
**สิ่งที่ต้องรู้:**
- Topics และ Subscriptions
- Message filtering
- Fan-out pattern (SNS + SQS)
- SMS, Email, HTTP/HTTPS endpoints
- Mobile push notifications

### 1.7 Amazon Kinesis
**สิ่งที่ต้องรู้:**
- Kinesis Data Streams
- Kinesis Data Firehose
- Kinesis Data Analytics
- Shards และ Partition keys
- Data retention (1-365 วัน)

### 1.8 AWS Step Functions
**สิ่งที่ต้องรู้:**
- State machines และ States
- Task, Choice, Parallel, Wait, Fail, Succeed states
- Error handling และ Retry logic
- Integration กับ AWS services

### 1.9 Amazon ElastiCache
**สิ่งที่ต้องรู้:**
- Redis vs Memcached
- Cache strategies: Lazy loading, Write-through
- TTL (Time To Live)
- Cluster mode

### 1.10 AWS X-Ray
**สิ่งที่ต้องรู้:**
- Distributed tracing
- Service map
- Segments และ Subsegments
- Annotations และ Metadata
- Sampling rules

---

## 2. Security

### 2.1 AWS IAM (Identity and Access Management)
**สิ่งที่ต้องรู้:**
- Users, Groups, Roles, และ Policies
- Policy types: Identity-based, Resource-based, SCPs
- Permission boundaries
- IAM roles สำหรับ EC2, Lambda
- AssumeRole และ Cross-account access
- MFA (Multi-Factor Authentication)

**Policy ตัวอย่าง:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### 2.2 AWS KMS (Key Management Service)
**สิ่งที่ต้องรู้:**
- Customer Master Keys (CMK)
- Envelope encryption
- Key rotation
- Grant-based access control
- Integration กับ S3, EBS, RDS

### 2.3 AWS Secrets Manager
**สิ่งที่ต้องรู้:**
- จัดเก็บและ rotate secrets
- Integration กับ RDS
- Automatic rotation
- Version management

**ตัวอย่าง:**
```python
import boto3
import json

client = boto3.client('secretsmanager')

# Get secret
response = client.get_secret_value(SecretId='MySecret')
secret = json.loads(response['SecretString'])
```

### 2.4 AWS Systems Manager Parameter Store
**สิ่งที่ต้องรู้:**
- String, StringList, SecureString parameters
- Hierarchical parameters
- Parameter policies
- Integration กับ CloudFormation

### 2.5 Amazon Cognito
**สิ่งที่ต้องรู้:**
- User Pools สำหรับ authentication
- Identity Pools สำหรับ AWS credentials
- Social identity providers (Facebook, Google)
- SAML integration
- JWT tokens

### 2.6 Security Best Practices
- **Principle of Least Privilege:** ให้สิทธิ์แค่ที่จำเป็น
- **Encryption at rest และ in transit**
- ใช้ IAM roles แทน access keys
- Enable MFA สำหรับ privileged users
- Regular security audits ด้วย AWS Config, CloudTrail
- Implement VPC security groups และ NACLs

---

## 3. Deployment

### 3.1 AWS Elastic Beanstalk
**สิ่งที่ต้องรู้:**
- Application versions และ Environments
- Deployment policies:
  - All at once
  - Rolling
  - Rolling with additional batch
  - Immutable
  - Blue/Green
- .ebextensions configuration
- Environment tiers: Web server, Worker

### 3.2 AWS CloudFormation
**สิ่งที่ต้องรู้:**
- Templates (JSON/YAML)
- Stacks และ StackSets
- Parameters, Mappings, Conditions, Outputs
- Intrinsic functions (Ref, GetAtt, Join, Sub)
- Change sets
- Stack policies
- Nested stacks

**Template ตัวอย่าง:**
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-unique-bucket
      VersioningConfiguration:
        Status: Enabled
```

### 3.3 AWS SAM (Serverless Application Model)
**สิ่งที่ต้องรู้:**
- SAM template syntax
- SAM CLI commands (build, deploy, local)
- Local testing ด้วย sam local
- Transform: AWS::Serverless-2016-10-31

### 3.4 AWS CodeCommit
**สิ่งที่ต้องรู้:**
- Git-based repository
- Branches และ Pull requests
- Triggers และ Notifications
- Integration กับ CodePipeline

### 3.5 AWS CodeBuild
**สิ่งที่ต้องรู้:**
- buildspec.yml configuration
- Build phases: install, pre_build, build, post_build
- Environment variables
- Artifacts และ Cache
- Integration กับ CodePipeline

**buildspec.yml ตัวอย่าง:**
```yaml
version: 0.2
phases:
  install:
    runtime-versions:
      nodejs: 14
  pre_build:
    commands:
      - npm install
  build:
    commands:
      - npm run build
artifacts:
  files:
    - '**/*'
```

### 3.6 AWS CodeDeploy
**สิ่งที่ต้องรู้:**
- Deployment groups และ Application
- Deployment types:
  - In-place
  - Blue/Green
- appspec.yml configuration
- Deployment configurations
- Rollback mechanisms

### 3.7 AWS CodePipeline
**สิ่งที่ต้องรู้:**
- Pipeline stages และ Actions
- Source, Build, Deploy stages
- Manual approval actions
- Integration กับ CodeCommit, CodeBuild, CodeDeploy

### 3.8 Amazon ECS/EKS
**สิ่งที่ต้องรู้:**
- Task definitions และ Services
- Fargate vs EC2 launch types
- Service discovery
- Load balancing
- Auto scaling

### 3.9 AWS Lambda Deployment
**สิ่งที่ต้องรู้:**
- Versions และ Aliases
- Deployment preferences (Linear, Canary, All-at-once)
- Environment variables
- Layers
- Container image support

---

## 4. Troubleshooting and Optimization

### 4.1 Amazon CloudWatch
**สิ่งที่ต้องรู้:**
- Metrics, Alarms, และ Dashboards
- CloudWatch Logs และ Log groups
- CloudWatch Events/EventBridge
- CloudWatch Logs Insights
- Custom metrics
- Log retention policies

**Custom Metric ตัวอย่าง:**
```python
import boto3

cloudwatch = boto3.client('cloudwatch')

cloudwatch.put_metric_data(
    Namespace='MyApp',
    MetricData=[
        {
            'MetricName': 'ProcessedItems',
            'Value': 123,
            'Unit': 'Count'
        }
    ]
)
```

### 4.2 AWS CloudTrail
**สิ่งที่ต้องรู้:**
- API call logging
- Management events vs Data events
- Multi-region trails
- Log file integrity validation
- Integration กับ CloudWatch Logs

### 4.3 Performance Optimization
**Lambda Optimization:**
- เพิ่ม memory เพื่อเพิ่ม CPU
- ใช้ Lambda layers สำหรับ dependencies
- Minimize cold starts
- Connection pooling สำหรับ database
- Asynchronous processing

**DynamoDB Optimization:**
- ออกแบบ partition key ให้ดี
- ใช้ GSI/LSI อย่างมีประสิทธิภาพ
- Enable auto-scaling
- ใช้ DAX สำหรับ read-heavy workloads
- Batch operations

**API Gateway Optimization:**
- Enable caching
- Use compression
- Implement throttling
- Minimize payload size

### 4.4 Cost Optimization
- ใช้ reserved capacity สำหรับ predictable workloads
- Enable auto-scaling
- Use appropriate storage classes
- Monitor และ analyze costs ด้วย Cost Explorer
- Set up billing alarms

### 4.5 Common Issues และ Solutions

**Lambda Timeout:**
- เพิ่ม timeout setting
- Optimize code performance
- ใช้ async processing สำหรับ long-running tasks

**DynamoDB Throttling:**
- เพิ่ม read/write capacity
- ใช้ exponential backoff
- Distribute load across partition keys
- Consider on-demand pricing

**API Gateway 5xx Errors:**
- ตรวจสอบ backend integration
- Review Lambda permissions
- Check CloudWatch Logs
- Verify timeout settings

---

## Additional Resources

### หนังสือและคอร์สแนะนำ
1. **AWS Official Documentation:** https://docs.aws.amazon.com
2. **AWS Training and Certification:** https://aws.amazon.com/training
3. **A Cloud Guru:** AWS Certified Developer Associate course
4. **Udemy:** Stephane Maarek's AWS courses
5. **Linux Academy/ACG:** Developer Associate learning path

### Hands-on Practice
1. **AWS Free Tier:** https://aws.amazon.com/free
2. **AWS Workshops:** https://workshops.aws
3. **AWS Well-Architected Labs:** https://wellarchitectedlabs.com
4. **Qwiklabs:** AWS quest และ labs

### เครื่องมือที่ควรรู้จัก
- **AWS CLI:** Command line interface
- **AWS SDKs:** Python (Boto3), JavaScript, Java, .NET
- **AWS SAM CLI:** Local testing serverless applications
- **AWS CDK:** Infrastructure as Code ด้วย programming languages

---

## Exam Tips

### เทคนิคการสอบ
1. **อ่านคำถามให้ละเอียด** - มักมีคีย์เวิร์ดสำคัญ เช่น "most cost-effective", "least operational overhead"
2. **Eliminate wrong answers** - ตัดตัวเลือกที่ผิดชัดเจนออกก่อน
3. **Time management** - อย่าใช้เวลากับคำถามยากเกิน 2-3 นาที
4. **Flag และ review** - ทำเครื่องหมายคำถามที่ไม่แน่ใจไว้ review ทีหลัง
5. **ไม่ต้องเดา** - ไม่มีการหักคะแนน ให้เดาถ้าไม่รู้คำตอบ

### คำถามที่พบบ่อย (Frequently Asked Topics)
- Lambda + API Gateway + DynamoDB architecture
- S3 versioning และ lifecycle policies
- IAM roles และ policies
- CloudFormation intrinsic functions
- CI/CD pipeline ด้วย CodePipeline
- DynamoDB partition key design
- API Gateway caching และ throttling
- Lambda environment variables และ layers
- CloudWatch metrics และ alarms
- X-Ray tracing

### สิ่งที่ควรจำ
- **Lambda limits:** 15 นาที timeout, 10 GB memory
- **API Gateway timeout:** 29 วินาที
- **SQS message retention:** 1 นาที - 14 วัน
- **SQS message size:** 256 KB
- **DynamoDB item size:** 400 KB
- **S3 object size:** 0 bytes - 5 TB
- **CloudFormation template size:** 51,200 bytes (body), 460,800 bytes (S3)

### สูตรความสำเร็จ
1. ✅ ทำ hands-on labs บ่อยๆ
2. ✅ อ่าน AWS documentation
3. ✅ ทำ practice exams
4. ✅ เข้าใจ use cases ของแต่ละ service
5. ✅ รู้ limitations และ best practices
6. ✅ เข้าใจ cost optimization strategies

---

## Quick Reference Cards

### Lambda
- Timeout: 1 sec - 15 min
- Memory: 128 MB - 10 GB
- Deployment package: 50 MB (zipped), 250 MB (unzipped)
- Environment variables: 4 KB
- Concurrent executions: 1000 (default)

### DynamoDB
- Item size: 400 KB
- Partition key: required
- Sort key: optional
- GSI: 20 per table
- LSI: 5 per table

### S3
- Object size: 5 TB
- Multipart upload: > 100 MB recommended
- Single PUT: 5 GB max
- Buckets per account: 100 (soft limit)

### API Gateway
- Integration timeout: 29 seconds
- Payload size: 10 MB
- Throttle rate: 10,000 requests/second

---

## Conclusion

การเตรียมสอบ AWS Certified Developer - Associate ต้องใช้ทั้งความรู้ทางทฤษฎีและประสบการณ์จริง แนะนำให้:

1. **Study AWS documentation** อย่างละเอียด
2. **Hands-on practice** กับ AWS services ที่สำคัญ
3. **ทำ practice exams** เพื่อทดสอบความรู้
4. **เข้าใจ scenarios** และ use cases
5. **Review ก่อนสอบ** สม่ำเสมอ

**Good luck กับการสอบ! 🎉**

---

*Last Updated: October 2025*
*Version: 1.0*