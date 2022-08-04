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

 **'Unable to connect to the server: getting credentials: exec plugin is configured to use API version client.authentication.k8s.io/v1beta1, plugin returned version client.authentication.k8s.io/v1alpha1'**  

**'Kubeconfig user entry is using deprecated API version client.authentication.k8s.io/v1alpha1. Run 'aws eks update-kubeconfig' to update.**' 

**invalid apiVersion client.authentication.k8s.io/v1alpha1**  

등의 메세지 발생

## 해결방법 요약
- AWS Cli 업데이트(최소 1.24.0)  
- .kube/config 의 사용자/인증의 apiVersion 업데이트 **client.authentication.k8s.io/v1beta1**
![EKS_1](/img/220802_eksissue_1.png)   
혹은 aws cli로 config 업데이트.  

``` bash
aws eks update-kubeconfig # 기존 config 및 클러스터 확인 후 업데이트
``` 

## 상세
1. K8s 인증이 동작 하는 방식.
   1. WIP
   2. 
2. USER
3. AWS eks get-token

## 참고

- 관련 Git Issue 
  - [aws eks update-kubeconfig invalid apiVersion #6920](https://github.com/aws/aws-cli/issues/6920)
  - [AWS eks get-token Docs](https://docs.aws.amazon.com/cli/latest/reference/eks/get-token.html#options)

