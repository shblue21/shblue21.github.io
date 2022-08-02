---
title: eks에서 exec plugin is configured to use API version 이슈
date: 2022-08-02T00:20:02
last_modified_at: 2022-08-02T00:20:02
categories:
  - aws
tags:
  - AWS
  - EKS
  - Kubernetes
  - kubectl
toc: true  
---

## 이슈
eks cluster에 대해 kubectl commands 실행시,  

 **'Unable to connect to the server: getting credentials: exec plugin is configured to use API version client.authentication.k8s.io/v1beta1, plugin returned version client.authentication.k8s.io/v1alpha1'**  

혹은 **'Kubeconfig user entry is using deprecated API version client.authentication.k8s.io/v1alpha1. Run 'aws eks update-kubeconfig' to update.**' 메세지 발생

## 해결방법 요약
1. AWS Cli 업데이트(최소 1.24.0)  
2. .kube/config 의 사용자/인증의 apiVersion vaule 값 변경 **client.authentication.k8s.io/v1beta1**
![EKS_1](/img/220802_eksissue_1.png)   
혹은 aws cli로 config 업데이트.  

``` bash
aws eks update-kubeconfig # 기존 config 및 클러스터 확인 후 업데이트
``` 

## 상세
WIP

## 참고

- 관련 Git Issue 
  - [aws eks update-kubeconfig invalid apiVersion #6920](https://github.com/aws/aws-cli/issues/6920)
  - 

