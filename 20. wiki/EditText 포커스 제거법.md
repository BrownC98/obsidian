---
tags:
  - android
  - 문제해결
created: 2024-06-23T21:37
updated: 2024-06-28T16:50
---
레이아웃 최상단에 
가로 세로 1dp 자리 뷰를 만든다음
```java
binding.svSearch.post(() -> {  
    binding.svSearch.clearFocus();  
});
```
editText에 clearView를 해주면 됨

# post()에 대해

post()로 감싼 이유는 post()는 ui 처리 큐에 인자로 주어진 runnable을 추가 하는 메소드다.
그래서 post()를 사용하면 다른 ui처리가 완료된 후에 인자로 주어진 runnable이 실행된다.

여기서 post()를 사용한 이유는 editText와 관련된 view 연산(ex) 포커싱)이 완료된 이후에 clearfocus()를 실행해야 의도된 대로 코드가 작동하기 때문이다.

editText에 포커스가 확실히 잡힌 후 다른 뷰로 포커스를 넘겨야 하기 때문

# 더미 뷰에 대해
더미 뷰는 다음 조건을 만족 해야함
android:focusable="true"  
android:focusableInTouchMode="true"  
android:visibility="visible"

visible에서만 포커스를 제대로 흡수한다.

width, height 둘 중 하나라도 0dp면 작동하지 않음

## 더미 뷰 위치
포커스 더미 뷰를 레이아웃 최상단에 위치 한 이유는  터치 모드가 아닐 때 clearFocus를 실행하면 레이아웃 최상단에서 부터 포커스 가능한 뷰를 찾아 포커스를 부여하기 때문이다. 

이 방법 말고 다른 방법을 사용하면
키보드는 제거되도 커서가 깜빡이거나 

루트뷰에 포커싱이 잡히는 등 의도치 않은 동작이 발생한다.