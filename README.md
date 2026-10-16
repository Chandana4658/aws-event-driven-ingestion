# AWS Event-Driven Order Ingestion Pipeline

## Project Overview

This project implements a serverless, event-driven data ingestion pipeline using AWS services. When a new order is inserted into Amazon DynamoDB, the system processes the event, sends the order to Amazon SQS, publishes an email notification through Amazon SNS, and stores the order as a JSON file in Amazon S3.

## Architecture

**Amazon DynamoDB → AWS Lambda → Amazon SQS → AWS Lambda → Amazon S3**

The first Lambda function also publishes order notifications through Amazon SNS.

## AWS Services Used

- **Amazon DynamoDB:** Stores order records and generates stream events.
- **AWS Lambda — OrderStreamProcessor:** Processes new order events and sends messages to SQS and SNS.
- **Amazon SQS — OrderQueue:** Buffers order messages for asynchronous processing.
- **AWS Lambda — OrderS3Consumer:** Reads SQS messages and writes order data to S3.
- **Amazon S3:** Stores processed orders as JSON files.
- **Amazon SNS — OrderNotifications:** Sends email notifications when new orders are processed.
- **Amazon CloudWatch:** Stores Lambda execution logs for monitoring and troubleshooting.
- **Amazon SQS — OrderDeadLetterQueue:** Configured to receive messages after repeated processing failures.

## Example Output

**S3 object path:**

`s3://chandana-order-data-20261016/orders/ORD1006.json`

**Example JSON:**

```json
{
  "amount": "55000",
  "product": "Laptop",
  "customer_id": "CUST106",
  "order_id": "ORD1006",
  "status": "CREATED"
}
```

## Key Features

- Event-driven order processing
- Serverless architecture
- Asynchronous messaging using SQS
- Email notifications using SNS
- JSON data storage in S3
- CloudWatch logging and monitoring
- Dead-letter queue configuration for failure handling

## Testing and Validation

The pipeline was tested using sample order records. CloudWatch logs confirmed that `OrderStreamProcessor` sent order `ORD1006` to SQS and published an SNS notification. The corresponding JSON object was verified in S3.

## AWS Region

Asia Pacific (Sydney) — `ap-southeast-2`

## Future Improvements

- Add automated deployment using AWS SAM or Terraform.
- Add structured logging, alarms, and failure notifications.
- Test and document DLQ redrive and recovery.
- Add automated tests for order processing.
- Configure S3 lifecycle policies to manage storage costs.

## Learning Outcomes

This project demonstrates practical knowledge of AWS serverless services, event-driven architectures, data ingestion, asynchronous processing, JSON handling, IAM permissions, and cloud monitoring.

## Project Screenshots

### 1. DynamoDB Orders Table
![DynamoDB Orders Table](screenshots/1.%20DynamoDB%20Orders%20Table.png)

### 2. Lambda Processing Logs
![Lambda Processing Logs](screenshots/2.%20Lambda%20Processing%20Logs.png)

### 3. S3 Output Files
![S3 Output Files](screenshots/3.S3%20Output%20Files.png)

### 4. SNS Email Notification
![SNS Email Notification](screenshots/4.SNS%20Email%20Notification.jpg)
