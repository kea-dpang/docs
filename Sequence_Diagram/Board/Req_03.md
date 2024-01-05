# 요구사항 3번) 게시글 수정

1. 게시글 수정
    - (사용자) 고객 문의 수정 - 관리자 답변 전(before)에 가능
    - (관리자) 마일리지 사용 방법 수정
    - (관리자) FAQ 수정

```mermaid
sequenceDiagram
   participant User as 사용자
   participant Webpage as 웹페이지
   participant System as 시스템
   participant Database as 데이터베이스

   User->>Webpage: 게시글 수정 버튼 클릭
   Webpage->>System: 게시글 수정 요청
   System->>Database: 게시글 정보 수정
   Database-->>System: 수정 데이터 반환
   System-->>Webpage: 요청 데이터 반환
   Webpage-->>User: 게시글 수정 확인
```