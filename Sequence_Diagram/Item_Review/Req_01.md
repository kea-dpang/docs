# 요구사항 1번) 상품 리뷰 작성

1. 사용자는 구매한 상품에 대해 별점과 함께 리뷰를 작성할 수 있다.
    - 리뷰는 평점과 함께 (0 ~ 5)
    - 리뷰는 300자 이하
    - 리뷰에 사진 첨부 가능
    - 리뷰는 구매자만 작성 가능
    - 구매한지 일주일 혹은 한달 이용 리뷰 등 구분해서 작성
2. 구매한 사람에게 ‘최근에 구매하신 ~ 에 대한 리뷰를 작성해주세요!’ 팝업으로 리뷰 작성 유도

```mermaid
sequenceDiagram
    participant User as 사용자
    participant WebPage as 웹페이지
    participant Server as 서버
    participant Database as 데이터베이스
    
    User ->> WebPage: 구매한 상품에 대한 리뷰 작성 요청
    Note over User, WebPage: 구매자만 작성 가능
    WebPage ->> Server: 리뷰 작성 요청 전달
    Server ->> Database: 리뷰 작성 처리 요청
    Database -->> Server: 리뷰 작성 완료 응답
    Server -->> WebPage: 리뷰 작성 완료 응답
    WebPage -->> User: 리뷰 작성 완료 알림
    Note over User, Database: 일정 시간 경과 후
    Server -) WebPage: 리뷰 작성 유도 정보 전송
    WebPage -) User: 리뷰 유도 팝업 표시

```