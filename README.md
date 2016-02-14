# 제목 
- Hadoop : The Definition Guide 3E

# 3. 하둡 분산 파일 시스템

## 3.1 HDFS 설계 
- 매우 큰 파일
- 데이터 엑세스를 스트리밍 방식으로 지원
- 범용 하드웨어
- 빠른 응답시간의 데이터 엑세스
- 많은 순의 작은 파일
- 다중 라이터, 임의의 파일 수정

## 3.2 HDFS개념

### 3.2.1 블록
- HDFS의 블록은 64MB보다 더 큰 단위.
- HDFS의 파일은 블록크기의 청크로 쪼개져서 독립적인 단위로 저장됨.
- 맵태스크는 기본적으로 한번에 하나의 블록을 처리함.
- HDFS에서는 블록을 추상화 함으로써 장점이 생긴다.
  - 한개의 디스크용량보다 더 큰 파일을 저장할 수 있다.
  - 장애대응을 위하 리플리케이션을 효율적으로 수행할 수 있다.

### 3.2.2 네임노드와 데이터노드
- 마스터-워커 패턴. 네임노드, 데이터노드.
- 데이터노드에 저장된 블록에 대한 메타정보는 네임노드에 저장된다.
- 네임노드에 장애가나면 메타정보를 잃어버려서, 더이상 클러스터를 사용 할 수 없게된다.
- 장애복구를 위한 메커니즘
  - 메타데이터의 지속상태를 보완해주는 파일을 백업
  - 보조 네임노드를 운영 (보조 네임노드가 마스터 역할을 하는게 아니다.)

### 3.2.3 HDFS 통합
- 메모리 기반의 네임노드는 확장성에 큰 걸림돌.
- 2.x 릴리즈에에는 각각의 네임노드가 파일시스템의 네임스페이스 일부를 관리하는 방식이 소개됨.

### 3.2.4 HDFS 고가용성
- 네임노드의 장애는 SPOF
- 하둡 2.x 릴리즈부터는 한쌍의 네임노드를 active/standby 로 구성해서 HA를 지원한다.
- failover
- fencing : 네임노드의 프로세스 강제종료, 공유 스토리지 접근 취소, 네트워크 포트 불능화.

## 3.3. 명령행 인터페이스
- 3.3.1 기본적인 파일시스템 연산

## 3.4 하둡 파일시스템
- 하둡은 파일시스템의 추상화 개념을 가지고 있고, HDFS는 단지 그 개념에 대한 구현일 뿐.

### 3.4.1 인터페이스
- HTTP
  - HDFS데몬이 HTTP 요청을 직접 처리
  - 프락시로 간접적으로 처리. (방화벽, 데역폭등의 제한 정책 활용 가능.)

## 3.5 자바 인터페이스

- 하둡 URL로부터 데이터 읽기
- 파일시스템 API를 사용하여 데이터 읽기
- 데이터 쓰기
- 디렉터리
- 파일시스템에 질의하기
- 데이터 삭제

## 3.6 데이터 흐름

### 3.6.1 파일 읽기 해부

### 3.6.2 파일 쓰기 상세

### 3.6.3 일관성 모델

## 3.7 데이터 이관을 위한 플룸과 스쿱
- 대규모 스트리밍 데이터를 HDFS로 이동
- RDB와 같이 구조화된 데이터 저장소로부터 HDFS로 데이터를 임포트

## 3.8 distcp 병렬 복사
- 클러스터1에서 클러스터2로 데이터를 복사할때 사용.

## 3.9 하둡 아카이브
- 블록크기보다 작은 파일들이 많으면 효율적이지 않음
- HAR

## 3.9.1 하둡 아카이브 사용하기
- HAR

## 3.9.2 제약사항
- HAR내에 일부만 추가/삭제할 수 없다.
- 변하지 않는 파일만 HAR를 사용하자.


# 4. 하둡 I/O

## 4.1 데이터 무결성

### 4.1.1 HDFS와 데이터 무결성
- HDFS는 모든 데이터를 쓰는 과정에서 체크섬을 계산한다.
- 별도로 주기적으로 백그라운드에서 블록스캐너를 수행한다.

### 4.1.2 LocalFileSystem

### 4.1.3 ChecksumFileSystem

## 4.2 압축
- 저장공간을 감소시키고, 네트워크 데이터 전송을 고속화
- gzip, bzip, LZO...

### 4.2.1 코덱
- 압축-해제 알고리즘의 구현.
- CompressionCodec

### 4.2.2 압축과 입력 분할

### 4.2.3 맵리듀스에서 압축 사용하기
- 비압축 데이터를 읽는 경우에도 중간출력을 압축하는게 유리하다.

## 4.3. 직렬화
- 직렬화 : 네트워크 전송을 위해 구조화된 객체를 바이트 스트림으로 전환하는 과정
- 역직렬화 : 바이트 스트림을 일련의 구조화된 객체로 역전환하는 과정이다.

### 4.3.1 Writable 인터페이스
- Writable
- WritableComparable

### 4.3.2 Writable 클래스
- Text
- BytesWritable
- NullWritable
- ObjectWritable, GenericWritable
- Writable 콜렉션

### 4.3.3 Custom Writable

# 5 맵리듀스 프로그래밍

## 5.1 환경 설정 API
- key-value 구조로 정의된 XML파일로 부터 속성값을 읽는다.
- xml 파일을 여러개 add하면 override 하는 방식으로 추가된다. (final 제외)
- 시스템속성은 xml 속성보다 우선순위가 높다. 
- 환경설정에 있는 값은 시스셈속성으로 override가능. 환경설정에 없는 값이면 불가능.

## 5.2 개발 환경 설정하기
- POM.xml

## 5.3 MRUnit으로 유닛 테스트 작성하기
- MRUnit, MapDriver

## 5.4 로컬에서 실행하기
- Tool 인터페이스 사용
- 파서 클래스를 별도의 클래스로 빼라.

## 5.5 클러스터에서 실행하기
- 패키징
- 배포
- 맵리듀스 웹 UI
- 결과 얻기 part-r-xxxxx
- 디버깅
- 하둡로그

## 5.6 잡 튜닝하기
- 튜닝 검사 리스트 : 매퍼개수, 리듀서개수, 컴바이너, 중간데이터압축, 맞춤형직렬화, 셔플꼬임
- 태스트 프로파일링

## 5.7 맵리듀스 작업 흐름
- 일반적으로 더욱 복잡한 맵/리듀스 함수를 만드는 것보단, 더 많은 맵/리듀스를 수행하는게 복잡성을 명쾌하게 해결 가능.
- 복잡한 문제해결을 위해서는 pig, hive 등의 고수준 언어 사용을 고려하자.
- ChainMapper, ChainReducer
- oozie


# 6. 맵리듀스 작동방법

## 6.1 맵리듀스 잡 실행 상세분석
- MR1
  - 클라이언트
  - 잡트래커
  - 태스크드래커
  - 분산파일시스템
- YARN
  - 클라이언트
  - 얀 리소스 매니저
  - 얀 노드 매니저
  - 맵리듀스 애플리케이션 마스터
  - 분산 파일시스템

## 6.2 실패
- MR1
  - 태스크 실패
  - 태스크트래커 실패
  - 잡트래커 실패
- YARN
  - 태스크실패
  - 애플리케이션 마스터 실패
  - 노드 매니저 실패
  - 리소스 매니저 실패

## 6.3 잡 스케줄링
- FIFO 스케줄러
- fair 스케줄러
- capacity 스케줄러

## 6.4 셔플과 정렬
- 맵
- 리듀스
- 설정 조정

# 6.5 태스크 실행
- 태스크 실행 환경. MR시 참고할 수 있는 속성값들을 제공한다. 
- 투기적 실행. 예상했던 것보다 태스크 수행이 더 느린경우 또다른 동일한 예비태스크를 실행. 
- 출력 커미터. 
- 태스크 JVM 재사용
- 비정상 레코드 생략하기. 생략모드를 사용하면 두번 실패한 후에만 켜진다.

# 7. 맵리듀스 타입과 포맷

## 7.1 맵리듀스 타입
- 맵의 출력과 리듀서의 입력 타입은 동일해야한다.
- 기본 separator는 tab분자

## 7.2 입력 포맷
- 입력 스플릿과 레코드
  - FileInputFormat
  - 하둡은 많은 수의 작은 파일보다, 적은 수의 큰 파일을 처리할 때 더 좋은 성능을 낸다.
- 텍스트 입력
  - TextInputFormat
  - KeyValueTextInputForamt
  - NLineInputFormat
  - XML
- 바이너리 입력
  - SequenceFileInputFormat
  - SequenceFileAsTextInputFormat
  - SequenceFileAsBinaryInputFormat
- 다중 입력. MultipleInputs

## 7.3 출력 포맷
- 텍스트 출력
  - TextOutputFormat
- 바이너리 출력
  - SequenceFileOutputFormat
  - SequenceFileAsBinaryOutputFormat
- 다중 출력
  - MultipleOutputs
- 느린 출력
  - empty file 생성 방지. 


# 8 맵리듀스 기능

## 8.1 카운터
- 태스크 카운터. 태스크 실행 시간 동안 해당 태스크에 관련된 정보수집. 
- 잡 카운터. 잡 수준의 통계값을 측정.
- 사용자 정의 자바 카운터. (정적, 다이나믹)


## 8.2 정렬
- 부분정렬
- 전체정렬
- 보조정렬

## 8.3 조인

## 8.4 사이드 데이터 분배
