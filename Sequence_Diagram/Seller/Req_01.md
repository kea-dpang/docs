# 요구사항 1번) 판매처 등록

판매처 관리를 위한 판매처 등록
판매처 정보에는 판매처 이름, 담당자명, 판매자 연락처에 대한 정보가 들어감.

```mermaid
sequenceDiagram
    participant Admin as 관리자
    participant WebPage as 웹페이지
    participant Server as 서버
    participant Database as 데이터베이스

    Admin ->> WebPage: 판매처 정보 입력 후 등록 버튼 클릭
    WebPage ->> Server: 판매처 정보 등록 요청
    Server ->> Database: 판매처 정보 데이터 등록
    Database -->> Server: 등록 결과 반환
    Server -->> WebPage: 요청 결과 반환
    WebPage -->> Admin: 판매처 정보 등록 확인
```
