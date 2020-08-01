---
layout: post
title: AWS EKS 클러스터 만들기
categories: [aws]
excerpt: ""
comments: true
share: true
tags: [aws, eks, kubernetes]
date: 2020-08-01
---
2020년 하반기에 **[DDD](https://www.facebook.com/dddstudy/)** 4기에 참가하여 서버개발 직군으로 프로젝트를 진행하며 
진행할 프로젝트의 서버환경 구축이 필요했고, AWS + 쿠버네티스로 배포 프로세스를 만들어보았습니다.


## 클러스터 만들기

1. EKS 시작하기
쿠버네티스 클러스터를 생성하는데 필요한 Amazon EKS를 시작하기 위해 eksctl을 설치합니다.
    - AWS CLI 설치
        ``` bash
        $ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"

        $ sudo installer -pkg AWSCLIV2.pkg -target /

        # 버전 확인
        $ aws --version
        ```
    - AWS CLI 자격증명 구성 (계정에서 액세스 키 확인)
        ``` bash
        $ aws configure
        AWS Access Key ID [****************MPLE]:
        AWS Secret Access Key [****************EKEY]:
        Default region name [ap-northeast-2]:
        Default output format [json]:
        ```
    - eksctl 설치
        ``` bash
        $ brew tap weaveworks/tap

        $ brew install weaveworks/tap/eksctl

        # 버전 확인
        $ eksctl version
        ```
    - kubectl 설치
    Kubernetes를 사용하여 클러스터 API 서버와 통신하기 위한 kubectl을 설치합니다.
        ``` bash
        # kubectl 바이너리 다운로드
        $ curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.17.7/2020-07-08/bin/darwin/amd64/kubectl    

        $ chmod +x ./kubectl

        $ sudo mv ./kubectl /usr/local/bin

        # 버전 확인
        $ kubectl version --short --client
        ```

2. AWS 리전에서 AWS STS 관리
    글로벌 endpoint 대신 지리적으로 가장 가까운 엔드포인트를 이용하기 위해 AWS STS(Security Token Service)를 관리해야 합니다.
    **[관련 가이드](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html#sts-regions-activate-deactivate)**

    - IAM -> 계정 설정에서 클러스터가 있는 리전의 STS 엔드포인트 활성화 여부 확인

3. EKS 클러스터 생성
    - 클러스터 생성 : AWS EKS가 포함된 Fargate는 사용 리전이 제한적이므로 [확인](https://docs.aws.amazon.com/eks/latest/userguide/fargate.html) 필요
        ``` bash
        $ eksctl create cluster \
        --name ddd-spark \
        --version 1.17 \
        --region ap-northeast-2 \
        --fargate
        # 쿠버네티스 서비스 확인
        $ kubectl get svc
        ```

