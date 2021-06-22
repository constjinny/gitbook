# 2. OSI 7 Model

![OSI 7 Model](https://miro.medium.com/max/1536/0*hg8YYznWeLEem02e.jpg)

## 1. 네트워크 구성요소 <a id="3f71"></a>

![](https://miro.medium.com/max/3268/1*AHO1OPSu8RmquhJyYMRIGA.png)

## 2. OSI Model <a id="e558"></a>

* IOS\(국제표준화기구\)에서 개발한 모델로 컴퓨터 네트워크나 네트워크 프로토콜을 설계하기 위한 지침
* 7단계의 각 계층별 고유 기능이 존재한다.
* 하드웨어, 소프트웨어 혹은 두가지 혼합형태로 구현될 수 있다.
* 상위요소는 하위요소의 데이터를 포함한다.
* L1~L3 : 하드웨어 / L4~L7 : 소프트웨어

## 3. OSI Model Stack <a id="8a68"></a>

### ✔️ L1 : 물리 계층 \(Physical Layer\) <a id="c368"></a>

* L1 데이터를 Bit\(비트\)라 함.
* 아날로그 데이터를 **디지털 데이터로 변환**하여 전송한다.
* 데이터 송수신시 오류가 발생해도 복구하지 않는다.
* 관련 장차 : 네트워크 허브, 모뎀 … \(물리적으로 연결되어있음\)

### ✔️ L2 : 데이터링크 계층 \(Datalink Layer\) <a id="8171"></a>

* L2 데이터를 Frame\(프라임\)이라 함.
* 네트워크 장치간 데이터 전송 담당한다.
* L1에서 생길 수 있는 오류를 찾아내고, 수정을 위한 정보를 제공한다.
* **MAC Address**\(물리적 주소\)를 Frame 헤더에 실어 L3로 전송한다.
* 관련 장치 : 스위치

**🔍 MAC Address**

* NIC 제조업체에서 하드웨어에 영구적으로 기입하여 **주소 변경이 불가**하다.
* 총 48Bit로 구성되어있음.

### ✔️ L3 : 네트워크 계층 \(Network Layer\) <a id="7c52"></a>

* L3 데이터를 Packet\(패킷\)이라 함.
* 라우터를 통한 라우팅 포함, **Packet Forwarding**을 담당
* **IP Address**\(논리적 주소\)를 사용해 라우팅 프로토콜을 따라 통신 경로를 설정하여 패킷을 전달한다.
* 라우터는 LAN 구간 통신을 위해 MAC Address를 포함하고있다. \(LAN : 라우터 — PC / WAN : 라우터 — 라우터\)
* 관련 장치 : 라우터

**🔍 Packet Forwarding**

* 네트워크를 연결하는 스위치, 라우팅 장비에서 수행되는 동작
* 패킷의 헤더 정보를 이용해 최종 목적 네트워크를 향해 패킷을 보내주는 일련의 단계

**🔍 IP Address**

* 네트워크 상 컴퓨터의 고유 주소

### ✔️ L4 : 전송 계층 \(Transport Layer\) <a id="499e"></a>

* L4 데이터를 Segment\(세그먼트\)라고 함.
* 종단 간\(End-to-End\) 데이터 통신을 보장한다.
* 오류 검수, 복구와 흐름제어, 중복 검사등을 수행하여 신뢰성 있는 데이터 전송을 보장한다.
* 효율적인 전송을 위해 **패킷을 분할/재 조립**한다.
* Segment에 **Port Number**, **Socket Address**가 포함되어있다.

**🔍 Port Number**

* 컴퓨터 내 프로세스 구별 수단

**🔍 Socket Address**

* IP Address + Port Number

### ✔️ L5 : 세션 계층 \(Session Layer\) <a id="1f85"></a>

![](https://miro.medium.com/max/2180/1*MykaLEH7Ho0mO6idJBH_qQ.png)

* **통신 설정, 관리 및 종료** 담당
* 라우터를 통과하며 목적지의 Port Number를 만나면 Session이 맺어진다.

### ✔️ L6 : 표현 계층 \(Persentation Layer\) <a id="d17f"></a>

* **코드 간의 번역**을 담당하여 어떻게 표현할지를 정의한다.
* 데이터 압축과 암호화를 통해 데이터의 보안을 높여준다.
* ex. ASCII, Binary, GIF, JPEG…

### ✔️ L7 : 응용 계층 \(Application Layer\) <a id="46a2"></a>

* **최종 사용자와 가장 가까운 계층**이다.
* **프로그래밍 인터페이스**를 사용하여 애플리케이션 프로그램과 네트워크를 연결하는 역할을 담당
* ex. 웹 브라우저, FTP, SMTP, HTTP, HTTPS ….

## 4. OSI 7 Layer Encapsulation & Descapsulation <a id="bf9f"></a>

* 계층별 구분하기 위해 헤더를 붙여 전송한다.
* **계층별 헤더**에는 각 프로토콜 동작에 필요한 요소들을 기록한다.
* 캡슐화\(Encapsulation\) : 데이터를 하위 계층으로 전송할 경우 해당 계층에서 필요한 헤더를 달아 전송한다.
* 역캡슐화 \(Descapsulation\) : 상위 전송시 불필요한 헤더 제거를 제거한 후 전송한다.

## 5. 참고

* [https://ko.wikipedia.org/wiki/OSI\_%EB%AA%A8%ED%98%95](https://ko.wikipedia.org/wiki/OSI_%EB%AA%A8%ED%98%95)
* [https://ko.wikipedia.org/wiki/MAC\_%EC%A3%BC%EC%86%8C](https://ko.wikipedia.org/wiki/MAC_%EC%A3%BC%EC%86%8C)
* [https://insights.profitap.com/osi-7-layers-explained-the-easy-way](https://insights.profitap.com/osi-7-layers-explained-the-easy-way)
* [https://www.webopedia.com/reference/7-layers-of-osi-model/](https://www.webopedia.com/reference/7-layers-of-osi-model/)
* [http://word.tta.or.kr/dictionary/dictionaryView.do?subject=%ED%8C%A8%ED%82%B7+%ED%8F%AC%EC%9B%8C%EB%94%A9](http://word.tta.or.kr/dictionary/dictionaryView.do?subject=%ED%8C%A8%ED%82%B7+%ED%8F%AC%EC%9B%8C%EB%94%A9)

