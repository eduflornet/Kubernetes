
## Creating my first AKS cluster with Terraform

I have been using the following tools:

- Terraform
- VSCode

To get the required credentials to access your cluster, you need to type the following command:

```
az aks get-credentials --resource-group k8sonazure --name k8scluster

```

In the same order, run the following commands:
```
  terraform init

  terraform validate
  
  terraform plan 

  terraform apply 

  terraform destroy 
  
  ```

## Referencias:
- [Terraform kubernetes cluster](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/kubernetes_cluster)

