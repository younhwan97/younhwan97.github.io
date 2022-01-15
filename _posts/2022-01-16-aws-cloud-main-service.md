---
layout: post
title: "[AWS] AWS 클라우드 핵심 서비스"
excerpt: "AWS 클라우드 핵심 서비스"
categories: [AWS]
comments: true
---

## 목차

- [1. AWS 컴퓨팅](#1-aws-컴퓨팅)
    + [1-1. AWS EC2 Service](#1-1-aws-ec2-service)
        - [1-1-1. EC2 Instance 생성](#1-1-1-ec2-instance-생성)
        - [1-1-2. EC2 비용 최적화](#1-1-2-ec2-비용-최적화)
    + [1-2. AWS Container Service](#1-2-aws-container-service)
        - [1-2-1. Amazon Elastic Container Service](#1-2-1-amazon-elastic-container-service)
    + [1-3. AWS Lambda](#1-3-aws-lambda)
        - [1-3-1. AWS Lambda 이벤트 소스](#1-3-1-aws-lambda-이벤트-소스)
    + [1-4. AWS Elastic Beanstalk](#1-4-aws-elastic-beanstalk)
- [2. AWS 스토리지](#2-aws-스토리지)
- [3. AWS 데이터베이스](#3-aws-데이터베이스)
- [4. AWS 네트워크](#4-aws-네트워크)



## <span style="color:#0f7b6c">1. AWS 컴퓨팅</span>

![aws main service : computing]({{site.url}}/img/AWS/aws-main-service-computing.png){:width="800"}

- 최적의 컴퓨팅 서비스 또는 사용하는 서비스들은 사용 사례에 따라 달라진다.
- 고례해야 할 몇가지 사항:
    + 애플리케이션 설계
    + 사용 패턴
    + 관리할 구성 설정
- 아키텍처에 적합하지 않은 컴퓨팅 솔루션을 선택할 경우 성능 효율이 저하될 수 있음

#### 1-1. AWS EC2 Service

AWS를 대표하는 상품 중 하나이고, 가장 범용적인 서비스이다. EC는 클라우드 서비스 모델 중에서 IaaS에 해당하고, 독립된 컴퓨터 1대를 통째로 임대해주는 서비스로 보면 된다. EC2에는 인스턴스라는 요소가 있는데 인스턴스 1개는 컴퓨터 1대라고 생각하면 된다. (만약 EC2를 통해 3대의 컴퓨터를 임대했으면, 3개의 인스턴스를 사용하고 있는 것)

AWS Management Console을 통해 EC2 > 인스턴스 페이지에 접속하면 현재 임대 중인 컴퓨터의 목록을 볼 수 있다.

![aws computing : ec2]({{site.url}}/img/AWS/aws-main-service-computing-ec2-instance.png){:width="800"}

**# EC2 사용 예**

- 애플리케이션 서버
- 웹 서버
- 게임 서버
- 데이터베이스 서버
- 메일 서버
- 미디어 서버
- 카탈로그 서버
- 파일 서버
- 컴퓨팅 서버
- 프록시 서버

##### 1-1-1. EC2 Instance 생성

EC2 > 인스턴스 페이지에서 우측 상단 **[인스턴스 생성]** 버튼 클릭

![aws create ec2 instance]({{site.url}}/img/AWS/aws-create-ec2-instance-step-1.png){:width="800"}

AMI(Amazon Machine Image)를 선택한다. 생성할 인스턴스에서 사용할 이미지를 선택하면 된다. 여기서 이미지는 운영체제와 소프트웨어가 포함된 템플릿으로 볼 수 있는데 다른 가상머신 서비스(VMware ..)와 달리 아마존은 이런 이미지를 제공함으로써 더 쉽고, 빠르게 컴퓨터를 임대할 수 있다는 장점이 있다. (VMware에서 우분투를 설치하려면 우분투 공식 페이지에서 이미지를 받고 직접 설치하는 등의 과정이 필요했다.)

AWS가 기본으로 제공하는 이미지와 다른 사용자, 기업이 만든 이미지(AWS Marketplace)가 있다. 여기서는 AWS가 제공하는 우분투를 이용해 인스턴스를 생성한다. 

![aws create ec2 instance]({{site.url}}/img/AWS/aws-create-ec2-instance-step-2.png){:width="800"}

인스턴스 유형 선택 단계는 임대할 컴퓨터의 사양을 선택하는 단계다. 비즈니스 분야에 따라 컴퓨터가 중점적으로 필요한 사양이 다를 수 있다. 주로 필요한 사양이 높은(좋은) 유형을 선택하면 된다. (일반적으로 c로 시작하는 유형은 cpu가 동일 가격대비 좋고, m으로 시작하는 유형은 메모리가 좋은 식이다.) 유형 선택 후 **[다음: 인스턴스 세부 정보 구성]**을 클릭한다.

![aws create ec2 instance]({{site.url}}/img/AWS/aws-create-ec2-instance-step-3.png){:width="800"}

인스턴스 세부 정보 구성 페이지에서 만들고자 하는 컴퓨터의 세부 구성을 설정할 수 있다. 설정을 마치고 **[다음: 스토리지 추가]** 버튼을 클릭한다.

![aws create ec2 instance]({{site.url}}/img/AWS/aws-create-ec2-instance-step-4.png){:width="800"}

임대 할 컴퓨터에 저장장치를 장착할 수 있는 페이지다. 여기서 생성한 저장장치는 AWS EBS Service에서 확인할 수 있다. 크기와 유형을 선택하고 **[다음: 태그 추가]** 버튼을 클릭한다.

![aws create ec2 instance]({{site.url}}/img/AWS/aws-create-ec2-instance-step-5.png){:width="800"}

키와 값이 쌍으로 존재하는 태그를 통해 생성할 컴퓨터에 설명을 추가할 수 있다. 태그 값을 입력하고 **[다음: 보안 그룹 구성]** 버튼을 클릭한다.

![aws create ec2 instance]({{site.url}}/img/AWS/aws-create-ec2-instance-step-6.png){:width="800"}

인스턴스에 적용할 보안 그룹을 구성한다. 해당 페이지에서 직접 구성하거나 이미 존재하는 보안 그룹을 적용할 수 있다. 웹 서버로 사용할 경우 HTTP, HTTPS에 대한 프로토콜을 허용하고, SSH 프로토콜의 경우 접속할 IP만 접속 가능하도록 설정하면 된다. 보안 그룹 설정을 마치고 **[검토 및 시작]** 버튼을 클릭해 인스턴스를 생성한다.

![aws create ec2 instance]({{site.url}}/img/AWS/aws-create-ec2-instance-step-7.png){:width="800"}

인스턴스 목록을 확인하면 굉장히 짧은 시간안에 컴퓨터 1대를 임대한 것을 볼 수 있다.

![aws create ec2 instance]({{site.url}}/img/AWS/aws-create-ec2-instance-step-8.png){:width="800"}

##### 1-1-2. EC2 비용 최적화

- 온디맨드 인스턴스: 
    + **[1-1-1. EC2 Instance 생성]**에서 생성한 인스턴스
    + 시간당 요금
    + 장기 계약이 필요 없음
    + 장점: 낮은 비용과 유연성
- 예약 인스턴스:
    + 예약하는 인스턴스에 대한 전체 결제, 부분 결제 또는 선 결제 요금이 없는 결제 방식을 제공하는 인스턴스
    + 해당 인스턴스를 정해진 기간만큼 사용 
    + 1년 또는 3년 약정
    + 장점: 필요할 때 컴퓨팅 파워를 이용할 수 있도록 보장하는 예측 가능성
- 스팟 인스턴스:
    + 사용 가능한 상태이고 입찰 가격이 스팟 인스턴스 가격보다 높으면 인스턴스가 실행됨
    + AWS는 2분 전에 알림으로써 스팟 인스턴스를 종료할 수 있음
    + 중단 옵션에는 종료, 중지 또는 최대 절전 모드가 포함
    + 온디맨드 인스턴스에 비해 훨씬 요금이 저렴할 수 있음
    + 애플리케이션을 실행할 수 있는 시점을 유연하게 선택할 수 있는 경우에 적합
    + 장점: 대규모 동적 워크로드
- 전용 호스트:
    + 고객을 위해 EC2 인스턴스 용량을 완전히 전용으로 사용하는 물리적 서버
    + 장점: 라이선싱 비용 절감, 규정 및 규제 요건 충족
- 전용 인스턴스:
    + 단일 고객을 위한 전용 하드웨어의 VPC에서 실행되는 인스턴스
- 정기 예약 인스턴스:
    + 사용자가 지정한 반복 일정에 따라 항상 사용할 수 있는 예약 용량을 구매

위와 같은 다양한 유형의 인스턴스에서 상황에 맞는 유형의 인스턴스를 선택해 EC2 사용 비용을 최적화 할 수 있다.

**# Amazon CloudWatch 서비스 사용**

Amazon CloudWatch 서비스를 통해 특정 인스턴스의 유휴 상태 정도, 유휴 상태가 되는 시점을 파악할 수 있다. 이를 통해 인스턴스의 사양(CPU 개수, 메모리 크기 ...)을 적절히 변경할 수 있게 된다. 만약 CloudWatch를 통해 관찰한 사용량 패턴이 일관적이라면 인스턴스 유형 자체를 예약 인스턴스로 함으로써 서비스 사용 비용을 최적화 할 수도 있을 것이다.

#### 1-2. AWS Container Service

컨테이너는 소프트웨어를 실행하기 위한 모든 항목(라이브러리, 코드, 런타임 ...)을 포함하여 이미지를 만드는 방법이다. 이미지를 만든다는 측면에서 앞서 EC2 생성 과정에서 사용한 AMI와 비슷해 보일 수 있지만 다른 개념이다. 컨테이너는 운영체제를 전체를 포함하는 이미지가 아니다. 

**# 컨테이너와 가상 머신 차이**

![aws-container-and-vm-difference]({{site.url}}/img/AWS/aws-container-and-vm-difference.png){:width="800"}

사진 왼쪽의 경우 하나의 EC2에 3개의 컨테이너가 존재한다. 컨테이너의 경우 왼쪽 사진과 같이 하나의 OS를 공유하고, 각각의 프로세스는 독립된다. 여기서 Docker는 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는 컨테이너 소프트웨어 플랫폼이다. Docker에서 컨테이너가 실행된다. 

사진 오른쪽의 경우 AWS Global Infra(Physical Server ~ Hypervisor) 위에 3대의 EC2가 동작 중이라고 볼 수 있다. 

컨테이너는 운영체제를 포함하는 개념이 아니며 소프트웨어를 실행하기 위한 항목의 집합(라이브러리, 코드, 런타임 ...) 정도로 볼 수 있고, 어떤 환경에서든 쉽게 설치하고 독립적인 실행을 보장받는다.

**# 컨테이너 사용 이점**

- 반복 가능
- 독립형 실행 환경
- 소프트웨어가 서로 다른 환경에서 동일하게 실행됨:
    + 개발자의 노트북, 테스트, 프로덕션
- 가상 머신보다 빠르게 시작이나 중지 또는 종료 할 수 있음

##### 1-2-1. Amazon Elastic Container Service

Amazon Elastic Container Service는 확장성과 속도가 뛰어난 컨테이너 관리 서비스이다.

**# 이점**

- Docker 컨테이너의 실행을 오케스트레이션
- 컨테이너를 실행하는 노드 플릿 유지 관리 및 확장
- 인프라 구축의 복잡성 해소

**# Amazon EC2 서비스 사용자에게 익숙한 기능과 통합**

- Elastic Load Balancing
- Amazon EC2 보안 그룹
- Amazon EBS 볼륨
- IAM

#### 1-3. AWS Lambda

AWS Lambda는 서버리스 컴퓨팅 서비스이다. Lambda는 클라우드 서비스 모델 중에서 PaaS에 해당하고, 서버 프로비저닝 또는 (인프라) 관리가 불필요한 서비스를 제공한다. 

Lambda 서비스의 핵심은 **이벤트**와 **일정**다. AWS 사용자가 Lambda 서비스에 코드를 올리고 특정 이벤트가 발생하거나 정해진 일정에 따라 코드가 실행된다. 그리고 사용자는 컴퓨팅 시간에 대해서만 비용을 지불하게 된다. 코드가 실행되지 않을 때는 요금이 부과되지 않는다. (이벤트 중심 서버리스 컴퓨팅 서비스)

**# 이점**

- 여러 프로그래밍 언어 지원
- 완전히 자동화된 관리 (PaaS의 특성)
- 내결함성 기본 제공 (PaaS의 특성)
- 여러 함수의 오케스트레이션 지원 (AWS Step Functions)
- 사용량에 따라 요금 지불

##### 1-3-1. AWS Lambda 이벤트 소스

![aws-lambda-event-source]({{site.url}}/img/AWS/aws-lambda-event-source.png){:width="800"}

개발자 생성 애플리케이션과 다양한 서비스에서 Lambda 함수의 실행을 트리거하는 이벤트를 생성할 수 있다.

Amazon S3및 Amazon CloudWatch Events와 같은 일부 서비스는 Lambda 함수를 직접 호출하여 이벤트를 개시할 수 있다. 다른 서비스의 경우 Lambda가 가져온다. 예를 들면 Lambda는 Amazon SQS에서 레코드를 가져올 수 있고, Amazon DynamoDB에서 직접 이벤트를 읽을 수 있다. 

#### 1-4. AWS Elastic Beanstalk

EC2를 이용해 웹 애플리케이션을 서비스하면 개발자의 자유도가 높다. 하지만 자유도가 높은 만큼 보안을 비롯한 많은 관리가 필요하다. 웹 애플리케이션이 동작할 수 있도록 PHP, Node, Java와 같은 플랫폼을 설치해야 하고, 플랫폼을 정기적으로 업그레이드 해줘야하며 서비스 운영중에 문제가 생기면 신속하게 과거 버전으로 백업도 필요하다. AWS Elastic Beanstalk은 이런 번거러운 일을 모두 맡기고, 개발자는 코드 작성만 신경쓰면 되도록 AWS가 제공하는 PaaS 형태의 서비스다. 

## <span style="color:#0f7b6c">2. AWS 스토리지</span>

## <span style="color:#0f7b6c">3. AWS 데이터베이스</span>

## <span style="color:#0f7b6c">4. AWS 네트워크</span>