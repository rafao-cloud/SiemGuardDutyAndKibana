# SiemGuardDutyAndKibana

A Siem environment using Guard Duty + Kinbana + S3 + ES + Cognito + Fire House

Fork with some changes helped by that content:

https://aws.amazon.com/pt/blogs/security/visualizing-amazon-guardduty-findings/

BY Michael Fortuna and Ravi Sakaria both SA of AWS.


1 - Amazon GuardDuty is enabled in an account and begins monitoring CloudTrail logs, VPC flow logs, and DNS query logs. If a threat is detected, GuardDuty forwards a finding to CloudWatch Events. For a newly generated finding, GuardDuty sends a notification based on its CloudWatch event within 5 minutes of the finding. CloudWatch Events allows you to send upstream notifications to various services filtered on your configured event patterns. We’ll configure an event pattern that only forwards events coming from the GuardDuty service.


2 - We define two targets in our CloudWatch Event Rule. The first target is a Kinesis Firehose stream for delivery into an Elasticsearch domain and an S3 bucket. The second target is an SNS Topic for Email/SMS notification of findings. We’ll send all findings to our targets; however, you can filter and format the findings you send by using a Lambda function (or by event pattern matching with a CloudWatch Event Rule). For example, you could send only high-severity alarms (that is, findings with detail.severity > 7).


3 - The Firehose stream delivers findings to Amazon Elasticsearch, which provides visualization and analysis for our event findings. The stream also delivers findings to an S3 bucket. The S3 bucket is used for long term archiving. This data can augment your data lake and you can use services such as Amazon Athena to perform advanced analytics.


4 - We’ll search, explore, and visualize the GuardDuty findings using Kibana and the Elasticsearch query Domain Specific Language (DSL) to gain valuable insights. Amazon Elasticsearch has a built-in Kibana plugin to visualize the data and perform operational analyses.


5 - To provide a simplified and secure authentication method, we provide user authentication to Kibana with Amazon Cognito User Pools. This method provides improved security from traditional IP whitelists or proxy infrastructure.


6 - Our second CloudWatch Event target is SNS, which has subscribed email endpoint(s) that allow your operations teams to receive email (or SMS messages) when a new GuardDuty Event is received.



If you would like to centralize your findings from multiple regions into a single S3 bucket, you can adapt this pipeline. You would deploy the frontend of the pipeline by configuring Kinesis Firehose in the remote regions to point to the S3 bucket in the centralized region. You can leverage prefixes in the Kinesis Firehose configuration to identify the source region. For example, you would configure a prefix of us-west-1 for events originating from the us-west-1 region. Analytic queries from tools such as Athena can then selectively target the desired region.
