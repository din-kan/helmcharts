# Logs from FluentD to AWS CloudWatch

The chart is created by referring the following document: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-setup-logs.html with modifications required to run in Openlshift 3.11.

Additional FluentD Daemonsets are deployed in openshift-logging project to send logs to Amazon Cloudwatch. 

The following logs groups are created by FluentD if they dont exist:
____________________________________________________________________________________________________________________________________________________________________
|			LOG GROUP NAME				|				LOG SOURCE							   |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
|	/aws/containerinsights/Cluster_Name/application  	|	All log files in /var/log/containers							   |
|								|												   |
|	/aws/containerinsights/Cluster_Name/host		|	Logs from /var/log/dmesg, /var/log/secure, and /var/log/messages			   |
|								|												   |
|	/aws/containerinsights/Cluster_Name/dataplane		|	The logs in /var/log/journal for kubelet.service, kubeproxy.service, and docker.service.   |
|								|	The following IAM policy has to be applied to all the nodes/EC2 instances in Openshift.	   |
|_______________________________________________________________|__________________________________________________________________________________________________|

## Prerequisites

1. Pre deployed openshift-logging project.
2. IAM policy to be applied on all nodes (EC2 instances) in the cluster
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "logs",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams"
            ],
            "Resource": [
                "arn:aws:logs:*:*:*"
            ]
        }
    ]
}
```
