---
title: Kafka Connector 개발 (1/3)
date: 2022-06-04T11:20:02
last_modified_at: 2022-06-04T11:20:02
categories:
  - Devlog
tags:
  - Kafka
published: false
  
---

# 개요

Kafka Connector 개발 하는 법에 대해 가이드 합니다.

## 목차

WIP

## 배경

최근 Kafka는 ETL을 넘어, Data Lake로도 주목받고 있다.

작년에 업무상, IoT 플랫폼 개발을 하였고 (정확히는 PLC)

Sensor 데이터 특성상 실시간-스트리밍 데이터 처리가 필요하여, (온도, 진동, 습도, 위치, 시간 등)

데이터 처리 Pipeline으로 Kafka를 사용하였다.

운영자 관점에서 설정과 Troubleshooting Guide는 Docs는 많이 존재하는데,

사내 전용 프로토콜과 Kafka와 쉽게 연동하기 위한

Kafka Connector 개발 방법을 가이드하고자 한다.

##