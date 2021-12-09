# terraform-aws-eks-airflow


The current architecture was implemented following this guide [Provisioning an EKS Cluster guide](https://learn.hashicorp.com/terraform/kubernetes/provision-eks-cluster)

### Prerequisites

- AWS account configured. For this example we are using default profile and us-east-2 region

#### Dependencies
- aws cli
- Cluster version: 1.20 
- Terraform >= 0.13

### Installing

To have K8s cluster running:

Execute Terraform commands:

```
terraform init
```
The "init" command will download and prepare all componenets that are needed for the specified infraestructure. 


```
terraform apply --var-file=terraform.tfvars
```
The "apply" command will implement all the infraestructure. The --var option tells terraform to use an specific file for setting up the variables.



Once that the cluster is created, set the kubectl context: (kubectl controls the Kubernetes cluster manager )

```
aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)
```
The "update-kubeconfig" command configures kubectl so that you can connect to an Amazon EKS cluster.



To destroy the EKS cluster, we run: (it is important to use the "--var" option if we used "terraform apply" with a variables file)

```
terraform destroy --var-file=terraform.tfvars
```


### Airflow
export the nfs server. (saves that variable in memory to use it later in the process.)
```
export NFS_SERVER=$(terraform output -raw efs)
```


To install airflow go to the directory `kubernetes/`. [Install Airflow](../kubernetes/README.md)

## Acknowledgments

This solution was based on this guide: [Provision an EKS Cluster learn guide](https://learn.hashicorp.com/terraform/kubernetes/provision-eks-cluster), containing
Terraform configuration files to provision an EKS cluster on AWS.