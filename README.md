# Architecture Overview
 `The system follows a modular serverless pattern :`
* Ingestion Layer : Data enters the system via Amazon S3 buckets or through a Gmail API integration (webhook/polling).
* Compute Layer: AWS Lambda functions handle the core logic, including data validation, parsing, and transformation.
* Orchestration: AWS Step Functions manage the workflow state, ensuring that tasks like database logging and filtering happen in the correct sequence.
* Data & Messaging: * Amazon DynamoDB serves as a high-speed NoSQL store for metadata and process logs.
* Amazon SNS/SQS handles message filtering and downstream notifications.
* Storage Layer: Final processed assets are archived in a secure Amazon S3 destination bucket.

# 🚀 Key Features
* Zero Infrastructure Management: Fully serverless execution using AWS Lambda and Step Functions.
* Event-Driven Execution: The pipeline triggers instantly upon receiving an email or file upload.
* Cost Efficiency: Utilizes a pay-per-use model, incurring costs only when the pipeline is active.
* Resilience: Built-in error handling and retries via Step Function state machines.

# Service #         # Role #
AWS Lambda         :  Serverless compute for business logic
Amazon S3          :  Durable object storage for raw and processed files
Amazon DynamoDB    : State management and metadata storage
AWS Step Functions : Visual workflow orchestration
Gmail API          :  External source for data ingestion
Amazon SNS         :  Pub/Sub messaging and alerts

# 📈 The Workflow
* Trigger : An incoming email (via Gmail API) or an S3 ObjectCreated event triggers the initial Lambda.
* Initialization : The first Lambda function validates the payload and initiates the Step Function.
* Persistence : The workflow creates a record in DynamoDB to track the processing status.
* Processing & Filtering : Data is passed through filtering logic to determine its destination or required transformations.
* Completion : The finalized data is moved to the output S3 bucket, and a success notification is sent via SNS.

# 🛠 Deployment Steps
* IAM Roles : Configure execution roles with least-privilege access for Lambda and Step Functions.
* Environment Variables : Set up API keys for Gmail integration and bucket names in Lambda configurations.
* State Machine : Deploy the Amazon States Language (ASL) definition for the Step Function.
* Triggers : Configure S3 Event Notifications or EventBridge rules for the Gmail webhook.
