# EC2_shutdown_tags
stop and start EC2 instances based on given tags , schedule using eventbridge 

create ec2 instance with tags example project = uat or owner = xyz

Create IAM policy and role for Lambda excution
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:Start*",
                "ec2:Stop*",
                "ec2:DescribeInstanceStatus"
            ],
            "Resource": [ "*" ]
        },
        {
            "Sid": "Statement2",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}

Create lambda funcation python 3.9 and create enviroment variables as below 

define variables in lambda 
Key                Value
DEFAULT_TAGS	  tag:project=uat
LOG_LEVEL	      INFO

Test: test event exanple to execute 

#below will default tag based on enviroment variables.
{
  "action": "start"
}

{
  "action": "stop"
}

#below will override default envroment variables. 
{
  "action": "stop",
  "tags": "tag:owner=yvr"
}

Create eventbrige to schedule Lambda.

rules --> select schedule
follow the the steps enter the lambda cron
select aws service
select additional settings to select json 
{
  "action": "start"
}

check the logs in cloudwatch loggroups.
