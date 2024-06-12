---
tags:
  - android
created: 2024-06-12T15:01
updated: 2024-06-12T17:18
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

2. [[Content Provider]] 사용
ContentResolver 객체를 사용해 시스템에 등록된 콘텐츠 프로바이더를 사용할 때 delete(), insert(), query() 등 함수의 매개변수로 Uri객체 가 사용됨

```kotlin
public final Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder)
```

```kotlin
getContent = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { if (it.resultCode == RESULT_OK) { val cursor = contentResolver.query( it.data!!.data!!, arrayOf<String>( ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME, ContactsContract.CommonDataKinds.Phone.NUMBER ), null, null, null ) // ... } }
```
아래 코드를 실행하면 콜백에 ActivityResult객체가 반환됨 해당 객체이 data 프로퍼티는  Intent 타입이고, intent 타입의 data는 Uri가 들어있다.
최종 Uri값은 다음과 같은 형태다
content://com.android.contacts/data/3
- 프로토콜 : content
- 호스트 : com.android.contacts
- 경로 : /data/3
- -> 연락처 앱(com.android.contacts)[^1] 의 data 테이블에서 3으로 식별되는 데이터를 가져왔다는 뜻이다.
# 기반 기술

# 이 것을 기반으로 한 기술

# 참고자료
- [[Android] 안드로이드 URL, URI 정의 (tistory.com)](https://bada744.tistory.com/138)

# 각주
[^1]: 이 경우 PackageName이 host로 쓰인 것이다.