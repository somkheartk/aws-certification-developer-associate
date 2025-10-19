# AWS Developer Associate - Quick Reference Guide

## ⚡ Service Limits & Quotas

### AWS Lambda
| Limit | Value |
|-------|-------|
| Timeout | 1 second - 15 minutes |
| Memory | 128 MB - 10 GB |
| Deployment package (zipped) | 50 MB |
| Deployment package (unzipped) | 250 MB |
| Environment variables | 4 KB |
| Concurrent executions | 1000 (default, can be increased) |
| /tmp directory storage | 512 MB - 10 GB |
| File descriptors | 1,024 |
| Processes and threads | 1,024 |

### Amazon API Gateway
| Limit | Value |
|-------|-------|
| Integration timeout | 29 seconds |
| Payload size | 10 MB |
| Default throttle rate | 10,000 requests/second |
| Default burst rate | 5,000 requests |
| Stages per API | 10 |
| Resources per API | 300 |

### Amazon DynamoDB
| Limit | Value |
|-------|-------|
| Item size | 400 KB |
| Partition key length | 1 - 2048 bytes |
| Sort key length | 1 - 1024 bytes |
| Global secondary indexes | 20 per table |
| Local secondary indexes | 5 per table |
| Projected secondary index attributes | 100 |
| Table name length | 3 - 255 characters |

### Amazon S3
| Limit | Value |
|-------|-------|
| Object size | 0 bytes - 5 TB |
| Single PUT operation | 5 GB |
| Multipart upload parts | 10,000 |
| Bucket name length | 3 - 63 characters |
| Buckets per account | 100 (soft limit) |
| Object key length | 1 - 1024 bytes |

### Amazon SQS
| Limit | Value |
|-------|-------|
| Message size | 256 KB |
| Message retention | 1 minute - 14 days (default: 4 days) |
| Visibility timeout | 0 seconds - 12 hours |
| Long polling wait time | 1 - 20 seconds |
| Batch size | 10 messages |
| Inflight messages (Standard) | 120,000 |
| Inflight messages (FIFO) | 20,000 |

### Amazon Kinesis Data Streams
| Limit | Value |
|-------|-------|
| Data retention | 1 - 365 days (default: 24 hours) |
| Record size | 1 MB |
| Shard capacity (writes) | 1 MB/sec or 1,000 records/sec |
| Shard capacity (reads) | 2 MB/sec |
| Number of shards | No limit (pay per shard) |

### AWS CloudFormation
| Limit | Value |
|-------|-------|
| Template body size | 51,200 bytes |
| Template from S3 | 460,800 bytes |
| Parameters | 200 |
| Outputs | 200 |
| Resources | 500 |
| Mappings | 200 |
| Stack name length | 1 - 128 characters |

---

## 🔑 Common Use Cases & Patterns

### Serverless Web Application
```
Users → CloudFront → API Gateway → Lambda → DynamoDB
                                        ↓
                                       S3
```

### Event-Driven Processing
```
S3 Event → Lambda → Process → DynamoDB/SNS
```

### Async Processing with Queue
```
API Gateway → Lambda (Producer) → SQS → Lambda (Consumer) → Processing
```

### Fan-out Pattern
```
Producer → SNS Topic → Multiple SQS Queues → Multiple Consumers
```

### Stream Processing
```
Data Source → Kinesis Data Stream → Lambda → Destination
                                      ↓
                                  Real-time Analytics
```

### Microservices Architecture
```
API Gateway → Lambda Functions → DynamoDB
                ↓                    ↓
            SNS/SQS             ElastiCache
                                    ↓
                                Other Services
```

---

## 🎯 Decision Trees

### When to use which queue?
- **Need ordering?** → FIFO Queue
- **Need deduplication?** → FIFO Queue
- **Need highest throughput?** → Standard Queue
- **Message processing in any order OK?** → Standard Queue

### When to use which database?
- **Key-value, millisecond latency?** → DynamoDB
- **Relational data, SQL?** → RDS
- **Caching?** → ElastiCache (Redis/Memcached)
- **Graph data?** → Neptune
- **Document DB with MongoDB API?** → DocumentDB
- **Time-series data?** → Timestream

### When to use which compute?
- **Event-driven, serverless?** → Lambda
- **Containerized, orchestration?** → ECS/EKS
- **Long-running applications?** → EC2
- **Batch processing?** → AWS Batch
- **Simple web app?** → Elastic Beanstalk

### When to use which storage?
- **Object storage?** → S3
- **Block storage for EC2?** → EBS
- **File storage with NFS?** → EFS
- **Archive data?** → S3 Glacier
- **Low-latency file access?** → FSx

---

## 📝 Important Commands

### AWS CLI - Lambda
```bash
# Create function
aws lambda create-function --function-name my-function \
  --runtime python3.9 --role arn:aws:iam::123456789012:role/lambda-role \
  --handler lambda_function.lambda_handler --zip-file fileb://function.zip

# Invoke function
aws lambda invoke --function-name my-function \
  --payload '{"key": "value"}' response.json

# Update function code
aws lambda update-function-code --function-name my-function \
  --zip-file fileb://function.zip

# List functions
aws lambda list-functions
```

### AWS CLI - DynamoDB
```bash
# Put item
aws dynamodb put-item --table-name Users \
  --item '{"userId": {"S": "123"}, "name": {"S": "John"}}'

# Get item
aws dynamodb get-item --table-name Users \
  --key '{"userId": {"S": "123"}}'

# Query
aws dynamodb query --table-name Users \
  --key-condition-expression "userId = :uid" \
  --expression-attribute-values '{":uid": {"S": "123"}}'

# Scan
aws dynamodb scan --table-name Users
```

### AWS CLI - S3
```bash
# Upload file
aws s3 cp file.txt s3://my-bucket/

# Download file
aws s3 cp s3://my-bucket/file.txt ./

# Sync directory
aws s3 sync ./local-dir s3://my-bucket/

# List objects
aws s3 ls s3://my-bucket/
```

### AWS CLI - SQS
```bash
# Send message
aws sqs send-message --queue-url https://sqs.region.amazonaws.com/123/my-queue \
  --message-body "Hello World"

# Receive message
aws sqs receive-message --queue-url https://sqs.region.amazonaws.com/123/my-queue

# Delete message
aws sqs delete-message --queue-url https://sqs.region.amazonaws.com/123/my-queue \
  --receipt-handle "receipt-handle-value"
```

### SAM CLI
```bash
# Initialize new project
sam init

# Build application
sam build

# Deploy application
sam deploy --guided

# Local invoke
sam local invoke MyFunction -e events/event.json

# Start local API
sam local start-api

# Generate sample event
sam local generate-event apigateway aws-proxy
```

---

## 🔒 Security Best Practices Checklist

### IAM
- ✅ Enable MFA for root account
- ✅ Don't use root account for daily tasks
- ✅ Create individual IAM users
- ✅ Use groups to assign permissions
- ✅ Grant least privilege
- ✅ Use IAM roles for applications
- ✅ Rotate credentials regularly
- ✅ Enable CloudTrail logging

### Application Security
- ✅ Encrypt data at rest (S3, EBS, RDS)
- ✅ Encrypt data in transit (SSL/TLS)
- ✅ Use VPC for network isolation
- ✅ Use security groups as firewalls
- ✅ Store secrets in Secrets Manager/Parameter Store
- ✅ Never hardcode credentials
- ✅ Use KMS for encryption keys
- ✅ Implement API authentication (Cognito, IAM)

### Monitoring & Logging
- ✅ Enable CloudTrail for all regions
- ✅ Enable VPC Flow Logs
- ✅ Monitor with CloudWatch
- ✅ Set up billing alarms
- ✅ Use AWS Config for compliance
- ✅ Enable GuardDuty for threat detection

---

## 💡 Common Exam Scenarios

### Scenario 1: Cost Optimization
**Question:** "Most cost-effective solution..."
**Look for:** 
- S3 lifecycle policies
- Auto-scaling
- Reserved instances/capacity
- On-demand vs provisioned capacity
- Appropriate storage classes

### Scenario 2: High Availability
**Question:** "Ensure high availability..."
**Look for:**
- Multi-AZ deployments
- Auto-scaling groups
- Load balancers
- DynamoDB global tables
- S3 cross-region replication

### Scenario 3: Security
**Question:** "Most secure way to..."
**Look for:**
- IAM roles (not access keys)
- Encryption at rest and in transit
- VPC and security groups
- Secrets Manager
- KMS encryption

### Scenario 4: Performance
**Question:** "Reduce latency..." or "Improve performance..."
**Look for:**
- ElastiCache/DAX
- CloudFront CDN
- API Gateway caching
- Lambda memory optimization
- DynamoDB optimization

### Scenario 5: Decoupling
**Question:** "Decouple components..."
**Look for:**
- SQS between components
- SNS for fan-out
- EventBridge for events
- Step Functions for workflows

---

## 📊 Comparison Tables

### Lambda vs Fargate vs EC2
| Feature | Lambda | Fargate | EC2 |
|---------|--------|---------|-----|
| Management | Fully managed | Container managed | Self-managed |
| Scaling | Automatic | Automatic | Manual/Auto-scaling |
| Billing | Per request | Per second | Per hour |
| Max runtime | 15 minutes | Unlimited | Unlimited |
| Best for | Event-driven | Containers | Long-running |

### SQS Standard vs FIFO
| Feature | Standard | FIFO |
|---------|----------|------|
| Ordering | Best-effort | Guaranteed |
| Throughput | Unlimited | 300 TPS (batch: 3000) |
| Delivery | At-least-once | Exactly-once |
| Deduplication | No | Yes |
| Use case | High throughput | Ordered processing |

### ElastiCache: Redis vs Memcached
| Feature | Redis | Memcached |
|---------|-------|-----------|
| Data types | Advanced (lists, sets) | Simple (strings) |
| Persistence | Yes | No |
| Replication | Yes | No |
| Multi-AZ | Yes | No |
| Backup/Restore | Yes | No |
| Pub/Sub | Yes | No |

### S3 Storage Classes
| Class | Use Case | Availability | Cost |
|-------|----------|--------------|------|
| Standard | Frequent access | 99.99% | $$$ |
| Standard-IA | Infrequent access | 99.9% | $$ |
| One Zone-IA | Non-critical, infrequent | 99.5% | $ |
| Glacier | Archive | 99.99% | ¢ |
| Glacier Deep Archive | Long-term archive | 99.99% | ¢/10 |

---

## 🎯 Last-Minute Review

### Top 10 Things to Remember
1. **Lambda timeout:** 15 minutes max
2. **API Gateway timeout:** 29 seconds
3. **SQS message size:** 256 KB
4. **DynamoDB item size:** 400 KB
5. **IAM:** Use roles, not access keys
6. **Encryption:** At rest AND in transit
7. **CloudFormation:** Use Ref and GetAtt
8. **CodePipeline:** Source → Build → Deploy
9. **CloudWatch:** Metrics, Logs, Alarms, Events
10. **X-Ray:** Distributed tracing and debugging

### Common Keywords in Questions
- **"Most cost-effective"** → Think S3 lifecycle, auto-scaling, reserved capacity
- **"Least operational overhead"** → Think managed services, serverless
- **"High availability"** → Think multi-AZ, auto-scaling, redundancy
- **"Secure"** → Think IAM roles, encryption, VPC, Secrets Manager
- **"Decouple"** → Think SQS, SNS, EventBridge
- **"Real-time"** → Think Kinesis, WebSockets, Lambda
- **"Near real-time"** → Think SQS, SNS
- **"Batch processing"** → Think AWS Batch, Step Functions

---

**Good luck on your exam! 🎉**
