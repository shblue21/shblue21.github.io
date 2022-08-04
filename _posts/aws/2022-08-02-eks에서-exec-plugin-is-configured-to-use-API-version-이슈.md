---
title: eks에서 exec plugin is configured to use API version 이슈
date: 2022-08-02T00:20:02
last_modified_at: 2022-08-03T00:20:02
categories:
  - aws
tags:
  - AWS
  - EKS
  - Kubernetes
  - kubectl
toc: true  
---
> client.authentication.k8s.io/v1alpha1 가 deprecated 되었으므로 v1beta1을 사용해야 한다.

## 이슈
eks cluster에 대해 kubectl commands 실행시,  

``` bash
  kubectl get pods

  Unable to connect to the server: getting credentials: exec plugin is configured to use API version client.authentication.k8s.io/v1beta1, plugin returned version client.authentication.k8s.io/v1alpha1
  or
  Kubeconfig user entry is using deprecated API version client.authentication.k8s.io/v1alpha1. Run 'aws eks update-kubeconfig' to update.
  or
  invalid apiVersion client.authentication.k8s.io/v1alpha1
```

의 메세지 발생

## 해결방법 요약
- AWS Cli 업데이트(최소 1.24.0)  
- .kube/config 의 사용자/인증의 apiVersion 업데이트 **client.authentication.k8s.io/v1beta1**
![EKS_1](/img/220802_eksissue_1.png)   
혹은 aws cli로 config 업데이트.  


## 상세
1. 에러 발생 이유.  
  EKS는 User 인증을 위해 Bearer Token을 사용하는데, 이를 위해 eks get-token를 호출한다.  
  K8s 1.24에서  **client.authentication.k8s.io/v1alpha1** 이 deprecated 되었고,
  AWS도 이에 api version을 변경하였으나, .kubeconfig와 일치하지 않아 발생하는 문제이다.

  ``` yaml
    users:
    - name: arn:aws:eks:ap-northeast-2:154462851762:cluster/conflunet-demo
      user:
        exec:
          apiVersion: client.authentication.k8s.io/v1alpha1 
          args:
          - --region
          - ap-northeast-2
          - eks
          - get-token ## aws eks get-token 으로 토큰을 가져온다.
          - --cluster-name
          - conflunet-demo
          command: aws
  ```
  ``` javascript
  // AWS EKS get-token API Docs
    {
    "kind": "ExecCredential",
    "apiVersion": "client.authentication.k8s.io/v1beta1", // apiVersion이 업데이트 되었다.
    "spec": {},
    "status": {
      "expirationTimestamp": "2019-08-14T18:44:27Z",
      "token": "k8s-aws-v1EXAMPLE_TOKEN_DATA_STRING..."
    }
  }
  ```  
2. K8s 인증 방법
   WIP
3. AWS eks get-token
   WIP

## 참고

- 관련 Git Issue 
  - [aws eks update-kubeconfig invalid apiVersion #6920](https://github.com/aws/aws-cli/issues/6920)
  - [AWS eks get-token Docs](https://docs.aws.amazon.com/cli/latest/reference/eks/get-token.html#options)

