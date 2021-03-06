# TCP

- TCP 통신이란?
  - 일반적으로 TCP와 IP를 함께 사용하는데, IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적 및 관리한다.
  - **신뢰성 있는 데이터 전송을 지원하는 연결 지향형 프로토콜** 이다.
  - 사전에 **3-way handshake**라는 과정을 통해 연결을 설정하고 통신을 시작한다.
  - **4-way handshake** 과정을 통해 연결을 해제(가상 회선 방식)한다.
  - **흐름제어, 혼잡제어, 오류제어를 통해 신뢰성을 보장**한다. 그러나 이 때문에 UDP보다 전송 속도가 느리다는 단점이 있다.
  - **데이터의 전송 순서를 보장**하며 수신 여부를 확인할 수 있다.
  - TCP를 사용하는 예로는 대부분의 웹 HTTP 통신, 이메일, 파일 전송에 사용된다.
  - TCP가 가상회선 방식을 제공한다는 것은 송신측과 수신측을 연결하여 패킷을 전송하기 위한 논리적 경로를 배정한다는 뜻이다.

##### 패킷(Packet)이란?

> 인터넷 내에서 데이터를 보내기 위한 경로 배정(라우팅)을 효율적으로 하기 위해서 데이터를 여러 개의 조각으로 나누어 전송을 하는데, 이 때 조각을 패킷이라고 한다.

##### TCP는 패킷을 어떻게 추적 및 관리하는가?

> 데이터는 패킷 단위로 나누어 같은 목적지(IP 계층)으로 전송된다.

## 흐름 제어

송신측과 수신측 사이의 데이터 처리 속도 차이(흐름)을 해결하기 위한 기법.
만약, 송신측의 전송량 > 수신측의 처리량일 경우 전송된 패킷은 수신측의 큐를 넘어서 손실될 수 있기 때문에 송신측의 패킷 전송량을 제어하게 된다.

##### 해결방법

1. Stop and Wait(정지 - 대기)

   - 매번 전송한 패킷에 대한 확인 응답을 받아야 그 다음 패킷을 전송할 수 있다.
   - 이러한 구조 때문에 비효율적이다.(단점)
   - Give & Take

2. Sliding Window(슬라이딩 윈도우)

   - 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인 응답 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 기법이다.
   - 윈도우 : 송신, 수신 스테이션 양쪽에서 만들어진 버퍼의 크기
   - 동작방식: 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는 대로 이 윈도우를 옆으로 옮김으로써 그 다음 패킷들을 전송

   - <img src="https://camo.githubusercontent.com/06edc51853591b10242cf34fba0b9b92468d860a/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323533463745343835373135454435463237">
   - 송신측에서 0,1,2,3,4,5,6 을 보낼 수 있는 프레임을 가지고 있고 데이터 0,1을 전송했다고 가정하면 슬라이딩 윈도우의 구조는 2,3,4,5,6 처럼 변하게 된다.
   - 이때 만약 수신측으로부터 ACK라는 프레임을 받게 된다면 송신측은 이전에 보낸 데이터 0,1을 수신측에서 정상적으로 받았음을 알게 되고 송신측의 슬라이딩 윈도우는 ACK 프레임에 따른 프레임의 수만큼 오른쪽으로 경계가 확장된다.

## 오류 제어

오류 검출과 재전송을 포함한다.
ARQ(Automatic Repeat Request) 기법을 사용해 프레임이 손상되었거나 손실되었을 경우, 재전송을 통해 오류를 복구한다.
ARQ 기법은 흐름제어기법과 관련되어 있다.

1. Stop and Wait ARQ

   - 송신측에서 1개의 프레임을 송신하고, 수신측에서 수신된 프레임의 에러 유무 판단에 따라 ACK or NAK를 보내는 방식이다.
   - 식별을 위해 데이터 프레임과 ACK 프레임은 각각 0,1 번호를 번갈아가며 부여한다.
   - 수신측이 데이터를 받지 못했을 경우, NAK를 보내고 NAK를 받은 송신측은 데이터를 재전송한다.
   - 만약 데이터나 ACK가 분실되었을 경우 일정 간격의 시간을 두고 타임아웃이 되면, 송신측은 데이터를 재전송 한다.

2. Go-Back-n ARQ

   - 전송된 프레임이 손상되거나 분실된 경우 그리고 ACK 패킷의 손실로 인한 TIME_OUT이 발생한 경우, 확인된 마지막 프레임 이후로 모든 프레임을 재전송한다.
   - 슬라이딩 윈도우는 연속적인 프레임 전송 기법으로 전송측은 전송된 모든 프레임의 복사본을 가지고 있어야 하며, ACK와 NAK 모두 각각 구별해야 한다.
   - ACK: 다음 프레임을 전송
   - NAK: 손상된 프레임 자체 번호를 반환

3. SR(Selective-Reject) ARQ

   - GBn ARQ의 확인된 마지막 프레임 이후의 모든 프레임을 재전송하는 단점을 보완한 기법이다.
   - SR ARQ는 손상된, 손실된 프레임만 재전송한다.
   - 그렇기 때문에 별도의 데이터 재정렬을 수행해야 하며, 별도의 버퍼를 필요로 한다.
   - 수신측에 버퍼를 두어 받은 데이터의 정렬이 필요하다.

## 혼잡 제어

송신측의 데이터 전달과 네트워크의 데이터 처리 속도를 해결하기 위한 기법이다. 한 라우터에게 데이터가 몰려 모든 데이터를 처리할 수 없는 경우, 호스트들은 재전송을 하게 되고 결국 혼잡만 가중시켜 오버플로우나 데이터 손실이 발생한다. 이러한 **네트워크의 혼잡을 피하기 위해 송신측에서 보내는 데이터의 전송 속도를 제어**하는 것이 혼잡 제어의 개념이다.

1. AIMD(Additive Increase Multicative Decrease)

   - 합 증가/ 곱 감소 알고리즘이라고 한다.
   - 처음에 패킷 하나를 보내는 것으로 시작하여 전송한 패킷이 문제 없이 도착한다면 Window Size를 1씩 증가시키며 전송하는 방법이다. 만약, 패킷 전송을 실패하거나 TIME_OUT이 발생하면 Window Size를 절반으로 감소시킨다.
   - 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만, 시간이 흐르면 평형 상태로 수렴하게 되는 특징이 있다.
   - 문제점은 초기 네트워크의 높은 대역폭을 사용하지 못하고 네트워크가 혼잡해지는 상황을 미리 감지하지 못하여 혼잡해지고 나서야 대역폭을 줄이는 방식이다.

2. Slow Start

   - AIMD가 네트워크의 수용량 주변에서는 효율적으로 동작하지만, 처음에 전송 속도를 올리는 데 시간이 너무 길다는 단점이 있다.
   - Slow Start는 AIMD와 마찬가지로 패킷을 하나씩 보내는 것부터 시작한다. 이 방식은 문제없이 도착하면 각각의 ACK 패킷마다 Window Size를 1씩 늘린다. 즉, 한 주기가 지나면 Window Size는 2배가 된다.
   - 따라서 그래프의 모양은 지수 함수 꼴이 된다.
   - 혼잡 현상이 발생하면 Window Size를 1로 떨어뜨린다.
   - 처음에는 네트워크의 수용량을 예측할 수 있는 정보가 없지만 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느정도 예상할 수 있으므로 혼잡 현상이 발생하였던 Window Size의 절반까지는 이전처럼 지수 함수 꼴로 Window Size를 증가시키고 그 이후부터는 완만하게 1씩 증가시키는 방식이다.
