---
title: exec plugin is configured to use API version issue in EKS
date: 2022-08-02T00:20:02
last_modified_at: 2022-08-14T00:20:02
categories:
  - aws
tags:
  - AWS
  - EKS
  - Kubernetes
  - kubectl
toc: true  
lang: en
---

> 'client.authentication.k8s.io/v1alpha1' has been removed and must be changed to 'v1beta1'.

## Issue
When running kubectl commands for eks cluster, Occurrence of a message from the following command:

``` bash
  $ kubectl get pods
  Unable to connect to the server: getting credentials: exec plugin is configured to use API version client.authentication.k8s.io/v1beta1, plugin returned version client.authentication.k8s.io/v1alpha1
  
  $ kubectl get pods
  Kubeconfig user entry is using deprecated API version client.authentication.k8s.io/v1alpha1. Run 'aws eks update-kubeconfig' to update.
  
  $ kubectl get pods
  error: exec plugin: invalid apiVersion "client.authentication.k8s.io/v1alpha1"
```


## Action
- AWS Cli update (minimum 1.24.0)
- Update apiVersion value in User section of .kube/config file to **client.authentication.k8s.io/v1beta1**  

``` yaml
  users:
  - name: arn:aws:eks:$region_code:$account_id:cluster/$cluster_name
    user:
      exec:
        ## apiVersion: client.authentication.k8s.io/v1alpha1 ## Update to v1beta1
        apiVersion: client.authentication.k8s.io/v1beta1
```  
Or run the awseks update-kubconfig command to update.


## Details
### EKS Authentication, Authorisation
  - Authentication : Webhook Token Authentication
  - Authorisation : RBAC Authorization(Role-based access control)  

It is linked with the Bearer token by the webhook service (AWS IAM Authenticator) provided by AWS. This Bearer token contains AWS Identity and Access Management (IAM) credentials.  
That is, the client authenticates to the EKS cluster using **IAM credentials**.

### KubeConfig File ###  
To use EKS with kubectl or other tools,I would have created a kube/conifg file. Open the file and check the kubconfig for EKS.

> When executing the aws eks update-kubeconfig or eksctl create cluster command, kube config for eks is automatically created.

A detailed description of the Kubernetes certification is omitted. Credentials for API server authentication are defined in the user section.
``` yaml
  users:
  - name: arn:aws:eks:$region_code:$account_id:cluster/$cluster_name
    user:
      exec:
        apiVersion: client.authentication.k8s.io/v1alpha1 
        args:
        - --region
        - $region_code
        - eks
        - get-token ## Get a token with the aws eks get-token command.
        - --cluster-name
        - $cluster_name
        command: aws
```  
The **exec** attribute is important. This attribute is provided by a feature of the Go client library called **client-go credential plugins**, which allows you to simply define external commands that generate and return credentials. (Link : [client-go-credential-plugins](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#client-go-credential-plugins))  

AWS EKS generates credentials with the aws get-token command and retrieves the token (aws-iam-authenticator is also available)

If you created config with an older version of AWS Cli, or eksctl, the apiVersion is set to **client.authentication.k8s.io/v1alpha1**. It was removed from version 1.24.  
 
### ExecCredential ###  
If authentication is successful, the results are returned with the [ExecCredential](https://kubernetes.io/docs/reference/config-api/client-authentication.v1beta1/#client-authentication-k8s-io-v1beta1-ExecCredential) specification, and the token is used to authenticate against the Kubernetes API. If you run the aws get-token command,

``` javascript
// AWS EKS get-token API Docs
  {
  "kind": "ExecCredential",
  "apiVersion": "client.authentication.k8s.io/v1beta1", // apiVersion has been updated.
  "spec": {},
  "status": {
    "expirationTimestamp": "2022-08-14T18:44:27Z",
    "token": "k8s-aws-v1EXAMPLE_TOKEN_DATA_STRING..."
  }
}
```  
apiVersion has been updated to v1beta. **client-go credential plugins** are officially released as stable. (GA)
The current apiVersion is **client.authentication.k8s.io/v1**, so the same problem may occur if AWS Credential is updated in the future.


## 참고

- Related Git Issue
  - [aws eks update-kubeconfig invalid apiVersion #6920](https://github.com/aws/aws-cli/issues/6920)
  - [AWS eks get-token Docs](https://docs.aws.amazon.com/cli/latest/reference/eks/get-token.html#options)

