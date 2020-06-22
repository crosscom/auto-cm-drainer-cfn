
# Create AmazonEKSAdminRole IAM Role

We will create the Amazon EKS cluster with this role, which will be the default admin of the cluster.

follow the steps [here](https://github.com/aws-samples/amazon-eks-refarch-cloudformation#create-amazoneksadminrole-iam-role)


When the role is created, make sure you can assume to this role with your current iam user.

```
$ aws sts assume-role --role-arn arn:aws:iam::{AWS_ACCOUNT}:role/AmazonEKSAdminRole --role-session-name test
{
    "AssumedRoleUser": {
        "AssumedRoleId": "AROAJCL4US4TE3MXZM272:test", 
        "Arn": "arn:aws:sts::{AWS_ACCOUNT}:assumed-role/AmazonEKSAdminRole/test"
    }, 
    "Credentials": {
        "SecretAccessKey": "...", 
        "SessionToken": "...", 
        "Expiration": "2019-02-20T08:19:58Z", 
        "AccessKeyId": "..."
    }
}
```


# Create the Amaozn EKS cluster

create the Amazon EKS cluster in your existing VPC by deploying `cluster.yaml`. Make sure you select `AmazonEKSAdminRole` as the 
cloudformation stack creation role.


# Install the `kubectl`

check [installing kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)

# update kubeconfig with aws cli

```bash
# update kubeconfig with --role-arn
# when you run kubectl, you will assume to the iam role to interact with kubernetes API
aws eks update-kubeconfig --name {CLUSTER_NAME} --role-arn arn:aws:iam::{AWS_ACCOUNT}:role/AmazonEKSAdminRole
# list nodes(should return empty)
kubectl get no
# list all resources(should return some resources)
kubectl get all -A
```

Now we'll creat the nodegroup with another cloudformation template(`test-auto-cm.yaml`)



