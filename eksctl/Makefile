region=us-east-2
policyName=AWSLoadBalancerControllerIAMPolicy
policyDocument=iam_policy.json
clusterName=dw-eks
serviceAccountName=aws-load-balancer-controller
accountId=862167864120
publicNgName=Public
privateNgName=Private
fargateProfileName=fargate-default
fargateNamespace=microservice

init: create_cluster create_node_group create_fargate_profile

create_cluster:
	eksctl create cluster \
		--name ${clusterName} \
		--region ${region} \
		--full-ecr-access \
		--alb-ingress-access \
		--without-nodegroup \
		--vpc-cidr "10.1.0.0/16" \
		--vpc-nat-mode Single

set_context:
	eksctl utils write-kubeconfig \
		--region ${region} \
		--cluster ${clusterName} \
		--set-kubeconfig-context

create_node_group:
	eksctl create nodegroup \
		--name ${publicNgName} \
		--cluster ${clusterName} \
		--region ${region} \
		--node-type t2.small \
		--nodes-min 1 \
		--nodes-max 2 \
		--full-ecr-access \
		--alb-ingress-access

	eksctl create nodegroup \
		--name ${privateNgName} \
		--node-private-networking \
		--cluster ${clusterName} \
		--region ${region} \
		--node-type t2.small \
		--nodes-min 1 \
		--nodes-max 2 \
		--full-ecr-access \
		--alb-ingress-access

create_fargate_profile:
	eksctl create fargateprofile \
		--cluster ${clusterName} \
		--region ${region} \
		--name ${fargateProfileName}
		--namespace ${faregateNamespace}

clean:
	eksctl delete cluster \
		--name ${clusterName} \
		--region ${region} \
		--force