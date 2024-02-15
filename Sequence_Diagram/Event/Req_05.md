# 요구사항 5번) 이벤트 삭제

1. 이벤트 삭제

```mermaid
sequenceDiagram
    participant Admin as 관리자
    participant Webpage as 웹페이지
    participant System as 시스템
    participant Database as 데이터베이스

    Admin ->> Webpage: 이벤트 페이지 삭제 버튼 클릭
    Webpage ->> System: 이벤트 페이지 삭제 요청
    System ->> Database: 이벤트 페이지 삭제
    Database ->> System: 삭제 결과 반환
    System -->> Webpage: 요청 결과 반환
    Webpage -->> Admin: 이벤트 페이지 삭제 확인
```