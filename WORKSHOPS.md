# AWS Developer Associate - Hands-on Workshops

คู่มือ Workshop และแล็บปฏิบัติการสำหรับการเตรียมสอบ AWS Certified Developer - Associate

## 📑 Table of Contents
- [Workshop Prerequisites](#workshop-prerequisites)
- [Workshop 1: Serverless Web Application](#workshop-1-serverless-web-application)
- [Workshop 2: RESTful API with DynamoDB](#workshop-2-restful-api-with-dynamodb)
- [Workshop 3: Asynchronous Processing](#workshop-3-asynchronous-processing)
- [Workshop 4: CI/CD Pipeline](#workshop-4-cicd-pipeline)
- [Workshop 5: Monitoring and Troubleshooting](#workshop-5-monitoring-and-troubleshooting)
- [Sample Projects](#sample-projects)

---

## Workshop Prerequisites

### Required Setup

1. **AWS Account**
   - Free Tier eligible account
   - Credit card for verification (stay within Free Tier limits)
   - Enable MFA for root account

2. **Development Environment**
   - AWS CLI installed and configured
   - Python 3.8+ or Node.js 14+
   - Git
   - Code editor (VS Code recommended)
   - AWS SAM CLI (for serverless workshops)

3. **AWS CLI Configuration**
```bash
# Configure AWS CLI
aws configure

# Verify configuration
aws sts get-caller-identity
```

4. **Install AWS SAM CLI**
```bash
# macOS
brew install aws-sam-cli

# Windows (using chocolatey)
choco install aws-sam-cli

# Verify installation
sam --version
```

---

## Workshop 1: Serverless Web Application

### 🎯 Objective
สร้าง serverless web application ที่รับ user input และจัดเก็บข้อมูลใน DynamoDB ผ่าน API Gateway และ Lambda

### Architecture
```
User → API Gateway → Lambda → DynamoDB
```

### Step 1: Create DynamoDB Table

```bash
# Create table
aws dynamodb create-table \
    --table-name UserComments \
    --attribute-definitions \
        AttributeName=userId,AttributeType=S \
        AttributeName=timestamp,AttributeType=N \
    --key-schema \
        AttributeName=userId,KeyType=HASH \
        AttributeName=timestamp,KeyType=RANGE \
    --billing-mode PAY_PER_REQUEST \
    --region us-east-1

# Verify table creation
aws dynamodb describe-table --table-name UserComments
```

### Step 2: Create Lambda Function Using SAM

**template.yaml**
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Serverless web application

Globals:
  Function:
    Timeout: 30
    Runtime: python3.9

Resources:
  CommentsFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: functions/comments/
      Handler: app.lambda_handler
      Environment:
        Variables:
          TABLE_NAME: UserComments
      Policies:
        - DynamoDBCrudPolicy:
            TableName: UserComments
      Events:
        CreateComment:
          Type: Api
          Properties:
            Path: /comments
            Method: post
        GetComments:
          Type: Api
          Properties:
            Path: /comments/{userId}
            Method: get

Outputs:
  ApiUrl:
    Description: "API Gateway endpoint URL"
    Value: !Sub "https://${ServerlessRestApi}.execute-api.${AWS::Region}.amazonaws.com/Prod"
```

### Step 3: Create Lambda Function Code

**functions/comments/app.py**
```python
import json
import boto3
import os
import time
from decimal import Decimal

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(os.environ.get('TABLE_NAME', 'UserComments'))

class DecimalEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Decimal):
            return float(obj)
        return super(DecimalEncoder, self).default(obj)

def lambda_handler(event, context):
    http_method = event['httpMethod']
    
    try:
        if http_method == 'POST':
            return create_comment(event)
        elif http_method == 'GET':
            return get_comments(event)
        else:
            return {
                'statusCode': 405,
                'headers': {'Access-Control-Allow-Origin': '*'},
                'body': json.dumps({'error': 'Method not allowed'})
            }
    except Exception as e:
        print(f"Error: {str(e)}")
        return {
            'statusCode': 500,
            'headers': {'Access-Control-Allow-Origin': '*'},
            'body': json.dumps({'error': 'Internal server error'})
        }

def create_comment(event):
    body = json.loads(event['body'])
    user_id = body.get('userId')
    comment = body.get('comment')
    
    if not user_id or not comment:
        return {
            'statusCode': 400,
            'headers': {'Access-Control-Allow-Origin': '*'},
            'body': json.dumps({'error': 'userId and comment are required'})
        }
    
    timestamp = int(time.time())
    table.put_item(
        Item={
            'userId': user_id,
            'timestamp': timestamp,
            'comment': comment
        }
    )
    
    return {
        'statusCode': 201,
        'headers': {'Access-Control-Allow-Origin': '*'},
        'body': json.dumps({
            'message': 'Comment created',
            'timestamp': timestamp
        })
    }

def get_comments(event):
    user_id = event['pathParameters']['userId']
    
    response = table.query(
        KeyConditionExpression='userId = :uid',
        ExpressionAttributeValues={':uid': user_id},
        ScanIndexForward=False  # Descending order
    )
    
    return {
        'statusCode': 200,
        'headers': {'Access-Control-Allow-Origin': '*'},
        'body': json.dumps(response['Items'], cls=DecimalEncoder)
    }
```

### Step 4: Deploy with SAM

```bash
# Build
sam build

# Deploy (first time with guided mode)
sam deploy --guided

# For subsequent deployments
sam deploy
```

### Step 5: Test the API

```bash
# Create a comment
curl -X POST https://YOUR_API_ID.execute-api.us-east-1.amazonaws.com/Prod/comments \
  -H "Content-Type: application/json" \
  -d '{"userId": "user123", "comment": "Hello from API!"}'

# Get comments for a user
curl https://YOUR_API_ID.execute-api.us-east-1.amazonaws.com/Prod/comments/user123
```

### 🧹 Cleanup

```bash
# Delete SAM stack
sam delete

# Delete DynamoDB table (if not managed by SAM)
aws dynamodb delete-table --table-name UserComments
```

---

## Workshop 2: RESTful API with DynamoDB

### 🎯 Objective
สร้าง complete CRUD API สำหรับจัดการข้อมูล products

### Architecture
```
Client → API Gateway → Lambda Functions → DynamoDB
```

### Features to Implement
- Create Product (POST /products)
- Get Product (GET /products/{id})
- List Products (GET /products)
- Update Product (PUT /products/{id})
- Delete Product (DELETE /products/{id})

### Implementation Details

**SAM Template Structure**
```yaml
Resources:
  ProductsTable:
    Type: AWS::DynamoDB::Table
    # Table definition

  CreateProductFunction:
    Type: AWS::Serverless::Function
    # POST /products

  GetProductFunction:
    Type: AWS::Serverless::Function
    # GET /products/{id}

  ListProductsFunction:
    Type: AWS::Serverless::Function
    # GET /products

  UpdateProductFunction:
    Type: AWS::Serverless::Function
    # PUT /products/{id}

  DeleteProductFunction:
    Type: AWS::Serverless::Function
    # DELETE /products/{id}
```

### Key Learning Points
- DynamoDB CRUD operations
- API Gateway integration patterns
- Error handling
- Input validation
- CORS configuration

---

## Workshop 3: Asynchronous Processing

### 🎯 Objective
สร้าง pipeline สำหรับประมวลผลไฟล์แบบ asynchronous ด้วย SQS

### Architecture
```
S3 Upload → S3 Event → Lambda (Producer) → SQS → Lambda (Consumer)
                                                     ↓
                                                  DynamoDB
```

### Step 1: Create SQS Queue

```bash
# Create standard queue
aws sqs create-queue --queue-name FileProcessingQueue

# Create Dead Letter Queue
aws sqs create-queue --queue-name FileProcessingDLQ

# Configure DLQ on main queue
aws sqs set-queue-attributes \
    --queue-url https://sqs.us-east-1.amazonaws.com/YOUR_ACCOUNT_ID/FileProcessingQueue \
    --attributes '{
        "RedrivePolicy": "{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-1:YOUR_ACCOUNT_ID:FileProcessingDLQ\",\"maxReceiveCount\":\"3\"}"
    }'
```

### Step 2: Create Producer Lambda

**producer/app.py**
```python
import json
import boto3
import os

s3 = boto3.client('s3')
sqs = boto3.client('sqs')

QUEUE_URL = os.environ['QUEUE_URL']

def lambda_handler(event, context):
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        size = record['s3']['object']['size']
        
        # Send message to SQS
        message = {
            'bucket': bucket,
            'key': key,
            'size': size,
            'eventTime': record['eventTime']
        }
        
        sqs.send_message(
            QueueUrl=QUEUE_URL,
            MessageBody=json.dumps(message),
            MessageAttributes={
                'FileType': {
                    'DataType': 'String',
                    'StringValue': key.split('.')[-1]
                }
            }
        )
        
        print(f"Sent message for file: {key}")
    
    return {'statusCode': 200}
```

### Step 3: Create Consumer Lambda

**consumer/app.py**
```python
import json
import boto3
import os

s3 = boto3.client('s3')
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(os.environ['TABLE_NAME'])

def lambda_handler(event, context):
    for record in event['Records']:
        try:
            # Parse message
            message = json.loads(record['body'])
            bucket = message['bucket']
            key = message['key']
            size = message['size']
            
            # Process file (example: get metadata)
            obj = s3.head_object(Bucket=bucket, Key=key)
            
            # Save to DynamoDB
            table.put_item(
                Item={
                    'fileId': key,
                    'bucket': bucket,
                    'size': size,
                    'contentType': obj.get('ContentType', 'unknown'),
                    'lastModified': obj['LastModified'].isoformat(),
                    'processed': True
                }
            )
            
            print(f"Processed file: {key}")
            
        except Exception as e:
            print(f"Error processing message: {str(e)}")
            raise  # Re-raise to send message to DLQ
    
    return {'statusCode': 200}
```

### Key Learning Points
- Asynchronous vs synchronous processing
- SQS message handling
- Dead Letter Queues
- Error handling and retries
- Event-driven architecture

---

## Workshop 4: CI/CD Pipeline

### 🎯 Objective
สร้าง automated CI/CD pipeline ด้วย AWS Developer Tools

### Architecture
```
CodeCommit → CodePipeline → CodeBuild → CodeDeploy
```

### Step 1: Create CodeCommit Repository

```bash
# Create repository
aws codecommit create-repository \
    --repository-name MyApp \
    --repository-description "Sample application"

# Clone
git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/MyApp
```

### Step 2: Create buildspec.yml

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      python: 3.9
    commands:
      - pip install -r requirements.txt

  pre_build:
    commands:
      - echo "Running tests..."
      - python -m pytest tests/

  build:
    commands:
      - echo "Building application..."
      - sam build

  post_build:
    commands:
      - sam package --output-template-file packaged.yaml --s3-bucket $ARTIFACT_BUCKET

artifacts:
  files:
    - packaged.yaml

cache:
  paths:
    - '/root/.cache/pip/**/*'
```

### Step 3: Create Pipeline with CloudFormation

**pipeline.yaml**
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: CI/CD Pipeline

Resources:
  ArtifactBucket:
    Type: AWS::S3::Bucket

  CodePipeline:
    Type: AWS::CodePipeline::Pipeline
    Properties:
      Name: MyAppPipeline
      RoleArn: !GetAtt PipelineRole.Arn
      ArtifactStore:
        Type: S3
        Location: !Ref ArtifactBucket
      Stages:
        - Name: Source
          Actions:
            - Name: SourceAction
              ActionTypeId:
                Category: Source
                Owner: AWS
                Provider: CodeCommit
                Version: 1
              Configuration:
                RepositoryName: MyApp
                BranchName: main
              OutputArtifacts:
                - Name: SourceOutput

        - Name: Build
          Actions:
            - Name: BuildAction
              ActionTypeId:
                Category: Build
                Owner: AWS
                Provider: CodeBuild
                Version: 1
              Configuration:
                ProjectName: !Ref CodeBuildProject
              InputArtifacts:
                - Name: SourceOutput
              OutputArtifacts:
                - Name: BuildOutput

        - Name: Deploy
          Actions:
            - Name: DeployAction
              ActionTypeId:
                Category: Deploy
                Owner: AWS
                Provider: CloudFormation
                Version: 1
              Configuration:
                ActionMode: CHANGE_SET_REPLACE
                StackName: MyApp-Stack
                ChangeSetName: MyApp-ChangeSet
                TemplatePath: BuildOutput::packaged.yaml
                Capabilities: CAPABILITY_IAM
                RoleArn: !GetAtt CloudFormationRole.Arn
              InputArtifacts:
                - Name: BuildOutput

  # Additional resources: CodeBuildProject, Roles, etc.
```

### Key Learning Points
- CI/CD concepts
- CodeCommit, CodeBuild, CodeDeploy, CodePipeline
- buildspec.yml configuration
- Automated testing
- Deployment strategies

---

## Workshop 5: Monitoring and Troubleshooting

### 🎯 Objective
ตั้งค่า monitoring, logging, และ alerting สำหรับ serverless application

### Step 1: Enable CloudWatch Logs

```python
# Lambda function with structured logging
import json
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

def lambda_handler(event, context):
    logger.info('Function started', extra={
        'requestId': context.request_id,
        'functionName': context.function_name
    })
    
    try:
        # Your business logic
        result = process_data(event)
        
        logger.info('Processing successful', extra={
            'itemsProcessed': len(result)
        })
        
        return {
            'statusCode': 200,
            'body': json.dumps(result)
        }
        
    except Exception as e:
        logger.error('Processing failed', extra={
            'error': str(e),
            'event': event
        }, exc_info=True)
        
        raise
```

### Step 2: Create CloudWatch Alarms

```bash
# Create alarm for Lambda errors
aws cloudwatch put-metric-alarm \
    --alarm-name lambda-errors-alarm \
    --alarm-description "Alert on Lambda errors" \
    --metric-name Errors \
    --namespace AWS/Lambda \
    --statistic Sum \
    --period 300 \
    --threshold 5 \
    --comparison-operator GreaterThanThreshold \
    --evaluation-periods 1 \
    --dimensions Name=FunctionName,Value=MyFunction \
    --alarm-actions arn:aws:sns:us-east-1:YOUR_ACCOUNT_ID:AlertTopic
```

### Step 3: Custom Metrics

```python
import boto3

cloudwatch = boto3.client('cloudwatch')

def put_custom_metric(metric_name, value, unit='Count'):
    cloudwatch.put_metric_data(
        Namespace='MyApp',
        MetricData=[
            {
                'MetricName': metric_name,
                'Value': value,
                'Unit': unit,
                'Dimensions': [
                    {
                        'Name': 'Environment',
                        'Value': 'Production'
                    }
                ]
            }
        ]
    )

# Usage
put_custom_metric('OrdersProcessed', 150)
put_custom_metric('ProcessingTime', 250, 'Milliseconds')
```

### Step 4: X-Ray Tracing

```python
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.core import patch_all

patch_all()

@xray_recorder.capture('process_order')
def process_order(order_id):
    # Add metadata
    xray_recorder.put_metadata('orderId', order_id)
    
    # Add annotation (indexed)
    xray_recorder.put_annotation('orderType', 'standard')
    
    # Your processing logic
    result = do_processing(order_id)
    
    return result
```

### Step 5: CloudWatch Logs Insights Queries

```
# Find errors in the last hour
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20

# Calculate average duration
stats avg(@duration) by bin(5m)

# Find slowest requests
fields @timestamp, @duration, @message
| sort @duration desc
| limit 10
```

### Key Learning Points
- CloudWatch Logs and Metrics
- Custom metrics
- Alarms and notifications
- X-Ray distributed tracing
- Log analysis with Insights
- Dashboard creation

---

## Sample Projects

### Project 1: Todo API with Authentication

**Features:**
- User registration/login with Cognito
- JWT authentication
- CRUD operations for todos
- Per-user data isolation

**Services:**
- API Gateway
- Lambda
- DynamoDB
- Cognito User Pool
- CloudFormation/SAM

### Project 2: Image Processing Pipeline

**Features:**
- Upload images to S3
- Automatic thumbnail generation
- Image metadata extraction
- CDN distribution

**Services:**
- S3
- Lambda
- DynamoDB
- CloudFront
- SNS notifications

### Project 3: E-commerce Order System

**Features:**
- Order placement
- Inventory management
- Payment processing (mock)
- Order status tracking
- Email notifications

**Services:**
- API Gateway
- Lambda
- DynamoDB
- SQS
- SNS
- Step Functions

### Project 4: Real-time Analytics Dashboard

**Features:**
- Stream data ingestion
- Real-time processing
- Aggregations
- Web dashboard

**Services:**
- Kinesis Data Streams
- Lambda
- DynamoDB
- API Gateway
- S3 (for static website)

---

## 💡 Workshop Tips

### Best Practices

1. **Always tag resources**
```bash
--tags Key=Project,Value=Workshop Key=Environment,Value=Dev
```

2. **Use environment variables**
- Never hardcode values
- Use Parameter Store or Secrets Manager

3. **Implement proper error handling**
- Try-catch blocks
- Meaningful error messages
- Log errors with context

4. **Clean up resources**
- Delete stacks after workshops
- Empty S3 buckets before deletion
- Check for running resources

5. **Security first**
- Use IAM roles, not access keys
- Least privilege principle
- Enable encryption
- Use VPC when needed

### Cost Management

1. **Set up billing alarms**
```bash
aws cloudwatch put-metric-alarm \
    --alarm-name billing-alarm \
    --alarm-description "Alert when costs exceed $10" \
    --metric-name EstimatedCharges \
    --namespace AWS/Billing \
    --statistic Maximum \
    --period 21600 \
    --threshold 10 \
    --comparison-operator GreaterThanThreshold
```

2. **Use Free Tier wisely**
- Monitor usage
- Stay within limits
- Clean up after learning

3. **Cost optimization**
- Use on-demand for variable workloads
- Use provisioned for predictable workloads
- Enable auto-scaling
- Right-size resources

### Troubleshooting Common Issues

1. **Lambda timeout**
- Increase timeout setting
- Optimize code
- Use async processing

2. **IAM permission errors**
- Check IAM policies
- Verify resource policies
- Use Policy Simulator

3. **API Gateway 5xx errors**
- Check Lambda logs
- Verify integrations
- Check timeouts

4. **DynamoDB throttling**
- Increase capacity
- Use exponential backoff
- Check partition key design

---

## 📚 Additional Resources

### Official AWS Workshops
- https://workshops.aws/
- AWS Serverless Workshops
- AWS Well-Architected Labs

### Sample Code Repositories
- AWS Samples: https://github.com/aws-samples
- Serverless Examples: https://github.com/serverless/examples
- AWS SAM Examples: https://github.com/aws/aws-sam-cli-app-templates

### Documentation
- AWS Developer Guide
- AWS CLI Reference
- SDK Documentation (Boto3, AWS SDK for JavaScript)

---

**Happy Building! 🚀**

*Remember: The best way to learn is by doing. Start with simple workshops and gradually increase complexity.*

*Last Updated: October 2025*
