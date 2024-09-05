
# permisos default para un admin:

- Search for and select the following policies:
    - `AmazonEKSFullAccess`
    - `AmazonECSFullAccess`
    - `CloudWatchFullAccess`
    - `AWSCloudFormationFullAccess`
    - `AmazonS3FullAccess`
    - `AmazonVPCFullAccess`
    - `IAMFullAccess`
    - `AWSBillingReadOnlyAccess`


# permiso inline para tener full access al servicio EKS: hftamayoEKSFullAccess

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "eks:*",
                "ec2:Describe*",
                "ec2:CreateSecurityGroup",
                "ec2:DeleteSecurityGroup",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:RevokeSecurityGroupIngress",
                "ec2:AuthorizeSecurityGroupEgress",
                "ec2:RevokeSecurityGroupEgress",
                "iam:GetRole",
                "iam:CreateRole",
                "iam:DeleteRole",
                "iam:CreateServiceLinkedRole",
                "iam:DeleteServiceLinkedRole",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:PassRole",
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeStacks",
                "cloudformation:UpdateStack",
                "cloudformation:ListStacks",
                "autoscaling:CreateAutoScalingGroup",
                "autoscaling:UpdateAutoScalingGroup",
                "autoscaling:DeleteAutoScalingGroup",
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:CreateLaunchConfiguration",
                "autoscaling:DeleteLaunchConfiguration",
                "elasticloadbalancing:CreateLoadBalancer",
                "elasticloadbalancing:DeleteLoadBalancer",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
                "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
                "elasticloadbalancing:DescribeInstanceHealth"
            ],
            "Resource": "*"
        }
    ]
}

```

este permiso no puede reutilizarse con otro perfil IAM, hay que volverlo a crear.

# permisos para un devopsadmin

AmazonEC2FullAccess
AmazonECS_FullAccess
AmazonS3FullAccess
AmazonVPCFullAccess
AWSCloudFormationFullAccess
CloudWatchFullAccess
CloudWatchFullAccessV2
hftamayoEKSFullAccess
IAMFullAccess