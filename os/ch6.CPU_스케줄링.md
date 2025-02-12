### 기계어 명령
- CPU 내에서 수행되는 명령 (일반명령)
  - CPU 내에서만 수행되므로 명령의 수행 속도가 매우 빠르다. (레지스터 사용 포함)
  - ex. Add
- 메모리 접근을 필요로 하는 명령 (일반명령)
  - CPU 내에서 수행되는 명령보다는 느리지만 비교적 짧은 시간에 수행할 수 있다.
  - ex. Load, Store
- 입출력을 동반하는 명령 (특권명령)
  - CPU나 메모리 접근 명령에 비해 굉장히 오랜 시간이 소요

### CPU 스케줄링
- CPU 버스트 위주의 CPU 바운드 프로세스와 IO 버스트 위주의 IO 바운드 프로세스가 동일한 시스템 내부에서 함께 실행되기 때문에 필요
  - 프로세스의 CPU 버스트가 모두 균일한 경우에는 CPU 스케줄링을 하는 것이 큰 의미가 없다.
  - 우리가 사용하는 시분할 시스템에서는 CPU 버스트가 균일하지 않은 다양한 프로그램들이 공존하므로 효율적인 CPU 스케줄링 기법이 반드시 필요하다.
- 컴퓨터 시스템 내에서 수행되는 프로세스들은 대부분 CPU 버스트가 짧고 대화형 작업으로 사용자와 인터랙션을 해가며 프로그램을 수행시킨다.
  - 사용자에 대한 빠른 응답이 중요하므로, CPU 스케줄링을 할 때 CPU 버스트가 짧은 프로세스에게 우선적으로 CPU를 사용할 수 있도록 하는 스케줄링이 필요
  - 이러한 스케줄링은 IO 장치의 효율성을 높이는 효과도 얻을 수 있다.

### CPU 스케줄러
- 준비 상태에 있는 프로세스들 중 어떠한 프로세스에게 CPU를 할당할지 결정하는 운영체제의 코드
- CPU 스케줄러가 필요한 경우
  - 실행 상태에 있던 프로세스가 IO 요청등으로 봉쇄 상태로 바뀌는 경우 (비선점형)
  - 실행 상태에 있던 프로세스가 타이머 인터럽트 발생에 의해 준비 상태로 바뀌는 경우 (선점형)
  - IO 요청으로 봉쇄 상테에 있던 프로세스의 IO 작업이 완료되어 인터럽트가 발생하고 그 결과 이 프로세스의 상태가 준비 상태로 바뀌는 경우 (선점형)
  - CPU에서 실행 상태에 있는 프로세스가 종료되는 경우 (비선점형)

### 디스패처
- 새롭게 선택된 프로세스가 CPU를 할당받고 작업을 수행할 수 있도록 환경설정을 하는 운영체제의 코드
  - 현재 수행 중이던 프로세스의 문맥을 PCB에 저장
  - 새롭게 선택된 프로세스의 문맥을 PCB로부터 복원한 후 그 프로세스에게 CPU를 넘김
- 디스패처가 하나의 프로세스를 정지시키고 다른 프로세스에게 CPU를 전달하기까지 걸리는 시간을 디스패치 지연시간이라고 한다.
  - 디스패치 지연시간의 대부분은 문맥교환 오버헤드에 해당

### 스케줄링의 성능 평가
- 시스템 관점의 지표
  - CPU 이용률(CPU utilization)
  - 처리량(throughput)
    - 주어진 시간 동안 준비 큐에서 기다리고 있는 프로세스 중 몇 개를 끝마쳤는가
    - 즉 CPU의 서비스를 원하는 프로세스 중 몇 개가 원하는 만큼의 CPU를 사용하고 이번 CPU 버스트를 끝내어 준비 큐를 떠났는지 측정
    - CPU 버스트가 짧은 프로세스에게 우선적으로 CPU를 할당하는 것이 처리량 관점에서 유리
- 사용자 관점의 지표
  - 소요시간(turnaround time)
    - 프로세스가 CPU를 요청한 시점부터 자신이 원하는 만큼 CPU를 다 쓰고 CPU 버스트가 끝날 때까지 걸린 시간
    - 즉 준비 큐에서 기다린 시간과 실제로 CPU를 사용한 시간의 합
    - 하나의 프로세스에서도 소요시간은 CPU 버스트의 수만큼 각각 별도로 측정됨에 주의
  - 대기시간(waiting time)
    - CPU 버스트 기간 중 프로세스가 준비 큐에서 CPU를 얻기 위해 기다린 시간의 합
    - 시분할 시스템에서는 특정 프로세스의 한 번의 CPU 버스트 중에도 준비 큐에서 기다린 시간이 여러 번 발생할 수 있다.
    - (처음 CPU를 할당받기까지 준비큐에서 대기한 시간 + CPU버스트가 끝나기 전에 타이머에 의해 CPU를 빼앗겨서 준비큐에서 대기한 시간)으로 보면 되나?
  - 응답시간
    - 프로세스가 준비 큐에 들어온 후 첫 번째 CPU를 획득하기까지 기다린 시간
    - 타이머 인터럽트가 빈번히 발생할수록 각 프로세스가 CPU를 연속적으로 사용할 수 있는 시간이 짧아지므로 처음 CPU를 얻기까지 걸리는 시간은 줄어들게 되어 응답시간이 향상된다.
    - 대화형 시스템에 적합한 성능 척도로서 사용자 입장에서 가장 중요한 성능 척도

### 스케줄링 알고리즘
- 선입선출 스케줄링 (First-Come First-Served)
  - 프로세스가 준비 큐에 도착한 시간 순서대로 CPU를 할당
  - 프로세스가 자발적으로 CPU를 반납할 때까지 빼앗지 않는다.
  - CPU 버스트가 긴 프로세스 하나 때문에 평균 대기시간은 물론 IO 장치들의 이용률까지도 동반 하락하게 된다.
    - Convoy effect라고 부르며 FCFS 스케줄링의 대표적인 단점에 해당된다.
    - 반대로 CPU 버스트가 짧은 프로세스가 먼저 도착하면 평균 대기시간이 줄어든다.

### 최단작업 우선 스케줄링 (Shortest-Job First)
- CPU 버스트가 가장 짧은 프로세스에게 제일 먼저 CPU를 할당하는 방식
- 평균 대기시간을 가장 짧게 하는 최적 알고리즘
- 비선점형
- 선점형 (Shortest Remaining Time First)
  - 준비 큐에서 CPU 버스트가 가장 짧은 프로세스에게 CPU를 할당했다 하더라도, CPU 버스트가 더 짧은 프로세스가 도착할 경우 CPU를 빼앗아 더 짧은 프로세스에게 부여
  - 프로세스들이 준비 큐에 도착하는 시간이 불규칙한 환경에서는 선점형 방식이 프로세스들의 평균 대기시간을 최소화하는 최적의 알고리즘이 된다.
- 프로세스의 CPU 버스트 시간을 미리 알 수 없기 때문에 과거의 CPU 버스트 시간을 통해 예측한다.
  - T_n+1 = a*t_n + (1-a)*T_n
    - t_n : n번째 실제 CPU 버스트 시간
    - T_n : n번째 CPU 버스트의 예측 시간
  - 최근의 CPU 버스트 시간일수록 오래 전의 CPU 버스트 시간에 비해 가중치를 높이는 방식이다. 1보다 작은 값인 (1-a)가 계속 곱해지므로
- SJF 알고리즘이 평균 대기시간을 최소화하는 알고리즘이기는 하지만 시스템에셔 평균을 줄이는 것이 항상 좋은 방식이라고는 말할 수 없다.
  - CPU 버스트가 긴 프로세스는 준비 큐에 줄 서서 무한정 기다려야 하는 심각한 문제가 발생할 수 있기 때문 (기아 현상)

### 우선순위 스케줄링 (priority scheduling)
- 준비 큐에서 기다리는 프로세스들 중 우선순위가 가장 높은 프로세스에게 제일 먼저 CPU를 할당하는 방식
  - 우선순위 값이 작을수록 높은 우선순위를 가지는 것으로 가정한다.
- CPU 버스트 시간을 우선순위 값으로 정의하면 SJF 알고리즘과 동일한 의미를 가지게 된다.
- 비선점형
- 선점형
  - 현재 CPU에서 수행 중인 프로세스보다 우선순위가 높은 프로세스가 도착하여 CPU를 선점해서 새롭게 도착한 프로세스에게 할당할 경우
- 노화(aging) 기법을 사용하면 기아 현상을 방지할 수 있다.

### 라운드 로빈 스케줄링
- 할당 시간(time quantum)이 너무 길면 FCFS와 같은 결과가 나타나고, 너무 짧으면 문맥교환의 오버헤드가 커진다.
  - 여러 프로세스가 동시에 수행되는 환경에서 대화형 프로세스가 CPU를 한 번 할당받기까지 지나치게 오래 기다리지 않을 정도의 수십 밀리초 정도로 설정한다.
- 여러 종류의 이질적인 프로세스가 같이 실행되는 환경에서 효과적
  - CPU를 많이 쓰는 프로세스는 대기시간이 그에 비례해서 길어지고, CPU를 적게 쓰는 프로세스는 대기시간도 짧아진다.
- 일반적으로 SJF 방식보다 평균 대기시간은 길지만 응답시간은 더 짧다.

### 멀티레벨 큐
- 준비 큐를 여러 개로 분할해 관리하는 스케줄링 기법
  - CPU는 하나 밖에 없으므로 어떤 줄에 서 있는 프로세스를 우선적으로 스케줄링할 것인가
  - 프로세스가 도착했을 때 어느 줄에 세울 것인가
- 멀티레벨 큐는 일반적으로 성격이 다른 프로세스들을 별도로 관리하고, 프로세스의 성격에 맞는 스케줄링을 적용하기 위해 준비 큐를 별도로 두는 것
  - 대화형 작업을 담기 위한 전위 큐(foreground queue)
    - 응답 시간을 짧게 하기 위해 라운드 로빈 스케줄링 사용
  - 계산 위주의 작업을 담기 위한 후위 큐(background queue)
    - 응답시간이 큰 의미를 가지지 않기 때문에 FCFS 스케줄링 기법을 사용해 문맥교환 오버헤드를 줄임
- 큐 자체에 대한 스케줄링이 필요하다.
  - 고정 우선순위 방식(fixed priority scheduling)
    - 큐에 고정적인 우선순위를 부여
  - 타임 슬라이스 방식(time slice)
    - 큐에 대한 기아 현상을 해소하기 위해 각 큐에 CPU 시간을 적절한 비율로 할당한다.
    - 예를 들어 전체 CPU 시간 중 전위 큐에는 80%, 후위 큐에는 20%를 할당해 스케줄링하는 방식을 사용할 수 있다.
  
### 멀티레벨 피드백 큐
- 멀티레벨 큐와 유사하나, 프로세스가 하나의 큐에서 다른 큐로 이동 가능하다는 점이 다르다.
- 우선순위 스케줄링에서 기아 현상을 해결하기 위해 등장했던 노화 기법을 멀티레벨 피드백 큐 방식으로 구현할 수 있다.
  - 우선순위가 낮은 큐에서 오래 기다렸으면 우선순위가 높은 큐로 승격
- 대표적인 방식
  1. 라운드 로빈(할당시간 5) 큐
  2. 라운드 로빈(할당시간 10) 큐
  3. FCFS 큐
  - 큐에 대한 스케줄링 방식으로는 최상위 큐가 우선적으로 CPU를 배당받고, 상위 큐가 비었을 때에만 하위 큐에 있는 프로세스들이 CPU를 할당받음
  - 라운드 로빈 스케줄링을 한층 더 발전시켜, 프로세스의 CPU 작업시간을 다단계로 분류함으로써 작업시간이 짧은 프로세스을수록 더욱 빠른 서비스가 가능하도록 하고, 작업시간이 긴 프로세스에 대해서는 문맥교환 없이 CPU 작업에만 열중할 수 있는 FCFS 방식을 채택할 수 있게 한다.

### 다중처리기 스케줄링(multiple-processor scheduling)
- 프로세스를 준비 큐에 한 줄로 세워서 각 CPU가 알아서 다음 프로세스를 꺼내가도록 하는 방식
- 반드시 특정 CPU에서 수행되어야 하는 프로세스가 있는 경우, 각 CPU 별로 줄 세우기를 할 수도 있다.
  - 이 경우 일부 CPU에 작업이 편중되는 현상이 발생할 수 있다.
  - 이를 방지하기 위해 각 CPU별 부하가 적절히 분산되도록 하는 부하균형(load balancing) 메커니즘을 필요로 한다.
- 다중처리기 스케줄링 방식
  - 대칭형 다중처리(symmetric multi-processing)
    - 각 CPU가 알아서 스케줄링을 결정하는 방식
  - 비대칭형 다중처리(asymmetric multi-processing)
    - 하나의 CPU가 다른 모든 CPU의 스케줄링 및 데이터 접근을 책임지고 나머지 CPU는 거기에 따라 움직이는 방식

### 실시간 스케줄링
- 데드라인 안에 반드시 작업을 처리해야 하는 실시간 시스템(real-time system)에서 필요한 스케줄링
- 경성 실시간 시스템(hard real-time system)
  - 미사일 발사와 같이 시간을 정확히 지켜야 하는 시스템
- 연성 실시간 시스템(soft real-time system)
  - 멀티미디어 스트리밍과 같이 데드라인이 존재하기는 하지만 지키지 못했다고 해서 위험한 상황이 발생하지는 않음
- 먼저 온 요청을 먼저 처리하기보다는 데드라인이 얼마 남지 않은 요청을 먼저 처리하는 EDF(Earlist Deadline First) 스케줄링을 널리 사용
  - 연성 실시간 시스템처럼 일반 작업과 VOD 작업 등이 혼합된 환경에서는 VOD 등 데드라인이 존재하는 프로세스에게 일반 프로세스보다 높은 우선순위를 할당하는 방식도 사용됨
