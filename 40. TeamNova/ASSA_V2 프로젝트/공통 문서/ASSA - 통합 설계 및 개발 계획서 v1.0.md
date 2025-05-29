---
created: 2025-05-29T23:28
updated: 2025-05-29T23:33
---
알겠습니다. 말씀하신 대로, 모든 LLM이 참조할 수 있는 핵심 문서인 **"ASSA - 통합 설계 및 개발 계획서 v1.0"** 을 작성해 드리겠습니다. 이 문서는 프로젝트의 청사진 역할을 하며, 각 LLM이 자신의 역할을 이해하고 다른 컴포넌트와 효과적으로 협업하는 데 필요한 정보를 담고 있습니다.

---

**ASSA - 통합 설계 및 개발 계획서 v1.0**

**문서 버전:** 1.0  
**최종 수정일:** 2023-10-27 (예시 날짜)  
**작성자:** [사용자 이름 또는 팀 이름]

**목차**

1. [프로젝트 개요](https://www.google.com/url?sa=E&q=%231-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EA%B0%9C%EC%9A%94)  
    1.1. [프로젝트명](https://www.google.com/url?sa=E&q=%2311-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EB%AA%85)  
    1.2. [프로젝트 목적 및 목표](https://www.google.com/url?sa=E&q=%2312-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%AA%A9%EC%A0%81-%EB%B0%8F-%EB%AA%A9%ED%91%9C)  
    1.3. [기대 효과](https://www.google.com/url?sa=E&q=%2313-%EA%B8%B0%EB%8C%80-%ED%9A%A8%EA%B3%BC)
    
2. [시스템 아키텍처](https://www.google.com/url?sa=E&q=%232-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98)  
    2.1. [전체 시스템 구성도](https://www.google.com/url?sa=E&q=%2321-%EC%A0%84%EC%B2%B4-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%84%B1%EB%8F%84)  
    2.2. [컴포넌트별 역할](https://www.google.com/url?sa=E&q=%2322-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%B3%84-%EC%97%AD%ED%95%A0)  
    2.2.1. [Android 클라이언트](https://www.google.com/url?sa=E&q=%23221-android-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8)  
    2.2.2. [PHP 백엔드 서버 (API 서버)](https://www.google.com/url?sa=E&q=%23222-php-%EB%B0%B1%EC%97%94%EB%93%9C-%EC%84%9C%EB%B2%84-api-%EC%84%9C%EB%B2%84)  
    2.2.3. [Java 실시간 소켓 서버](https://www.google.com/url?sa=E&q=%23223-java-%EC%8B%A4%EC%8B%9C%EA%B0%84-%EC%86%8C%EC%BC%93-%EC%84%9C%EB%B2%84)
    
3. [기술 스택](https://www.google.com/url?sa=E&q=%233-%EA%B8%B0%EC%88%A0-%EC%8A%A4%ED%83%9D)  
    3.1. [Android 클라이언트](https://www.google.com/url?sa=E&q=%2331-android-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8)  
    3.2. [PHP 백엔드 서버](https://www.google.com/url?sa=E&q=%2332-php-%EB%B0%B1%EC%97%94%EB%93%9C-%EC%84%9C%EB%B2%84)  
    3.3. [Java 실시간 소켓 서버](https://www.google.com/url?sa=E&q=%2333-java-%EC%8B%A4%EC%8B%9C%EA%B0%84-%EC%86%8C%EC%BC%93-%EC%84%9C%EB%B2%84)  
    3.4. [공통 (데이터베이스, 인프라 등)](https://www.google.com/url?sa=E&q=%2334-%EA%B3%B5%ED%86%B5-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%9D%B8%ED%94%84%EB%9D%BC-%EB%93%B1)
    
4. [데이터베이스 설계](https://www.google.com/url?sa=E&q=%234-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%84%A4%EA%B3%84)  
    4.1. [ERD (Entity Relationship Diagram)](https://www.google.com/url?sa=E&q=%2341-erd-entity-relationship-diagram)  
    4.2. [테이블 명세](https://www.google.com/url?sa=E&q=%2342-%ED%85%8C%EC%9D%B4%EB%B8%94-%EB%AA%85%EC%84%B8) (상세 스키마는 별도 문서 또는 이전 답변 참조)
    
5. [API 명세 (PHP 백엔드)](https://www.google.com/url?sa=E&q=%235-api-%EB%AA%85%EC%84%B8-php-%EB%B0%B1%EC%97%94%EB%93%9C)  
    5.1. [공통 규칙](https://www.google.com/url?sa=E&q=%2351-%EA%B3%B5%ED%86%B5-%EA%B7%9C%EC%B9%99)  
    5.2. [주요 엔드포인트 요약](https://www.google.com/url?sa=E&q=%2352-%EC%A3%BC%EC%9A%94-%EC%97%94%EB%93%9C%ED%8F%AC%EC%9D%B8%ED%8A%B8-%EC%9A%94%EC%95%BD) (상세 명세는 별도 OpenAPI 문서 또는 이전 답변 참조)
    
6. [실시간 소켓 메시지 프로토콜 (Java 소켓 서버)](https://www.google.com/url?sa=E&q=%236-%EC%8B%A4%EC%8B%9C%EA%B0%84-%EC%86%8C%EC%BC%93-%EB%A9%94%EC%8B%9C%EC%A7%80-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-java-%EC%86%8C%EC%BC%93-%EC%84%9C%EB%B2%84)  
    6.1. [공통 메시지 구조](https://www.google.com/url?sa=E&q=%2361-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%8B%9C%EC%A7%80-%EA%B5%AC%EC%A1%B0)  
    6.2. [주요 액션 타입 및 페이로드 요약](https://www.google.com/url?sa=E&q=%2362-%EC%A3%BC%EC%9A%94-%EC%95%A1%EC%85%98-%ED%83%80%EC%9E%85-%EB%B0%8F-%ED%8E%98%EC%9D%B4%EB%A1%9C%EB%93%9C-%EC%9A%94%EC%95%BD) (상세 프로토콜은 별도 문서 또는 이전 답변 참조)
    
7. [주요 데이터 모델 (DTOs/VOs)](https://www.google.com/url?sa=E&q=%237-%EC%A3%BC%EC%9A%94-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%AA%A8%EB%8D%B8-dtosvos)
    
8. [개발 우선순위 및 MVP 정의](https://www.google.com/url?sa=E&q=%238-%EA%B0%9C%EB%B0%9C-%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%EB%B0%8F-mvp-%EC%A0%95%EC%9D%98)  
    8.1. [MVP 기능 목록](https://www.google.com/url?sa=E&q=%2381-mvp-%EA%B8%B0%EB%8A%A5-%EB%AA%A9%EB%A1%9D)  
    8.2. [단계별 개발 계획 (Phase)](https://www.google.com/url?sa=E&q=%2382-%EB%8B%A8%EA%B3%84%EB%B3%84-%EA%B0%9C%EB%B0%9C-%EA%B3%84%ED%9A%8D-phase)
    
9. [공통 개발 지침](https://www.google.com/url?sa=E&q=%239-%EA%B3%B5%ED%86%B5-%EA%B0%9C%EB%B0%9C-%EC%A7%80%EC%B9%A8)  
    9.1. [TDD (Test-Driven Development)](https://www.google.com/url?sa=E&q=%2391-tdd-test-driven-development)  
    9.2. [Git 전략 및 커밋 컨벤션](https://www.google.com/url?sa=E&q=%2392-git-%EC%A0%84%EB%9E%B5-%EB%B0%8F-%EC%BB%A4%EB%B0%8B-%EC%BB%A8%EB%B2%A4%EC%85%98)  
    9.3. [코딩 스타일 가이드](https://www.google.com/url?sa=E&q=%2393-%EC%BD%94%EB%94%A9-%EC%8A%A4%ED%83%80%EC%9D%BC-%EA%B0%80%EC%9D%B4%EB%93%9C)  
    9.4. [에러 처리 및 로깅](https://www.google.com/url?sa=E&q=%2394-%EC%97%90%EB%9F%AC-%EC%B2%98%EB%A6%AC-%EB%B0%8F-%EB%A1%9C%EA%B9%85)
    
10. [용어 정의](https://www.google.com/url?sa=E&q=%2310-%EC%9A%A9%EC%96%B4-%EC%A0%95%EC%9D%98)
    

---

## 1. 프로젝트 개요

### 1.1. 프로젝트명

ASSA (Android Social & Streaming Application) - 차세대 플랫폼 구축

### 1.2. 프로젝트 목적 및 목표

- **목적:** 기존 ASSA 서비스의 핵심 기능을 계승하면서, 최신 기술 스택과 개선된 아키텍처를 적용하여 확장성, 유지보수성, 성능이 뛰어난 새로운 소셜 및 스트리밍 플랫폼을 구축한다.
    
- **목표:**
    
    - 안정적이고 반응성이 뛰어난 사용자 경험 제공.
        
    - 실시간 상호작용(채팅, 영상통화) 기능 강화.
        
    - 개발 생산성 향상을 위해 LLM을 활용한 "Vive Coding" 방식 적극 도입.
        
    - 향후 기능 확장이 용이한 모듈화된 구조 설계.
        

### 1.3. 기대 효과

- 사용자 만족도 향상 및 서비스 경쟁력 강화.
    
- 운영 비용 절감 및 개발 효율성 증대.
    
- 새로운 비즈니스 모델 도입을 위한 기술적 기반 마련.
    

## 2. 시스템 아키텍처

### 2.1. 전체 시스템 구성도

      `+---------------------+      HTTP/S (REST API)     +-----------------------+ |                     | <------------------------> |                       | |  Android 클라이언트  |                            |  PHP 백엔드 서버      | |  (Java / Kotlin)    |                            |  (Laravel, API 서버)  | |                     | <------------------------> |                       | +---------------------+      WebSocket / TCP       +-----------------------+        ^     ^                                             |         ^        |     |                                             | MySQL   |        |     +-------------------------------------------->+---------+--+        |                                                   | 데이터베이스 |        +-------------------------------------------------->+---------+--+                              WebSocket / TCP                     ^                                    ^                             |                                    |                             |                           +-----------------------+              |                           |  Java 실시간 소켓 서버 |              |                           |  (Netty / Spring)     |---------------+                           +-----------------------+  [기타 외부 서비스] - FCM (Push Notifications) - OAuth Providers (Kakao, Google 등) - File Storage (S3, Local 등)`
    

### 2.2. 컴포넌트별 역할

#### 2.2.1. Android 클라이언트

- 사용자 인터페이스(UI) 및 사용자 경험(UX) 제공.
    
- PHP 백엔드 서버와 REST API 통신 (데이터 요청/전송, 인증).
    
- Java 실시간 소켓 서버와 WebSocket/TCP 통신 (실시간 채팅, WebRTC 시그널링).
    
- 로컬 데이터 저장 (캐시, 사용자 설정, 채팅 기록 등).
    
- 푸시 알림 수신 및 처리.
    

#### 2.2.2. PHP 백엔드 서버 (API 서버)

- 주요 비즈니스 로직 처리.
    
- RESTful API 제공 (사용자 인증, 게시물 관리, 소셜 기능, 채팅방 관리 등).
    
- 데이터베이스(MySQL)와 연동하여 데이터 영속성 관리.
    
- 파일 업로드 처리 및 관리.
    
- 필요시 Java 소켓 서버와 내부 통신 (예: 특정 이벤트 발생 시 알림).
    
- 외부 서비스 연동 (OAuth, 결제 등).
    

#### 2.2.3. Java 실시간 소켓 서버

- 클라이언트 간 실시간 양방향 통신 채널 제공 (WebSocket/TCP).
    
- 실시간 채팅 메시지 중계 및 브로드캐스팅.
    
- WebRTC 시그널링 메시지 중계 (P2P 연결 설정 지원).
    
- 사용자 접속 상태 관리 (온라인/오프라인).
    
- 채팅 관련 데이터 (메시지, 채팅방 정보 등)를 데이터베이스에 저장/조회.
    
- 실시간 이벤트 처리 및 알림.
    

## 3. 기술 스택

### 3.1. Android 클라이언트

- **언어:** Java (또는 Kotlin 고려)
    
- **아키텍처:** MVVM (Model-View-ViewModel) with Android Jetpack
    
- **네트워크:** Retrofit2, OkHttp3
    
- **실시간 통신:** OkHttp WebSocket Client (또는 java-websocket)
    
- **이미지 로딩:** Glide
    
- **로컬 DB:** Room
    
- **최소 지원 버전:** Android 6.0 (API 23) Marshmallow
    

### 3.2. PHP 백엔드 서버

- **프레임워크:** Laravel (최신 LTS 버전)
    
- **데이터베이스:** MySQL 8.x
    
- **인증:** JWT (tymon/jwt-auth 또는 Laravel Sanctum/Passport)
    
- **API 문서화:** Swagger/OpenAPI (자동 생성 도구 활용 권장)
    

### 3.3. Java 실시간 소켓 서버

- **언어:** Java (최신 LTS 버전, 예: Java 17 또는 21)
    
- **네트워킹:** Netty (또는 Spring WebFlux WebSocket)
    
- **JSON 처리:** Jackson
    
- **빌드 도구:** Maven (또는 Gradle)
    
- **데이터베이스 연동:** JPA/Hibernate (또는 Spring Data JPA)
    

### 3.4. 공통 (데이터베이스, 인프라 등)

- **데이터베이스:** MySQL 8.x
    
- **푸시 알림:** Firebase Cloud Messaging (FCM)
    
- **버전 관리:** Git
    
- **인프라:** (배포 환경에 따라 명시 - 예: AWS, Docker, Kubernetes)
    

## 4. 데이터베이스 설계

### 4.1. ERD (Entity Relationship Diagram)

(ERD 이미지를 첨부하거나, 주요 관계를 텍스트로 설명합니다. 이전 답변의 SQL 스키마를 기반으로 ERD를 그릴 수 있습니다.)

**예시 (주요 엔티티 및 관계):**

- Users (1) -- (N) Posts
    
- Users (1) -- (N) Comments
    
- Posts (1) -- (N) Comments
    
- Comments (1) -- (N) Replies
    
- Users (M) -- (N) Users (via Follows, Friends, Blocked_Users)
    
- Users (M) -- (N) Chat_Rooms (via Chat_Room_Members)
    
- Chat_Rooms (1) -- (N) Messages
    
- Users (1) -- (N) Messages
    
- Users (1) -- (N) Notifications
    

### 4.2. 테이블 명세

- 상세 테이블 스키마는 **[별도 문서 링크 또는 이전 답변의 SQL 스키마 섹션 참조]** 하십시오.
    
- 주요 테이블: users, oauth_providers, posts, post_images, post_likes, comments, comment_likes, replies, reply_likes, follows, friends, blocked_users, chat_rooms, chat_room_members, messages, message_read_status, video_call_sessions, video_call_participants, notifications, user_notification_settings.
    

## 5. API 명세 (PHP 백엔드)

### 5.1. 공통 규칙

- **Base URL:** /api/v1
    
- **인증:** Authorization: Bearer <JWT_TOKEN> (필요시)
    
- **요청/응답 형식:** JSON
    
- **성공 응답:** { "success": true, "message": "...", "data": { ... } }
    
- **실패 응답:** { "success": false, "message": "...", "errors": { ... } }
    
- **페이지네이션:** page, per_page 쿼리 파라미터, 응답에 meta 포함.
    

### 5.2. 주요 엔드포인트 요약

- 상세 API 엔드포인트 목록 및 명세는 **[별도 OpenAPI 문서 링크 또는 이전 답변의 API 명세 섹션 참조]** 하십시오.
    
- **주요 모듈:** Auth, Users, Posts, Comments, Replies, Follows, Friends, Blocked Users, Chat Rooms, Notifications, Files.
    

## 6. 실시간 소켓 메시지 프로토콜 (Java 소켓 서버)

### 6.1. 공통 메시지 구조

      `{     "action": "ACTION_TYPE",     "payload": { ... },     "seq_id": "client_generated_uuid" // 선택적 }`
    

IGNORE_WHEN_COPYING_START

content_copy download

Use code [with caution](https://support.google.com/legal/answer/13505487). Json

IGNORE_WHEN_COPYING_END

### 6.2. 주요 액션 타입 및 페이로드 요약

- 상세 소켓 메시지 프로토콜(액션 타입, 페이로드 구조)은 **[별도 프로토콜 정의 문서 링크 또는 이전 답변의 소켓 프로토콜 섹션 참조]** 하십시오.
    
- **주요 액션 카테고리:** 인증 (AUTH), 메시지 (SEND_MESSAGE, NEW_MESSAGE), 채팅방 관리 (CREATE_CHAT_ROOM, JOIN_CHAT_ROOM), 읽음 처리 (MARK_AS_READ), WebRTC 시그널링 (WEBRTC_SIGNAL), 영상통화 제어 (VIDEO_CALL_REQUEST).
    

## 7. 주요 데이터 모델 (DTOs/VOs)

(시스템 전반에서 사용되는 핵심 데이터 객체의 구조를 간략히 정의합니다. 이는 DB 스키마, API 응답, 소켓 메시지와 일관되어야 합니다.)

- **User:** id, email, nickname, profile_image_url, bio, created_at
    
- **Post:** id, user_id (또는 author User 객체), content, images (list of PostImage), view_count, like_count, comment_count, created_at
    
- **Comment:** id, user_id (또는 author User 객체), post_id, content, like_count, reply_count, created_at
    
- **Message:** id, chat_room_id, user_id (또는 sender User 객체), message_type, content, image_url, created_at
    
- **ChatRoom:** id, name, type, members (list of User 객체), last_message (Message 객체), unread_count
    

(위 예시는 대표적이며, 실제로는 더 많은 필드와 객체가 정의될 수 있습니다.)

## 8. 개발 우선순위 및 MVP 정의

### 8.1. MVP 기능 목록

1. **사용자 인증:** 이메일 회원가입, 로그인, 자동 로그인, 내 프로필 조회/기본 수정.
    
2. **게시물:** 텍스트 게시물 작성, 전체 게시물 목록 조회, 게시물 상세 조회.
    
3. **1:1 실시간 텍스트 채팅:** 채팅방 목록, 1:1 채팅방 입장, 실시간 텍스트 메시지 송수신.
    

### 8.2. 단계별 개발 계획 (Phase)

- **Phase 1: MVP - 핵심 인증, 게시물, 1:1 채팅 (약 4~6주)**
    
    - (상세 내용은 이전 답변의 "개발 우선순위 및 MVP 정의" 섹션 참조)
        
- **Phase 2: 소셜 기능 확장 및 채팅 고도화 (약 4~6주)**
    
    - (상세 내용은 이전 답변 참조)
        
- **Phase 3: 그룹/오픈 채팅 및 영상통화 기반 (약 4~6주)**
    
    - (상세 내용은 이전 답변 참조)
        
- **Phase 4: 영상통화 완성 및 부가 기능 (약 3~4주)**
    
    - (상세 내용은 이전 답변 참조)
        
- **Phase 5: 최종 검토, 최적화 및 배포 준비 (약 2~3주)**
    
    - (상세 내용은 이전 답변 참조)
        

## 9. 공통 개발 지침

### 9.1. TDD (Test-Driven Development)

- 모든 기능 구현은 테스트 코드 작성을 선행합니다 (Red -> Green -> Refactor).
    
- 단위 테스트, 통합 테스트, UI 테스트(클라이언트)를 적절히 활용합니다.
    
- 테스트 커버리지 목표: (예: 주요 로직 80% 이상).
    

### 9.2. Git 전략 및 커밋 컨벤션

- **브랜치 전략:** Git Flow 또는 GitHub Flow 기반 (예: main, develop, feature/xxx, fix/xxx, release/xxx).
    
- **커밋 메시지 컨벤션:** Conventional Commits 형식 준수.
    
    - type(scope): subject
        
    - type: feat, fix, docs, style, refactor, test, chore 등.
        
    - scope: 기능 모듈 (예: auth, post, chat, user).
        
    - 예: feat(auth): Implement user registration API
        

### 9.3. 코딩 스타일 가이드

- 각 언어/프레임워크의 표준 코딩 컨벤션 준수 (예: PSR-12 for PHP, Google Java Style Guide).
    
- 일관된 네이밍 컨벤션 사용.
    
- 가독성을 높이는 코드 작성 (적절한 공백, 들여쓰기, 함수/클래스 분리).
    
- 주석은 "왜"를 설명하는 데 집중하고, "무엇을" 하는지는 코드로 명확히 표현.
    

### 9.4. 에러 처리 및 로깅

- 예상되는 모든 예외 상황에 대해 명확하고 일관된 방식으로 처리.
    
- 사용자에게 친화적인 에러 메시지 제공 (클라이언트).
    
- 서버 측에서는 상세한 에러 로그 기록 (발생 시간, 에러 내용, 스택 트레이스, 관련 요청 정보 등).
    
- Log4j2(Java), Monolog(PHP) 등 표준 로깅 라이브러리 활용.
    
- 로그 레벨(DEBUG, INFO, WARN, ERROR) 적절히 사용.
    

## 10. 용어 정의

(프로젝트 내에서 혼용될 수 있거나 특별한 의미를 가지는 용어들을 정의합니다.)

- **ASSA:** 본 프로젝트의 명칭.
    
- **MVP:** Minimum Viable Product (최소 기능 제품).
    
- **JWT:** JSON Web Token.
    
- **API:** Application Programming Interface.
    
- **WebRTC:** Web Real-Time Communication.
    
- **TDD:** Test-Driven Development.
    
- **LLM:** Large Language Model (본 프로젝트 개발에 활용되는 AI 모델).
    
- **Vive Coding:** LLM에게 지시를 내리고 결과만 기다리는 방식의 개발 방법론.
    

---

이 문서는 살아있는 문서로, 프로젝트 진행 상황에 따라 지속적으로 업데이트되어야 합니다. 모든 팀원(LLM 포함)은 항상 최신 버전의 문서를 참조하여 작업을 진행해야 합니다.