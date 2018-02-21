# S3 Action<a name="receiving-email-action-s3"></a>

The **S3** action delivers the mail to an Amazon S3 bucket and, optionally, notifies you through Amazon SNS\. This action has the following options\.

+ **S3 Bucket—**The name of the Amazon S3 bucket to which to save received emails\. You can also create a new Amazon S3 bucket when you set up your action by choosing **Create S3 Bucket**\. Amazon SES provides you the raw, unmodified email, which is typically in Multipurpose Internet Mail Extensions \(MIME\) format\. For more information about MIME format, see [RFC 2045](https://tools.ietf.org/html/rfc2045)\.
**Important**  
When you save your emails to an Amazon S3 bucket, the maximum email size \(including headers\) is 30 MB\.

+ **Object Key Prefix—**A key name prefix to use within the Amazon S3 bucket\. Key name prefixes enable you to organize your Amazon S3 bucket in a folder structure\. For example, if you use *Email* as your **Object Key Prefix**, your emails will appear in your Amazon S3 bucket in a folder named *Email*\.

+ **KMS Key \(if "Encrypt Message" is selected in the Amazon SES console\)—**The customer master key that Amazon SES should use to encrypt your emails before saving them to the Amazon S3 bucket\. You can use the default master key or a custom master key you created in AWS KMS\.
**Note**  
The master key you choose must be in the same AWS region as the Amazon SES endpoint you use to receive email\. 

  + To use the default master key, choose **aws/ses** when you set up the receipt rule in the Amazon SES console\. If you use the Amazon SES API, you can specify the default master key by providing an ARN in the form of `arn:aws:kms:REGION:AWSACCOUNTID:alias/aws/ses`\. For example, if your AWS account ID is 123456789012 and you want to use the default master key in the US West \(Oregon\) region, the ARN of the default master key would be `arn:aws:kms:us-west-2:123456789012:alias/aws/ses`\. If you use the default master key, you don't need to perform any extra steps to give Amazon SES permission to use the key\.

  + To use a custom master key you created in AWS KMS, provide the ARN of the master key and ensure that you add a statement to your key's policy to give Amazon SES permission to use it\. For more information about giving permissions, see [Giving Permissions to Amazon SES for Email Receiving](receiving-email-permissions.md)\.

  For more information about using AWS KMS with Amazon SES, see the [AWS Key Management Service Developer Guide](http://docs.aws.amazon.com/kms/latest/developerguide/services-ses.html)\. If you do not specify a master key in the console or API, Amazon SES will not encrypt your emails\.
**Important**  
Your mail is encrypted by Amazon SES using the Amazon S3 encryption client before the mail is submitted to Amazon S3 for storage\. It is not encrypted using Amazon S3 server\-side encryption\. This means that you must use the Amazon S3 encryption client to decrypt the email after retrieving it from Amazon S3, as the service has no access to use your AWS KMS keys for decryption\. This encryption client is available in the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/) and the [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/)\. For more information about client\-side encryption using AWS KMS master keys, see the [Amazon Simple Storage Service Developer Guide](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingClientSideEncryption.html)\.

+ **SNS Topic—**The name or ARN of the Amazon SNS topic to notify when an email is saved to the Amazon S3 bucket\. An example of an Amazon SNS topic ARN is *arn:aws:sns:us\-west\-2:123456789012:MyTopic*\. You can also create an Amazon SNS topic when you set up your action by choosing **Create SNS Topic**\. For more information about Amazon SNS topics, see the [Amazon Simple Notification Service Developer Guide](http://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\.
**Note**  
The Amazon SNS topic you choose must be in the same AWS region as the Amazon SES endpoint you use to receive email\. 