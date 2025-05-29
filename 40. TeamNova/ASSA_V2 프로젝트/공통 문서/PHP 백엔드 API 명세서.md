---
created: 2025-05-29T23:34
updated: 2025-05-29T23:34
---
알겠습니다. 안드로이드 클라이언트와 Java 소켓 서버가 참조하고, PHP 백엔드 개발의 기준이 될 **"PHP 백엔드 API 명세서"** 를 작성해 드리겠습니다. 이 명세는 OpenAPI 3.0 형식을 따르도록 노력하되, 가독성을 위해 주요 내용을 설명하는 방식으로 구성하겠습니다. (실제 OpenAPI YAML/JSON 파일은 별도로 생성하여 관리하는 것이 좋습니다.)

---

**ASSA - PHP 백엔드 API 명세서 v1.0**

**문서 버전:** 1.0
**최종 수정일:** 2023-10-27 (예시 날짜)
**작성자:** [사용자 이름 또는 팀 이름]
**OpenAPI Spec (별도 파일):** `assa-api-openapi-v1.0.yaml` (가상)

**1. 개요**

본 문서는 ASSA 프로젝트의 PHP 백엔드 서버가 제공하는 RESTful API의 명세를 정의합니다. 안드로이드 클라이언트 및 기타 연동 시스템(예: Java 소켓 서버)은 이 명세를 기준으로 API를 호출해야 합니다.

**2. 공통 규칙**

*   **Base URL:** `https://your-assa-api-domain.com/api/v1` (환경에 따라 변경될 수 있음)
    *   로컬 개발 환경 예시: `http://localhost:8000/api/v1`
*   **요청/응답 데이터 형식:** `application/json`
*   **인증 (Authentication):**
    *   대부분의 보호된 엔드포인트는 JWT(JSON Web Token)를 사용한 Bearer 토큰 인증을 요구합니다.
    *   **요청 헤더:** `Authorization: Bearer <YOUR_JWT_TOKEN>`
    *   토큰은 로그인 API를 통해 획득하며, 만료 시 토큰 갱신 API를 통해 새로운 토큰을 발급받아야 합니다.
*   **HTTP 상태 코드:** 표준 HTTP 상태 코드를 사용합니다.
    *   `200 OK`: 요청 성공 (GET, PUT, PATCH, DELETE 일부)
    *   `201 Created`: 리소스 생성 성공 (POST)
    *   `204 No Content`: 요청 성공했으나 반환할 컨텐츠 없음 (DELETE 일부)
    *   `400 Bad Request`: 잘못된 요청 (예: 필수 파라미터 누락, 유효성 검사 실패 - 상세 에러는 응답 본문에 포함)
    *   `401 Unauthorized`: 인증 실패 (유효하지 않은 토큰, 토큰 누락)
    *   `403 Forbidden`: 권한 없음 (인증은 되었으나 해당 리소스 접근 권한 없음)
    *   `404 Not Found`: 요청한 리소스를 찾을 수 없음
    *   `422 Unprocessable Entity`: 유효성 검사 실패 (요청 본문 데이터 문제)
    *   `500 Internal Server Error`: 서버 내부 오류
*   **응답 본문 구조:**
    *   **성공 시 (Success Response):**
        ```json
        {
          "success": true,
          "message": "요청이 성공적으로 처리되었습니다.", // 선택적 메시지
          "data": {
            // 실제 데이터 객체 또는 배열
          }
        }
        ```
        *   `data` 필드는 리소스 생성/수정 시 해당 리소스 객체를, 목록 조회 시 객체 배열을 포함합니다. 단일 조회 시 객체를 포함합니다. `204 No Content`의 경우 `data` 필드가 없을 수 있습니다.
    *   **실패 시 (Error Response):**
        ```json
        {
          "success": false,
          "message": "오류 메시지 요약",
          "errors": { // 400, 422 등의 경우 상세 오류 정보
            "field_name": ["Error message 1 for this field.", "Error message 2 for this field."],
            "general_error": ["General error message not specific to a field."]
          },
          "error_code": "SPECIFIC_ERROR_CODE" // 선택적: 내부 에러 코드
        }
        ```
*   **페이지네이션 (Pagination):** 목록을 반환하는 API의 경우 페이지네이션을 지원합니다.
    *   **요청 Query 파라미터:**
        *   `page` (integer, optional, default: 1): 요청할 페이지 번호
        *   `per_page` (integer, optional, default: 15): 페이지 당 항목 수
    *   **응답 본문 `data` 필드 내 `meta` 객체:**
        ```json
        "data": [ /* ...list of items... */ ],
        "meta": {
          "current_page": 1,
          "from": 1,
          "last_page": 10,
          "path": "https://your-assa-api-domain.com/api/v1/posts",
          "per_page": 15,
          "to": 15,
          "total": 150
        },
        "links": {
            "first": "https://your-assa-api-domain.com/api/v1/posts?page=1",
            "last": "https://your-assa-api-domain.com/api/v1/posts?page=10",
            "prev": null,
            "next": "https://your-assa-api-domain.com/api/v1/posts?page=2"
        }
        ```
*   **날짜 및 시간 형식:** 모든 날짜 및 시간은 ISO 8601 형식의 UTC로 표현됩니다 (예: `2023-10-27T10:30:00Z`).

**3. 인증 (Auth) API**

**3.1. `POST /auth/register` - 회원가입**

*   **설명:** 새로운 사용자 계정을 생성합니다.
*   **인증:** 불필요
*   **요청 본문 (application/json):**
    ```json
    {
      "email": "user@example.com", // string, required, email format
      "password": "password123",   // string, required, min:8 characters
      "nickname": "NewUser123"     // string, required, unique, min:2, max:50
    }
    ```
*   **응답:**
    *   **201 Created (성공):**
        ```json
        {
          "success": true,
          "message": "회원가입이 성공적으로 완료되었습니다.",
          "data": {
            "user": {
              "id": 1,
              "email": "user@example.com",
              "nickname": "NewUser123",
              "profile_image_url": null,
              "created_at": "2023-10-27T10:30:00Z"
            },
            "token": "your_jwt_token_string",
            "token_type": "bearer",
            "expires_in": 3600 // 초 단위
          }
        }
        ```
    *   **422 Unprocessable Entity (유효성 검사 실패):** `errors` 필드에 상세 오류 포함.
    *   **400 Bad Request (이메일/닉네임 중복 등):** `message` 필드에 오류 설명.

**3.2. `POST /auth/login` - 로그인**

*   **설명:** 이메일과 비밀번호로 사용자 인증 후 JWT 토큰을 발급합니다.
*   **인증:** 불필요
*   **요청 본문 (application/json):**
    ```json
    {
      "email": "user@example.com", // string, required, email format
      "password": "password123"   // string, required
    }
    ```
*   **응답:**
    *   **200 OK (성공):**
        ```json
        {
          "success": true,
          "message": "로그인 성공",
          "data": {
            "user": { /* 사용자 정보 객체 (위 회원가입 응답과 유사) */ },
            "token": "your_jwt_token_string",
            "token_type": "bearer",
            "expires_in": 3600
          }
        }
        ```
    *   **401 Unauthorized (인증 실패):** 잘못된 이메일 또는 비밀번호.
    *   **422 Unprocessable Entity (유효성 검사 실패):**

**3.3. `POST /auth/login/social` - 소셜 로그인**

*   **설명:** 소셜 프로바이더(예: Kakao, Google)의 액세스 토큰을 사용하여 로그인 또는 회원가입 처리 후 JWT 토큰을 발급합니다.
*   **인증:** 불필요
*   **요청 본문 (application/json):**
    ```json
    {
      "provider": "kakao",       // string, required, "kakao", "google" 등
      "access_token": "social_provider_access_token" // string, required
    }
    ```
*   **응답:** `POST /auth/login`과 동일한 성공 응답 구조. 신규 사용자인 경우 자동 회원가입 처리.

**3.4. `POST /auth/logout` - 로그아웃**

*   **설명:** 현재 사용자의 JWT 토큰을 무효화합니다. (서버 측에서 토큰 블랙리스트 처리)
*   **인증:** 필요 (`Authorization: Bearer <token>`)
*   **요청 본문:** 없음
*   **응답:**
    *   **200 OK (성공):**
        ```json
        {
          "success": true,
          "message": "성공적으로 로그아웃되었습니다."
        }
        ```
    *   **401 Unauthorized (인증 실패):**

**3.5. `POST /auth/refresh` - 토큰 갱신**

*   **설명:** 만료되었거나 만료가 임박한 JWT 토큰을 사용하여 새로운 토큰을 발급받습니다. (기존 토큰이 Refresh Token 역할을 겸하거나, 별도의 Refresh Token 사용 가능 - 여기서는 기존 토큰 사용 가정)
*   **인증:** 필요 (`Authorization: Bearer <token_to_be_refreshed>`)
*   **요청 본문:** 없음
*   **응답:**
    *   **200 OK (성공):**
        ```json
        {
          "success": true,
          "message": "토큰이 성공적으로 갱신되었습니다.",
          "data": {
            "token": "new_jwt_token_string",
            "token_type": "bearer",
            "expires_in": 3600
          }
        }
        ```
    *   **401 Unauthorized (토큰 유효하지 않음):**

**3.6. `GET /auth/me` - 내 정보 조회**

*   **설명:** 현재 인증된 사용자의 프로필 정보를 조회합니다.
*   **인증:** 필요 (`Authorization: Bearer <token>`)
*   **요청 파라미터:** 없음
*   **응답:**
    *   **200 OK (성공):**
        ```json
        {
          "success": true,
          "data": {
            "id": 1,
            "email": "user@example.com",
            "nickname": "CurrentUser",
            "profile_image_url": "https://example.com/profile.jpg",
            "bio": "Hello, I am a user.",
            "is_email_verified": true,
            "created_at": "2023-10-27T10:30:00Z",
            "updated_at": "2023-10-27T11:00:00Z"
          }
        }
        ```
    *   **401 Unauthorized (인증 실패):**

**3.7. `POST /auth/email/verify-request` - 이메일 인증 요청**

*   **설명:** 가입 시 또는 필요시 이메일 인증을 위한 메일을 발송합니다.
*   **인증:** 필요 (또는 이메일 파라미터로 받아 미인증 상태에서도 가능하도록 설계 가능)
*   **요청 본문 (application/json):** (인증된 사용자의 경우 본문 불필요, 미인증 시 이메일 포함)
    ```json
    // { "email": "user_to_verify@example.com" } // 선택적
    ```
*   **응답:**
    *   **200 OK (성공):**
        ```json
        {
          "success": true,
          "message": "인증 이메일이 발송되었습니다. 이메일을 확인해주세요."
        }
        ```

**3.8. `POST /auth/email/verify` - 이메일 인증 확인**

*   **설명:** 이메일로 발송된 인증 코드 또는 토큰을 사용하여 이메일 소유권을 확인합니다.
*   **인증:** 불필요 (토큰 자체로 사용자 식별)
*   **요청 본문 (application/json):**
    ```json
    {
      "token": "verification_token_from_email" // string, required
      // 또는 "code": "123456"
    }
    ```
*   **응답:**
    *   **200 OK (성공):**
        ```json
        {
          "success": true,
          "message": "이메일 인증이 완료되었습니다."
        }
        ```
    *   **400 Bad Request (잘못된 토큰/코드):**

**3.9. `POST /auth/password/forgot` - 비밀번호 찾기 요청**

*   **설명:** 비밀번호 재설정을 위한 링크 또는 코드를 이메일로 발송합니다.
*   **인증:** 불필요
*   **요청 본문 (application/json):**
    ```json
    {
      "email": "user@example.com" // string, required, email format
    }
    ```
*   **응답:**
    *   **200 OK (성공):**
        ```json
        {
          "success": true,
          "message": "비밀번호 재설정 안내 메일이 발송되었습니다."
        }
        ```

**3.10. `POST /auth/password/reset` - 비밀번호 재설정**

*   **설명:** 이메일로 받은 토큰과 새 비밀번호를 사용하여 비밀번호를 재설정합니다.
*   **인증:** 불필요 (토큰 자체로 사용자 식별)
*   **요청 본문 (application/json):**
    ```json
    {
      "token": "password_reset_token_from_email", // string, required
      "email": "user@example.com",                 // string, required (토큰과 이메일 매칭 확인용)
      "password": "newPassword123",              // string, required, min:8
      "password_confirmation": "newPassword123"  // string, required, must match password
    }
    ```
*   **응답:**
    *   **200 OK (성공):**
        ```json
        {
          "success": true,
          "message": "비밀번호가 성공적으로 재설정되었습니다."
        }
        ```
    *   **400 Bad Request (잘못된 토큰, 비밀번호 불일치 등):**

---

**4. 사용자 (Users) API**

**4.1. `GET /users/{userId}` - 특정 사용자 프로필 조회**

*   **설명:** 특정 사용자의 공개 프로필 정보를 조회합니다.
*   **인증:** 선택적 (인증 시 차단 여부 등 추가 정보 제공 가능)
*   **Path 파라미터:**
    *   `userId` (integer, required): 조회할 사용자의 ID
*   **응답:**
    *   **200 OK (성공):** `GET /auth/me` 와 유사한 사용자 정보 객체 반환 (비밀번호 등 민감 정보 제외).
    *   **404 Not Found (사용자 없음):**

**4.2. `PUT /users/me` - 내 프로필 수정**

*   **설명:** 현재 인증된 사용자의 프로필 정보(닉네임, 자기소개, 프로필 이미지)를 수정합니다.
*   **인증:** 필요
*   **요청 본문 (application/json):** (수정할 필드만 포함)
    ```json
    {
      "nickname": "UpdatedNickname", // string, optional, unique, min:2, max:50
      "bio": "Updated bio content.",   // string, optional
      "profile_image_url": "https://example.com/new_profile.jpg" // string, optional, URL (실제로는 파일 업로드 후 URL 저장)
    }
    ```
*   **응답:**
    *   **200 OK (성공):** 수정된 사용자 정보 객체 반환.
    *   **422 Unprocessable Entity (유효성 검사 실패):**
    *   **401 Unauthorized:**

**4.3. `GET /users/search` - 사용자 검색**

*   **설명:** 닉네임 또는 이메일(부분 일치)로 사용자를 검색합니다.
*   **인증:** 필요
*   **Query 파라미터:**
    *   `q` (string, required): 검색어
    *   `page`, `per_page` (페이지네이션)
*   **응답:**
    *   **200 OK (성공):** 사용자 객체 배열 및 페이지네이션 정보 반환.
        ```json
        {
          "success": true,
          "data": [
            { /* 사용자 객체 1 */ },
            { /* 사용자 객체 2 */ }
          ],
          "meta": { /* 페이지네이션 정보 */ },
          "links": { /* 페이지네이션 링크 */ }
        }
        ```

---

**(이하 Posts, Comments, Replies, Follows, Friends, Blocked Users, Chat Rooms, Notifications, Files API 명세를 위와 같은 형식으로 상세히 기술합니다. 이전 답변의 API 엔드포인트 목록을 참고하여 각 엔드포인트별 요청/응답 형식, 인증 여부, 파라미터 등을 명시합니다.)**

**예시: Posts API 일부**

**5. 게시물 (Posts) API**

**5.1. `POST /posts` - 새 게시물 작성**

*   **설명:** 새로운 게시물을 작성합니다.
*   **인증:** 필요
*   **요청 본문 (application/json):**
    ```json
    {
      "content": "This is my new post content.", // string, required
      "image_urls": [ // array of strings, optional (파일 업로드 후 받은 URL 목록)
        "https://example.com/image1.jpg",
        "https://example.com/image2.jpg"
      ]
    }
    ```
*   **응답:**
    *   **201 Created (성공):** 생성된 게시물 객체 반환.
        ```json
        {
          "success": true,
          "message": "게시물이 성공적으로 작성되었습니다.",
          "data": {
            "id": 101,
            "user_id": 1,
            "content": "This is my new post content.",
            "images": [
                { "id": 1, "image_url": "https://example.com/image1.jpg", "sort_order": 0 },
                { "id": 2, "image_url": "https://example.com/image2.jpg", "sort_order": 1 }
            ],
            "view_count": 0,
            "created_at": "2023-10-27T12:00:00Z",
            "author": { /* 작성자 정보 객체 */ }
          }
        }
        ```
    *   **422 Unprocessable Entity:**
    *   **401 Unauthorized:**

**5.2. `GET /posts` - 전체 게시물 목록 조회**

*   **설명:** 모든 사용자의 게시물 목록을 최신순으로 조회합니다. (페이지네이션)
*   **인증:** 선택적 (인증 시 사용자별 좋아요 여부 등 추가 정보 제공 가능)
*   **Query 파라미터:** `page`, `per_page`
*   **응답:**
    *   **200 OK (성공):** 게시물 객체 배열 및 페이지네이션 정보. 각 게시물 객체에는 작성자 정보, 좋아요 수, 댓글 수 등이 포함될 수 있습니다.

---

**문서의 나머지 부분:**

위와 같은 상세 수준으로 나머지 API 모듈(Posts, Comments, Replies, Follows, Friends, Blocked Users, Chat Rooms, Notifications, Files)에 대한 명세를 작성해야 합니다. 각 엔드포인트마다 다음 사항을 명확히 기술합니다:

*   **HTTP 메소드 및 URL**
*   **간단한 설명**
*   **인증 필요 여부**
*   **Path 파라미터 (이름, 타입, 필수 여부, 설명)**
*   **Query 파라미터 (이름, 타입, 필수 여부, 기본값, 설명)**
*   **Request Headers (필요한 경우, 예: `Content-Type`)**
*   **Request Body (JSON 스키마 또는 예시, 각 필드의 이름, 타입, 필수 여부, 설명)**
*   **Response Codes (성공 및 예상되는 오류 코드 목록)**
*   **Response Headers (필요한 경우, 예: `Location` for 201 Created)**
*   **Response Body (각 응답 코드별 JSON 스키마 또는 예시, 각 필드의 이름, 타입, 설명)**

**OpenAPI Spec 연동:**

이 문서는 사람이 읽기 위한 요약본으로 활용하고, 실제 개발에서는 Swagger Editor나 유사 도구를 사용하여 OpenAPI 3.0 스펙 파일 (`.yaml` 또는 `.json`)을 작성하고 관리하는 것이 가장 좋습니다. 이 스펙 파일을 통해 API 문서를 자동으로 생성하고, 클라이언트 SDK 코드 생성, API 테스트 등 다양한 이점을 얻을 수 있습니다. LLM에게도 이 스펙 파일을 직접 참조하도록 지시할 수 있습니다.

이 명세서는 프로젝트의 API 계약(contract) 역할을 하므로, 변경 시에는 모든 관련 팀(LLM 포함)에게 즉시 공유되고 버전 관리가 이루어져야 합니다.