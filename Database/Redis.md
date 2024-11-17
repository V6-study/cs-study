#### 특징

1.  **인메모리 기반**의 **키-값 구조** 데이터 관리 시스템
    -   모든 데이터를 메모리에 저장하여 빠른 읽기/쓰기 속도 제공
2.  싱글 스레드 기반
    -   한 번의 하나의 명령만 실행
    -   Redis 6.0부터는 멀티스레딩 I/O를 지원하기 시작했지만 코어 로직은 여전히 싱글 스레드 [https://charsyam.wordpress.com/2020/05/05/%EC%9E%85-%EA%B0%9C%EB%B0%9C-redis-6-0-threadedio%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90/](https://charsyam.wordpress.com/2020/05/05/%EC%9E%85-%EA%B0%9C%EB%B0%9C-redis-6-0-threadedio%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90/)
3.  데이터 구조
    -   Strings
        -   Redis의 가장 기본적인 데이터 타입
        -   최대 512MB의 길이를 가질 수 있음
        -   binary safe하며 JPEG image 같은 데이터도 포함할 수 있음  
            <div align="center">
              <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fo5LTq%2FbtsKH7gTzMQ%2FLjCkyX7bbwP5sZhb8njOa1%2Fimg.png" alt="" width="500"/>
            </div>

    -   Lists
        -   삽입 순서로 정렬된 String List
        -   String 형이 배열 구조가 된 것  
            
            <div align="center">
              <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHSsPI%2FbtsKJ77dCWW%2FIthv0BmnPH7gIkY364XXW0%2Fimg.png" alt="" width="500"/>
            </div>
    -   Sets
        -   정렬되지 않은 String Collection
        -   List 형으로부터 인덱스가 없어진 형태
        -   value의 중복이 허용되지 않음  
            
            <div align="center">
              <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcspTbX%2FbtsKMmpwtve%2F4bwdhuxHngodxa8XkqIpXk%2Fimg.png" alt="" width="500"/>
            </div>  
            
    -   Sorted Sets
        -   Set 자료형에 스코어라는 개념이 추가
        -   스코어를 기준으로 정렬  
            
            <div align="center">
              <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcrRTRX%2FbtsKLiasliD%2FQDUYgP2hPaoiR7mzfPCFL0%2Fimg.png" alt="" width="500"/>
            </div>  
            
    -   Hashes
        -   key 하나에 여러개의 filed와 value쌍으로 구성
        -   Set 자료형에 문자열로 지정할 수 있는 filed라는 개념 추가 (Set + Filed)
        -   최대 2^32 -1 개의 filed-value 쌍을 저장할 수 있음 (약 40억 개 이상)
        
            <div align="center">
              <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fezvune%2FbtsKJ3lenba%2FTqOiARJqLNow44Cmk9Pt7K%2Fimg.png" alt="" width="500"/>
            </div>
              
            
4.  영속성 제공
    -   인메모리 기반의 데이터베이스로 서버가 내려갔을 때 데이터가 유실되지만  
        다음의 기술들을 통해 영속성을 제공한다
    -   RDB(Redis Database, snapshotting)  
        -   기본적으로 설정되어있는 백업 방식
        -   메모리에 있는 내용을 저장공간에 바이너리 파일로 저장하는 방식
        -   서버를 **재시작**할 때 데이터를 그대로 읽어 **빠른 복구** 가능
    -   AOF(Append Only File)
        -   조회 명령을 제외 연산을 log 파일에 기록
        -   서버를 **재시작할 때 마다** 기록된 연산을 순서대로 **재실행**
5.  복제 및 클러스터 지원
    -   고가용성과 확장성 제공

#### 장점

-   성능
    -   초당 5만~25만의 요청을 처리 가능한 빠른 속도
-   확장성
    -   클러스터 구성을 통한 수평적 확장 가능

#### 단점

-   메모리 제한
    -   사용 가능한 메모리 용량에 제한이 있음
-   데이터 휘발성
    -   인메모리 기반의 저장소로 서버 장애 발생 시 데이터 유실 가능성이 존재

출처

[https://cabi.oopy.io/15321a93-7217-48f4-8c2f-36e8c6550548](https://cabi.oopy.io/15321a93-7217-48f4-8c2f-36e8c6550548)

[https://www.devkuma.com/docs/redis/data-structure/](https://www.devkuma.com/docs/redis/data-structure/)
