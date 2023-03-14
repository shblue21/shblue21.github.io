---
title: Amazon linux 2022 RC 출시
date: 2023-02-23T00:00:00
last_modified_at: 2023-03-13T00:00:00
categories:
  - aws
tags:
  - AWS
  - Amazon Linux
toc: true
lang: ko
---
> 3월 2일에 AL2022 -> AL2023으로 변경되었습니다. 릴리즈 주기도 2023년부터 2년마다로 변경되었습니다.

AWS 컴퓨팅 서비스를 사용하는 경우, AWS에 최적화된 Amazon Linux를 사용하고 있을 텐데요.  
2021년부터 Amazon Linux 2022개발이 시작되어, 현재 RC3.1까지 출시되어 곧 GA가 출시될 예정입니다.

**지원 정책**, **기반 OS** 등 많은 변경 사항이 있는데, 이번 포스팅에서는 Amazon Linux 2022의 주요 변경 사항을 소개합니다.

## 주요 비교 사항

가장 큰 차이점은, Base OS 변경 및, SELinux 추가, 지원정책 상세화 등이 있습니다.

| 변경 사항 | Amazon Linux 2 | Amazon Linux 2022 |
| 배포일 | 2018년 06월 | 2023년 내(예정) |
| EOL | 2025년 06월 30일 | 2026년 |
| 지원 정책 | 2년 | 5년 |
| 커널버전 | 5.10 | 5.15 ~ |
| 기반 OS | Centos를 포함한 여러 Upstream | Fedora |
| gcc | 7.3 | 11.2 |
| SELinux | X | O |
| Package Manager | Yum | DNF |

## 지원 정책 및 버전관리

기존 Amazon Linux는 EOL 시점이 다가오면서 2년간 지원이 연장되는등 명확한 정책을 가지고 있지 않았습니다.(AL1은 2023년 6월로 연장, AL2는 2025년 6월로 연장)
그러나 Amazon Linux 2022를 발표하면서, 2년마다 메이저 업데이트를 발표하고, 각 메이저 업데이트는 5년간 지원될 것이라고 발표했습니다. 

공식 Guide 문서에 잘 정리되어 있습니다.  
![AL2022 Support](/img/230223_al2022_1.png){: width="70%" height="70%"}  
 
위의 표의 Standard Support는 마이너 업데이트를 포함한 지원을 의미하고, Maintenance Support는 보안 업데이트, 중요 버그 픽스 등을 포함한 지원을 의미합니다.  


## SELinux ?
AL2022에서는 SELinux가 기본적으로 활성화되어 있습니다.  
Security-Enhanced Linux의 약자로, Linux 커널의 보안 기능을 확장한 것으로 기존의 Linux 커널의 보안 기능을 보완하여, 보안을 강화하는 기능입니다.  
Context 기반의 접근 제어를 통해, 보안을 강화하고, 보안 위반에 대한 로그를 남기는 등의 기능을 제공합니다.


## In-Place Upgrade ?
Amazon Linux 2 -> Amazon Linux 2022로의 In-Place Upgrade는 지원하지 않을 가능성이 높습니다.  

1. Amazon Linux 2022는 Fedora를 기반으로 하고 있기 때문에, Amazon Linux 2와 호환성이 떨어질 가능성이 높습니다.
2. Amazon Linux 1 -> Amazon Linux 2로의 In-Place Upgrade는 지원하지 않았기 때문에, Amazon Linux 2 -> Amazon Linux 2022로의 In-Place Upgrade도 지원하지 않을 가능성이 높습니다.

Amazon Linux 1 에서 2로 업그레이드를 도와줄 수 있는 도구는 제공했었지만 [URL](https://github.com/amazonlinux/upgrade-modules), 
그마저도 도움이 되진 않았고 In-place 업그레이드는 지원하지 않았습니다.  

--------
2022년 07월부터 RC 배포가 시작되어 릴리즈를 목전에 두고 있기 때문에, 곧 AWS 인프라를 새로 프로비저닝해야 하는 경우라면 GA까지 기다리는 것이 좋을 것 같습니다.
