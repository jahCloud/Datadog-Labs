## [AWS Integration - Role delegation]()

* Create a AWS Role (DatadogIntegration-Role) for Datadog access
 
* With Permissions policy (DatadogIntegration-Policy) as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "cloudwatch:Describe*",
                "cloudwatch:Get*",
                "cloudwatch:List*",
                "ec2:Describe*",
                "elasticloadbalancing:describe*",
                "elasticloadbalancing:describeloadbalancers",
                "fsx:DescribeFileSystems",
                "fsx:ListTagsForResource",
                "health:DescribeEvents",
                "health:DescribeEventDetails",
                "health:DescribeAffectedEntities",
                "lambda:GetPolicy",
                "lambda:List*",
                "logs:DeleteSubscriptionFilter",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams",
                "logs:DescribeSubscriptionFilters",
                "logs:FilterLogEvents",
                "logs:PutSubscriptionFilter",
                "logs:TestMetricFilter",
                "organizations:DescribeOrganization",
                "rds:Describe*",
                "rds:List*",
                "route53:List*",
                "s3:GetBucketLogging",
                "s3:GetBucketLocation",
                "s3:GetBucketNotification",
                "s3:GetBucketTagging",
                "s3:ListAllMyBuckets",
                "s3:PutBucketNotification",
                "states:ListStateMachines",
                "states:DescribeStateMachine",
                "support:*",
                "tag:GetResources",
                "tag:GetTagKeys",
                "tag:GetTagValues"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```


* With Trusted Entities (Trust Relationships):

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::*********:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {
                "StringEquals": {
                    "sts:ExternalId": "********"
                }
            }
        }
    ]
}
```

