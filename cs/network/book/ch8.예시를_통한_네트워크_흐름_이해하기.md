### OSI 7 계층에서 의문
- 네트워크 계층은 왠에서 다른 네트워크로 데이터를 전달을 담당하고, 데이터 링크은 랜에서 데이터 송수신을 담당
  - 그럼 하나의 랜에 포함된 컴퓨터끼리의 통신에서는 네트워크 계층이 필요 없나?

### 데이터를 보내는 송신자 관점에서 네트워크 이해하기
- 웹 브라우저를 사용한다는 의미는 현재 우리가 응용 계층에 있다는 의미
  - 인간인 우리가 컴퓨터와 상호 작용하기 위해서는 애플리케이션이라는 매개체가 필요
- 웹 브라우저에 www.google.com 입력
  - index.html이 생략됨
  - DNS 조회로 IP 주소 확
  - 사용자와 서버는 3방향 핸드셰이크로 연결이 확립된 상태
  - HTTP 프로토콜과 80번 포트 사용 
    - (전송 계층 헤더가 추가되기 전까지는 80번 포트인거 모르지 않나?)
    - 전송 계층 헤더가 추가되기 전에 브라우저는 이미 포트 번호를 알고 있습니다.
      이는 브라우저가 프로토콜 규칙에 따라 기본 포트를 설정하기 때문입니다.
  - HTTP request를 완성시키면 응용 계층에서 요청할 데이터가 정의된 것
  - 데이터를 전송 계층으로 전달
- 전송 계층에서 헤더 추가
  - 응용 계층에서 넘어온 데이터에 어떤 포트를 사용할 지 포함되어 있고, 이를 헤더로 옮기는 정도인 듯?
  - 사용자의 포트는 80 포트가 아니다. 사용자가 사용할 수 있는 포트는 1024~49151번 사이 번호이다.
  - 전송 계층의 헤더가 덧붙여진 데이터를 세그먼트라고 부른다.
- 네트워크 계층에서 헤더 추가
  - 라우터를 통해 상대방의 IP 주소를 알아낸다? 상대방의 IP 주소는 DNS 서버로부터 알아내는거 아닌가?
  - 송신자와 수신자의 IP 주소와 기타 정보가 포함된 헤더를 세그먼트에 추가하면 패킷이 된다.
- 데이터 링크 계층에서 헤더 추가
  - 헤더에는 송신자와 수신자의 MAC 주소 포함한 헤더를 패킷에 추가하면 프레임이 된다. 
  - 서로 다른 네트워크 간의 통신에서는 수신자의 MAC 주소를 모르기 때문에 내가 속한 라우터를 수신자의 MAC 주소로 기입해야 한다.
    - 그러면 내가 속한 라우터가 이후 통신을 책임진다.
- 물리 계층에서 랜 카드가 프레임을 전기 신호로 변환한다.

### 장비 관점에서의 데이터 흐름 이해하기
- 송신자에서 스위치 A의 물리 계층으로 전기 신호가 오면
  - 스위치 A의 물리 계층에서 전기 신호를 프레임으로 복원하여 데이터 링크 계층으로 보낸다. 
  - 스위치 A의 데이터 링크 계층에서, 이더넷 헤더에 적힌 수신자 MAC 주소를 라우터 A의 MAC 주소로 변경한다.
  - 스위치 A의 물리 계층에서 프레임을 전기 신호로 변환하여 라우터 A로 전달
- 라우터 A로 물리 계층으로 전기 신호가 오면
  - 라우터 A의 데이터 링크 계층에서 수신자의 MAC 주소를 자기 자신의 것과 비교한 후, 같으면 이더넷 헤더를 떼어내고 패킷을 네트워크 계층에 전달
  - 라우터 A의 네트워크 계층에서 라우팅 테이블에 수신자 IP 주소가 등록되어 있는지 확인. 등록되어 있다면
  - 송신자의 IP 주소를 라우터 A 자신의 IP 주소, 특히 왠 환경에서 사용할 수 있는 IP 주소로 교체
    - 그래야 다른 라우터와 통신 가능
    - 라우터 A는 랜에 포함된 내부 컴퓨터들이 사용하는 IP 주소가 있고, 다른 외부 네트워크와 통신하기 위한 IP 주소가 따로 있다.
  - 라우터 A의 데이터 링크 계층에서 송신자의 MAC 주소를 라우터 A 자신의 MAC 주소로, 수신자의 MAC 주소를 라우터 B의 MAC 주소로 변경
  - 이렇게 해서 송신자 IP 주소와 MAC 주소가 모두 라우터 A가 사용하는 것들로 변경된다.
- 라우터 B의 물리 계층으로 전기 신호가 오면
  - 라우터 A와 겪은 과정과 비슷한 과정을 거친다.
  - 데이터 링크 계층에서 수신자 MAC 주소가 자신의 MAC 주소와 동일하면 이더넷 헤더를 분리하고 네트워크 계층으로 보냄
  - 네트워크 계층에서는 수신자 IP 주소가 자신의 라우팅 테이블에 있는지 확인
  - 송신자 IP 주소를 라우터 A의 IP 주소에서 라우터 B의 IP 주소로 변경
    - 이 때 라우터 B도 내부 IP 주소와 외부 IP 주소를 갖는데, 라우터 B와 스위치 B는 내부 통신이기 때문에 내부 IP 주소를 적어줘야 한다.
  - 데이터 링크 계층에서 송신자의 MAC 주소를 라우터 A의 MAC 주소에서 라우터 B의 MAC 주소로 변경하고, 수신자의 MAC 주소는 스위치 B에 연결된 웹 서버 주소로 변경
