# 요구사항 2번) 반품/환불 신청 목록 확인

1. 반품/환불 신청 목록 확인

```mermaid
sequenceDiagram
    participant Admin as 관리자
    participant WebPage as 웹페이지
    participant System as 시스템
    participant Database as 데이터베이스

    Admin ->> WebPage: 반품/환불 신청 목록 확인
    WebPage ->> System: 신청 목록 조회 요청
    System ->> Database: 반품/환불 신청 데이터 등록
    Database -->> System: 등록 결과 반환
    System -->> WebPage: 요청 결과 반환
    WebPage -->> Admin: 반품/환불 신청 목록 표시
```