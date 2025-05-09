### 전송 계층
- 데이터를 목적지에 문제없이 전달하는 것을 목표로 한다.
- 데이터를 전달하는 목적에 따라 연결형과 비연결형 통신으로 나뉜다.
  - 연결형 통신
    - 데이터를 정확하게 전달하는 것이 목표
    - TCP 프로토콜 사용
  - 비연결형 통신
    - 효율적으로 데이터를 보내는 통신
    - UDP 프로토콜 사용

### 혼잡 제어
- 네트워크로 들어가는 정보량을 조절하여 네트워크가 혼잡해지지 않게 조절
- 송신자는 먼저 하나의 데이터만 보내고 수신자 측에서 ACK가 오면 전송량을 2배씩 증가시켜 나간다.
  - 타임아웃이 발생하면 여러 개 보냈던 데이터를 줄여서 보낸다.
  - 수신자 측에서 패킷 손실이 발생하면 마지막으로 정상 수신한 패킷에 대한 ACK를 계속 보내므로, 동일한 ACK를 여러 번 받는 경우에도 데이터를 줄여서 보낸다.
  
### 흐름 제어
- 데이터 링크 계층에서 사용했던 정지 대기 방식과 동일
  - 그러면 데이터 링크 계층에서도 정지 대기로 흐름 제어를 하고, 전송 계층에서도 중복해서 흐름 제어를 하는건가?

### 오류 제어
- 목적은 데이터 링크 계층과 동일하지만 방법이 다르다.
  - 확인 응답 방법
    - 수신자 측으로부터 ACK가 없다면 오류로 판단 
  - 시간 초과 방법
    - 특정 시간 내에 ACK가 없으면 세그먼트에 오류가 있다고 판단
- 데이터를 재전송이 필요한 상황
  - 데이터가 중간에 손실될 때
  - 데이터 순서가 바뀌었을 때
  - 데이터가 훼손되었을 때

### TCP 프로토콜의 3방향 핸드셰이크
- TCP 프로토콜은 데이터를 정확하게 전달하는 것이 목표
- 상대방이 내 신호를 받을 수 있는 상태인지 확인하고 전송하기 위해 3방향 핸드셰이크 활용
  - 송신자는 수신자에서 SYN이라는 임의의 숫자를 보냄
  - 수신자는 송신자가 보낸 SYN에 1이 더해진 숫자인 ACK와 새로운 SYN을 보냄
  - 송신자는 수신자가 보낸 SYN에 1이 더해진 숫자인 ACK를 보냄

### TCP의 구조
- 포트 번호
  - 전송 계층은 어떤 애플리케이션과 통신하는지 정의하는 곳인데, 그 기능을 하는 것이 포트 번호
  - 하나의 컴퓨터에서 포트에 따라 사용하는 프로그램이 달라진다.
  - 0~1023 : 잘 알려진 포트. 특정한 쓰임을 위해 사용됨
  - 1024~49151 : 기관이나 기업들이 사용하는 포트
  - 49152~65535 : 일반 사용자들이 자유롭게 사용 가능. 다이내믹 포트

### 일련번호와 확인 응답 번호
- 일련번호 : 송신자가 수신자에게 보내려는 데이터가 몇 번째인지 알려준다.
  - 처음의 일련번호가 1이라면, 다음의 일련번호는 1 + 패킷의 크기(바이트)
- 확인 응답 번호 : 수신자가 몇 번째 데이터를 받았는지 송신자에게 알려주는 역할
  - 송신자가 다음으로 보내야 하는 일련 번호를 확인 응답 번호로 전달한다.

### 윈도우 크기
- 송신자가 한 번에 보낼 수 있는 데이터의 최대 크기
- 수신자의 윈도우 크기는 3방향 핸드셰이크 과정에서 알아낸다.
- 매번 데이터를 보내고 ACK를 기다리는 것이 아니라 수신자가 받을 수 있을 만큼 데이터를 한 번에 보낼 수 있다.
  - 송신한 데이터마다 ACK가 오긴 하는데, ACK를 기다렸다가 다음 데이터를 보내는 것이 아니라, 윈도우만큼 미리 보낼 수 있다.

### 코드 비트
- 다음과 같은 항목이 있다.
  - TURG : 긴급 처리 데이터가 있음
  - ACK : 확인 응답 번호 사용
  - RSH : TCP가 받은 데이터를 상위 계층에 전달
  - RST : 연결 재설정
  - SYN : 연결을 초기화하기 위해 순서 번호를 동기화
  - FIN : 데이터 송신 종료
- 기본 값은 0이고, 비트가 활성화되면 1이 된다.

### UDP
- 데이터를 수신자 측에서 오류 없이 받든, 그렇지 않든 상관하지 않고 데이터를 보내기만 한다.
  - 신뢰성을 보장하지 않는 만큼 데이터 전송 속도가 빠르므로 주로 실시간 방송 등에 사용
  - 브로드캐스트의 경우 2대 이상의 컴퓨터에게 동일한 데이터를 전달하는데, 이 때 사용되는 프로토콜이 UDP
    - 브로드캐스트라도 신뢰성이 중요할 수도 있는거 아닌가? 왜 브로드캐스트라고 해서 UDP를 사용할까?
      - 브로드캐스트에서는 수신자가 여러 대이기 때문에, 각각의 수신자와 연결을 따로 맺어야 하는 구조적 문제가 발생
      - 브로드캐스트는 속도와 확산이 핵심, 신뢰성은 상황에 따라 애플리케이션 레벨에서 해결
  - TCP는 데이터를 세그먼트라는 단위로 쪼갠 후 순서를 부여하여 전송하여 수신자의 입장에서 세그먼트의 순서가 뒤바뀌는 일이 없도록 오류를 제어한다.
- UDP 프로토콜의 헤더는 TCP와 비교했을 때 상당히 축약된 정보만을 포함
  - 신뢰성을 보장하지 않기 때문
  - 송 수신자의 포트 번호, 헤더 길이, 검사합 정도 정보만 포함

### 전송 계층에서 사용하는 로드 밸런서
- 로드밸런서는 여러 대의 서버를 두고 사용자가 한쪽으로 몰리는 것을 분산시켜주는 장치
- 부하를 분산시키는 방법
  - 라운드 로빈
    - 분배의 가장 기본적인 방식으로 각 서버별로 돌아가면서 연결 처리
  - 가중 라운드 로빈 
    - 각 서버별로 돌아가면서 연결을 처리하지만 일부 서버는 큰 트래픽을 몰아 받는 방식
  - 랜덤
    - 무작위로 분배
  - 해시
    - 특정 클라이언트는 특정 서버에서만 처리하는 방식
    - stateful한 경우 사용할 듯?
- 로드 밸런서는 포트 번호를 이용하여 부하를 분산시킬 수도 있다.
  - 로드 밸런서 IP 주소로 접속을 시도하는 것 중에 8001번 포트를 사용하는 것은 172.16.2.201 서버로 전달
  - 8002번 포트를 사용하는 것은 172.16.2.203 서버로 전달
