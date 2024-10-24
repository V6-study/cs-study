컴퓨터에서 일어나는 다중 작업은 동시에 처리하는 것이 아닌

각 프로그램을 일정시간 동안 번갈아가면서 실행하는 것이다.

다만 그 속도가 매우 빠르기에 동시에 처리한다고 느끼는 것이다.

프로세스들이 교체되어 수행되고 나면 원래 수행하던 **프로세스의 정보를 저장**하고

다른 프로세스를 처리해야 하는데, 이를 **저장하는 공간을 PCB**(Process Control Block) 라고 한다.

### PCB

PCB : 운영체제에서 관리하는 프로세스에 대한 메타데이터를 저장한 데이터블록

-   프로세스가 생성될 때 고유의 PCB가 생성되며 종료되면 PCB가 제거됨
-   커널의 데이터 영역에 저장됨
    -   실행 상태일 때는 프로그램 카운터 레지스터 값 등이 CPU 레지스터에 로드됨
<div align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlK8hI%2FbtsKg8eTHZj%2FaiX4TO2tw9806bQbhBkwxk%2Fimg.png" alt="Image description" width="300"/>
</div>

-   Pointer: 프로세스 전환 시 위치 정보를 유지하는 스택 포인터
-   Process State: 생성, 준비, 실행, 대기, 종료 등의 프로세스의 상태
-   Process Number(PID): 각 프로세스의 고유 식별 번호
-   Program Counter(PC): 프로세스에서 실행해야 할 다음 명렁어의 주소를 포함하는 카운터를 저장
-   Registers: 프로세스의 레지스터 상태를 저장하는 공간
-   Memory Limits: 프로세스의 메모리 관련정보
-   List of Open Files: 프로세스를 위해 열린 파일 목록들

---

### Context Switching

CPU에서 실행중인 프로레스를 다른 프로세스로 교체하는 작업으로 다음의 이유로 발생한다

1.  인터럽트 발생
2.  시스템 콜 호출
3.  타임 슬라이스 만료
4.  I/O 요청
5.  프로세스의 우선 순위 변경
<div align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdY32Tt%2FbtsKgQ6yN29%2FmHdBfuy90eQD0mfx4LbNFK%2Fimg.png" alt="Image description"/>
</div>

1.  현재 실행 중인 프로세스(P0)의 정보를 PCB에 저장
2.  다음 실행할 프로세스(P1)의 정보를 PCB에서 로드
3.  P1을 실행

<div align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fm5Qx4%2FbtsKgNowBAA%2FNETMrS8Qv4I4vDTDzhJagK%2Fimg.png" alt="Image description"/>
</div>

유휴 상태가 겹치는 시기들을 **오버헤드**라고 부르며

CPU가 다른프로세스의 정보를 메모리에 로드하거나 저장하는 동안 **아무 작업도 수행하지 않는 시간**을 의미한다.

<div align="center">
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcb0Olf%2FbtsKg2eN4RO%2FUTbCKsPyFxx19YnCnKKOaK%2Fimg.png" alt="Image description" />
</div>

Context Switching Cost

-   캐시 무효화(Cache Invalidation)
-   메모리 매핑 재구성
-   커널 모드로의 전환 비용

아래와 같은 방법을 통해 Context Switching의 비용을 최적화할 수 있다.

-   메모리 공유가 필요한 작업은 **스레드 사용**
-   프로세스의 특성에 따라 **우선순위 동적 조정**
-   시스템 부하와 프로세스 특성에 따른 **타임 슬라이스 최적화**

출처

[https://www.geeksforgeeks.org/process-table-and-process-control-block-pcb/](https://www.geeksforgeeks.org/process-table-and-process-control-block-pcb/)

[https://didu-story.tistory.com/312](https://didu-story.tistory.com/312)

[https://parkmuhyeun.github.io/etc/operating%20system/2022-08-15-PCB-Context-Switching/](https://parkmuhyeun.github.io/etc/operating%20system/2022-08-15-PCB-Context-Switching/)
