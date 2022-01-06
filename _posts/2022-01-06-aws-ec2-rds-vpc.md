---
layout: post
title: "[AWS] AWS VPC를 이용한 EC2 및 RDS 관리"
excerpt: "EC2와 RDS를 같은 VPC에서 관리하는 방법을 알아본다."
categories: [AWS]
comments: true
---

서비스를 운영하는데 가장 보안이 중요한 영역은 아마 데이터베이스가 될 것이다. 때문에 우리는 데이터베이스를 private 상태에서 관리할 필요가 있다. 이러한 상황에서 사용해 볼 수 있는 방법이 VPC를 이용한 방법이다. 


## <span style="color:#0f7b6c">1. VPC란?</span>

먼저 VPC가 무엇인지 간단하게 알아볼 필요가 있을 것 같다. 아마존에서 소개하는 VPC는 "Amazon Virtual Private Cloud(Amazon VPC)를 이용하면 사용자가 정의한 가상 네트워크로 AWS 리소스를 시작할 수 있습니다. 이 가상 네트워크는 의 확장 가능한 인프라를 사용한다는 이점과 함께 고객의 자체 데이터 센터에서 운영하는 기존 네트워크와 매우 유사합니다"이다. (출처: [Amazon VPC란 무엇인가?](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/what-is-amazon-vpc.html))

이를 간단하게 정의하면 VPC는 AWS에서 제공하는 가상 네트워크이며, 우리가 주로 사용하는 EC2, RDS 등은 VPC라는 가상 네트워크 내부에서 동작한다.
    
#### 1-1. VPC의 종류

**# Defalut VPC**

- 계정 생성시 각 리전에 자동으로 셋업
- 모든 서브넷의 인터넷 접근이 가능
- EC2가 Public IP와 Private IP 모두를 가지고 있음
- 삭제시 복구 불가

**# Custom VPC**

- 직접 만들어야 함
- Default VPC의 특징을 가지고 있지 않음

#### 1-2. IP 주소를 묶는 방법, CIDR (Classless Inter Domain Routing)

IPv4의 경우 총 32비트의 숫자로 구성되며 이는 4,294,967,296개에 해당한다. 여기서 이미 588,514,304개는 특정한 목적을 위한 IP로 선정되어있다. 그 때문에 가용가능한 IP는 3,706,452,992개 (4,294,967,296 - 588,514,304)가 된다. 하지만 오늘날 37억개라는 IP는 전세계 사람들이 사용하는데 턱없이 부족하다.

![IPv4]({{site.url}}/img/AWS/ipv4-address.png){: width="600"}

이런 문제를 해결하기 위한 방법이 바로 **Private Network**이다.

**# Private Network**

- 하나의 Public IP를 여러 기기가 공유할 수 있는 방법
- 하나의 망에는 Private IP를 부여받은 기기들과 Gateway로 구성 (각 기기는 인터넷과 통신시 Gateway를 통해 통신)
- Private IP는 지정된 대역의 IP만 사용 가능

흔히 가정에서 사용하는 공유기도 하나의 Public IP와 Private Network를 갖는다. 

![private network]({{site.url}}/img/AWS/private-network.png){: width="600"}

위 사진을 보면 Public IP(=외부 IP 주소)는 210.222.211.4이고, Private Network(=내부 IP 주소)는 192.168.0.2 ~ 192.168.0.254의 IP를 할당받는 것을 알 수 있다. 이런 할당 범위를 192.168.0.0/24로 표현할 수도 있는데 이러한 표현 방식이 CIDR이다. 숫자 24가 의미하는 값은 전체 IPv4의 32비트에서 24비트를 네트워크 라우팅을 위한 주소로 사용하고, 나머지 8비트를 호스트의 IP주소로 사용하겠다는 의미다. 예를 들어 192.168.0.0/16은 192.168.0.0 ~ 192.168.255.255와 같은 표기방법이다.

#### 1-3. Subnet

Subnet은 VPC의 하위 단위로서 Public Subnet과 Private Subnet으로 나뉜다. 이를 통해 데이터베이스와 같은 서비스는 Private Subnet에 위치시키고, 서버와 같은 서비스는 Public Subnet에 위치시킨다. 

- 하나의 AZ에만 생성이 가능하다. (= 다른 AZ로 확장 불가)
- 하나의 AZ에는 여러 Subnet이 존재할 수 있다.
- 계정 생성시 생성되는 기본 VPC의 경우 각 가용영역에 자동으로 Subnet을 셋업
- CIDR을 통해 Private IP 범위를 할당받는다.

지금까지의 내용을 그림으로 나타내면 다음과 같다.

![AWS VPC concept 1]({{site.url}}/img/AWS/aws-vpc-concept-1.png){: width="600"}

(그림에서는 AZ를 고려하지 않았다.)

#### 1-4. SG와 NACL

SG(Security Group)와 NACL(Network Access Control List) 모두 AWS에서 방화벽(검문소) 역할을 하는 보안장치다. 각각의 차이는 적용 대상과 방식에 있다. 

**# SG**

SG는 인스턴스에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽 역할을 한다. VPC에서 인스턴스를 시작할 때 최대 5개의 SG를 인스턴스에 할당할 수 있으며, SG는 서브넷 수준이 아니라 인스턴스 수준에서 작동하므로 VPC에 있는 서브넷의 각 인스턴스를 서로 다른 SG에 할당할 수 있다. 

- 트래픽이 지나갈 수 있는 Port와 Source를 설정 가능
- Deny는 불가능 -> NACL로 가능함
- 설정된 모든 룰을 사용해 필터링 (NACL의 경우 룰의 순서대로 필터링)
- Stateful (인바운드로 들어온 트래픽은 별다른 아웃바운드 설정이 필요없이 나갈 수 있다.)

![AWS sg]({{site.url}}/img/AWS/aws-sg.png){: width="600"}

위 사진을 보면 EC2 인스턴스에 인바운드 규칙이 할당되었는데, 80번 포트를 사용하는 HTTP 프로토콜의 경우 모든 접근을 허용하고, SSH 접근의 경우 특정 IP만 접근이 허용된 것을 볼 수 있다. 여기서 핵심은 SG는 인스턴스 단위로 적용된다는 것이다.

**# NACL**

NACL은 서브넷에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽 역할을 한다. 

- 규칙 번호가 낮은것부터 우선 적용
- Allow, Deny 규칙 모두 적용가능
- Stateless (인바운드로 들어온 트래픽에 반환 트래픽이 별도로 허용되어야 한다.)

