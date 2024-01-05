# 요구사항 1번) 회원가입

1. 직원 확인용으로 사원번호, 아이디(회사 이메일), 비밀번호, 이름, 입사일 적고 회원가입 신청
    - (관리자) 회원가입 신청하면 인사팀에서 가지고 있는 데이터와 위의 사원번호와 입사일을 대조하여 직원 확인 후 회원가입 승인
    - 비밀번호 8자 이상 / 대문자 + 소문자 + 특수문자 포함
    - 60일마다 비밀번호 갱신 요청
2. 개인 정보 입력 : 간단한 개인 정보들로 편리하게 가입할 수 있도록 함
3. 이용약관 및 개인정보 처리방침 동의
4. 가입 신청 완료 안내 화면 표시
    - 가입 실패시 '등록된 정보가 없습니다. 인사팀에 문의해주세요'라는 알림창이 뜬다.

```mermaid
sequenceDiagram
    participant User as 사용자
    participant Admin as 관리자
    participant Webpage as 웹페이지
    participant AuthService as 인증 서비스
    participant AuthDatabase as 인증 데이터베이스
    participant HR as 인사팀
    User ->> Webpage: 회원가입 버튼 클릭
    Webpage -->> User: 이용약관 및 개인정보 처리방침 동의 요청
    User ->> Webpage: 동의
    Webpage -->> User: 회원가입 화면 제공
    User ->> Webpage: 개인 정보 입력 후 제출 버튼 클릭
    Webpage ->> AuthService: 회원 가입 요청
    AuthService ->> HR: 사원 확인 요청
    HR ->> HR: 사원 확인 응답
    HR -->> AuthService: 사원 확인 응답

    alt 확인 성공
        AuthService ->> AuthDatabase: 회원 등록 요청
        AuthDatabase -->> AuthService: 회원 등록 응답
        AuthService -->> User: 가입 신청 완료 안내

    else 확인 실패
        AuthService -->> User: 회원 확인 실패 안내
        AuthService -) Admin: 회원 가입 승인 요청
        Admin ->> AuthService: 승인 요청
        alt 승인
            AuthService -) User: 가입 신청 완료 안내
            AuthService -->> Admin: 승인 응답
        else 미승인
            AuthService -) User: '등록된 정보가 없습니다. 인사팀에 문의해주세요' 알림
            AuthService -->> Admin: 승인 미응답
        end
    end


```