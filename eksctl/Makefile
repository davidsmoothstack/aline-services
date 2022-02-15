region=us-east-2
policyName=AWSLoadBalancerControllerIAMPolicy
policyDocument=iam_policy.json
clusterName=dw-eks-cluster
serviceAccountName=aws-load-balancer-controller
accountId=862167864120
publicNgName=Public
privateNgName=Private

init: create_cluster create_node_group

create_cluster:
	eksctl create cluster \
		--name ${clusterName} \
		--region ${region} \
		--full-ecr-access \
		--alb-ingress-access \
		--without-nodegroup

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

clean:
	eksctl delete nodegroup \
		--region ${region} \
		--name ${publicNgName}
		--approve
	
	eksctl delete nodegroup \
		--region ${region} \
		--name ${privateNgName}
		--approve

	eksctl delete cluster \
		--name ${clusterName}
		--region ${region}
		--force