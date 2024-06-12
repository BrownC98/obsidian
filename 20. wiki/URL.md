---
tags:
  - 개발
created: 2024-06-12T15:13
updated: 2024-06-12T16:33
---
Uniform Resource Locator
경로(Location)를 구분하는 식별자
URL은 [[URI]]에 포함되는 개념이다

URL은 [[자원(프로그래밍)]]을 경로 기반으로 식별한다.

URL은 웹에서 [[자원(프로그래밍)|리소스]]의 위치를 가리키는 데 사용되는 구체적인 유형의 URI이다.

컴퓨터 네트워크 상에 리소스가 어디있는지 알려주기 위한 규약이다.
# 구성
URL은 스키마(프로토콜), 도메인, 경로, 포트 번호, 쿼리 문자열 등 자원의 위치를 명시하는 정보로 구성된다.

![[Pasted image 20240612160721.png]]

## Protocol(Scheme)
- 어플리케이션 계층의통신 규약을 명시한다.
- FTP, HTTP, HTTPS 등이 해당됨
## Host Name (IP Address)
- 도메인 이름이나 IP 주소가 들어간다.
## Port
- 접속 포트
## Path
- 리소스 경로
- 계층적 구조로 구성됨
	- /home/file1.jpg
	- /members
	- /members/100
	- /items/iphone12
## query
- key=value 형태
- ?로 시작, &로 여러개 추가 가능
- 서버에 제공하는 추가 정보

출처: [https://inpa.tistory.com/entry/WEB-🌐-URL-구성-요소-요청-흐름-정리](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-%EA%B5%AC%EC%84%B1-%EC%9A%94%EC%86%8C-%EC%9A%94%EC%B2%AD-%ED%9D%90%EB%A6%84-%EC%A0%95%EB%A6%AC) [Inpa Dev 👨‍💻:티스토리]
# 참고자료
- [[Android] 안드로이드 URL, URI 정의 (tistory.com)](https://bada744.tistory.com/138)
- [URI, URL, URN 의 차이 한 번에 정리하기 : 리소스 구분 관점에서 보는 URI, URL, URN의 차이 — 조세영의 Kotlin World](https://kotlinworld.com/96#URI%EC%--%--%--URL%-C%--URN%EC%-D%--%--%EA%B-%--%EA%B-%--)
- [🌐 URL 구성 요소 & 요청 흐름 정리 (tistory.com)](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-%EA%B5%AC%EC%84%B1-%EC%9A%94%EC%86%8C-%EC%9A%94%EC%B2%AD-%ED%9D%90%EB%A6%84-%EC%A0%95%EB%A6%AC)
# 각주
---