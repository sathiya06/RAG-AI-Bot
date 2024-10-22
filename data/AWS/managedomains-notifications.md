# Notifications in Amazon OpenSearch Service<a name="managedomains-notifications"></a>

Notifications in Amazon OpenSearch Service contain important information about the performance and health of your domains\. OpenSearch Service notifies you about service software updates, Auto\-Tune enhancements, cluster health events, and domain errors\. Notifications are available for all versions of OpenSearch and Elasticsearch OSS\.

You can view notifications in the **Notifications** panel of the OpenSearch Service console\. All notifications for OpenSearch Service are also surfaced in [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)\. For a full list of notifications and sample events, see [Monitoring OpenSearch Service events with Amazon EventBridge](monitoring-events.md)\.

## Getting started with notifications<a name="managedomains-notifications-start"></a>

Notifications are enabled automatically when you create a domain\. Go to the **Notifications** panel of the OpenSearch Service console to monitor and acknowledge notifications\. Each notification includes information such as the time it was posted, the domain it relates to, a severity and status level, and a brief explanation\. You can view historical notifications for up to 90 days in the console\.

After accessing the **Notifications** panel or acknowledging a notification, you might receive an error message about not having permissions to perform `es:ListNotifications` or `es:UpdateNotificationStatus`\. To resolve this problem, give your user or role the following permissions in IAM:

```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "es:UpdateNotificationStatus",
      "es:ListNotifications"
    ],
    "Resource": "arn:aws:es:*:123456789012:domain/*"
  }]
}
```

The IAM console throws an error \("IAM does not recognize one or more actions\."\) that you can safely ignore\. You can also restrict the `es:UpdateNotificationStatus` action to certain domains\. To learn more, see [Policy element reference](ac.md#ac-reference)\.

## Notification severities<a name="managedomains-notifications-severities"></a>

Notifications in OpenSearch Service can be *informational*, which relate to any action you've already taken or the operations of your domain, or *actionable*, which require you to take specific actions such as applying a mandatory security patch\. Each notification has a severity associated with it, which can be `Informational`, `Low`, `Medium`, `High`, or `Critical`\. The following table summarizes each severity:


| Severity | Description | Examples | 
| --- | --- | --- | 
| Informational |  Information related to the operation of your domain\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opensearch-service/latest/developerguide/managedomains-notifications.html)  | 
| Low |  A recommended action, but has no adverse impact on domain availability or performance if no action is taken\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opensearch-service/latest/developerguide/managedomains-notifications.html)  | 
| Medium |  There might be an impact if the recommended action is not taken, but comes with an extended time window for the action to be taken\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opensearch-service/latest/developerguide/managedomains-notifications.html)  | 
| High |  Urgent action is required to avoid adverse impact\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opensearch-service/latest/developerguide/managedomains-notifications.html)  | 
| Critical |  Immediate action is required to avoid adverse impact, or to recover from it\.   | None currently available | 

## Sample EventBridge event<a name="managedomains-notifications-cloudwatch"></a>

The following example shows an OpenSearch Service notification event sent to Amazon EventBridge\. The notification has a severity of `Informational` because the update is optional:

```
{
  "version": "0",
  "id": "01234567-0123-0123-0123-012345678901",
  "detail-type": "Amazon OpenSearch Service Software Update Notification",
  "source": "aws.es",
  "account": "123456789012",
  "time": "2016-11-01T13:12:22Z",
  "region": "us-east-1",
  "resources": ["arn:aws:es:us-east-1:123456789012:domain/test-domain"],
  "detail": {
    "event": "Service Software Update",
    "status": "Available",
    "severity": "Informational",
    "description": "Service software update [R20200330-p1] available."
  }
}
```