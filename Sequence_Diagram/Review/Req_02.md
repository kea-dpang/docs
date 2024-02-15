# 요구사항 2번) 상품 리뷰 조회

1. 사용자는 다른 사용자의 리뷰를 조회할 수 있다.
    - 상품의 평점은 사용자의 전체 평점을 평균을 내고 소숫점 첫 자리까지 표시

```mermaid
sequenceDiagram
    participant User as 사용자
    participant WebPage as 웹페이지
    participant Server as 서버
    participant Database as 데이터베이스
    
    User ->> WebPage: 상품 리뷰 조회 요청
    WebPage ->> Server: 상품 리뷰 정보 요청
    Server ->> Database: 상품 리뷰 조회 요청
    Database -->> Server: 상품 리뷰 정보 응답
    Server -->> WebPage: 상품 정보 전송
    WebPage ->> User: 리뷰 정보 표시

```