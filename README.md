🚀 AWS-Based Fraud Detection System
This project implements a cloud-native fraud detection system using Amazon SageMaker, AWS Fraud Detector, Lambda, API Gateway, and Amazon Kinesis Data Firehose. It supports both supervised and unsupervised learning models, incorporates SMOTE for imbalanced data, and integrates with AWS S3 for storage and result delivery.

📁 Project Overview
💡 Features:
Unsupervised Learning: Utilizes Amazon SageMaker’s Random Cut Forest (RCF) for anomaly detection.

Supervised Learning: Trains an XGBoost model using labeled data.

Data Handling: Includes SMOTE for data upsampling and balancing.

Serverless Architecture: Employs AWS Lambda triggered via API Gateway.

Data Ingestion: Uses Amazon Kinesis Data Firehose to stream results to Amazon S3.

Fraud Detector Service: Optionally integrates with AWS Fraud Detector (note: this is a paid service).

🧱 Architecture
yaml
Copy
Edit
Kaggle Dataset
     |
     v
Amazon S3 (Stores dataset and models)
     |
     v
Amazon SageMaker (Model Training and Inference)
     |
     v
AWS Lambda (Runs Inference Logic)
     |
     v
API Gateway (Exposes Lambda as API)
     |
     v
Kinesis Data Firehose
     |
     v
Amazon S3 (Stores results/output)
📦 Installation & Setup
Prerequisites:
AWS Account

IAM permissions for all services used

Python environment (with boto3, pandas, numpy, scikit-learn, imbalanced-learn)

1. Download Dataset
Download a ~150MB fraud detection dataset from Kaggle and upload it to an S3 bucket.

bash
Copy
Edit
# Sample command to upload dataset
aws s3 cp fraud_dataset.csv s3://your-bucket-name/datasets/
2. Create S3 Bucket
bash
Copy
Edit
aws s3 mb s3://your-bucket-name
3. Train Model on SageMaker
You can:

Use SageMaker Canvas or Studio for prebuilt models

Or train a custom model using Python (e.g., RCF or XGBoost with SMOTE)

4. Deploy the Model
Deploy the trained model to a SageMaker endpoint and store the model artifact in the S3 bucket.

5. Setup AWS Lambda
Create a Lambda function that:

Takes input from API Gateway

Runs inference using the SageMaker endpoint

Streams results to Kinesis

Install required packages:

bash
Copy
Edit
pip install boto3 pandas scikit-learn imbalanced-learn
6. Connect Lambda to API Gateway
Set up a REST API using API Gateway as a trigger for the Lambda function.

7. Setup Amazon Kinesis Data Firehose
Create a Kinesis Firehose delivery stream.

Set the destination to your S3 bucket.

8. Connect Firehose to Lambda
In your Lambda function, publish results to Firehose:

python
Copy
Edit
import boto3
firehose = boto3.client('firehose')
firehose.put_record(
    DeliveryStreamName='YourFirehoseStream',
    Record={'Data': output_data}
)
📌 Notes
Ignore errors related to AWS Fraud Detector if you don't have access—it's a paid service.

Make sure all necessary IAM roles and policies are configured properly.

Don’t forget to install the required Python packages inside your Lambda deployment package or layer.
