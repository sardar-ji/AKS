# Learn Terraform - Provision AKS Cluster

This repo contains Terraform configuration files to provision an AKS cluster on Azure.

AKS-DEMO


Step 1: Create a free azure account.

Step 2:

•	Set kubectl and azure-cli on your machine.
o	brew install kubectl
o	brew install kubernetes-cli

•	Set service-principle account.
o	az ad sp create-for-rbac --skip-assignment
o	it will give the id password details which will be used in terraform.tfvars

Step 3:
•	Create a folder named AKS-DEMO.
•	Create the following structure

 
o	aks-demo.tf is the main terraform file which contains resources.
o	outputs.tf file contains values that is useful to interact with AKS cluster
o	terraform.tfstate[.backup] store the states of tf when applied. It created automatically when you run it.
o	terraform.tfvars file contains credentials to connect azure.
o	variables.tf declare appID and password.
o	versions.tf set tf version to min 0.14 for provider.


Step 4: init/plan/apply your terraform code.

Step 5:  Configure kubectl, by running the following command.

$az aks get-credentials --resource-group $(terraform output -raw resource_group_name) --name $(terraform output -raw kubernetes_cluster_name)

this command will get the context and set in .kube/config file.



Step 5: Access Kubernetes dashboard

kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard --user=clusterUser

Step 6: Launch Kubernetes dashboard

az aks browse --resource-group $(terraform output -raw resource_group_name) --name $(terraform output -raw kubernetes_cluster_name)

Step 7: Generate the token to access dashboard.

•	kubectl -n kube-system get secrets | grep dashboard
•	kubectl -n kube-system describe secret kubernetes-dashboard-token-2r6n9| awk '$1=="token:"{print $2}'

Now kubernetes dashboard is up and running with one node.


Deploy Nginx on AKS

Step 1: Create your namespace

Step 2: Create nginx deployment

kubectl create deployment my-nginx --image=nginx -n nginx-hs

kubectl expose deployment my-nginx --port=80 --type=LoadBalancer -n nginx-hs


	

Thanks!!
