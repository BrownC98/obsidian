---
created: 2025-05-29T23:30
updated: 2025-05-29T23:34
---
알겠습니다. 모든 LLM이 공유해야 할 핵심 정보 중 하나인 **"데이터베이스 스키마 (MySQL)"** 상세 문서를 작성해 드리겠습니다.

이 문서는 "ASSA - 통합 설계 및 개발 계획서 v1.0"에서 참조하도록 링크될 수 있으며, PHP 백엔드, Java 소켓 서버, 그리고 간접적으로 안드로이드 클라이언트 개발에 필요한 DB 구조 정보를 담고 있습니다.

---

**ASSA - 데이터베이스 스키마 정의서 (MySQL) v1.0**

**문서 버전:** 1.0
**최종 수정일:** 2023-10-27 (예시 날짜)
**작성자:** [사용자 이름 또는 팀 이름]

**1. 개요**

본 문서는 ASSA 프로젝트에서 사용될 MySQL 데이터베이스의 스키마를 정의합니다. 여기에는 테이블 구조, 컬럼 명세, 제약 조건, 인덱스, 테이블 간의 관계 및 주요 데이터 일관성 규칙이 포함됩니다. 모든 컴포넌트(PHP 백엔드, Java 소켓 서버)는 이 스키마 정의를 기준으로 데이터베이스와 상호작용해야 합니다.

**2. 일반 규칙 및 네이밍 컨벤션**

*   **테이블명:** 복수형, 소문자, 스네이크 케이스 (예: `users`, `chat_messages`).
*   **컬럼명:** 소문자, 스네이크 케이스 (예: `user_id`, `created_at`).
*   **Primary Key (PK):** 대부분의 테이블은 `id` (BIGINT UNSIGNED AUTO_INCREMENT)를 PK로 가집니다.
*   **Foreign Key (FK):** `[참조테이블단수형]_id` 형식을 따릅니다 (예: `posts` 테이블의 `user_id`는 `users` 테이블 참조). FK에는 ON DELETE, ON UPDATE 액션을 명시합니다.
*   **타임스탬프:**
    *   `created_at`: 레코드 생성 시각 (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP).
    *   `updated_at`: 레코드 마지막 수정 시각 (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP).
*   **소프트 삭제:** `deleted_at` (TIMESTAMP, NULLABLE, DEFAULT NULL) 컬럼을 사용하여 논리적 삭제를 지원합니다. 실제 데이터는 보존되며, `deleted_at`이 NULL인 레코드만 활성 상태로 간주합니다.
*   **인덱스:** PK는 자동으로 인덱스가 생성됩니다. FK 및 자주 검색 조건으로 사용되는 컬럼에는 명시적으로 인덱스를 추가합니다.
*   **문자셋 및 콜레이션:** 기본적으로 `UTF8MB4` 문자셋과 `utf8mb4_unicode_ci` 콜레이션을 사용합니다.

**3. 테이블 상세 정의**

---

**3.1. 사용자 관련 테이블**

**3.1.1. `users` 테이블**

*   **설명:** 사용자 기본 정보를 저장합니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 사용자 고유 ID
    *   `email` VARCHAR(255) UNIQUE NOT NULL - 사용자 이메일 (로그인 ID)
    *   `password` VARCHAR(255) NOT NULL - 해시된 사용자 비밀번호
    *   `nickname` VARCHAR(50) UNIQUE NOT NULL - 사용자 닉네임
    *   `profile_image_url` VARCHAR(2048) DEFAULT NULL - 프로필 이미지 URL
    *   `bio` TEXT DEFAULT NULL - 자기소개
    *   `is_email_verified` BOOLEAN DEFAULT FALSE - 이메일 인증 여부
    *   `email_verified_at` TIMESTAMP NULL DEFAULT NULL - 이메일 인증 완료 시각
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    *   `deleted_at` TIMESTAMP NULL DEFAULT NULL - 소프트 삭제 시각
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `UNIQUE KEY uk_email (email)`
    *   `UNIQUE KEY uk_nickname (nickname)`
    *   `KEY idx_deleted_at (deleted_at)`
*   **관계:**
    *   `oauth_providers` (1:N) - User : OAuthProvider
    *   `posts` (1:N) - User : Post
    *   `comments` (1:N) - User : Comment
    *   `replies` (1:N) - User : Reply
    *   `post_likes`, `comment_likes`, `reply_likes` (M:N) - User : Post/Comment/Reply (좋아요 관계)
    *   `follows` (M:N) - User : User (팔로우 관계)
    *   `friends` (M:N) - User : User (친구 관계)
    *   `blocked_users` (M:N) - User : User (차단 관계)
    *   `chat_rooms` (1:N, owner 관계) - User : ChatRoom
    *   `chat_room_members` (M:N) - User : ChatRoom (채팅방 멤버 관계)
    *   `messages` (1:N) - User : Message
    *   `notifications` (1:N) - User : Notification
    *   `user_notification_settings` (1:1) - User : UserNotificationSetting

**3.1.2. `oauth_providers` 테이블**

*   **설명:** 소셜 로그인 제공자 정보를 저장합니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 고유 ID
    *   `user_id` BIGINT UNSIGNED NOT NULL - `users` 테이블의 `id` (FK)
    *   `provider_name` VARCHAR(50) NOT NULL - 소셜 제공자 이름 (예: 'kakao', 'google')
    *   `provider_id` VARCHAR(255) NOT NULL - 소셜 제공자로부터 받은 사용자 고유 ID
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `UNIQUE KEY uk_provider_user (provider_name, provider_id)`
    *   `KEY idx_user_id (user_id)`
*   **관계:**
    *   `users` (N:1) - OAuthProvider : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)

---

**3.2. 게시물 관련 테이블**

**3.2.1. `posts` 테이블**

*   **설명:** 사용자가 작성한 게시물을 저장합니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 게시물 고유 ID
    *   `user_id` BIGINT UNSIGNED NOT NULL - 작성자 ID (`users.id` FK)
    *   `content` TEXT NOT NULL - 게시물 내용
    *   `view_count` INT UNSIGNED DEFAULT 0 - 조회수
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    *   `deleted_at` TIMESTAMP NULL DEFAULT NULL
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `KEY idx_user_id (user_id)`
    *   `KEY idx_created_at (created_at)` (피드 정렬용)
    *   `KEY idx_deleted_at (deleted_at)`
*   **관계:**
    *   `users` (N:1) - Post : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
    *   `post_images` (1:N) - Post : PostImage
    *   `post_likes` (1:N) - Post : PostLike (M:N 관계의 연결 테이블)
    *   `comments` (1:N) - Post : Comment

**3.2.2. `post_images` 테이블**

*   **설명:** 게시물에 첨부된 이미지 정보를 저장합니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 이미지 고유 ID
    *   `post_id` BIGINT UNSIGNED NOT NULL - 게시물 ID (`posts.id` FK)
    *   `image_url` VARCHAR(2048) NOT NULL - 이미지 파일 URL
    *   `sort_order` INT UNSIGNED DEFAULT 0 - 이미지 표시 순서
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `KEY idx_post_id (post_id)`
*   **관계:**
    *   `posts` (N:1) - PostImage : Post (FK: `post_id` -> `posts.id` ON DELETE CASCADE)

**3.2.3. `post_likes` 테이블**

*   **설명:** 사용자의 게시물 좋아요 정보를 저장하는 연결 테이블입니다. (User와 Post 간의 M:N 관계)
*   **컬럼:**
    *   `user_id` BIGINT UNSIGNED NOT NULL - 좋아요 한 사용자 ID (`users.id` FK)
    *   `post_id` BIGINT UNSIGNED NOT NULL - 좋아요 받은 게시물 ID (`posts.id` FK)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (user_id, post_id)`
    *   `KEY idx_post_id (post_id)` (특정 게시물의 좋아요 목록 조회용)
*   **관계:**
    *   `users` (N:1) - PostLike : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
    *   `posts` (N:1) - PostLike : Post (FK: `post_id` -> `posts.id` ON DELETE CASCADE)

---

**3.3. 댓글 및 답글 관련 테이블**

**3.3.1. `comments` 테이블**

*   **설명:** 게시물에 대한 댓글을 저장합니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 댓글 고유 ID
    *   `user_id` BIGINT UNSIGNED NOT NULL - 작성자 ID (`users.id` FK)
    *   `post_id` BIGINT UNSIGNED NOT NULL - 원본 게시물 ID (`posts.id` FK)
    *   `content` TEXT NOT NULL - 댓글 내용
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    *   `deleted_at` TIMESTAMP NULL DEFAULT NULL
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `KEY idx_post_id_created_at (post_id, created_at)` (특정 게시물의 댓글 목록 정렬 조회용)
    *   `KEY idx_user_id (user_id)`
    *   `KEY idx_deleted_at (deleted_at)`
*   **관계:**
    *   `users` (N:1) - Comment : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
    *   `posts` (N:1) - Comment : Post (FK: `post_id` -> `posts.id` ON DELETE CASCADE)
    *   `comment_likes` (1:N) - Comment : CommentLike (M:N 관계의 연결 테이블)
    *   `replies` (1:N) - Comment : Reply

**3.3.2. `comment_likes` 테이블**

*   **설명:** 사용자의 댓글 좋아요 정보를 저장하는 연결 테이블입니다.
*   **컬럼:**
    *   `user_id` BIGINT UNSIGNED NOT NULL - 좋아요 한 사용자 ID (`users.id` FK)
    *   `comment_id` BIGINT UNSIGNED NOT NULL - 좋아요 받은 댓글 ID (`comments.id` FK)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (user_id, comment_id)`
    *   `KEY idx_comment_id (comment_id)`
*   **관계:**
    *   `users` (N:1) - CommentLike : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
    *   `comments` (N:1) - CommentLike : Comment (FK: `comment_id` -> `comments.id` ON DELETE CASCADE)

**3.3.3. `replies` 테이블**

*   **설명:** 댓글에 대한 답글(대댓글)을 저장합니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 답글 고유 ID
    *   `user_id` BIGINT UNSIGNED NOT NULL - 작성자 ID (`users.id` FK)
    *   `comment_id` BIGINT UNSIGNED NOT NULL - 원본 댓글 ID (`comments.id` FK)
    *   `parent_reply_id` BIGINT UNSIGNED NULL DEFAULT NULL - 부모 답글 ID (대대댓글 기능 시, `replies.id` FK, 자기참조)
    *   `content` TEXT NOT NULL - 답글 내용
    *   `mentioned_user_id` BIGINT UNSIGNED NULL DEFAULT NULL - 멘션된 사용자 ID (`users.id` FK)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    *   `deleted_at` TIMESTAMP NULL DEFAULT NULL
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `KEY idx_comment_id_created_at (comment_id, created_at)` (특정 댓글의 답글 목록 정렬 조회용)
    *   `KEY idx_user_id (user_id)`
    *   `KEY idx_parent_reply_id (parent_reply_id)`
    *   `KEY idx_mentioned_user_id (mentioned_user_id)`
    *   `KEY idx_deleted_at (deleted_at)`
*   **관계:**
    *   `users` (N:1) - Reply : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
    *   `comments` (N:1) - Reply : Comment (FK: `comment_id` -> `comments.id` ON DELETE CASCADE)
    *   `replies` (N:1, 선택적) - Reply : Reply (FK: `parent_reply_id` -> `replies.id` ON DELETE CASCADE)
    *   `users` (N:1, 선택적) - Reply : User (FK: `mentioned_user_id` -> `users.id` ON DELETE SET NULL)
    *   `reply_likes` (1:N) - Reply : ReplyLike (M:N 관계의 연결 테이블)

**3.3.4. `reply_likes` 테이블**

*   **설명:** 사용자의 답글 좋아요 정보를 저장하는 연결 테이블입니다.
*   **컬럼:**
    *   `user_id` BIGINT UNSIGNED NOT NULL - 좋아요 한 사용자 ID (`users.id` FK)
    *   `reply_id` BIGINT UNSIGNED NOT NULL - 좋아요 받은 답글 ID (`replies.id` FK)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (user_id, reply_id)`
    *   `KEY idx_reply_id (reply_id)`
*   **관계:**
    *   `users` (N:1) - ReplyLike : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
    *   `replies` (N:1) - ReplyLike : Reply (FK: `reply_id` -> `replies.id` ON DELETE CASCADE)

---

**3.4. 소셜 관계 테이블**

**3.4.1. `follows` 테이블**

*   **설명:** 사용자 간의 팔로우 관계를 저장합니다. (단방향)
*   **컬럼:**
    *   `follower_id` BIGINT UNSIGNED NOT NULL - 팔로우 하는 사용자 ID (`users.id` FK)
    *   `following_id` BIGINT UNSIGNED NOT NULL - 팔로우 당하는 사용자 ID (`users.id` FK)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (follower_id, following_id)`
    *   `KEY idx_following_id (following_id)` (나를 팔로우하는 사람 목록 조회용)
*   **관계:**
    *   `users` (N:1) - Follow : User (FK: `follower_id` -> `users.id` ON DELETE CASCADE)
    *   `users` (N:1) - Follow : User (FK: `following_id` -> `users.id` ON DELETE CASCADE)

**3.4.2. `friends` 테이블**

*   **설명:** 사용자 간의 친구 관계를 저장합니다. (양방향, 요청/수락/거절/차단 상태 포함)
*   **컬럼:**
    *   `user_id_1` BIGINT UNSIGNED NOT NULL - 사용자1 ID (`users.id` FK). 항상 `user_id_2` 보다 작은 값으로 저장하여 중복 방지.
    *   `user_id_2` BIGINT UNSIGNED NOT NULL - 사용자2 ID (`users.id` FK).
    *   `status` ENUM('pending_user1_to_user2', 'pending_user2_to_user1', 'accepted', 'declined', 'blocked') NOT NULL - 친구 관계 상태
        *   `pending_user1_to_user2`: user1이 user2에게 친구 요청
        *   `pending_user2_to_user1`: user2가 user1에게 친구 요청
        *   `accepted`: 친구 수락됨
        *   `declined`: 친구 요청 거절됨 (일정 시간 후 삭제 또는 상태 유지)
        *   `blocked`: 한쪽이 다른 쪽을 차단 (이 테이블 대신 `blocked_users` 테이블을 주로 사용하고, 친구 관계는 `accepted`에서 삭제될 수 있음. 설계에 따라 다름. 여기서는 상태로 포함)
    *   `action_user_id` BIGINT UNSIGNED NOT NULL - 마지막 상태 변경을 수행한 사용자 ID (`users.id` FK)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (user_id_1, user_id_2)`
    *   `KEY idx_user_id_2 (user_id_2)` (특정 사용자의 친구 관계 조회 시 `user_id_1` 또는 `user_id_2`로 검색해야 하므로)
    *   `KEY idx_action_user_id (action_user_id)`
*   **관계:**
    *   `users` (N:1) - Friend : User (FK: `user_id_1` -> `users.id` ON DELETE CASCADE)
    *   `users` (N:1) - Friend : User (FK: `user_id_2` -> `users.id` ON DELETE CASCADE)
    *   `users` (N:1) - Friend : User (FK: `action_user_id` -> `users.id` ON DELETE CASCADE)
*   **데이터 일관성 규칙:**
    *   `user_id_1` < `user_id_2` 규칙을 애플리케이션 레벨에서 강제하여 데이터 중복 저장 방지.
    *   `status` 값에 따른 로직 처리 (예: `accepted` 상태만 실제 친구로 간주).

**3.4.3. `blocked_users` 테이블**

*   **설명:** 사용자가 다른 사용자를 차단한 정보를 저장합니다.
*   **컬럼:**
    *   `blocker_id` BIGINT UNSIGNED NOT NULL - 차단한 사용자 ID (`users.id` FK)
    *   `blocked_id` BIGINT UNSIGNED NOT NULL - 차단된 사용자 ID (`users.id` FK)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (blocker_id, blocked_id)`
    *   `KEY idx_blocked_id (blocked_id)` (내가 차단당했는지 확인용)
*   **관계:**
    *   `users` (N:1) - BlockedUser : User (FK: `blocker_id` -> `users.id` ON DELETE CASCADE)
    *   `users` (N:1) - BlockedUser : User (FK: `blocked_id` -> `users.id` ON DELETE CASCADE)
*   **데이터 일관성 규칙:**
    *   차단 시, `friends` 테이블에서 해당 사용자 간의 `accepted` 관계는 삭제되거나 `blocked` 상태로 변경될 수 있습니다. (애플리케이션 로직)
    *   차단된 사용자의 게시물, 댓글 등은 차단한 사용자에게 보이지 않도록 처리합니다. (애플리케이션 로직)

---

**3.5. 채팅 관련 테이블**

**3.5.1. `chat_rooms` 테이블**

*   **설명:** 채팅방 정보를 저장합니다. (1:1, 그룹, 오픈 채팅)
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 채팅방 고유 ID
    *   `name` VARCHAR(255) DEFAULT NULL - 채팅방 이름 (그룹/오픈 채팅용)
    *   `description` TEXT DEFAULT NULL - 채팅방 설명 (오픈 채팅용)
    *   `type` ENUM('one_to_one', 'group', 'open') NOT NULL - 채팅방 타입
    *   `owner_id` BIGINT UNSIGNED NULL DEFAULT NULL - 방장 ID (`users.id` FK, 오픈 채팅 또는 그룹 채팅 생성자)
    *   `last_message_id` BIGINT UNSIGNED NULL DEFAULT NULL - 마지막 메시지 ID (`messages.id` FK, 채팅방 목록 정렬 및 미리보기용)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    *   `deleted_at` TIMESTAMP NULL DEFAULT NULL - 채팅방 비활성화/삭제 시각
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `KEY idx_type (type)`
    *   `KEY idx_owner_id (owner_id)`
    *   `KEY idx_last_message_id (last_message_id)`
    *   `KEY idx_deleted_at (deleted_at)`
*   **관계:**
    *   `users` (N:1, 선택적) - ChatRoom : User (FK: `owner_id` -> `users.id` ON DELETE SET NULL)
    *   `messages` (1:1, 선택적) - ChatRoom : Message (FK: `last_message_id` -> `messages.id` ON DELETE SET NULL) - 순환 참조 가능성 있으므로 주의, 또는 애플리케이션 레벨 관리
    *   `chat_room_members` (1:N) - ChatRoom : ChatRoomMember
    *   `messages` (1:N) - ChatRoom : Message

**3.5.2. `chat_room_members` 테이블**

*   **설명:** 채팅방 참여자 정보를 저장하는 연결 테이블입니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 고유 ID
    *   `chat_room_id` BIGINT UNSIGNED NOT NULL - 채팅방 ID (`chat_rooms.id` FK)
    *   `user_id` BIGINT UNSIGNED NOT NULL - 참여자 ID (`users.id` FK)
    *   `role` ENUM('member', 'admin', 'owner') DEFAULT 'member' - 채팅방 내 역할
    *   `joined_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP - 참여 시각
    *   `last_read_message_id` BIGINT UNSIGNED NULL DEFAULT NULL - 해당 유저가 마지막으로 읽은 메시지 ID (`messages.id` FK)
    *   `unread_count` INT UNSIGNED DEFAULT 0 - 안 읽은 메시지 수 (실시간 업데이트 필요)
    *   `show_notifications` BOOLEAN DEFAULT TRUE - 해당 채팅방 알림 수신 여부
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `UNIQUE KEY uk_chatroom_user (chat_room_id, user_id)`
    *   `KEY idx_user_id (user_id)` (내가 참여한 채팅방 목록 조회용)
    *   `KEY idx_last_read_message_id (last_read_message_id)`
*   **관계:**
    *   `chat_rooms` (N:1) - ChatRoomMember : ChatRoom (FK: `chat_room_id` -> `chat_rooms.id` ON DELETE CASCADE)
    *   `users` (N:1) - ChatRoomMember : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
    *   `messages` (N:1, 선택적) - ChatRoomMember : Message (FK: `last_read_message_id` -> `messages.id` ON DELETE SET NULL) - 순환 참조 가능성 주의
*   **데이터 일관성 규칙:**
    *   `unread_count`는 새 메시지 수신 시 증가, `last_read_message_id` 업데이트 시 재계산됩니다. (애플리케이션 또는 DB 트리거)

**3.5.3. `messages` 테이블**

*   **설명:** 채팅 메시지를 저장합니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 메시지 고유 ID
    *   `chat_room_id` BIGINT UNSIGNED NOT NULL - 채팅방 ID (`chat_rooms.id` FK)
    *   `user_id` BIGINT UNSIGNED NOT NULL - 메시지 보낸 사용자 ID (`users.id` FK)
    *   `content` TEXT - 텍스트 메시지 내용
    *   `message_type` ENUM('text', 'image', 'video_call_signal', 'system') NOT NULL DEFAULT 'text' - 메시지 타입
        *   `text`: 일반 텍스트
        *   `image`: 이미지
        *   `video_call_signal`: WebRTC 시그널링 정보 (JSON 문자열)
        *   `system`: 입장/퇴장, 방 정보 변경 등 시스템 알림
    *   `image_url` VARCHAR(2048) DEFAULT NULL - 이미지 메시지인 경우 이미지 URL
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `deleted_at` TIMESTAMP NULL DEFAULT NULL - 메시지 삭제 시각 (보낸 사람만 가능)
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `KEY idx_chatroom_created_at (chat_room_id, created_at)` (채팅방 메시지 목록 정렬 조회용)
    *   `KEY idx_user_id (user_id)`
    *   `KEY idx_deleted_at (deleted_at)`
*   **관계:**
    *   `chat_rooms` (N:1) - Message : ChatRoom (FK: `chat_room_id` -> `chat_rooms.id` ON DELETE CASCADE)
    *   `users` (N:1) - Message : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
    *   `message_read_status` (1:N) - Message : MessageReadStatus
*   **데이터 일관성 규칙:**
    *   새 메시지 생성 시, `chat_rooms.last_message_id` 와 `chat_room_members.unread_count` 가 업데이트되어야 합니다. (애플리케이션 또는 DB 트리거)

**3.5.4. `message_read_status` 테이블 (선택적)**

*   **설명:** 그룹 채팅에서 각 메시지를 누가 읽었는지 상세 상태를 저장합니다. (1:1 채팅에서는 `chat_room_members.last_read_message_id`로 충분할 수 있음)
*   **컬럼:**
    *   `message_id` BIGINT UNSIGNED NOT NULL - 메시지 ID (`messages.id` FK)
    *   `user_id` BIGINT UNSIGNED NOT NULL - 메시지를 읽은 사용자 ID (`users.id` FK)
    *   `read_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP - 읽은 시각
*   **인덱스:**
    *   `PRIMARY KEY (message_id, user_id)`
    *   `KEY idx_user_id (user_id)`
*   **관계:**
    *   `messages` (N:1) - MessageReadStatus : Message (FK: `message_id` -> `messages.id` ON DELETE CASCADE)
    *   `users` (N:1) - MessageReadStatus : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)

---

**3.6. 영상통화 관련 테이블**

**3.6.1. `video_call_sessions` 테이블**

*   **설명:** WebRTC 영상통화 세션 정보를 저장합니다.
*   **컬럼:**
    *   `id` VARCHAR(255) PK - 세션 고유 ID (UUID 또는 채팅방 ID 기반 유니크 값)
    *   `chat_room_id` BIGINT UNSIGNED NOT NULL - 영상통화가 시작된 채팅방 ID (`chat_rooms.id` FK)
    *   `created_by_user_id` BIGINT UNSIGNED NOT NULL - 세션 생성자 ID (`users.id` FK)
    *   `status` ENUM('pending', 'active', 'ended') DEFAULT 'pending' - 세션 상태
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `KEY idx_chat_room_id (chat_room_id)`
    *   `KEY idx_created_by_user_id (created_by_user_id)`
*   **관계:**
    *   `chat_rooms` (N:1) - VideoCallSession : ChatRoom (FK: `chat_room_id` -> `chat_rooms.id` ON DELETE CASCADE)
    *   `users` (N:1) - VideoCallSession : User (FK: `created_by_user_id` -> `users.id` ON DELETE CASCADE)
    *   `video_call_participants` (1:N) - VideoCallSession : VideoCallParticipant

**3.6.2. `video_call_participants` 테이블**

*   **설명:** 영상통화 세션 참여자 정보를 저장합니다.
*   **컬럼:**
    *   `id` BIGINT UNSIGNED AUTO_INCREMENT PK - 고유 ID
    *   `video_call_session_id` VARCHAR(255) NOT NULL - 세션 ID (`video_call_sessions.id` FK)
    *   `user_id` BIGINT UNSIGNED NOT NULL - 참여자 ID (`users.id` FK)
    *   `joined_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP - 참여 시각
    *   `left_at` TIMESTAMP NULL DEFAULT NULL - 퇴장 시각
    *   `is_camera_on` BOOLEAN DEFAULT TRUE - 카메라 상태
    *   `is_mic_on` BOOLEAN DEFAULT TRUE - 마이크 상태
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `UNIQUE KEY uk_session_user (video_call_session_id, user_id)`
    *   `KEY idx_user_id (user_id)`
*   **관계:**
    *   `video_call_sessions` (N:1) - VideoCallParticipant : VideoCallSession (FK: `video_call_session_id` -> `video_call_sessions.id` ON DELETE CASCADE)
    *   `users` (N:1) - VideoCallParticipant : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)

---

**3.7. 알림 관련 테이블**

**3.7.1. `notifications` 테이블**

*   **설명:** 사용자에게 발송될 알림 내용을 저장합니다.
*   **컬럼:**
    *   `id` CHAR(36) PK - 알림 고유 ID (UUID)
    *   `user_id` BIGINT UNSIGNED NOT NULL - 알림을 받을 사용자 ID (`users.id` FK)
    *   `type` VARCHAR(255) NOT NULL - 알림 유형 (예: 'new_post_like', 'new_comment', 'new_follower', 'chat_message')
    *   `message` TEXT NOT NULL - 알림 메시지 내용
    *   `related_entity_type` VARCHAR(255) DEFAULT NULL - 관련된 엔티티 타입 (예: 'post', 'user', 'chat_room')
    *   `related_entity_id` BIGINT UNSIGNED DEFAULT NULL - 관련된 엔티티의 ID
    *   `is_read` BOOLEAN DEFAULT FALSE - 읽음 여부
    *   `read_at` TIMESTAMP NULL DEFAULT NULL - 읽은 시각
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (id)`
    *   `KEY idx_user_id_created_at (user_id, created_at)` (사용자별 알림 목록 조회 및 정렬용)
    *   `KEY idx_user_id_is_read (user_id, is_read)` (안 읽은 알림 개수 조회용)
*   **관계:**
    *   `users` (N:1) - Notification : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
*   **데이터 일관성 규칙:**
    *   `related_entity_type`과 `related_entity_id`는 알림 클릭 시 해당 컨텐츠로 이동하는 데 사용됩니다.

**3.7.2. `user_notification_settings` 테이블**

*   **설명:** 사용자별 알림 수신 설정을 저장합니다.
*   **컬럼:**
    *   `user_id` BIGINT UNSIGNED PK - 사용자 ID (`users.id` FK)
    *   `settings` JSON - 알림 종류별 수신 여부 설정 (예: `{"new_post_like_enabled": true, "new_comment_enabled": false}`)
    *   `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    *   `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
*   **인덱스:**
    *   `PRIMARY KEY (user_id)`
*   **관계:**
    *   `users` (1:1) - UserNotificationSetting : User (FK: `user_id` -> `users.id` ON DELETE CASCADE)
*   **데이터 일관성 규칙:**
    *   `settings` JSON 필드의 구조는 사전에 정의되어야 하며, 애플리케이션에서 이를 파싱하여 사용합니다.

---

**4. 데이터 일관성 규칙 (전반적)**

*   **소프트 삭제:** `deleted_at` 컬럼이 NULL이 아닌 레코드는 애플리케이션 로직에서 비활성화된 것으로 간주하여 조회 대상에서 제외하거나 특별히 처리해야 합니다.
*   **상태 값 (ENUM):** ENUM 타입으로 정의된 컬럼의 각 값은 명확한 의미를 가지며, 애플리케이션 로직은 이 상태 값에 따라 분기 처리해야 합니다. (예: `friends.status`, `chat_rooms.type`)
*   **카운트 컬럼:** `posts.view_count`, `chat_room_members.unread_count` 등과 같은 카운트 컬럼은 관련 이벤트 발생 시 트랜잭션 내에서 정확하게 업데이트되어야 합니다. (애플리케이션 로직 또는 DB 트리거)
*   **참조 무결성:** FK 제약조건을 통해 데이터 간의 참조 무결성을 유지합니다. `ON DELETE`, `ON UPDATE` 액션은 신중하게 정의되어야 합니다. (대부분 `CASCADE` 또는 `SET NULL` 사용)
*   **중복 방지:** `UNIQUE` 제약조건을 통해 주요 식별자(이메일, 닉네임 등) 및 관계의 중복을 방지합니다. `friends` 테이블의 `user_id_1 < user_id_2` 와 같은 규칙은 애플리케이션 레벨에서 추가로 관리합니다.

---

이 문서는 프로젝트의 데이터베이스 구조에 대한 핵심 참조 자료로 사용됩니다. 변경 사항이 발생할 경우 즉시 업데이트되어 모든 관련자에게 공유되어야 합니다.