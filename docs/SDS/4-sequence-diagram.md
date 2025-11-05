# 4. Sequence diagram

## 4.1 인증/보안 (AuthController)

### 4.1.1 회원가입
<img width="542" height="564" alt="스크린샷 2025-11-05 18 41 19" src="https://github.com/user-attachments/assets/33bacccb-bbd7-4d9d-9421-30ac1d393dee" />

- 이 Sequence Diagram은 사용자가 **회원가입을 진행하는 과정**을 나타낸다.  
사용자가 회원가입 요청을 전송하면 `AuthController`는 요청 데이터를 `UserService`로 전달한다.  
`UserService`는 내부적으로 `UserServiceImpl`을 호출하여 아이디, 이메일, 닉네임 중복 여부를  
각각 `existsByUsername()`, `existsByEmail()`, `existsByNickname()` 메서드를 통해 검사한다.  
모든 중복 검사를 통과하면 `SignUpRequestDto`의 `toEntity()` 메서드를 호출해 새로운 사용자 엔티티를 생성하고,  
이를 저장한 뒤 회원가입 성공 메시지를 포함한 `ApiResponse`를 반환한다.  
모든 과정이 정상적으로 완료되면 서버는 `201 Created` 응답과 함께 `"회원가입에 성공했습니다."` 메시지를 전송한다.

### 4.1.2 로그인 (액세스/리프레시 토큰 발급)
<img width="512" height="421" alt="스크린샷 2025-11-05 18 42 19" src="https://github.com/user-attachments/assets/517fa232-3787-4717-a246-9d85a77538cb" />

- 이 Sequence Diagram은 사용자가 로그인하여 **Access Token** 및 **Refresh Token**을 발급받는 과정을 나타낸다.  
사용자가 로그인 정보를 입력하고 요청을 전송하면, `AuthController`는 `AuthService`의 `login()` 메서드를 호출한다.  
`AuthServiceImpl`은 사용자 인증을 수행한 뒤 `JwtUtil`을 사용해  
`createAccessToken()`과 `createRefreshToken()` 메서드를 호출하여 JWT 토큰 쌍을 생성한다.  
생성된 Access Token은 HTTP 응답 헤더의 `Authorization` 필드에 추가되고,  
Refresh Token은 `HttpOnly` 속성이 적용된 쿠키로 설정된다.  
모든 처리가 완료되면 `"로그인에 성공했습니다."` 메시지를 포함한 `ApiResponse.ok()`가 반환된다.

### 4.1.3 로그아웃
<img width="488" height="324" alt="스크린샷 2025-11-05 18 42 48" src="https://github.com/user-attachments/assets/1a8dfa1a-e386-4aa9-9ea0-bd73c8e303e8" />

- 이 Sequence Diagram은 사용자가 **로그아웃을 수행하는 과정**을 나타낸다.  
인증된 사용자가 로그아웃 요청을 보내면, `AuthController`는 `AuthService`의 `deleteRefreshToken()` 메서드를 호출하여  
해당 사용자의 Refresh Token을 데이터베이스에서 제거한다.  
그 후 `ResponseCookie`를 사용해 동일한 이름의 쿠키(`refresh_token`)를  
`maxAge(0)`으로 설정하여 즉시 만료시킴으로써 클라이언트 측 쿠키를 제거한다.  
로그아웃이 완료되면 `"로그아웃에 성공했습니다."` 메시지를 포함한 `ApiResponse.ok()`가 반환된다.

### 4.1.4 Refresh 토큰 재발급
<img width="628" height="607" alt="스크린샷 2025-11-05 18 44 08" src="https://github.com/user-attachments/assets/fca8c61b-3ac8-4466-ad32-728d7ceb3640" />

- 이 Sequence Diagram은 사용자가 보유한 **Refresh Token을 이용해 새로운 토큰을 재발급받는 과정**을 나타낸다.  
사용자가 `/auth/refresh` 요청을 전송하면, `AuthController`는 쿠키에서 Refresh Token을 추출하여  
`AuthService`의 `reissueTokens()` 메서드를 호출한다.  
`AuthServiceImpl`은 `JwtUtil`의 `validateToken()`으로 유효성을 검증하고,  
`isRefreshToken()`으로 토큰 유형을 확인하며, `getUserId()`로 사용자 정보를 추출한다.  
이후 `UserService`의 `findById()` 메서드를 호출해 실제 사용자를 조회한 후,  
`JwtUtil`의 `createAccessToken()`과 `createRefreshToken()` 메서드를 통해 새 토큰 쌍을 발급한다.  
새로운 Access Token은 응답 헤더의 `Authorization` 필드에,  
새로운 Refresh Token은 `HttpOnly` 쿠키로 재설정된다.  
최종적으로 `"토큰 재발급에 성공했습니다."` 메시지를 포함한 `ApiResponse.ok()`가 반환된다.

### 4.1.5 쿠키 설정
<img width="627" height="616" alt="스크린샷 2025-11-05 18 49 00" src="https://github.com/user-attachments/assets/02ec01ab-4324-4513-9829-7b739eaaa7e0" />

- 이 Sequence Diagram은 서버가 JWT 토큰을 **클라이언트 응답 쿠키에 안전하게 설정하는 과정**을 나타낸다.  
`AuthController`의 `addCookie()` 메서드는 전달받은 토큰을 `ResponseCookie` 빌더를 통해 구성하며,  
다음의 보안 속성이 설정된다:

  - `HttpOnly = true` : JavaScript 접근 차단 (XSS 공격 방지)  
  - `Secure = true` : HTTPS 환경에서만 전송 가능  
  - `SameSite = Lax` : 외부 사이트 요청으로 인한 CSRF 기본 방어  
  - `Path = "/"` : 애플리케이션 전역에서 사용 가능  
  - `Max-Age` : 토큰 만료 시간(초 단위) 지정  

완성된 쿠키는 HTTP 응답 헤더의 `Set-Cookie` 필드에 추가되어 브라우저에 저장되며,  
이후 인증 요청 시 자동으로 포함된다.

----

## 4.2 사용자 프로필 (UserController)
### 4.2.1 아이디 중복 여부 검사
<img width="643" height="583" alt="스크린샷 2025-11-05 18 53 03" src="https://github.com/user-attachments/assets/c75d5903-07e9-49f6-88ed-144a0e832051" />

- 이 Sequence Diagram은 사용자가 입력한 아이디의 **중복 여부를 검사하는 과정**을 나타낸다.  
사용자가 아이디를 입력하고 중복 확인 요청을 보내면, `UserController`는 `UserService`의 `existsByUsername()` 메서드를 호출한다.  
`UserServiceImpl`은 내부적으로 `UserRepository.existsByUsername()`을 실행하여 데이터베이스에서 동일한 아이디가 존재하는지 확인한다.  
조회 결과가 반환되면, 존재 여부(`true` 또는 `false`)에 따라 분기(`alt`)가 수행된다.  
이미 존재하는 경우 `"이미 사용 중인 아이디입니다."`, 존재하지 않는 경우 `"사용 가능한 아이디입니다."` 메시지를 포함한 `ApiResponse.ok()`가 반환된다.

### 4.2.2 이메일 중복 여부 검사
<img width="541" height="577" alt="스크린샷 2025-11-05 18 54 14" src="https://github.com/user-attachments/assets/58b71344-1035-4708-a85e-d9e39ece1063" />

- 이 Sequence Diagram은 사용자가 입력한 이메일의 **중복 여부를 검사하는 과정**을 나타낸다.  
사용자가 이메일을 입력하고 중복 확인 요청을 전송하면, `UserController`는 `UserService`의 `existsByEmail()` 메서드를 호출한다.  
`UserServiceImpl`은 `UserRepository.existsByEmail()`을 호출하여 동일한 이메일이 이미 등록되어 있는지 검사한다.  
조회 결과가 반환되면, 존재 여부에 따라 조건 분기(`alt`)가 수행되어  
중복 시 `"이미 사용 중인 이메일입니다."`, 사용 가능 시 `"사용 가능한 이메일입니다."` 메시지를 포함한 `ApiResponse.ok()`가 반환된다.

### 4.2.3 닉네임 중복 여부 검사
<img width="635" height="591" alt="스크린샷 2025-11-05 18 54 31" src="https://github.com/user-attachments/assets/34b6e3ed-d990-4aa9-9d5c-917c3559a9e2" />

- 이 Sequence Diagram은 사용자가 입력한 닉네임의 **중복 여부를 확인하는 과정**을 나타낸다.  
사용자가 닉네임 중복 확인 요청을 전송하면, `UserController`는 `UserService`의 `existsByNickname()` 메서드를 호출한다.  
`UserServiceImpl`은 `UserRepository.existsByNickname()`을 호출하여 동일한 닉네임의 존재 여부를 데이터베이스에서 확인한다.  
결과가 반환되면 `alt` 블록에서 조건에 따라 분기되어,  
이미 존재할 경우 `"이미 사용 중인 닉네임입니다."`, 존재하지 않을 경우 `"사용 가능한 닉네임입니다."` 메시지를 담은 `ApiResponse.ok()`가 반환된다.

### 4.2.4 비밀번호 변경
<img width="578" height="508" alt="스크린샷 2025-11-05 18 55 13" src="https://github.com/user-attachments/assets/86a1bc9b-04bc-49dd-8e65-4061e8e47853" />

- 이 Sequence Diagram은 사용자가 **비밀번호를 변경하는 과정**을 나타낸다.  
사용자가 기존 비밀번호, 새 비밀번호, 확인 비밀번호를 포함한 요청을 전송하면,  
`UserController`는 `UserService`의 `changePassword()` 메서드를 호출한다.  
`UserServiceImpl`은 `findById()`로 현재 사용자를 조회한 뒤, 비밀번호 일치 여부를 확인하고,  
문제가 없을 경우 `User` 엔티티의 `changePassword()` 메서드를 통해 암호화된 새 비밀번호로 변경한다.  
모든 과정이 완료되면 `"비밀번호 변경에 성공했습니다."` 메시지를 포함한 `ApiResponse.ok()`가 반환된다.

### 4.2.5 프로필 조회
<img width="635" height="530" alt="스크린샷 2025-11-05 18 55 47" src="https://github.com/user-attachments/assets/19df89e5-79f2-4ecf-88d1-7dc658beecf5" />

- 이 Sequence Diagram은 로그인한 사용자가 자신의 **프로필 정보를 조회하는 과정**을 나타낸다.  
`UserController`는 인증 객체(`CustomUserDetails`)로부터 `userId`를 추출하고,  
`UserService`의 `getUserInfo()` 메서드를 호출한다.  
`UserServiceImpl`은 `UserRepository.findByIdWithSkills()`를 통해 사용자 정보와 기술 스택(`skills`)을 함께 조회한다.  
조회된 엔티티는 `UserResponseDto.from()`을 통해 DTO로 변환되며,  
변환된 데이터는 `"회원정보를 불러오는데 성공했습니다."` 메시지와 함께 `ApiResponse.ok()`로 반환된다.

### 4.2.6 프로필 수정
<img width="655" height="802" alt="스크린샷 2025-11-05 18 56 10" src="https://github.com/user-attachments/assets/3507ff8c-f950-4456-b2c7-0118cfc18b2c" />

- 이 Sequence Diagram은 로그인한 사용자가 자신의 **프로필 정보를 수정하는 과정**을 나타낸다.  
`UserController`는 인증 정보(`CustomUserDetails`)와 수정 요청 DTO(`UserUpdateRequestDto`)를 전달하며  
`UserService.updateUserInfo()` 메서드를 호출한다.  
`UserServiceImpl`은 먼저 `findByIdWithSkills()`를 통해 현재 사용자를 조회한 뒤,  
닉네임 변경 요청이 있을 경우 `existsByNickname()`으로 중복 여부를 검사한다.  
이후 `updateNickname()`, `updateGrade()`, `updatePosition()`, `updateSkills()`, `updateShortIntro()` 메서드들이 순차적으로 호출되어  
각 필드가 업데이트된다.  
최종적으로 수정된 엔티티는 `UserResponseDto.from()`으로 DTO 변환 후,  
`"회원정보를 수정하는데 성공했습니다."` 메시지와 함께 `ApiResponse.ok()`로 반환된다.

### 4.2.7 회원 탈퇴
<img width="453" height="447" alt="스크린샷 2025-11-05 18 56 40" src="https://github.com/user-attachments/assets/2214dd8d-ecb4-4c36-808f-cf94d04c98a4" />

- 이 Sequence Diagram은 사용자가 자신의 **계정을 탈퇴하는 과정**을 나타낸다.  
인증된 사용자가 탈퇴 요청을 전송하면, `UserController`는 `UserService`의 `withdraw()` 메서드를 호출한다.  
`UserServiceImpl`은 `findById()`로 사용자를 조회한 후, 입력된 비밀번호를 검증하고  
비밀번호가 일치하면 `User.withdraw()`를 호출하여 상태를 `WITHDRAWN`으로 변경한다.  
이후 `addCookie()` 메서드를 통해 `refresh_token` 쿠키를 `maxAge(0)`으로 설정하여 만료시킨다.  
모든 과정이 정상적으로 완료되면 `"회원탈퇴에 성공했습니다."` 메시지를 포함한 `ApiResponse.ok()`가 반환된다.

### 4.2.8 리뷰 조회
<img width="1040" height="564" alt="스크린샷 2025-11-05 18 57 18" src="https://github.com/user-attachments/assets/329a5780-9d8a-4a81-8bca-1681861d7074" />

- 이 Sequence Diagram은 사용자가 특정 회원이 **받은 리뷰 목록을 조회하는 과정**을 나타낸다.  
`UserController`는 요청 경로의 `userId`를 바탕으로 `UserService.existsById()`를 호출해 대상 사용자의 존재 여부를 확인한다.  
사용자가 존재하면, `ReviewService.getReviewsByRevieweeId()`가 호출되고,  
`ReviewServiceImpl`은 `ReviewRepository.findByRevieweeIdAndStatus()`를 통해 리뷰 데이터를 조회한다.  
조회된 결과는 `ReviewResponseDto.from()`을 통해 DTO로 변환되며,  
`PageResponse<ReviewResponseDto>` 형태로 페이징 처리된다.  
최종적으로 `"리뷰 조회에 성공했습니다."` 메시지와 함께 `ApiResponse.ok()`가 반환된다.








----

## 4.3 프로젝트 관리 (ProjectController)
### 4.3.1 프로젝트 생성
### 4.3.2 프로젝트 수정
### 4.3.3 프로젝트 상세 조회
### 4.3.4 프로젝트 목록/검색/필터 조회
### 4.3.5 프로젝트 삭제
### 4.3.6 모집 시작/마감(상태 전환)
### 4.3.7 프로젝트 북마크 토글 연동(후술 4.9 참조)
### 4.3.8 프로젝트 관련 알림 발생 연동(후술 4.10 참조)

----

## 4.4 지원/신청 (ApplicationController)
### 4.4.1 프로젝트 지원 생성
### 4.4.2 지원 취소
### 4.4.3 지원 내역 조회(내 지원/프로젝트별 지원자)
### 4.4.4 지원 상태 변경(승인/거절) [Assignment/Team 연계, 4.5/4.6 참조]
### 4.4.5 지원 관련 알림 발생(후술 4.10 참조)

----

## 4.5 배정/역할 관리 (AssignmentController)
### 4.5.1 멤버 배정 생성(프로젝트/팀/역할)
### 4.5.2 배정 변경/해제
### 4.5.3 배정 현황 조회
### 4.5.4 지원 승인과의 연계(4.4.4 연동)
### 4.5.5 배정 관련 알림 발생(후술 4.10 참조)

----

## 4.6 팀 관리 (TeamController)
### 4.6.1 팀 생성
### 4.6.2 팀 정보 수정
### 4.6.3 팀 상세/구성원 조회
### 4.6.4 팀원 추가/제거(Assignment 연계)
### 4.6.5 팀 해체(해당 시)
### 4.6.6 팀 관련 알림 발생(후술 4.10 참조)

----

## 4.7 스터디 관리 (StudyController)
### 4.7.1 스터디 생성
### 4.7.2 스터디 수정
### 4.7.3 스터디 상세 조회
### 4.7.4 스터디 목록/검색 조회
### 4.7.5 스터디 삭제
### 4.7.6 스터디 모집/신청/관리(해당 시 Application/Assignment 유사 흐름)

----

## 4.8 리뷰/평가 (ReviewController)
### 4.8.1 리뷰 작성
### 4.8.2 리뷰 수정/삭제
### 4.8.3 대상별 리뷰 조회(프로젝트/사용자 등)
### 4.8.4 리뷰 신고/차단(해당 시)
### 4.8.5 리뷰 관련 알림 발생(후술 4.10 참조)

----

## 4.9 북마크 (BookmarkController)
### 4.9.1 프로젝트 북마크 토글
### 4.9.2 북마크 목록 조회
### 4.9.3 북마크와 권한/상태 연동(비공개/마감 시 처리)

----

### 4.10 알림/이벤트 발행 흐름 (Notification 도메인 연계)
### 4.10.1 지원 생성 시 알림 발행
### 4.10.2 지원 승인/거절 시 알림 발행
### 4.10.3 배정/팀 변경 시 알림 발행
### 4.10.4 리뷰 작성 시 알림 발행
### 4.10.5 프로젝트 상태 변경(모집 시작/마감) 알림
### 4.10.6 알림 읽음/수신함 처리(해당 시)

----

## 4.11 전역 예외 처리 (GlobalExceptionHandler)
### 4.11.1 검증 예외 흐름
### 4.11.2 인증/인가 예외 흐름
### 4.11.3 도메인 예외 흐름
### 4.11.4 공통 에러 응답 포맷 반환

----

## 4.12 교차 도메인 시나리오(엔드투엔드)
### 4.12.1 프로젝트 모집 → 지원 → 승인 → 배정 → 팀 합류
### 4.12.2 프로젝트 즐겨찾기(북마크)와 목록/상세/권한 연동
### 4.12.3 스터디 생성 → 모집/신청 → 참여 확정
### 4.12.4 협업 종료 후 리뷰 작성 → 알림/노출
