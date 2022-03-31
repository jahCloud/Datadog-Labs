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


## [Datadog Monitors]()

* Any host - Datadog Agent up
```
"datadog.agent.up".over("*").by("host").last(2).count_by_status()
```

* Any host - clock goes out of sync with the time given by NTP. The offset threshold is configured in the Agent's ntp.yaml file.
```
"ntp.in_sync".over("*").by("host").last(2).count_by_status()
```

* Any host - Internet Information Server (W3SVC) up
```
"windows_service.state".over("windows_service:w3svc").by("host").last(2).count_by_status()
```

* Any host - Message Queuing (MSMQ) up 
```
"windows_service.state".over("windows_service:msmq").exclude("host:STP-Proxy-B").by("host").last(2).count_by_status()
```

* Any host - CPU Utilization
```
avg(last_5m):avg:system.cpu.idle{*} by {name,region} < 10
```


* Any host - Memory Utilization
```
avg(last_5m):avg:system.mem.used{*} by {host} > 9000000000
```

* DIR-1 XMP services
```
"windows_service.state".over("host:STP-DIR1").exclude("windows_service:msmq","windows_service:ustore.aceservice","windows_service:ustore.officeservice","windows_service:ustore.taskscheduler","windows_service:w32time","windows_service:w3svc").by("host","windows_service").last(2).count_by_status()
```

* DIR-1 XMP services
```
"windows_service.state".over("host:STP-DIR1").exclude("windows_service:msmq","windows_service:w32time","windows_service:w3svc","windows_service:xmpservicecloudgateway","windows_service:xmpserviceicp","windows_service:xmpserviceicpasynch","service:xmpservicejobreport","windows_service:xmpservicequeuemgr","windows_service:xmpservicerip2image","windows_service:xmpservicescheduler","windows_service:xmpservicetracking","windows_service:xmpservicetracking2cloud").by("host","windows_service").last(2).count_by_status()
```
