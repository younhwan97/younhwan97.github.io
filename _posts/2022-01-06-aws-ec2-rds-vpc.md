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

## <span style="color:#0f7b6c">2. VPC를 이용한 EC2 및 RDS 관리</span>

**# 목표**

![AWS sg]({{site.url}}/img/AWS/aws-ec2-rds-vpc.png){: width="600"}

#### 2-1. VPC 생성

우선 게이트웨이에 할당할 탄력적 IP주소가 필요하다. 우측 상단 **[탄력적 IP주소 할당]** 버튼을 클릭해 탄력적 IP를 할당받는다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-1.png){: height="500" width="800"}

VPC를 생성하기 위해 AWS에서 VPC 서비스 페이지에 접속한 뒤, 우측 상단 **[VPC 마법사 시작]** 버튼을 클릭한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-2.png){: height="500" width="800"}

웹 서버를 위치시킬 퍼블릭 서브넷과 데이터베이스를 위치시킬 프라이빗 서브넷이 모두 필요하다. VPC 구성에서 **[퍼블릭 및 프라이빗 서브넷이 있는 VPC]**를 선택한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-3.png){: height="500" width="800"}

**[탄력적 IP 할당]**에서 첫 번째 단계에서 할당받은 탄력적 IP의 ID를 선택하고, VPC를 생성한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-4.png){: height="500" width="800"}

VPC가 생성되면 퍼블릭, 프라이빗 서브넷도 각각 1개씩 생성된 것을 볼 수 있다. 하지만 DB 인스턴스를 VPC에서 사용하기 위한 DB 서브넷 그룹을 만들려면 두 개의 프라이빗 서브넷 또는 두 개의 퍼블릭 서브넷이 있어야한다. 우측 상단 **[서브넷 생성]** 버튼을 클릭한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-5.png){: height="500" width="800"}

서브넷을 생성할 때는 이전 단계에서 생성한 VPC를 선택한다. 

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-6.png){: height="500" width="800"}

가용영역의 경우 첫 번째 프라이빗 서브넷에서 선택한 가용 영역과 다른 영역을 선택하고, **[서브넷 생성]** 버튼을 클릭해 서브넷을 생성한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-7.png){: height="500" width="800"}

성공적으로 두 번째 프라이빗 서브넷이 생성된 것을 확인할 수 있다. 이제 이 두 프라이빗 서브넷이 동일한 라우팅 테이블을 갖는지 확인한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-8.png){: height="500" width="800"}

이제 퍼블릭 엑세스를 위한 보안그룹을 생성할 차례다. VPC의 퍼블릭 인스턴스에 연결하려면 인터넷으로부터의 트래픽 연결을 허용하는 VPC 보안 그룹에 인바운드 규칙을 추가해야한다. **[보안 그룹 생성]** 버튼을 클릭

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-9.png){: height="500" width="800"}

앞에서 생성한 VPC를 선택한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-10.png){: height="500" width="800"}

인바운드 규칙을 추가한다. HTTP, HTTPS는 누구나 접속할 수 있도록 설정하고, SSH의 경우 내 IP만 접근할 수 있도록 설정했다. **[보안 그룹 생성]** 버튼을 클릭해 보안 그룹을 생성한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-11.png){: height="500" width="800"}

외부에서 퍼블릭 서브넷에 위치한 EC2 접근 보안 그룹을 만들었다면, 이제 프라이빗 서브넷에 위차한 RDS 접근을 위한 보안 그룹을 생성할 차례다. 

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-12.png){: height="500" width="800"}

인바운드 규칙에서 접근 가능한 소스를 앞서 생성한 보안그룹만 선택하고, **[보안 그룹 생성]** 버튼을 클릭해 보안 그룹을 생성한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-13.png){: height="500" width="800"}

AWS RDS 서비스 페이지로 이동해 서브넷 그룹을 생성할 차례다. **[DB 서브넷 그룹 생성]** 버튼을 클릭한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-14.png){: height="500" width="800"}

VPC를 선택하고, 서브넷 추가 섹션에서 가용 영역 및 서브넷을 선택한다. 앞서 서브넷을 'ap-northeast-2a'와 'ap-northeast-2b'에서 생성했으므로 가용 영역으로 'ap-northeast-2a'와 'ap-northeast-2b'를 선택하고 **[서브넷]**에서 IPv4 CIDR 블록 10.0.1.0/24 및 10.0.2.0/24용 서브넷을 선택했다. 그리고 **[생성]** 버튼을 클릭해 서브넷 그룹을 생성한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-15.png){: height="500" width="800"}

이제 생성한 VPC, 서브넷 그리고 보안 그룹을 이용해 EC2와 RDS를 생성할 차례다. EC2를 생성하기위해 AWS EC2 서비스 페이지에서 **[인스턴스 시작]** 버튼을 클릭한다. 이어서 AMI, 인스턴스 타입을 선택한다. 인스턴스 세부 구성 페이지에서 **[네트워크]**는 앞서 생성한 vpc를 선택하고, **[서브넷]**은 퍼블릭 서브넷을 선택한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-16.png){: height="500" width="800"}

보안 그룹 설정에서 앞서 생성한 EC2 퍼블릭 엑세스 보안 그룹을 선택하고 **[검토 및 시작]** 버튼을 클릭해 EC2 인스턴스를 생성한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-17.png){: height="500" width="800"}

이어서 데이터베이스를 생성하기 위해 AWS RDS 서비스 페이지로 이동해 **[데이터베이스 생성]** 버튼을 클릭한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-18.png){: height="500" width="800"}

이전에 생성한 VPC를 선택하고, 서브넷 그룹과 보안 그룹을 선택한다. 이후 나머지 설정을 마치고 **[데이터베이스 생성]** 버튼을 클릭하여 데이터베이스를 생성하면, 프라이빗 서브넷에 RDS, 퍼블릭 서브넷에 EC2가 존재하는 환경을 만들 수 있다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-19.png){: height="500" width="800"}

※ VPC 생성 과정은 [자습서: DB 인스턴스에 사용할 Amazon VPC 생성](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_Tutorials.WebServerDB.CreateVPC.html)을 참고하여 작성했습니다. 

#### 2-2. Private Subnet

앞서 **[2-1]**에서 EC2를 Public Subnet, RDS를 Private Subnet에 위치시키기 위하여 각각의 서브넷을 생성했다. 하지만 서브넷 생성과정에서는 퍼블릭/프라이빗을 전혀 선택하거나 하지는 않았다. 단지 프라이빗 서브넷으로 사용하고자하는 서브넷에 이름을 Private로 지정했다. 

**# 그렇다면 프라이빗과 퍼블릭 서브넷은 어떤 차이가 있는걸까?**

정답은 라우팅 테이블에 있다. 라우팅 테이블이란 각각의 서브넷이 가지고 있는 테이블(표)이다. 그리고 해당 테이블은 IP주소에 트래픽 라우팅 경로를 정의하여 서브넷 안팎으로 나가는 트래픽에 대한 라우팅 경로 설정 기능을 수행하며, 라우팅 테이블에 따라 퍼블릭 서브넷의 경우 Internet Gateway와 연결된다. 다시말해 Internet Gateway에 연결된 서브넷은 퍼블릭, 그렇지 않다면 프라이빗 서브넷이라는 개념이다.

다음 사진의 서브넷은 Defalut VPC와 함께 생성 기본으로 생성되는 서브넷이다. 그리고 사진 하단 라우팅 테이블을 보면 0.0.0.0/0 대상은 igw-xx라는 Internet Gateway와 연결된 것을 볼 수 있다. 때문에 VPC 생성 시 기본으로 생성되는 서브넷은 모두 퍼블릭 서브넷인 것을 확인할 수 있다. 직접 서브넷을 생성하면 어떨지 확인해보기 위해 우측 상단 **[서브넷 생성]** 버튼을 클릭한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-20.png){: height="500" width="800"}

**[VPC ID]**에서 기본 VPC를 선택한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-21.png){: height="500" width="800"}

서브넷 이름, 가용 영역 및 CIRD 블록에 대한 값을 입력하고 **[서브넷 생성]** 버튼을 클릭하여 서브넷을 생성한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-22.png){: height="500" width="800"}

생성된 서브넷을 확인하면, Internet Gateway가 연결된 퍼블릭 서브넷이 생성된 것을 확인할 수 있다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-23.png){: height="500" width="800"}

Custom VPC에서 생성한 서브넷은 어떨지 확인하기 위해 **[서브넷 생성]** 버튼을 클릭하고, VPC ID를 선택한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-24.png){: height="500" width="800"}

서브넷 이름, 가용 영역 및 CIRD 블록에 대한 값을 입력하고 **[서브넷 생성]** 버튼을 클릭하여 서브넷을 생성한다.

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-25.png){: height="500" width="800"}

생성된 서브넷을 확인하면, Internet Gateway가 연결된 퍼블릭 서브넷이 아니라 NAT Gateway가 연될된 서브넷이 생성된 것을 확인할 수 있다. 여기서 NAT Gateway란 네크워크 주소 변환 서비스로서 프라이빗 서브넷의 인스턴스가 VPC 외부의 서비스에 연결할 수 있지만 외부 서비스에서 이러한 인스턴스와의 연결을 시작할 수 없도록 NAT 게이트웨이를 사용한다. 즉 NAT Gateway가 연결된 프라이빗 서브넷이 생성된 것이다. 

![AWS create vpc]({{site.url}}/img/AWS/aws-create-vpc-step-26.png){: height="500" width="800"}

위 과정을 통해 Default VPC에서 생성한 서브넷의 경우 기본적으로 Internet Gateway가 연결된 퍼블릭 서브넷이라는 것을 확인할 수 있었고, Custom VPC에서 생성한 프라이빗 서브넷의 경우 NAT Gateway와 연결된다는 사실을 확인할 수 있었다.