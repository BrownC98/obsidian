---
tags:
  - android
created: 2024-06-12T17:15
updated: 2024-06-12T18:48
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

# 기반 기술

# 이 것을 기반으로 한 기술

# 참고자료

# 각주