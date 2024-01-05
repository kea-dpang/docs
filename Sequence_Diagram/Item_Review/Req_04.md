# 요구사항 4번) 상품 리뷰 삭제

1. 상품 리뷰 삭제

```mermaid
sequenceDiagram
    participant Admin as 관리자
    participant WebPage as 웹페이지
    participant Server as 서버
    participant Database as 데이터베이스
    
    Admin ->> WebPage: 상품 리뷰 삭제 요청
    WebPage ->> Server: 상품 리뷰 삭제 요청 전송
    Server ->> Database: 상품 리뷰 삭제 요청
    Database -->> Server: 상품 리뷰 삭제 완료 응답
    Server -->> WebPage: 상품 리뷰 삭제 완료 응답
    WebPage -->> Admin: 삭제 성공 여부 메시지 표시

```