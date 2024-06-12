---
tags:
  - android
created: 2024-06-12T17:15
updated: 2024-06-12T20:56
---
# 개요
특정 앱이 사용하는 데이터를 그 앱 외부에서 접근가능하도록 만든 통로

예를 들어 사진 앱에 ContentProvider가 구현되어 있어 데이터를 다른 앱에서 사용할 수 있도록 통로를 제공해줌

# 왜 필요한가?
외부 앱의 데이터를 끌어 쓸떄 콘텐츠 프로바이더를 통해 데이터를 가져와야 한다.

# 사용법
다음 두 가지 경우에 ContentProvider를 사용하게 된다.
1. 내 앱에서 다른 앱의 ContentProvider 접근
2. 내 앱에 ContentProvider를 구현해서 다른 앱과 데이터 공유
	1. 안드로이드 기본 구성요소이기 때문에 Manifest에 등록 해야함

CP는 데이터에 대해 CRUD작업을 수행할 수 있는 메서드를 제공한다. 주요 메서드는 query(), insert(), update(), delete() 등이 있다.
## 1. 다른 앱의 ContentProvider 접근
ContentResolver 객체를 사용하여 ContentProvider와 서버-클라이언트 구조로 통신을 주고 받아야함. 즉, CR이 CP에 데이터를 요청하면 CP는 요청된 작업을 실행하고 결과를 반환한다.

아래 단계를 거쳐 데이터를 받아올 수 있다.
![[Pasted image 20240612180626.png]]
CP에서 공유할 수 있는 데이터는 DB, FILE, SharedPreferences 3가지다. CP 는 CRUD동작을 기본으로 하기 때문에 DB가 주로 사용된다.

### Content URI
CP를 만들기 위해서는 고유한 값을 가진 Content URI를 만들어야함
Content URI는 Provider에서 데이터를 식별하는 역할을 함
Provider의 권한과 테이블, 파일을 가리키는 이름이 포함됨
이 URI가 CP의 모든 메소드에 필수 인자로 들어가고 이를 통해 엑세스할 테이블, 행, 파일을 결정한다.
#### Content URI 예시
 content://com.example.contentprovider/person/1
-  content://
    ContentProvider에 제어되는 데이터라는 의미로 항상 content://로 시작됨  
-  Authority
    com.example.contentprovider를 가리키며, ContentProvider를 구분하는 고유의 값으로 사용됨  
- Base Path
    테이블 또는 파일을 가리키는 이름으로 해당 URI에서는 person 테이블을 가리킴  
- ID
    마지막 숫자로 테이블 내 행(레코드)을 가리킴
### CP를 통해 데이터 엑세스
ContentResolver의 query 메소드에 Uri 등의 인자를 입력하면 Cursor객체가 반환된다. Cursor객체는 쿼리의 결과를 읽고 쓸수 있게 해주는 인터페이스 역할을 한다.
컬럼명에 해당하는 인덱스를 알고 싶으며 getColumnIndex, 모든 컬럼명을 확인하고 싶으면 getColumnNames를 사용하면 된다.
## ContentProvider를 Manifest에 등록
provider 태그 추가, authrities에 CP의 Authrity정보와 동일하게 임력
name은 CP를 상속한 클래스 명 입력
read, write permission 정의 후, 해당 CP를 사용하는 앱의 매니페스트에 해당 권한 추가해야함

# 기반 기술

# 이 것을 기반으로 한 기술

# 참고자료

# 각주