---
layout: post
title: "[Database] 관계형 데이터 모델링"
excerpt: "관계형 데이터 모델링에 관해 알아본다."
categories: [Database]
comments: true
---

생활코딩이라는 유튜브 채널을 통해 공부하고 기록한 글입니다. <br>
출처: [생활코딩, 관계형 데이터 모델링](https://youtu.be/1d38YZKCM88)

## <span style="color:#0f7b6c">1. 관계형 데이터 모델링이란?</span>

**# 모델링이란 무엇을 의미할까?**

모델링이란 어떤 목적을 가지고 진짜을 모방한 것이다. 그렇기 때문에 좋은 모델이란 목적에 부합하는 모델이 될 것이다.

**# 그렇다면 관계형 데이터 모델링이란 무엇을 의미할까?**

우리의 목적은 현실의 문제를 컴퓨터로 옮기는 것이다. 그 과정에서 현실을 모델링하기 위해 **표**를 사용하는 방법이 관계형 데이터이다.

> 그렇기 때문에 관계형 데이터 모델링이란 표를 사용하여 현실의 복잡성을 컴퓨터로 옮기는 방법이다.

지금부터 복잡한 현실을 컴퓨터로 이사시키는 방법에 관해 알아볼 것이다.

## <span style="color:#0f7b6c">2. 관계형 데이터 모델링의 순서</span>

**1. 업무 파악**

![rdb flow 1]({{site.url}}/img/Database/rdb-flow-1.png){: height="400" width="800"}

우리가 하려고 하는 일을 잘 파악해야 정확한 모델링이 이뤄질 수 있다.

**2. 개념적 데이터 모델링**

![rdb flow 2]({{site.url}}/img/Database/rdb-flow-2.png){: height="400" width="800"}

현실의 업무, 일에는 어떠한 개념이 있고 각각의 개념이 어떻게 상호작용하고 있는지 심사숙고 해보는 과정이다. 개념적 데이터 모델링을 마치면 ER 다이어그램을 얻을 수 있다.

**3. 논리적 데이터 모델링**

![rdb flow 3]({{site.url}}/img/Database/rdb-flow-3.png){: height="400" width="800"}

우리가 생각한 개념을 표로 전환하는 작업이 이뤄진다.

**4. 물리적 데이터 모델링**

![rdb flow 4]({{site.url}}/img/Database/rdb-flow-4.png){: height="400" width="800"}

적합한 데이터베이스 제품을 선택하고, 해당 데이터베이스에서 실제 표를 만들기 위한 코드를 작성하는 과정이다.

## <span style="color:#0f7b6c">3. 업무 파악</span>

**# 인기 앱의 공통점은 뭘까?** 

절대적이지는 않지만, 대부분의 인기 있는 앱의 공통점은 '정확한 니즈 파악과 충족'이라고 생각된다. <br>
사용자가 원하고 필요로 하는 것을 해결해 주는 것이다. 그리고 이러한 앱, 소프트웨어를 만들기 위한 가장 중요한 단계가 업무(=니즈) 파악이다. <br>

관계형 데이터 모델링도 마찬가지다. 현실의 문제를 컴퓨터로 옮기기 전에 해결하고자 하는 문제가 무엇인지 정확히 파악해야 올바른 데이터 모델링이 이뤄질 수 있다.

## <span style="color:#0f7b6c">4. 개념적 데이터 모델링</span>

개념적 데이터 모델링은 일에 어떤 개념이 있고 각각의 개념이 어떻게 상호작용하는지 알아보는 과정이다. 

**# 예를 들어 하나의 영화관을 모델링 해보자.**

영화관이 운영되려면 당연히 영화에 대한 개념이 필요하다. 그리고 고객, 직원 정보도 필요할 것이다. 이처럼 개념을 추출하면 다음 단계는 각 개념의 속성을 알아봐야 한다. 영화라는 개념의 속성에는 제목, 관람 수, 상영관, 연령 제한 등의 속성이 있을 것이다. 고객이라는 개념의 속성에는 고객명, 고객 번호, 멤버쉽 등의 속성이 있을 것이다. 모델링에 있어서 개념과 속성의 차이는 다른 속성을 포함하는지에 대한 여부이다. 만약 상영관이 좌석 등급, 좌석 수와 같이 다른 속성을 포함한 개념이라면 상영관은 영화라는 개념에 포함된 속성이 아닌 하나의 분리된 개념이 되어야 한다. 마지막 단계는 각 개념의 관계를 정의하는 것이다. 예를 들면 고객과 직원은 서비스라는 관계로 연결되고, 고객과 영화는 관람이라는 관계로 연결될 것이다.

데이터 모델링 관점에서 위와 같은 모델링은 절대적인 게 아니다. 상영관을 하나의 개념으로 만들되 영화라는 개념에 포함해도 잘못된 모델링은 아니다. 하지만 관계형 데이터 모델링의 관점에서는 포함관계를 허용하지 않는다. 만약 특정 대상이 속성이 아닌 개념이 되어야 한다면 하나의 다른 개념으로 분리하고 연결 관계만 맺어주는 게 옳다.

![conceptual-data-modeling]({{site.url}}/img/Database/conceptual-data-modeling.jpg){: height="400" width="800"}

![conceptual-data-modeling]({{site.url}}/img/Database/conceptual-data-modeling.png){: height="400" width="800"}

**# ERD**

![conceptual-data-modeling]({{site.url}}/img/Database/erd-step-1.png){: height="400" width="800"}

위에서 추출한 개념, 속성 그리고 관계를 이용해 ER diagram을 그리면 위와 같은 형태가 될 것이다. 하지만 아직 끝난 것은 아니다. 우리는 관계를 좀 더 자세히 명시해줘야 한다.

**# Cardinality**

각 개념의 연결 관계에서 꼭 따져봐야 하는 요소로써, 각 관계가 '몇 대 몇'의 관계를 맺고있는지 따져보는 것이다. 

영화관 모델링을 봤을 때, 영화와 상영관은 몇 대 몇의 관계일까? 한 영화는 여러 상영관에서 상영될 수 있다. 하지만 상영관은 하나의 영화만 상영할 수 있다. 이는 일대다(1:N)의 관계로 생각할 수 있다. 직원과 상영관을 어떨까? 한 명의 직원은 여러 상영관을 관리할 수 있다. 또한 하나의 상영관은 여러 직원에게 관리될 수 있다. 이런한 관계는 다대다(N:M)의 관계라고 생각할 수 있다. 모든 관계를 따져보면 우리는 다음과 같은 ERD를 얻을 수 있다. 

![conceptual-data-modeling]({{site.url}}/img/Database/erd-step-2.png){: height="400" width="800"}

**# Optionality**

각 개념의 연결 관계에서 꼭 따져봐야 하는 요소로써, 각 관계가 '필수적인' 관계인지 따져보는 것이다.

역시 영화관 모델을 봤을 때, 영화와 상영관은 필수적인 관계일까? 각각의 개념의 입장이 서로 다른데, 영화는 상영관이 필수적이지 않지만 상영관은 반드시 영화를 상영해야 한다. 직원과 상영관은 어떨까? 직원은 상영관을 반드시 관리할 필요는 없다. 하지만 상영관은 최소한 1명의 직원에게는 관리되야 한다. 그 결과 우리는 다음과 같은 ERD를 얻을 수 있다.

![conceptual-data-modeling]({{site.url}}/img/Database/erd-step-3.png){: height="400" width="800"}

이와 같이 개념의 연결관계를 Cardinality와 Optionality를 거쳐 자세히 명시해야 완성된 ERD를 얻을 수 있다.

## <span style="color:#0f7b6c">5. 논리적 데이터 모델링</span>

논리적 데이터 모델링에서는 앞서 얻은 ERD를 관계형 데이터 베이스 패러다임에 어울리게 데이터를 정리하는 작업이 이뤄진다. 이 과정에서는 구체적인 데이터베이스의 특성이나 성능은 크게 신경쓰지 않는다. 대신 관계형 데이터 베이스 패러다임에 어울리는 이상적인 모습으로 개념을 잘 정리정돈하는 것이 포인트이다.

**# Mapping Rule**

![mapping rule]({{site.url}}/img/Database/mapping-rule.png){: height="300" width="500"}

ERD를 통해 표현한 내용을 관계형 데이터 베이스에 맞는 형식으로 전환할 때 사용해 볼 수 있는 방법이다.