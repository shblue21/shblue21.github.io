---
title: AWS EKS에 Confluent(Kafka) 구성 - 기본
date: 2022-07-31T11:20:02
last_modified_at: 2022-07-31T11:20:02
categories:
  - aws
tags:
  - AWS
  - EKS
  - Confluent
  - Kafka
toc: true  
lang: ko
---

## 개요

Confluent에서 제공하는 Helm Chart를 이용하여, 간단한 구성으로  
AWS EKS(Elastic Kubernetes Service)에 Confluent Platform을 구성 및 배포하는 방법을 가이드합니다.

## 사전 준비

- AWS CLI(Optional)
- kubectl
- helm
- eksctl(Optional)

## 구성

Confluent 사용시 SaaS가 아닌, Cloud 환경에 직접 구성 및 관리를 원하는 경우,  
Managed Kubernetes Service를 이용하여, K8s 사용시의 이점을 가져 갈 수 있습니다.  

AWS의 Managed Kubernetes Service(EKS) 이용하여 Confluent를 구성하는 방법을 하기에 설명합니다.
본가이드는 데모 및 PoC용 입니다. (Spot 인스턴스 사용)  
EKS 구성은 콘솔로 진행하며, eksctl로 빠르게 구성 할 수도 있습니다.


### 0. 사전 준비

1. AWS CLI(Optional)  
  EKS용 kubeconfig를 생성하기 위해 필요하며, AWS CLI 설치는 유관 문서가 많으므로 참고.
2. kubectl(1.22)
   Confluent는 Kubernetes 1.21 - 1.24 버전을 지원하며, EKS는 1.19 - 1.22 버전을 지원하므로, 1.22를 설치  
   (22.07.31 기준)
3. helm  
   Confluent에서 제공하는 Helm차트로 구성하기 위해서 필요.
4. eksctl(Optional)  
   EKS 클러스터, 노드등을 빠르게 구성할 수 있게 해줌.

### 1. EKS 구성(콘솔)
| EKS를 간략히 구성하며, EKS에 대한 설명은 최대한 생략함.

1. Cluster 구성(Control Plane)  
   Control Plane 및 네트워크등 EKS에 대한 전반적인 구성,
   
   Cluster 이름과, K8s 버전을 선택
   ![EKS_1](/img/220731_EKS_1.png)  

   EKS의 네트워크 구성 설정(Control Plane에 대해 Bastion 등 통하지 않고, Public으로 구성 예제)  
   ![EKS_2](/img/220731_EKS_2.png)  

2. Worker Node 구성(Date Plane)  
   DatePlane에 해당하는 실제 Pods를 구성할 워커노드를 구성.  
   Kubernetes Pods로 실행되는 컨테이너에 대해 EC2와 Fargate를 사용할 수 있음. 본 가이드 에선, EC2로 진행

   Worker Node 그룹 이름과, IAM Role 선택
   ![EKS_3](/img/220731_EKS_3.png)  
   
   관리형 노드 그룹의 이미지 및 컴퓨팅 타입, 최소 최대 Nodes수 등 결정.
   ![EKS_4](/img/220731_EKS_4.png)  

3. EKS용 kubeconfig 생성  
EKS 클러스터에 대한 kubeconfig 구성,
   ```bash
   aws eks update-kubeconfig --region ap-northeast-2 --name confluent-demo ## 
   kubectl get nodes ## EKS nodes 확인
   ```
   ```

### 2. Helm Chart 로 Confluent Platform 구성
WIP

### 3. Helm Chart 살펴보기
WIP

### 4. 테스트
WIP

### * 참고
- WIP