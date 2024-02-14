## 인증 서비스

- **role**
    * 1: user
    * 2: admin
    * 3: super_admin

```mermaid
erDiagram
    auth {
        bigint user_id PK "사용자 ID"
        varchar(320) email "이메일 (ID)"
        varchar(255) password "비밀번호"
        enum role "역할"
        varchar(255) refresh_token "리프레쉬 토큰"
        datetime(6) created_at "생성 날짜"
        datetime(6) updated_at "변경 날짜"
    }
   user ||--|| auth: ""
```

## 사용자 서비스

- **탈퇴 사유**
    * 1: 고객서비스 불만
    * 2: 배송 불만
    * 3: 환불 정책 불만
    * 4: 방문 빈도 낮음
    * 5: 상품 가격 불만
    * 6: 개인 정보 유출 우려
    * 7: 신뢰도 불만
    * 8: 퇴사

```mermaid
erDiagram
    user {
        bigint user_id PK "사용자 ID"
        varchar(255) email "사용자 이메일"
        datetime(6) created_time "생성 날짜"
        datetime(6) updated_time "변경 날짜"
    }

    user_detail {
        bigint user_detail_id PK "사용자 상세 ID"
        bigint user_id FK "사용자 ID"
        int employee_number "사원 번호"
        datetime(6) join_date "입사 날짜"
        varchar(20) name "이름"
        varchar(15) phone_number "전화번호"
        varchar(5) zip_code "우편번호"
        varchar(255) address "주소"
        varchar(255) detail_address "상세 주소"
    }

    user_withdrawal {
        bigint user_withdrawal_id PK "유저 탈퇴 아이디"
        enum withdrawal_reason "탈퇴 사유"
        varchar(500) message "남길 말씀"
        datetime(6) withdrawal_date "탈퇴 날짜"
    }

    cart {
        bigint cart_id PK "장바구니 ID"
        bigint user_id FK "사용자 ID"
    }

   cart_item {
      bigint cart_item_id PK "장바구니-상품 ID"
      bigint cart_id FK "장바구니 ID"
      bigint item_id "상품 ID"
      int quantitu "수량"
   }

    user ||--|| cart: ""
    user ||--o| user_withdrawal: ""
    user ||--|| user_detail: ""
    cart ||--|{ cart_item: ""
    item }|--|| cart_item: ""

```

## 마일리지 서비스

- **상태**
    * 1: 요청
    * 2: 승인
    * 3: 반려

```mermaid
erDiagram
    mileage {
        bigint user_id FK "사용자 ID"
        int mileage "사원 마일리지"
        int personal_charged_mileage "개인 충전 마일리지"
        date(6) join_date "가입 날짜"
        datetime(6) updated_at "업데이트 날짜"
    }

    charge_request {
        bigint charge_request_id PK "충전 요청 ID"
        bigint user_id FK "유저 ID"
        enum status "상태"
        datetime(6) request_date "요청 날짜"
        varchar(20) depositor_name "입금자명"
        int requested_mileage "충전 희망 마일리지"
    }

    user ||--|| mileage: ""
    user ||--o{ charge_request: ""
```

## 주문 서비스

- **주문 및 배송 상태**
    * 1: 결제 완료
    * 2: 배송 요청
    * 3: 배송 준비중
    * 4: 배송중
    * 5: 배송 완료

- **환불 사유**
    * 1: 사이즈 안 맞음
    * 2: 단순 변심
    * 3: 제품 불만족
    * 4: 배송 지연
    * 5: 제품 오배송
    * 6: 기타

- **환불 상태**
    * 1: 반품 요청
    * 2: 회수중
    * 3: 반품 완료

```mermaid
erDiagram
    orders {
        bigint order_id PK "주문 번호"
        bigint user_id FK "사용자 ID"
        varchar(500) delivery_request "배송시 요청사항"
        int product_payment_amount "결제 금액"
        int delivery_fee "배송비"
        datetime(6) order_date "생성 날짜"
        datetime(6) updated_at "변경 날짜"
    }

    order_recipients {
        bigint order_id FK "주문 ID"
        varchar(20) receiver_name "받는 사람"
        varchar(15) receiver_phone_number "받는 사람 전화번호"
        varchar(5) receiver_zip_code "받는 사람 우편번호"
        varchar(255) receiver_address "받는 사람 주소"
        varchar(255) receiver_detail_address "받는 사람 상세 주소"
    }

    order_details {
        bigint order_detail_id PK "주문 상세 ID"
        bigint order_id FK "주문 번호"
        bigint item_id FK "상품 ID"
        enum status "주문 및 배송 상태"
        int purchase_price "구매 금액"
        int quantity "수량"
    }

    order_refunds {
        bigint refund_id PK "환불 ID"
        bigint order_detail_id FK "주문 상세 ID"
        enum refund_reason "환불 사유"
        varchar(500) note "비고"
        datetime(6) refund_request_date "환불 요청 날짜"
        datetime(6) refund_complete_date "환불 완료 날짜"
        int refund_amount "환불 예정액"
        enum refund_status "환불 상태"
    }

    order_recalls {
        bigint recall_id PK "회수 정보 ID"
        bigint refund_id FK "환불 ID"
        varchar(20) retriever_name "회수자 명"
        varchar(15) retriever_phone_number "회수자 연락처"
        varchar(255) retriever_address "회수자 주소"
        varchar(500) retrieval_message "회수 메시지"
    }

    order_cancels {
        bigint cancel_id PK "취소 ID"
        bigint order_id FK "주문 ID"
        datetime(6) cancel_request_date "취소 요청 날짜"
        datetime(6) cancel_complete_date "취소 완료 날짜"
        int refund_amount "환불 예정액"
    }

    orders ||--|{ order_details: ""
    order_details ||--o| order_refunds: ""
    orders ||--|| order_recipients: ""
    order_details ||--o| order_cancels: ""
    order_refunds ||--|| order_recalls: ""
    user ||--o{ orders: ""
    item ||--o| order_details: ""

```

## 상품 서비스

- **카테고리**
    * 1: 패션
    * 2: 뷰티
    * 3: 스포츠/레저
    * 4: 디지털/가전
    * 5: 인테리어
    * 6: 출산/유아동
    * 7: 생활

- **상세 카테고리**
    * 1: 여성의류
    * 2: 남성의류
    * 3: 언더웨어
    * 4: 신발
    * 5: 가방/지갑/잡화
    * 6: 쥬얼리/시계/액세서리

```mermaid
erDiagram
    items {
        bigint item_id PK "상품 ID"
        bigint seller_id FK "판매처 ID"
        bigint event_id FK "이벤트 ID"
        varchar(255) item_name "상품명"
        int item_price "상품 가격"
        enum item_category "카테고리"
        enum sub_category "상세 카테고리"
        float average_rating "평균 평점"
        int stock_quantity "재고 수량"
        int review_count "리뷰 개수"
        int discount_rate "할인율"
        int discount_price "할인가"
        varchar(500) description "상품 상세 설명"
        varchar(255) thumbnail_image "상품 썸네일 사진"
        datetime(6) created_at "생성 날짜"
        datetime(6) updated_at "변경 날짜"
    }

    reviews {
        bigint item_review_id PK "리뷰 ID"
        bigint reviewer_id FK "리뷰 작성자 ID"
        bigint item_id FK "상품 ID"
        varchar(500) review_content "리뷰 내용"
        int rating "평점"
        datetime(6) created_time "생성 날짜"
        datetime(6) updated_time "변경 날짜"
    }

    item_information_images {
        bigint item_id FK "상품 ID"
        string information_images "상품 정보 사진"
    }

    items ||--o{ item_information_images: ""
    items ||--o{ reviews: ""
    seller ||--|{ items: ""
    event |o--|{ items: ""
    user ||--o{ reviews: ""
```

## FAQ 서비스

- **문의 카테고리**
    * 1: 자주 찾는 FAQ
    * 2: 배송
    * 3: 취소/교환/환불
    * 4: 결제
    * 5: 회원
    * 6: 기타

```mermaid
erDiagram
    faq {
        bigint faq_id PK "FAQ ID"
        bigint author_id "작성자 ID"
        varchar(1000) question "질문"
        varchar(1000) answer "답변"
        enum category "카테고리"
        datetime(6) created_time "생성 날짜"
        datetime(6) updated_time "변경 날짜"
    }
    user ||--o{ faq: ""
```

## 1:1 문의 서비스

- **문의 카테고리**
    * 1: 상품 문의
    * 2: 회원 정보
    * 3: 상품 확인
    * 4: 배송
    * 5: 교환/취소
    * 6: 기타

- **문의 상태**
    * 1: 처리 중
    * 2: 완료 상태

- **공개 여부**
    * 1: true
    * 2: false

```mermaid
erDiagram
    qna {
        bigint qna_id PK "문의 ID"
        bigint author_id FK "문의 작성자 ID"
        bigint responder_id FK "답변자 ID"
        bigint item_id FK "상품 ID"
        varchar(100) title "문의 제목"
        enum category "문의 카테고리"
        varchar(1000) content "문의 내용"
        varchar(1000) answer "문의 답변"
        enum state "문의 상태"
        varchar(255) attachment_url "첨부 사진 링크"
        datetime(6) created_at "생성 날짜"
        datetime(6) updated_at "변경 날짜"
    }

    user ||--|{ qna: ""
    items ||--o{ qna: ""
```

## 이벤트 서비스

- **이벤트 상태**
    * 1: 진행중
    * 2: 종료

- **이벤트 종류**
    * 1: 상품
    * 2: 판매처

```mermaid
erDiagram
    event {
        bigint event_id PK "이벤트 ID"
        varchar(100) event_name "이벤트 이름"
        varchar(255) image_path "이벤트 이미지 경로"
        enum event_status "이벤트 상태"
        double discount_rate "이벤트 할인율"
        datetime(6) registration_date "이벤트 등록일"
        date(3) start_date "이벤트 시작일"
        date(3) end_date "이벤트 종료일"
    }

    event_item {
        bigint event_id PK "이벤트 ID"
        bigint item_id FK "상품 ID"
    }

    event_seller {
        bigint event_id PK "이벤트 ID"
        bigint seller_id FK "판매처 아이디"
    }

    event_target_item {
        bigint event_target_item_id PK "대상 상품 코드 식별자"
        bigint event_id FK "이벤트 식별자"
        bigint item_id FK "상품 ID"
    }

    event ||--|{ event_type: "서브타입"
    event_type ||--|| event_item: "서브타입1(상품)"
    event_type ||--|| event_seller: "서브타입2(판매처)"
    event_item ||--|{ event_target_item: ""
    seller ||--|| event_seller: ""

```

## 판매처 서비스

```mermaid
erDiagram
    seller {
        bigint seller_id PK "판매처 ID"
        varchar(15) seller_phone_number "판매처 연락처"
        varchar(100) sales_name "판매처명"
        varchar(20) seller_staff "판매처 담당 직원"
    }

  seller_detail {
      bigint seller_detail_id PK "판매처 세부 ID"
      bigint seller_id FK "판매처 ID"
      varchar(20) seller_manager "담당자"
      date contract_expiry_date "계약 만료일"
      text note "비고"
  }

    seller ||--|| seller_detail: ""

```
