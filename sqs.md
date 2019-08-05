# SQS

SQS offers two types of message queues. Standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery. SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent.

SQS is meant to deliver messages to a single consumer type where as with Kafka same messages can be consumed by multiple consumer types

## Trigger

You can trigger a Lambda to consume the Queue. FIFO queues don't support Lambda function triggers.
FIFO queues don't support Lambda function triggers.

https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-lambda-function-trigger.html

You can associate only one queue with one or more Lambda functions.

## Limits

https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-limits.html

