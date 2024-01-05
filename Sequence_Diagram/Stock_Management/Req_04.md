# 요구사항 4번) 재고 이상 알림

1. 재고 이상 알림
    - 알림 방식은 관리자 이메일과 브라우저 PUSH 알림
    - 재고 부족 알림: 재고 수량이 최소 수준으로 떨어질 경우 관리자에게 자동으로 알림 전송
    - 재고 초과 알림: 재고 수량이 최대 수준을 초과할 경우 관리자에게 알림 전송
        - 관리자가 상품 등록 및 수정에서 최대/최소 수준 지정 가능

```mermaid
sequenceDiagram
    participant Admin as 관리자
    participant WebPage as 웹페이지
    participant StockService as 재고 서비스
    participant StockDatabase as 재고 데이터베이스
    participant NotificationService as 알림 서비스
    
    Admin ->> WebPage: 상품 재고 수정 요청
    WebPage ->> StockService: 상품 재고 수정 요청 전송
    StockService ->> StockDatabase: 상품 재고 수정 요청
    StockDatabase -->> StockService: 상품 재고 수정 완료

    alt 재고 수량 최소 수준 이하
        StockService ->> NotificationService: 재고 부족 알림 요청
        NotificationService -) Admin: 재고 부족 이메일 알림 생성
        NotificationService -) Admin: 재고 부족 브라우저 PUSH 알림 전송
        NotificationService -->> StockService: 알림 발송 상태 응답

    else 재고 수량 최대 수준 초과
        StockService ->> NotificationService: 재고 초과 알림 생성
        NotificationService -) Admin: 재고 초과 이메일 알림 생성
        NotificationService -) Admin: 재고 초과 브라우저 PUSH 알림 전송
       NotificationService -->> StockService: 알림 발송 상태 응답
    end

```