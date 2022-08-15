---
title: eks에서 exec plugin is configured to use API version 이슈
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
---
> client.authentication.k8s.io/v1alpha1 가 Removed 되었으므로 v1beta1로 변경해야 한다.

## 이슈
eks cluster에 대해 kubectl commands 실행시,  

``` bash
  $ kubectl get pods
  Unable to connect to the server: getting credentials: exec plugin is configured to use API version client.authentication.k8s.io/v1beta1, plugin returned version client.authentication.k8s.io/v1alpha1
  
  $ kubectl get pods
  Kubeconfig user entry is using deprecated API version client.authentication.k8s.io/v1alpha1. Run 'aws eks update-kubeconfig' to update.
  
  $ kubectl get pods
  error: exec plugin: invalid apiVersion "client.authentication.k8s.io/v1alpha1"
```

의 메세지 발생

## 조치방법
- AWS Cli 업데이트(최소 1.24.0)  
- .kube/config 파일의 User섹션의 apiVersion 값을 **client.authentication.k8s.io/v1beta1** 로 업데이트  

``` yaml
  users:
  - name: arn:aws:eks:$region_code:$account_id:cluster/$cluster_name
    user:
      exec:
        ## apiVersion: client.authentication.k8s.io/v1alpha1 ## v1beta1로 업데이트
        apiVersion: client.authentication.k8s.io/v1beta1
```  
혹은 aws eks update-kubeconfig 명령어를 실행하여 업데이트.  


## 상세
### EKS 인증, 인가
  - 인증 : Webhook Token Authentication
  - 인가 : RBAC Authorization(Role-based access control)  

AWS에서 제공하는 웹훅 서비스(AWS IAM Authenticator)에 의해 Bearer token과 연동된다. 이 Bearer 토큰에 AWS Identity and Access Management(IAM) 자격 증명이 포함되어 있다. 
즉, 클라이언트는 **IAM 자격 증명**을 사용하여 EKS 클러스터에 인증한다.  

### KubeConfig 파일 ###  
kubectl이나 기타 Tool로 EKS를 사용하기 위해, .kube/conifg 파일 생성하였을 것이다. 파일을 열어 EKS용 kubeconfig 을 확인한다.

> aws eks update-kubeconfig 혹은 eksctl create cluster 명령어를 실행할 때, eks용 kube config가 자동으로 생성된다.

Kubernetes 인증에 대한 자세한 설명은 생략한다. user 섹션에 API 서버 인증을 위한 자격 증명이 정의되어 있다.
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
        - get-token ## aws eks get-token 으로 토큰을 가져온다.
        - --cluster-name
        - $cluster_name
        command: aws
```  
**exec** 속성이 중요하다. 이 속성은 **client-go credential plugins** 라는 Go 클라이언트 라이브러리의 기능에 의해 제공되는데, 간단히 자격 증명을 생성하고 반환하는 외부 명령을 정의할 수 있다. (참고 : [client-go-credential-plugins](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#client-go-credential-plugins))  

AWS EKS는 aws get-token 명령으로 자격 증명을 생성하고, 토큰을 가져온다. (aws-iam-authenticator도 사용 가능)  

구버전의 AWS Cli, 혹은 eksctl로 config를 생성한 경우, apiVersion이 **client.authentication.k8s.io/v1alpha1** 로 되어있으나. 1.24버전에서 제거 되었다.  
 
### ExecCredential ###  
인증에 성공시 [ExecCredential](https://kubernetes.io/docs/reference/config-api/client-authentication.v1beta1/#client-authentication-k8s-io-v1beta1-ExecCredential) 스펙으로 결과를 반환하며, 토큰을 사용하여 Kubernetes API에 대해 인증합니다. aws get-token 명령을 실행하면,

``` javascript
// AWS EKS get-token API Docs
  {
  "kind": "ExecCredential",
  "apiVersion": "client.authentication.k8s.io/v1beta1", // apiVersion이 업데이트 되었다.
  "spec": {},
  "status": {
    "expirationTimestamp": "2022-08-14T18:44:27Z",
    "token": "k8s-aws-v1EXAMPLE_TOKEN_DATA_STRING..."
  }
}
```  
apiVersion이 v1beta로 업데이트 되어 있다. **client-go credential plugins**은 Stable으로 정식 릴리즈가 되어 있다.(GA)  
현재 apiVersion은 **client.authentication.k8s.io/v1** 이므로 추후 AWS Credential이 업데이트 될 경우 동일한 문제가 발생할 수 있다.


## 참고

- 관련 Git Issue 
  - [aws eks update-kubeconfig invalid apiVersion #6920](https://github.com/aws/aws-cli/issues/6920)
  - [AWS eks get-token Docs](https://docs.aws.amazon.com/cli/latest/reference/eks/get-token.html#options)

