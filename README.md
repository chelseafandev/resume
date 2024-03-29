# 이준행

C/C++ & Go 백엔드 개발자  
:link:  https://chelseafandev.github.io  

---

<br>

## About Me

4년차 개발자로 C/C++ 기반의 백엔드 서비스 개발 경험이 있습니다. 대용량의 네트워크 패킷을 분석 및 처리한 경험이 있으며, TCP 기반의 다중 클라이언트 요청 처리를 위한 서버 개발 경험이 있습니다. 다양한 프로젝트에 참여하여 타 부서 혹은 타 업체와의 협업 프로세스를 경험할 수 있었고, 개발자간 혹은 고객과의 커뮤니케이션 능력을 길러왔습니다. 안정적인 서비스를 제공하기 위한 다중화나 자동 복구 솔루션에 관심이 많으며 내가 개발한 서비스가 어떻게 하면 더 효율적으로 제공될 수 있을지에 대해 끊임없이 고민하고 있습니다.

---

<br>

## Skill

### Programming Language

- C/C++
- Go

### IDE

- Visual Studio 2010
- VSCode

### Tools

- Wireshark
- repmgr
- JIRA
- Confluence
- tmux
- tcpreplay
- OpenStack
- GDB

### DBMS

- PostgreSQL
- Oracle
- SQL Server
- Redis
- SQLite

### Libraries

- Poco
- SQLAPI++
- RADVISION SIP Stack
- Boost
- Hyperscan

### Code Management

- SVN
- Git
- GitLab
- Jenkins

---

<br>

## Experience

|**회사명**|**근무기간**|**담당업무**|
|---|:---:|:---:|
|퓨렌스|2016.08.25 ~ 2020.03.31 (3년 7개월)|C/C++ 기반의 컨텍센터 솔루션 개발|
|소만사|2020.09.14 ~ 현재|네트워크 보안 에이전트 개발|

---

<br>

## Ability

### 레거시 코드 개선을 통한 비즈니스 로직 성능 향상

- 기존 레거시 코드에서는 비지니스 로직을 처리하는 쓰레드 내에서 로그 메시지를 파일에 기록하는 작업도 함께 수행하도록 되어있었음
- 상대적으로 매우 느린 작업에 해당하는 파일 I/O가 비즈니스 로직과 동일한 쓰레드에서 처리되므로 성능 하락이 발생함
- 로깅 작업을 별도의 쓰레드에서 처리하여 비즈니스 로직에 영향을 주지 않도록 Logger 클래스를 새롭게 설계하여 도입
- Logger 클래스를 설계하는 과정에서 성능 개선을 위한 고려한 사항들
    - 큐에 저장되는 문자열 객체의 불필요한 복사 제거
      : std::string 타입의 지역 변수가 생명 주기 내에 다른 함수로 전달되는 역할만을 하는 경우에는 rvalue reference를 사용하여 불필요한 문자열 복사를 제거할 수 있음
    - 큐에 로그 메시지 객체를 저장하는 경우, 임시 객체 생성을 피하기 위해 push_back 대신 emplace_back 사용함
    - 메시지 포맷팅 비용을 줄이기 위해 boost::format 객체를 static 처리함

<br>

### 코어 덤프 분석을 위한 가이드라인 제공

- 기존에 엔진이 비정상적으로 종료(Segmentation Fault)되는 현상에 대한 정확한 원인 분석이 이루어지지 않고 있었음
- 위와 같은 이슈를 해결하기 위해 `이슈 대응 방안`에 대한 명확한 가이드 라인을 제공함
- 이슈 담당 엔지니어와의 불필요한 핑퐁과정을 제거하여 보다 신속한 이슈 대응이 가능해짐
- 코어 덤프 분석을 통해 문제 발생 지점을 보다 명확히 확인할 수 있기때문에 추후 재발 방지를 위한 코드 수정이 가능해짐

<br>

### Intel Hyperscan을 활용한 정규표현식 패턴 매칭 라이브러리 제작

- 고성능 다중 정규표현식 매칭 라이브러리인 Intel Hyperscan을 활용하여 특정 도메인 환경에 맞게 정규표현식 패턴 매칭 기능을 사용할 수 있도록 하는 라이브러리 제작  
- 구현 과정에서 중요하게 고려된 사항들
    - Thread-Safe인가?
    - 불필요한 Hyperscan Pattern DB compile을 어떻게 줄일 수 있을까?
    - Hyperscan Pattern DB에 compile이 불가능한 패턴을 어떻게 필터링 할 수 있을까?
- 성능 개선(패턴 검출 속도 향상)을 위한 고려 사항들
    - Hyperscan Pattern DB를 어떻게 분리할 것 인가?
    - 어떻게 하면 Lock 구간의 최소화할 수 있을까?
        - step1. mutex
        : 검출 함수 내에 mutex를 설정함
        - step2. scratch space alloc
        : 검출 함수를 호출할때 마다 scratch space를 새롭게 할당하도록 함
        - step3. scratch space clone by thread
        : 검출 함수를 호출하는 thread id별로 복제(clone)된 scratch space를 관리하도록 함

<br>

### 레거시 모듈을 신규 언어(Golang)로 포팅

- Golang을 사용하여 기존 (Java와 C/C++로 작업된)레거시 코드를 대체할 수 있는 모듈 개발
- 최초 요구사항 분석에서부터 상위 및 세부설계 과정 전체를 경험
- 구현 과정에서 고민했던 부분들
  - Golang 프로젝트를 어떻게 구조화(프로젝트 레이아웃)할 것인가?
  - 모듈과 패키지의 차이점은?
  - 패키지들을 구분하는 기준은?
- REST API 서버 개발(golang)
  - 올바른 REST API 설계 방식에 대한 이해
  - pre-compressed resource
  - API 문서화 방식 with Swagger
  - 확장성을 고려한 구조 설계
- REST API 클라이언트 개발(C++)
  - boost::beast 라이브러리 활용
  - transfer-encoding에 따른 처리(chunked or not)
- Publish/Subscribe 메시징 프로토콜인 MQTT를 활용하여 시그널 처리

<br>

### 공유메모리를 활용한 IPC

- C++ 인스턴스를 공유메모리를 적재하는 방법 with boost::interprocess
  - boost::interprocess::managed_shared_memory(segment) -> 공유 메모리 공간을 할당할 수 있는 영역(할당 영역이 segment의 전체가 될 수도 있고, 일부가 될 수도있음)
  - boost::interprocess::offset_ptr -> segment 공간에 할당된 공유 메모리 객체에 접근할 수 있는 포인터 정보
- 공유메모리에서 container(map) 사용하기
- 공유메모리에 대한 이해
  - 가상메모리
  - /dev/shm 에서 파일이 삭제되는 경우에도 이전에 find() 함수를 통해 offset_ptr 변수에 값을 저장해둔 경우라면 여전히 사용이 가능한데 그 이유는 뭘까?
    - In unix/linux, files are deleted from disk only when all the references to them are closed. A file name in a directory is simply a reference to a file, and so is a (open) file handle in a running program. Even if you delete the file name, the running program still has a handle, so the file is not really deleted - but you can not see it because the file name in the directory is gone.

어떤 문제 혹은 요구사항 있었는지
그것을 해결하기 위해 어떤 접근 방식을 보여주었는지
해결 과정에서 부딪힌 문제점과 극복 방안은 무엇이었는지

---

<br>

## Project

### 신한생명 콜센터 및 비대면 녹취 시스템 구축　　　　　　　2018.11 ~ 2019.06

- 신한생명의 노후화된 녹취 시스템을 IP 녹취 방식으로 통합하는 고도화 프로젝트
- Linux 기반의 녹취 서버 인프라 구축
- 대용량의 네트워크 패킷 분석 및 처리
- 녹취 파일 및 데이터베이스 이중화 프로그램 개발
- 상담 어플리케이션과 녹취 시스템 연동을 위한 TCP 기반의 3rd Party API 개발

<br>

### 신한생명 STT 시스템 구축　　　　　　　2018.05 ~ 2018.09

- 신한생명에서 발생하는 녹취 파일을 대상으로 STT 분석을 위한 시스템 구축
- 녹취 서버에서 STT 분석 서버로 녹취 파일과 녹취 이력 정보를 전송하는 프로그램 개발
- 파일 송수신을 위한 TCP 기반의 자체적인 프로토콜 개발
- Text Analysis 어플리케이션과 녹취 시스템 연동을 위한 HTTP 기반의 3rd Party API 개발

<br>

### 녹취 시스템과 STT 분석 엔진 통합　　　　　　　2017.10 – 2018.02

- ETRI에서 기술 이전을 받아온 STT 솔루션을 자사 녹취 시스템에 탑재하는 프로젝트
- STT 분석 결과를 3가지 형태(형태소, 문장, 전체 텍스트)로 변형하여 저장하는 모듈 개발
- 데이터베이스를 활용한 STT 처리 상태 및 분석 결과 관리 (DBMS: PostgreSQL)
- 녹취 시스템과 음성 인식 기술을 융합하여 상담 업무의 효율성 증대 효과

<br>

### ETRI 음성 인식 솔루션 유지 보수　　　　　　　2017.10 ~ 2018.02

- ETRI에서 기술 이전을 받아온 STT 솔루션 유지 보수(모델 학습, 전사데이터 관리)
- Linux 기반의 언어 및 음향 모델 학습 서버 구축
- 모델 학습 자동화를 위한 쉘 스크립트 제작
- C# 기반의 녹취 파일 전사 툴 개발로 녹취 파일 전사 편의성 제공

<br>

### 에릭슨LG 녹취 시스템 고도화 프로젝트　　　　　　　2017.08 ~ 2018.01

- C 스타일의 구 버전 녹취 시스템을 C++ 기반의 신규 녹취 프로세스로 고도화하는 프로젝트
- IPKTS 메시지 처리 프로세스 모듈화
- SIP 메시지 처리 프로세스 모듈화
- Redis를 활용한 녹취 상태 정보 인터페이스 기능 추가
- repmgr을 활용한 PostgreSQL 이중화 서비스 로직 개발로 DB 동기화 문제 해결

<br>

### 솔루션 상태 및 서버 리소스 모니터링 툴 개발　　　　　　　2017.01 ~ 2017.02

- 리눅스 버전의 자사 녹취 프로세스 상태 관리를 위한 프로그램 개발
- 녹취 프로세스 상태 및 서버 리소스 사용량 체크 기능 개발
- 예약 작업을 위한 스케줄러 기능 개발
- Poco SMTP API를 활용한 알람 메일 발송 모듈 개발
- 기존에 윈도우 환경에서만 제공했던 모니터링 기능을 리눅스로 확장

---

<br>

## Education

### 광운대학교 학사　　　　　　　2010.03 ~ 2016.02

- 로봇학부 지능시스템 전공(부전공: 컴퓨터소프트웨어학과)


---

<br>

[해당 링크](https://blog.shiren.dev/2020-11-23/) 참조

## 주요 역량 항목
- 어떤 프로젝트를 경험했는지
- 어떤 도구/프레임워크를 사용했는지
- 어떤 개발 방법론을 사용했었는지
- 프로젝트에 어떤 책임으로 참여했는지 개발자인지 개발리더인지
- 팀이나 회사에 프로젝트외에 어떤 기여를 했는지
- 프로젝트에서 발생한 기술적인 문제를 어떻게 해결하는지
- 리더쉽, 협업, 커뮤니케이션 스킬

## 이력서
단순히 경험을 구구절절 늘어놓는 게 아니라 역량을 어필할 수 있는 포인트를 하나 이상 적는 것이다.

## 성과
- 효율적인 기술들을 검토해서 스택을 결정하고 팀에 전파
- 난이도가 높은 기술을 사용 했거나 직접 개발 했던 경험
- 개발 환경이나 구조를 구축
- 레거시 코드를 포팅하거나 새로운 코드로 교체
- 월등한(혹은 꽤) 성능 개선
- 익숙하지만 노후된 프레임웍을 본인의 설파와 계몽을 통해 새로운 프레임웍으로 변경
- 본인이 개발한 모듈에 대한 자랑거리(성능, 적은 코드, 구조, 가독성)

## 사전 과제 및 코딩 테스트
- 효율적인 구조
- 컴포넌트나 모듈의 책임과 역할
- 코드의 일관성, 가독성, 유지보수성
- 변수, 함수, 클래스등의 네이밍
- 안티 패턴에 대한 이해(주로 프론트쪽에서 사용하는 용어로 보임)
- 성능에 대한 이해
- 모던 개발 환경에 대한 이해

## 면접
- 각 프로젝트의 참여 범위, 특이점
- 이력서에 명시한 기술들에 대한 이해
- 사용하는 언어에 대한 이해
- 문제 해결 능력
- 협업, 커뮤니케이션에 대한 구체적인 경험
- 리더쉽, 회사, 조직안에서의 영향력
- 업무를 대하는 태도, 업무 처리 및 관리 능력


---

|**프로젝트**|**고객사**|**역할**|**성과**|**기술적인 문제에 대한 해결 방법**|
|---|---|---|---|---|