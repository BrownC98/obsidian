---
created: 2025-05-29T23:37
updated: 2025-05-29T23:37
---
# ASSA 채팅 서버 프로젝트 요건 정의서 (역추출)

## 1. 프로젝트 개요

### 1.1 프로젝트 명
ASSA 채팅 서버 (실시간 채팅 및 WebRTC 영상통화 서버)

### 1.2 개발 환경
- **언어**: Java 20
- **빌드 도구**: Maven 3.x
- **데이터베이스**: MySQL 8.x
- **로깅**: Apache Log4j2 2.24.3
- **JSON 처리**: Google Gson 2.13.1
- **네트워킹**: Java Socket Programming

### 1.3 프로젝트 목적
ASSA 소셜 플랫폼의 실시간 채팅 및 영상통화 기능을 제공하는 독립적인 소켓 서버로, Laravel 백엔드와 연동하여 실시간 메시징, 채팅방 관리, WebRTC 기반 영상통화 시그널링을 담당

## 2. 시스템 아키텍처

### 2.1 서버 구조
- **멀티스레드 소켓 서버**: 각 클라이언트 연결마다 독립적인 스레드 할당
- **싱글톤 패턴**: DBHelper를 통한 데이터베이스 연결 관리
- **동시성 처리**: ConcurrentHashMap, CopyOnWriteArrayList 사용
- **성능 모니터링**: 실시간 성능 추적 및 로깅

### 2.2 핵심 컴포넌트
- **ChatServer**: 메인 서버 클래스, 클라이언트 연결 관리
- **User**: 개별 사용자 연결 및 세션 관리
- **ChatRoom**: 채팅방 관리 및 메시지 브로드캐스팅
- **VideoRoom**: WebRTC 영상회의 방 관리
- **DBHelper**: 데이터베이스 연동 및 쿼리 처리
- **MessageHandler**: 메시지 처리 및 라우팅

## 3. 기능별 요구사항

### 3.1 서버 연결 관리

#### 3.1.1 클라이언트 연결
- **소켓 연결 수락**: 포트 12345에서 클라이언트 연결 대기
- **멀티스레드 처리**: 각 클라이언트마다 독립적인 User 스레드 생성
- **연결 상태 추적**: 연결 시간, 메시지 수, 처리 시간 등 세션 정보 관리
- **자동 정리**: 연결 해제 시 리소스 자동 정리

#### 3.1.2 사용자 세션 관리
- **세션 ID 생성**: 각 연결마다 고유한 세션 ID 할당
- **사용자 인증**: CONNECT 명령을 통한 사용자 ID 매핑
- **소켓 교체**: 재연결 시 기존 세션 유지
- **오프라인 사용자**: 초대 등을 위한 오프라인 사용자 객체 지원

### 3.2 채팅 기능

#### 3.2.1 채팅방 관리
- **채팅방 생성**: CREATE_ROOM 명령을 통한 새 채팅방 생성
- **방 정보 조회**: ROOM_INFO 명령을 통한 채팅방 상세 정보 제공
- **방 나가기**: EXIT_ROOM 명령을 통한 채팅방 퇴장
- **방 삭제**: 마지막 사용자 퇴장 시 자동 방 삭제

#### 3.2.2 메시지 처리
- **실시간 메시징**: SEND_MESSAGE 명령을 통한 즉시 메시지 전송
- **메시지 브로드캐스팅**: 채팅방 내 모든 참여자에게 메시지 전파
- **메시지 저장**: 데이터베이스에 메시지 영구 저장
- **읽음 상태 관리**: 메시지별 읽음/안읽음 상태 추적

#### 3.2.3 사용자 초대
- **초대 기능**: INVITE 명령을 통한 채팅방 사용자 초대
- **오프라인 초대**: 접속하지 않은 사용자도 초대 가능
- **초대 알림**: 초대받은 사용자에게 실시간 알림

#### 3.2.4 메시지 수신 확인
- **수신 확인**: CHECK_RECEIVE 명령을 통한 메시지 수신 상태 확인
- **미전송 메시지**: 전송 실패한 메시지 재전송 지원
- **상태 업데이트**: 메시지 전송 상태 실시간 업데이트

### 3.3 WebRTC 영상통화

#### 3.3.1 영상회의 방 관리
- **영상방 생성**: CREATE_VIDEO_ROOM 명령을 통한 새 영상회의 방 생성
- **영상방 참가**: JOIN_VIDEO_ROOM 명령을 통한 영상회의 참가
- **영상방 퇴장**: EXIT_VIDEO_ROOM 명령을 통한 영상회의 퇴장
- **참가자 조회**: GET_VIDEO_ROOM_PARTICIPANT 명령을 통한 참가자 목록 제공

#### 3.3.2 WebRTC 시그널링
- **SDP 교환**: SDP 명령을 통한 세션 설명 프로토콜 중계
- **ICE 후보 교환**: ICE_CANDIDATE 명령을 통한 연결 후보 정보 중계
- **미디어 상태**: MEDIA_STATUS 명령을 통한 오디오/비디오 상태 관리
- **P2P 연결 지원**: 클라이언트 간 직접 연결을 위한 시그널링 서버 역할

#### 3.3.3 영상방 상태 관리
- **호스트 관리**: 첫 번째 참가자를 호스트로 자동 지정
- **호스트 변경**: 호스트 퇴장 시 다음 참가자로 자동 이전
- **방 정리**: 모든 참가자 퇴장 시 영상방 자동 삭제
- **ICE 후보 추적**: 참가자별 ICE 후보 교환 상태 관리

### 3.4 데이터베이스 연동

#### 3.4.1 채팅 데이터 관리
- **채팅방 정보**: 채팅방 생성, 수정, 삭제 처리
- **사용자-채팅방 매핑**: 채팅방 참여자 관계 관리
- **메시지 저장**: 채팅 메시지 영구 저장 및 조회
- **읽음 상태**: 메시지별 사용자 읽음 상태 관리

#### 3.4.2 서버 데이터 복구
- **서버 재시작 지원**: 서버 재시작 시 기존 채팅방 정보 복구
- **사용자 매핑 복구**: 채팅방별 참여자 정보 자동 로드
- **메시지 히스토리**: 채팅방별 메시지 히스토리 제공

#### 3.4.3 성능 최적화
- **연결 풀링**: 데이터베이스 연결 재사용
- **쿼리 최적화**: PreparedStatement 사용으로 성능 향상
- **트랜잭션 관리**: 데이터 일관성 보장

### 3.5 명령어 시스템

#### 3.5.1 명령어 처리
- **JSON 기반**: 모든 명령어는 JSON 형태로 송수신
- **액션 기반 라우팅**: Action enum을 통한 명령어 분류
- **요청-응답 패턴**: 각 요청에 대한 적절한 응답 제공
- **에러 처리**: 잘못된 명령어에 대한 에러 응답

#### 3.5.2 지원 명령어
- **CONNECT**: 사용자 연결 및 인증
- **CREATE_ROOM**: 채팅방 생성
- **EXIT_ROOM**: 채팅방 퇴장
- **ROOM_INFO**: 채팅방 정보 조회
- **SEND_MESSAGE**: 메시지 전송
- **CHECK_RECEIVE**: 메시지 수신 확인
- **INVITE**: 사용자 초대
- **CREATE_VIDEO_ROOM**: 영상회의 방 생성
- **JOIN_VIDEO_ROOM**: 영상회의 참가
- **EXIT_VIDEO_ROOM**: 영상회의 퇴장
- **SDP**: WebRTC SDP 교환
- **ICE_CANDIDATE**: WebRTC ICE 후보 교환
- **MEDIA_STATUS**: 미디어 상태 관리

## 4. 기술적 요구사항

### 4.1 성능 요구사항
- **동시 접속자**: 1000명 이상 동시 접속 지원
- **메시지 처리**: 실시간 메시지 전송 (100ms 이내)
- **메모리 관리**: 자동 메모리 모니터링 및 최적화
- **쿼리 성능**: 데이터베이스 쿼리 100ms 이내 처리

### 4.2 안정성 요구사항
- **예외 처리**: 모든 예외 상황에 대한 적절한 처리
- **연결 복구**: 네트워크 장애 시 자동 복구 지원
- **데이터 일관성**: 트랜잭션을 통한 데이터 무결성 보장
- **리소스 정리**: 연결 해제 시 자동 리소스 정리

### 4.3 확장성 요구사항
- **모듈화**: 기능별 클래스 분리로 유지보수성 향상
- **설정 관리**: Properties 파일을 통한 외부 설정
- **로깅 시스템**: 상세한 로깅을 통한 디버깅 지원
- **성능 모니터링**: 실시간 성능 지표 수집

### 4.4 보안 요구사항
- **입력 검증**: 모든 클라이언트 입력에 대한 검증
- **SQL 인젝션 방지**: PreparedStatement 사용
- **민감정보 보호**: 로그에서 민감정보 마스킹
- **연결 제한**: 비정상적인 연결 시도 차단

## 5. 데이터베이스 설계

### 5.1 주요 테이블 연동
- **chat_rooms**: 채팅방 정보
- **user_chatroom_map**: 사용자-채팅방 매핑
- **messages**: 채팅 메시지
- **message_read_status**: 메시지 읽음 상태
- **users**: 사용자 정보 (Laravel과 공유)

### 5.2 데이터 동기화
- **Laravel 연동**: Laravel 백엔드와 데이터베이스 공유
- **실시간 동기화**: 채팅방 생성/삭제 시 즉시 반영
- **상태 일관성**: 양방향 데이터 동기화 보장

## 6. 외부 연동

### 6.1 Laravel 백엔드 연동
- **HTTP API**: Laravel에서 채팅방 생성 요청 수신
- **데이터베이스 공유**: 동일한 MySQL 데이터베이스 사용
- **사용자 인증**: Laravel의 사용자 ID 체계 활용

### 6.2 클라이언트 연동
- **WebSocket 대안**: TCP 소켓을 통한 실시간 통신
- **JSON 프로토콜**: 표준화된 JSON 메시지 형식
- **크로스 플랫폼**: 다양한 클라이언트 플랫폼 지원

## 7. 운영 및 모니터링

### 7.1 로깅 시스템
- **구조화된 로깅**: 검색 가능한 형태의 로그 기록
- **성능 로깅**: 메시지 처리 시간, 데이터베이스 쿼리 시간 추적
- **에러 추적**: 상세한 에러 정보 및 스택 트레이스
- **사용자 활동**: 연결, 메시지, 방 참여 등 사용자 활동 로깅

### 7.2 성능 모니터링
- **실시간 지표**: 동시 접속자 수, 메시지 처리량 등
- **리소스 모니터링**: 메모리 사용량, CPU 사용률 추적
- **응답 시간**: 명령어별 처리 시간 측정
- **주기적 리포트**: 5분마다 성능 리포트 생성

### 7.3 장애 대응
- **자동 복구**: 네트워크 장애 시 자동 재연결 시도
- **그레이스풀 셧다운**: 서버 종료 시 안전한 연결 정리
- **백업 및 복구**: 데이터베이스 백업을 통한 데이터 보호

## 8. 제약사항 및 비즈니스 규칙

### 8.1 기술적 제약사항
- **Java 20 이상**: 최신 Java 기능 활용
- **MySQL 연동**: Laravel과 동일한 데이터베이스 사용
- **포트 12345**: 고정 포트 사용
- **UTF-8 인코딩**: 모든 텍스트 데이터 UTF-8 처리

### 8.2 비즈니스 규칙
- **실시간 처리**: 모든 메시지는 즉시 전송
- **데이터 영속성**: 모든 채팅 메시지 영구 저장
- **사용자 프라이버시**: 개인정보 보호 준수
- **확장성 고려**: 향후 기능 확장 가능한 구조

### 8.3 운영 제약사항
- **24/7 운영**: 무중단 서비스 제공
- **백업 정책**: 정기적인 데이터 백업
- **모니터링**: 실시간 서버 상태 모니터링
- **로그 관리**: 로그 파일 크기 및 보관 정책

이 요건 정의서는 assa-chatserver 프로젝트의 실제 구현된 코드를 분석하여 역으로 추출한 것으로, 현재 구현된 모든 기능과 아키텍처를 포함하고 있습니다. 