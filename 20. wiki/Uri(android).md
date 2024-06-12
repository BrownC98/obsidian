---
tags:
  - android
created: 2024-06-12T15:01
updated: 2024-06-12T16:57
---
# 개요
[[URI]]정보를 저장하는 클래스다.

# 왜 필요한가?
기기 내부의 자원(파일, 앱, 전화, 카메라 등)에 접근하기 위해 필요함
안드로이드에서 무언가를 사용하기 위해 가져오거나 실행하기 위한 식별 정보

# 사용법
## 메소드
![[Pasted image 20240612163505.png]]
## 메소드의 인자, 반환값으로서의 Uri 객체 사용 예시
1. Intent 객체의 data 프로퍼티
-  예) 기본 웹 부라우저로 Google에 접속하는 Intent의 data 프로퍼티 값 출력
```kotlin
var intent = Intent(Intent.ACTION_VIEW)
intent.data = Uri.parse("http://google.com")
Log.d("test", "Uri : ${intent.data})
getContent.launch(intent)
```
실행결과
> Uri : http://google.com

- 예) 연락처 앱을 실행하는 Intent의 data 프로퍼티 값 출력
```kotlin
var intent = Intent(Intent.ACTION_PICK, ContactsContract.CommonDataKinds.Phone.CONTENT_URI)
Log.d("test", "Uri : ${intent.data})
getContent.launch(intent)
```
실행결과
> Uri : content://com.android.contacts/data/phones

ContentProvider 사용


# 기반 기술

# 이 것을 기반으로 한 기술

# 참고자료

# 각주