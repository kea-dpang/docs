# 요구사항 1번) 이벤트 등록

1. 이벤트 등록

```mermaid
sequenceDiagram
    participant Admin as 관리자
    participant Webpage as 웹페이지
    participant System as 시스템

    Admin ->> Webpage: 이벤트 등록 요청
    Webpage ->> System: 이벤트 등록 요청 전송
    System ->> System: 이벤트 등록
    System -->> Webpage: 이벤트 등록 요청 응답
    Webpage -->> Admin: 이벤트 페이지 표시
```