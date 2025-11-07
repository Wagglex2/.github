# 4. Sequence Diagram

본 장에서는 Waggle 시스템의 주요 기능별 시퀀스 다이어그램을 정리한다. 각 다이어그램은 요청부터 응답까지의 전체 처리 흐름을 시간 순서에 따라 표현하며, Controller, Service, Repository 계층 간의 상호작용과 데이터 흐름을 명확히 보여준다.

## 4.1 Authentication & Authorization

인증 및 인가 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.1.1 회원가입 (Sign Up)

<img width="583" height="451" alt="auth-sign-up-sequence" src="https://github.com/user-attachments/assets/286faaca-6a71-483e-957e-3ac8901b90c7" />

이 Sequence Diagram은 사용자가 회원가입을 진행하는 과정을 나타낸다.

- 사용자가 POST /api/v1/auth/sign-up 요청을 전송하면, AuthController는 UserService의 signUp() 메서드를 호출한다.

- UserServiceImpl은 UserRepository의 existsByUsername(), existsByEmail(), existsByNickname() 메서드를 순차적으로 호출하여 아이디, 이메일, 닉네임의 중복 여부를 확인한다. 중복이 발견되면 해당하는 BusinessException을 발생시킨다.

- 모든 중복 검사가 통과하면, PasswordEncoder를 통해 비밀번호를 암호화한 후 User 엔티티를 생성하고 UserRepository의 save() 메서드를 호출하여 저장한다.

- 최종적으로 "회원가입에 성공했습니다." 메시지와 함께 생성된 userId를 포함한 ApiResponse.ok()가 201 Created 상태로 반환된다.


### 4.1.2 로그인 (Sign In)

<img width="1259" height="696" alt="auth-sign-in-sequence" src="https://github.com/user-attachments/assets/e889c994-7e3a-4e36-b1e1-cb9ca16c92ff" />

이 Sequence Diagram은 사용자가 로그인하여 JWT 토큰을 발급받는 과정을 나타낸다.

- 사용자가 POST /api/v1/auth/sign-in 요청을 전송하면, AuthController는 AuthService의 login() 메서드를 호출한다.

- AuthServiceImpl은 AuthenticationManager의 authenticate() 메서드를 호출하여 사용자 인증을 수행한다. 인증이 실패하면 BusinessException(INVALID_CREDENTIALS)을 발생시킨다.

- 인증이 성공하면, CustomUserDetails에서 사용자 정보를 추출한 후 JwtUtil의 createAccessToken()과 createRefreshToken() 메서드를 호출하여 새로운 토큰 쌍을 생성한다.

- 이후 RedisTemplate을 통해 기존 Refresh Token을 삭제하고, 새로 생성한 Refresh Token을 Redis에 저장한다.

- Controller는 새로 생성한 Access Token을 응답 헤더의 Authorization 필드에 추가하고, Refresh Token을 HttpOnly 쿠키로 설정한 후 "로그인에 성공했습니다." 메시지를 포함한 ApiResponse.ok()를 반환한다.


### 4.1.3 로그아웃 (Sign Out)

<img width="816" height="380" alt="auth-sign-out-sequence" src="https://github.com/user-attachments/assets/d27e6b7e-5f5d-46d8-9d8b-448fdbc86a7d" />

이 Sequence Diagram은 사용자가 로그아웃을 진행하는 과정을 나타낸다.

- 사용자가 POST /api/v1/auth/sign-out 요청을 전송하면, AuthController는 AuthService의 logout() 메서드를 호출한다.

- AuthServiceImpl은 RedisTemplate을 통해 현재 사용자의 Refresh Token을 삭제한다.

- 최종적으로 "로그아웃에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.1.4 토큰 재발급 (Refresh Token)

<img width="1223" height="1054" alt="auth-refresh-token-sequence" src="https://github.com/user-attachments/assets/8a5bca93-2c46-486a-91a9-4315a11b54e3" />

이 Sequence Diagram은 사용자가 보유한 Refresh Token을 이용해 새로운 토큰을 재발급받는 과정을 나타낸다.

- 사용자가 POST /api/v1/auth/refresh 요청을 전송하면, AuthController는 쿠키에서 Refresh Token을 추출하여 AuthService의 reissueTokens() 메서드를 호출한다.

- AuthServiceImpl은 JwtUtil의 validateToken()으로 유효성을 검증하고, isRefreshToken()으로 토큰 유형을 확인하며, getUserId()로 사용자 정보를 추출한다.

- 이후 RedisTemplate을 통해 저장된 Refresh Token을 조회하여 요청으로 받은 토큰과 일치 여부를 확인한다. 토큰이 일치하지 않거나 만료된 경우 BusinessException을 발생시킨다.

- 토큰 검증이 완료되면 UserService의 findById() 메서드를 호출해 실제 사용자를 조회한 후, JwtUtil의 createAccessToken()과 createRefreshToken() 메서드를 통해 새 토큰 쌍을 발급한다.

- 새로운 Access Token은 응답 헤더의 Authorization 필드에, 새로운 Refresh Token은 HttpOnly 쿠키로 재설정된다.

- 최종적으로 "토큰 재발급에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.

---

## 4.2 User Profile

사용자 프로필 관리 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.2.1 내 프로필 조회 (Get My Profile)

<img width="910" height="534" alt="user-get-me-sequence" src="https://github.com/user-attachments/assets/f7f5b746-6d29-472c-8f96-6591e1fad804" />

이 Sequence Diagram은 현재 로그인한 사용자가 자신의 프로필 정보를 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/users/me 요청을 전송하면, UserController는 UserService의 getUserInfo() 메서드를 호출한다.

- UserServiceImpl은 UserRepository의 findByIdWithSkills() 메서드를 호출하여 사용자 정보와 기술 스택을 Fetch Join으로 함께 조회한다.

- 조회된 User 엔티티를 UserResponseDto.from() 메서드를 통해 DTO로 변환한 후 반환한다.

- 최종적으로 "프로필 조회에 성공했습니다." 메시지와 함께 UserResponseDto를 포함한 ApiResponse.ok()가 반환된다.


### 4.2.2 내 프로필 수정 (Update My Profile)

<img width="1276" height="1178" alt="user-update-me-sequence" src="https://github.com/user-attachments/assets/5999cc60-499a-4008-99d2-7bb2efb72e3e" />

이 Sequence Diagram은 사용자가 자신의 프로필 정보를 수정하는 과정을 나타낸다.

- 사용자가 PATCH /api/v1/users/me 요청을 전송하면, UserController는 UserService의 updateUserInfo() 메서드를 호출한다.

- UserServiceImpl은 UserRepository의 findByIdWithSkills() 메서드를 호출하여 현재 사용자 정보를 조회한다.

- 닉네임이 변경되는 경우, UserRepository의 existsByNickname() 메서드를 호출하여 중복 여부를 확인한다. 중복이 발견되고 기존 닉네임과 다른 경우 BusinessException(DUPLICATED_NICKNAME)을 발생시킨다.

- 모든 검증이 통과하면, User 엔티티의 updateNickname(), updateGrade(), updatePosition(), updateSkills(), updateShortIntro() 메서드를 호출하여 각 필드를 업데이트한다.

- Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한 후, "프로필 수정에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.2.3 비밀번호 변경 (Change Password)

<img width="1463" height="860" alt="user-change-password-sequence" src="https://github.com/user-attachments/assets/4e12cbe0-3ced-426e-b416-d7704840123f" />

이 Sequence Diagram은 사용자가 자신의 비밀번호를 변경하는 과정을 나타낸다.

- 사용자가 POST /api/v1/users/me/password-change 요청을 전송하면, UserController는 UserService의 changePassword() 메서드를 호출한다.

- UserServiceImpl은 UserRepository의 findById() 메서드를 호출하여 사용자 정보를 조회한 후, PasswordEncoder의 matches() 메서드를 호출하여 기존 비밀번호가 일치하는지 확인한다. 일치하지 않으면 BusinessException(OLD_PASSWORD_INCORRECT)을 발생시킨다.

- 비밀번호가 일치하면, PasswordEncoder의 encode() 메서드를 호출하여 새 비밀번호를 암호화하고, user.changePassword() 메서드를 호출하여 엔티티의 비밀번호를 변경한다.

- Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한 후, "비밀번호 변경에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.2.4 회원 탈퇴 (Withdraw)

<img width="1566" height="1079" alt="user-withdraw-sequence" src="https://github.com/user-attachments/assets/905ee5fe-aaeb-4296-bed4-5828d0df57f6" />

이 Sequence Diagram은 사용자가 회원 탈퇴를 진행하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/users/me/withdraw 요청을 전송하면, UserController는 UserService의 withdraw() 메서드를 호출한다.

- UserServiceImpl은 UserRepository의 findById() 메서드를 호출하여 사용자 정보를 조회한 후, 이미 탈퇴한 사용자인지 확인한다. 이미 탈퇴한 경우 BusinessException(ALREADY_WITHDRAWN_USER)을 발생시킨다.

- 탈퇴하지 않은 사용자인 경우, PasswordEncoder의 matches() 메서드를 호출하여 비밀번호가 일치하는지 확인한다. 일치하지 않으면 BusinessException(MISMATCHED_PASSWORD)을 발생시킨다.

- 비밀번호가 일치하면 user.withdraw() 메서드를 호출하여 사용자 상태를 WITHDRAWN으로 변경하고, Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 이후 RedisTemplate을 통해 저장된 Refresh Token을 삭제하고, Controller에서 Refresh Token 쿠키를 만료 처리한다.

- 최종적으로 "회원 탈퇴에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.2.5 Username 중복 확인 (Check Username)

<img width="1186" height="565" alt="user-check-username-sequence" src="https://github.com/user-attachments/assets/a2aa5b3e-1496-41b7-8e50-0cf77529f831" />

이 Sequence Diagram은 회원가입 시 Username 중복 여부를 확인하는 과정을 나타낸다.

- 사용자가 GET /api/v1/users/check-username?username= 요청을 전송하면, UserController는 UserService의 existsByUsername() 메서드를 호출한다.

- UserServiceImpl은 UserRepository의 existsByUsername() 메서드를 호출하여 해당 Username이 이미 존재하는지 확인한다.

- 중복 여부를 boolean 값으로 반환하며, "Username 중복 확인에 성공했습니다." 메시지와 함께 결과를 포함한 ApiResponse.ok()가 반환된다.


### 4.2.6 Email 중복 확인 (Check Email)

<img width="1028" height="565" alt="user-check-email-sequence" src="https://github.com/user-attachments/assets/89045d3e-0d87-4216-ab04-8d926bf472fa" />

이 Sequence Diagram은 회원가입 시 Email 중복 여부를 확인하는 과정을 나타낸다.

- 사용자가 GET /api/v1/users/check-email?email= 요청을 전송하면, UserController는 UserService의 existsByEmail() 메서드를 호출한다.

- UserServiceImpl은 UserRepository의 existsByEmail() 메서드를 호출하여 해당 Email이 이미 존재하는지 확인한다.

- 중복 여부를 boolean 값으로 반환하며, "Email 중복 확인에 성공했습니다." 메시지와 함께 결과를 포함한 ApiResponse.ok()가 반환된다.


### 4.2.7 Nickname 중복 확인 (Check Nickname)

<img width="1180" height="565" alt="user-check-nickname-sequence" src="https://github.com/user-attachments/assets/2fc8cf1c-5879-4e2a-9c01-a4efa6921c1d" />

이 Sequence Diagram은 프로필 수정 시 Nickname 중복 여부를 확인하는 과정을 나타낸다.

- 사용자가 GET /api/v1/users/check-nickname?nickname= 요청을 전송하면, UserController는 UserService의 existsByNickname() 메서드를 호출한다.

- UserServiceImpl은 UserRepository의 existsByNickname() 메서드를 호출하여 해당 Nickname이 이미 존재하는지 확인한다.

- 중복 여부를 boolean 값으로 반환하며, "Nickname 중복 확인에 성공했습니다." 메시지와 함께 결과를 포함한 ApiResponse.ok()가 반환된다.


### 4.2.8 받은 리뷰 목록 조회 (Get Reviews)

<img width="1634" height="769" alt="user-get-reviews-sequence" src="https://github.com/user-attachments/assets/c4d65e8e-3d75-4806-8fc9-e41eabd3f470" />

이 Sequence Diagram은 특정 사용자가 받은 리뷰 목록을 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/users/{userId}/reviews/received 요청을 전송하면, UserController는 먼저 UserService의 existsById() 메서드를 호출하여 해당 사용자가 존재하는지 확인한다.

- 사용자가 존재하지 않으면 404 Error를 반환하고, 존재하면 ReviewService의 getReviewsByRevieweeId() 메서드를 호출한다.

- ReviewServiceImpl은 ReviewRepository의 findByRevieweeIdAndStatus() 메서드를 호출하여 ACTIVE 상태의 리뷰만 조회하고, ReviewResponseDto.from() 메서드를 통해 DTO로 변환한다.

- 변환된 리뷰 목록을 PageResponse.from() 메서드를 통해 래핑한 후, "받은 리뷰 목록 조회에 성공했습니다." 메시지와 함께 PageResponse<ReviewResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


---

## 4.3 Project

프로젝트 공고 관리 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.3.1 프로젝트 공고 생성 (Create Project)

<img width="1514" height="895" alt="project-create-sequence" src="https://github.com/user-attachments/assets/38176eb0-da8b-4cd1-a501-e2aec0ee0edd" />

이 Sequence Diagram은 사용자가 프로젝트 공고를 생성하고 팀이 자동으로 생성되는 과정을 나타낸다.

- 사용자가 POST /api/v1/projects 요청을 전송하면, ProjectController는 ProjectService의 createProject() 메서드를 호출한다.

- ProjectServiceImpl은 requestDto.validate() 메서드를 호출하여 DTO 유효성을 검증한 후, UserService의 findById() 메서드를 호출하여 사용자 정보를 조회한다.

- 이후 ProjectCreationRequestDto.toEntity() 메서드를 통해 Project 엔티티를 생성하고, ProjectRepository의 save() 메서드를 호출하여 저장한다.

- Project 생성이 완료되면, Team 엔티티와 TeamMember 엔티티(리더 역할)를 생성하고, TeamService의 save() 메서드를 호출하여 저장한다.

- 최종적으로 "프로젝트 공고 생성에 성공했습니다." 메시지와 함께 생성된 projectId를 포함한 ApiResponse.ok()가 201 Created 상태로 반환된다.


### 4.3.2 프로젝트 공고 조회 (Get Project)

<img width="1367" height="991" alt="project-get-sequence" src="https://github.com/user-attachments/assets/c274a528-e84c-4b81-bd20-96a67732824b" />

이 Sequence Diagram은 프로젝트 공고의 상세 정보를 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/projects/{projectId} 요청을 전송하면, ProjectController는 ProjectService의 getProject() 메서드를 호출한다.

- ProjectServiceImpl은 먼저 ProjectRepository의 increaseViewCount() 메서드를 호출하여 조회수를 증가시킨다. 조회수가 증가되지 않으면 해당 프로젝트가 존재하지 않는 것으로 판단하여 BusinessException(PROJECT_NOT_FOUND)을 발생시킨다.

- 조회수 증가가 성공하면, ProjectRepository의 findByIdWithUser() 메서드를 호출하여 Project 엔티티와 작성자 정보를 함께 조회한다.

- 이후 ProjectRepository의 findPositionsByProjectId(), findSkillsByProjectId(), findGradesByProjectId() 메서드를 호출하여 포지션별 참여 정보, 기술 스택, 학년 정보를 별도로 조회한다.

- 모든 정보를 ProjectDetailResponseDto에 설정한 후, "프로젝트 공고 조회에 성공했습니다." 메시지와 함께 ProjectDetailResponseDto를 포함한 ApiResponse.ok()가 반환된다.


### 4.3.3 프로젝트 공고 목록 조회 (Get Project Summaries)

<img width="1872" height="701" alt="project-get-summaries-sequence" src="https://github.com/user-attachments/assets/45b078db-a901-4358-a7d0-6c5a4aafda85" />

이 Sequence Diagram은 프로젝트 공고 목록을 조회하고 검색하는 과정을 나타낸다.

- 사용자가 GET /api/v1/projects 요청을 전송하면, ProjectController는 KomoranUtil의 getNouns() 메서드를 호출하여 검색 키워드에서 명사를 추출한다.

- 추출된 명사들과 다른 검색 조건(목적, 포지션, 기술스택, 상태)을 ProjectSearchCondition 객체로 생성한 후, ProjectService의 getProjectSummaries() 메서드를 호출한다.

- ProjectServiceImpl은 ProjectRepository의 getProjectSummaries() 메서드를 호출하여 QueryDSL 기반의 동적 검색 쿼리를 실행한다. 이 과정에서 키워드, 목적, 포지션, 기술스택, 상태로 필터링하고 페이징 처리를 수행한다.

- 조회된 Project 엔티티들을 ProjectSummaryResponseDto.fromEntity() 메서드를 통해 DTO로 변환한 후, "프로젝트 공고 목록 조회에 성공했습니다." 메시지와 함께 Page<ProjectSummaryResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


### 4.3.4 프로젝트 공고 수정 (Update Project)

<img width="1298" height="1022" alt="project-update-sequence" src="https://github.com/user-attachments/assets/647f5788-9866-4bbe-88e4-7ff114759f73" />

이 Sequence Diagram은 사용자가 프로젝트 공고 정보를 수정하는 과정을 나타낸다.

- 사용자가 PUT /api/v1/projects/{projectId} 요청을 전송하면, ProjectController는 ProjectService의 updateProject() 메서드를 호출한다.

- ProjectServiceImpl은 requestDto.validate() 메서드를 호출하여 DTO 유효성을 검증한 후, ProjectRepository의 findById() 메서드를 호출하여 프로젝트 정보를 조회한다.

- 이후 권한 검증(작성자 여부)과 공고 상태 검증(삭제된 공고는 수정 불가)을 수행한다.

- 검증이 완료되면 project.update() 메서드를 호출하여 엔티티 필드를 업데이트하고, Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 최종적으로 "프로젝트 공고 수정에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.3.5 프로젝트 공고 삭제 (Delete Project)

<img width="1253" height="877" alt="project-delete-sequence" src="https://github.com/user-attachments/assets/f1643ef9-8805-4984-8351-e2dd53a0583e" />

이 Sequence Diagram은 사용자가 프로젝트 공고를 삭제하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/projects/{projectId} 요청을 전송하면, ProjectController는 ProjectService의 deleteProject() 메서드를 호출한다.

- ProjectServiceImpl은 ProjectRepository의 findById() 메서드를 호출하여 프로젝트 정보를 조회한 후, 권한 검증(작성자 여부)을 수행한다.

- 검증이 완료되면 project.cancel() 메서드를 호출하여 공고 상태를 CANCELED로 변경한다(논리 삭제). Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 최종적으로 "프로젝트 공고 삭제에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


---

## 4.4 Assignment

과제 공고 관리 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.4.1 과제 공고 생성 (Create Assignment)

<img width="1661" height="852" alt="assignment-create-sequence" src="https://github.com/user-attachments/assets/68efebf9-7a7b-4830-b4cb-d1403b438d08" />

이 Sequence Diagram은 사용자가 과제 공고를 생성하고 팀이 자동으로 생성되는 과정을 나타낸다.

- 사용자가 POST /api/v1/assignments 요청을 전송하면, AssignmentController는 AssignmentService의 createAssignment() 메서드를 호출한다.

- AssignmentServiceImpl은 UserService의 findById() 메서드를 호출하여 사용자 정보를 조회한 후, AssignmentCreationRequestDto.toEntity() 메서드를 통해 Assignment 엔티티를 생성한다.

- 이후 AssignmentRepository의 save() 메서드를 호출하여 저장한다.

- Assignment 생성이 완료되면, Team 엔티티와 TeamMember 엔티티(리더 역할)를 생성하고, TeamService의 save() 메서드를 호출하여 저장한다.

- 최종적으로 "과제 공고 생성에 성공했습니다." 메시지와 함께 생성된 assignmentId를 포함한 ApiResponse.ok()가 201 Created 상태로 반환된다.


### 4.4.2 과제 공고 조회 (Get Assignment)

<img width="1502" height="808" alt="assignment-get-sequence" src="https://github.com/user-attachments/assets/1dfdb488-ddc6-4f59-95af-6b89083ee4d0" />

이 Sequence Diagram은 과제 공고의 상세 정보를 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/assignments/{assignmentId} 요청을 전송하면, AssignmentController는 AssignmentService의 getAssignment() 메서드를 호출한다.

- AssignmentServiceImpl은 먼저 AssignmentRepository의 increaseViewCount() 메서드를 호출하여 조회수를 증가시킨다. @Modifying 쿼리를 사용하여 업데이트된 행 수를 반환하며, 0이면 해당 과제가 존재하지 않는 것으로 판단하여 BusinessException(ASSIGNMENT_NOT_FOUND)을 발생시킨다.

- 조회수 증가가 성공하면, AssignmentRepository의 findById() 메서드를 호출하여 Assignment 엔티티를 조회한다.

- 조회된 Assignment 엔티티를 AssignmentDetailResponseDto.fromEntity() 메서드를 통해 DTO로 변환한 후, "과제 공고 조회에 성공했습니다." 메시지와 함께 AssignmentDetailResponseDto를 포함한 ApiResponse.ok()가 반환된다.


### 4.4.3 과제 공고 목록 조회 (Get Assignment Summaries)

<img width="1791" height="701" alt="assignment-get-summaries-sequence" src="https://github.com/user-attachments/assets/306e6f04-d70a-44f6-905a-d827980e7a99" />

이 Sequence Diagram은 과제 공고 목록을 조회하고 검색하는 과정을 나타낸다.

- 사용자가 GET /api/v1/assignments 요청을 전송하면, AssignmentController는 KomoranUtil의 getNouns() 메서드를 호출하여 검색 키워드에서 명사를 추출한다.

- 추출된 명사들과 다른 검색 조건(학년, 기술스택, 상태)을 AssignmentSearchCondition 객체로 생성한 후, AssignmentService의 getAssignmentSummaries() 메서드를 호출한다.

- AssignmentServiceImpl은 AssignmentRepository의 getAssignmentSummaries() 메서드를 호출하여 QueryDSL 기반의 동적 검색 쿼리를 실행한다. 이 과정에서 키워드, 학년, 기술스택, 상태로 필터링하고 페이징 처리를 수행한다.

- 조회된 Assignment 엔티티들을 AssignmentSummaryResponseDto.fromEntity() 메서드를 통해 DTO로 변환한 후, "과제 공고 목록 조회에 성공했습니다." 메시지와 함께 Page<AssignmentSummaryResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


### 4.4.4 과제 공고 수정 (Update Assignment)

<img width="1471" height="980" alt="assignment-update-sequence" src="https://github.com/user-attachments/assets/cd5df132-8614-4834-9c96-6799e451f54f" />

이 Sequence Diagram은 사용자가 과제 공고 정보를 수정하는 과정을 나타낸다.

- 사용자가 PUT /api/v1/assignments/{assignmentId} 요청을 전송하면, AssignmentController는 AssignmentService의 updateAssignment() 메서드를 호출한다.

- AssignmentServiceImpl은 AssignmentRepository의 findById() 메서드를 호출하여 과제 정보를 조회한 후, 권한 검증(작성자 여부)과 공고 상태 검증을 수행한다.

- 검증이 완료되면 assignment.update() 메서드를 호출하여 엔티티 필드를 업데이트하고, 학년 정보를 갱신한다. Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 최종적으로 "과제 공고 수정에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.4.5 과제 공고 삭제 (Delete Assignment)

<img width="1546" height="877" alt="assignment-delete-sequence" src="https://github.com/user-attachments/assets/76611166-6f01-461a-bf91-e6f66245b418" />

이 Sequence Diagram은 사용자가 과제 공고를 삭제하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/assignments/{assignmentId} 요청을 전송하면, AssignmentController는 AssignmentService의 deleteAssignment() 메서드를 호출한다.

- AssignmentServiceImpl은 AssignmentRepository의 findById() 메서드를 호출하여 과제 정보를 조회한 후, 권한 검증(작성자 여부)을 수행한다.

- 검증이 완료되면 assignment.cancel() 메서드를 호출하여 공고 상태를 CANCELED로 변경한다(논리 삭제). Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 최종적으로 "과제 공고 삭제에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


---

## 4.5 Study

스터디 공고 관리 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.5.1 스터디 공고 생성 (Create Study)

<img width="989" height="487" alt="study-create-sequence" src="https://github.com/user-attachments/assets/e609f858-e164-4798-b7ed-086c65f9caf8" />

이 Sequence Diagram은 사용자가 스터디 공고를 생성하는 과정을 나타낸다.

- 사용자가 POST /api/v1/studies 요청을 전송하면, StudyController는 StudyService의 createStudy() 메서드를 호출한다.

- StudyServiceImpl은 UserService의 findById() 메서드를 호출하여 사용자 정보를 조회한 후, StudyCreationRequestDto.toEntity() 메서드를 통해 Study 엔티티를 생성한다.

- 이후 StudyRepository의 save() 메서드를 호출하여 저장한다. JOINED 상속 전략을 사용하여 BaseRecruitment와 Study가 별도 테이블에 저장된다.

- 최종적으로 "스터디 공고 생성에 성공했습니다." 메시지와 함께 생성된 studyId를 포함한 ApiResponse.ok()가 201 Created 상태로 반환된다.


### 4.5.2 스터디 공고 조회 (Get Study)

<img width="989" height="569" alt="study-get-sequence" src="https://github.com/user-attachments/assets/c99806e9-f9ca-46d1-9485-fc1299c619f0" />

이 Sequence Diagram은 스터디 공고의 상세 정보를 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/studies/{studyId} 요청을 전송하면, StudyController는 StudyService의 getStudy() 메서드를 호출한다.

- StudyServiceImpl은 먼저 StudyRepository의 increaseViewCount() 메서드를 호출하여 조회수를 증가시킨다. 조회수가 증가되지 않으면 해당 스터디가 존재하지 않는 것으로 판단하여 BusinessException(STUDY_NOT_FOUND)을 발생시킨다.

- 조회수 증가가 성공하면, StudyRepository의 findById() 메서드를 호출하여 Study 엔티티를 조회한다.

- 조회된 Study 엔티티를 StudyResponseDto.fromEntity() 메서드를 통해 DTO로 변환한 후, "스터디 공고 조회에 성공했습니다." 메시지와 함께 StudyResponseDto를 포함한 ApiResponse.ok()가 반환된다.


### 4.5.3 스터디 공고 목록 조회 (Get Study Summaries)

<img width="1288" height="561" alt="study-get-summaries-sequence" src="https://github.com/user-attachments/assets/686e5986-2b0b-43a2-815e-24ef26ec22d0" />

이 Sequence Diagram은 스터디 공고 목록을 조회하고 검색하는 과정을 나타낸다.

- 사용자가 GET /api/v1/studies 요청을 전송하면, StudyController는 KomoranUtil의 getNouns() 메서드를 호출하여 검색 키워드에서 명사를 추출한다.

- 추출된 명사들과 다른 검색 조건(학년, 상태)을 StudySearchCondition 객체로 생성한 후, StudyService의 getStudySummaries() 메서드를 호출한다.

- StudyServiceImpl은 StudyRepository의 getStudySummaries() 메서드를 호출하여 QueryDSL 기반의 동적 검색 쿼리를 실행한다. 이 과정에서 키워드, 학년, 상태로 필터링하고 페이징 처리를 수행한다.

- 조회된 Study 엔티티들을 StudySummaryResponseDto.fromEntity() 메서드를 통해 DTO로 변환한 후, "스터디 공고 목록 조회에 성공했습니다." 메시지와 함께 Page<StudySummaryResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


### 4.5.4 스터디 공고 수정 (Update Study)

<img width="995" height="784" alt="study-update-sequence" src="https://github.com/user-attachments/assets/dca7b925-1997-40b2-933c-917a2a920aa0" />

이 Sequence Diagram은 사용자가 스터디 공고 정보를 수정하는 과정을 나타낸다.

- 사용자가 PUT /api/v1/studies/{studyId} 요청을 전송하면, StudyController는 StudyService의 updateStudy() 메서드를 호출한다.

- StudyServiceImpl은 StudyRepository의 findById() 메서드를 호출하여 스터디 정보를 조회한 후, 권한 검증(작성자 여부)과 공고 상태 검증(삭제된 공고는 수정 불가)을 수행한다.

- 검증이 완료되면 study.update() 메서드를 호출하여 엔티티 필드를 업데이트하고, Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 최종적으로 "스터디 공고 수정에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.5.5 스터디 공고 삭제 (Delete Study)

<img width="973" height="701" alt="study-delete-sequence" src="https://github.com/user-attachments/assets/4a5fdcbb-ee2b-4ea6-a650-1c465004c5bb" />

이 Sequence Diagram은 사용자가 스터디 공고를 삭제하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/studies/{studyId} 요청을 전송하면, StudyController는 StudyService의 deleteStudy() 메서드를 호출한다.

- StudyServiceImpl은 StudyRepository의 findById() 메서드를 호출하여 스터디 정보를 조회한 후, 권한 검증(작성자 여부)을 수행한다.

- 검증이 완료되면 study.cancel() 메서드를 호출하여 공고 상태를 CANCELED로 변경한다(논리 삭제). Repository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 최종적으로 "스터디 공고 삭제에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.

---

## 4.6 Application

공고 지원 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.6.1 지원 제출 (Submit Application)

<img width="2168" height="2065" alt="application-submit-sequence" src="https://github.com/user-attachments/assets/ae085c67-8c38-48b0-9804-1beee8df122c" />

이 Sequence Diagram은 사용자가 공고에 지원서를 제출하는 과정을 나타낸다.

- 사용자가 POST /api/v1/applications/recruitments/{recruitmentId} 요청을 전송하면, ApplicationController는 ApplicationService의 submitApplication() 메서드를 호출한다.

- ApplicationServiceImpl은 먼저 UserService의 findById()와 RecruitmentService의 findById() 메서드를 호출하여 지원자와 공고 정보를 조회한다.

- 이후 다양한 검증 로직을 수행한다: 대학 간 지원 제한 검증, 카테고리 일치 여부 확인, 모집 상태 확인(RECRUITING 여부), 본인 공고 지원 여부 확인, 중복 지원 여부 확인(ApplicationRepository의 existsByApplicantIdAndRecruitmentId() 메서드 호출).

- 모든 검증이 통과하면, 공고 카테고리에 따라 분기 처리한다. PROJECT의 경우 포지션별 참여 정보를 조회하고 포지션 모집 여부 및 정원 확인을 수행하며, ASSIGNMENT와 STUDY의 경우 전체 참여 인원 정원 확인을 수행한다.

- 검증이 완료되면 Application 엔티티를 생성하고 ApplicationRepository의 save() 메서드를 호출하여 저장한다.

- 지원이 성공적으로 생성되면, NotificationService의 createNotification() 메서드를 호출하여 공고 작성자에게 지원 제출 알림을 발송한다.

- 최종적으로 "지원 제출에 성공했습니다." 메시지와 함께 생성된 applicationId를 포함한 ApiResponse.ok()가 201 Created 상태로 반환된다.


### 4.6.2 내 지원 목록 조회 (Get My Applications)

<img width="2016" height="799" alt="application-get-my-sequence" src="https://github.com/user-attachments/assets/ef12a828-f860-4f9e-b474-3be91f230fbd" />

이 Sequence Diagram은 현재 로그인한 사용자가 자신의 지원 내역을 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/applications/me 요청을 전송하면, ApplicationController는 PageRequest를 생성하여 ApplicationService의 getAllByUserIdAndRecruitmentCategory() 메서드를 호출한다.

- ApplicationServiceImpl은 ApplicationRepository의 findAllByApplicantIdAndRecruitmentCategoryAndIsDeletedFalse() 메서드를 호출하여 삭제되지 않은 지원 목록을 카테고리별로 필터링하여 조회한다.

- 조회된 지원 목록이 비어있으면 빈 페이지를 반환하고, 지원 목록이 존재하면 카테고리에 따라 다른 DTO로 변환한다. PROJECT의 경우 ApplicationProjectResponseDto.fromEntity()를, ASSIGNMENT와 STUDY의 경우 ApplicationSimpleResponseDto.fromEntity()를 사용한다.

- 최종적으로 "내 지원 목록 조회에 성공했습니다." 메시지와 함께 Page<ApplicationCommonResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


### 4.6.3 지원 수락 (Accept Application)

<img width="1928" height="1825" alt="application-accept-sequence" src="https://github.com/user-attachments/assets/c264695e-d326-4e4e-9545-e2e6cd48e805" />

이 Sequence Diagram은 공고 작성자가 지원을 수락하고 팀에 멤버를 추가하는 과정을 나타낸다.

- 사용자가 POST /api/v1/applications/{applicationId}/accept 요청을 전송하면, ApplicationController는 ApplicationService의 acceptApplication() 메서드를 호출한다.

- ApplicationServiceImpl은 ApplicationRepository의 findByIdAndNotDeletedWithRecruitmentAndAuthor() 메서드를 호출하여 지원 정보와 공고 작성자 정보를 함께 조회한다.

- 이후 권한 검증(공고 작성자 여부), 지원 상태 검증(SUBMITTED 여부)을 수행한다.

- 검증이 완료되면, 공고 카테고리에 따라 분기 처리한다. PROJECT의 경우 포지션별 참여 정보를 조회하고 포지션 정원 확인을 수행하며, ASSIGNMENT와 STUDY의 경우 전체 참여 인원 정원 확인을 수행한다.

- 검증이 완료되면 application.accept() 메서드를 호출하여 지원 상태를 ACCEPTED로 변경하고, participantInfo.incrementCurrParticipants() 메서드를 호출하여 참여 인원을 증가시킨다.

- 이후 TeamService의 findByRecruitmentId() 메서드를 호출하여 팀을 조회하고, TeamMember 엔티티를 생성하여 팀에 추가한다.

- 마지막으로 NotificationService의 createNotification() 메서드를 호출하여 지원자에게 수락 알림을 발송한다.

- 최종적으로 "지원 수락에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.6.4 지원 거절 (Reject Application)

<img width="1778" height="964" alt="application-reject-sequence" src="https://github.com/user-attachments/assets/bedf268d-07a0-47c3-8a7d-a587377083f3" />

이 Sequence Diagram은 공고 작성자가 지원을 거절하는 과정을 나타낸다.

- 사용자가 POST /api/v1/applications/{applicationId}/reject 요청을 전송하면, ApplicationController는 ApplicationService의 rejectApplication() 메서드를 호출한다.

- ApplicationServiceImpl은 ApplicationRepository의 findByIdAndNotDeletedWithRecruitmentAndAuthor() 메서드를 호출하여 지원 정보를 조회한 후, 권한 검증(공고 작성자 여부)과 지원 상태 검증(SUBMITTED 여부)을 수행한다.

- 검증이 완료되면 application.reject() 메서드를 호출하여 지원 상태를 REJECTED로 변경한다.

- 이후 NotificationService의 createNotification() 메서드를 호출하여 지원자에게 거절 알림을 발송한다.

- 최종적으로 "지원 거절에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.6.5 지원 취소 (Cancel Application)

<img width="1510" height="1022" alt="application-cancel-sequence" src="https://github.com/user-attachments/assets/021e4641-1988-4da4-94b3-e270373d10e7" />

이 Sequence Diagram은 지원자가 자신이 제출한 지원을 취소하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/applications/{applicationId} 요청을 전송하면, ApplicationController는 ApplicationService의 cancelApplication() 메서드를 호출한다.

- ApplicationServiceImpl은 ApplicationRepository의 findByIdAndNotDeleted() 메서드를 호출하여 지원 정보를 조회한 후, 권한 검증(지원자 본인 여부)과 지원 상태 검증을 수행한다.

- 검증이 완료되면 application.cancel() 메서드를 호출하여 논리 삭제(isDeleted=true) 처리한다.

- 최종적으로 "지원 취소에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.

---

## 4.7 Notification

알림 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.7.1 내 알림 목록 조회 (Get My Notifications)

<img width="1738" height="709" alt="notification-get-my-sequence" src="https://github.com/user-attachments/assets/bfc062ee-ad95-4cc6-bb0b-825d49676886" />

이 Sequence Diagram은 현재 로그인한 사용자가 자신의 알림 목록을 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/notifications 요청을 전송하면, NotificationController는 PageRequest를 생성하여 NotificationService의 getAllByUserIdAndCategory() 메서드를 호출한다.

- NotificationServiceImpl은 PageableValidator의 validate()와 validateSort() 메서드를 호출하여 페이징 요청의 유효성과 정렬 필드를 검증한다.

- 검증이 완료되면 NotificationRepository의 getAllByUserIdAndCategory() 메서드를 호출하여 사용자 ID와 카테고리(선택적)로 필터링된 알림 목록을 최신순으로 조회한다.

- 조회된 Notification 엔티티들을 NotificationResponseDto.from() 메서드를 통해 DTO로 변환한 후, "알림 목록 조회에 성공했습니다." 메시지와 함께 Page<NotificationResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


### 4.7.2 알림 삭제 (Delete Notification)

<img width="1393" height="819" alt="notification-delete-by-id-sequence" src="https://github.com/user-attachments/assets/b2358b32-560e-411b-9fab-d46fadb3e83e" />

이 Sequence Diagram은 사용자가 개별 알림을 삭제하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/notifications/{notificationId} 요청을 전송하면, NotificationController는 NotificationService의 deleteById() 메서드를 호출한다.

- NotificationServiceImpl은 NotificationRepository의 findById() 메서드를 호출하여 알림 정보를 조회한 후, 권한 검증(알림 수신자 여부)을 수행한다.

- 검증이 완료되면 NotificationRepository의 delete() 메서드를 호출하여 알림을 삭제한다.

- 최종적으로 "알림 삭제에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


### 4.7.3 알림 전체 삭제 (Delete All Notifications)

<img width="1233" height="597" alt="notification-delete-all-sequence" src="https://github.com/user-attachments/assets/1030c8fe-f18a-42ca-a03a-00e7e810a7b9" />

이 Sequence Diagram은 사용자가 자신의 모든 알림 또는 카테고리별 알림을 일괄 삭제하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/notifications 요청을 전송하면, NotificationController는 NotificationService의 deleteAllByUserId() 또는 deleteAllByUserIdAndCategory() 메서드를 호출한다.

- NotificationServiceImpl은 NotificationRepository의 deleteAllByUserId() 또는 deleteAllByUserIdAndCategory() 메서드를 호출하여 해당 조건에 맞는 모든 알림을 일괄 삭제한다.

- 최종적으로 "알림 전체 삭제에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.

---

## 4.8 Review

리뷰 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.8.1 리뷰 작성 (Create Review)

<img width="1371" height="786" alt="review-create-sequence" src="https://github.com/user-attachments/assets/58862fcb-98e2-45f4-89ab-4d9120b22a75" />

이 Sequence Diagram은 사용자가 다른 사용자에게 리뷰를 작성하는 과정을 나타낸다.

- 사용자가 POST /api/v1/reviews 요청을 전송하면, ReviewController는 ReviewService의 createReview() 메서드를 호출한다.

- ReviewServiceImpl은 먼저 reviewerId와 dto.revieweeId가 동일한지 확인하여 자기 자신에게 리뷰를 작성하는 것을 방지한다. 동일한 경우 BusinessException(SELF_REVIEW_NOT_ALLOWED)을 발생시킨다.

- 검증이 통과하면 UserService의 findById() 메서드를 두 번 호출하여 리뷰 작성자와 수신자 정보를 각각 조회한다.

- 이후 dto.toEntity() 메서드를 통해 Review 엔티티를 생성하고, ReviewRepository의 save() 메서드를 호출하여 저장한다.

- 최종적으로 "리뷰 작성에 성공했습니다." 메시지와 함께 생성된 reviewId를 포함한 ApiResponse.ok()가 201 Created 상태로 반환된다.


### 4.8.2 내가 작성한 리뷰 목록 조회 (Get Written Reviews)

<img width="1470" height="577" alt="review-get-written-sequence" src="https://github.com/user-attachments/assets/2b0a56e3-c9e8-45b0-ae65-3139866b601e" />

이 Sequence Diagram은 현재 로그인한 사용자가 자신이 작성한 리뷰 목록을 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/reviews/me/written 요청을 전송하면, ReviewController는 ReviewService의 getReviewsByReviewerId() 메서드를 호출한다.

- ReviewServiceImpl은 ReviewRepository의 findByReviewerIdAndStatus() 메서드를 호출하여 ACTIVE 상태의 리뷰만 리뷰어 기준으로 조회하고, ReviewResponseDto.from() 메서드를 통해 DTO로 변환한다.

- 변환된 리뷰 목록을 PageResponse.from() 메서드를 통해 래핑한 후, "내가 작성한 리뷰 목록 조회에 성공했습니다." 메시지와 함께 PageResponse<ReviewResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


### 4.8.3 내가 받은 리뷰 목록 조회 (Get Received Reviews)

<img width="1488" height="577" alt="review-get-received-sequence" src="https://github.com/user-attachments/assets/354600c3-5b46-4a6e-810d-26905a981ea6" />

이 Sequence Diagram은 현재 로그인한 사용자가 자신이 받은 리뷰 목록을 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/reviews/me/received 요청을 전송하면, ReviewController는 ReviewService의 getReviewsByRevieweeId() 메서드를 호출한다.

- ReviewServiceImpl은 ReviewRepository의 findByRevieweeIdAndStatus() 메서드를 호출하여 ACTIVE 상태의 리뷰만 리뷰 수신자 기준으로 조회하고, ReviewResponseDto.from() 메서드를 통해 DTO로 변환한다.

- 변환된 리뷰 목록을 PageResponse.from() 메서드를 통해 래핑한 후, "내가 받은 리뷰 목록 조회에 성공했습니다." 메시지와 함께 PageResponse<ReviewResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


### 4.8.4 리뷰 수정 (Update Review)

<img width="1354" height="1007" alt="review-update-sequence" src="https://github.com/user-attachments/assets/be354e97-1b6d-419e-be64-e28a5d78870b" />

이 Sequence Diagram은 사용자가 자신이 작성한 리뷰를 수정하는 과정을 나타낸다.

- 사용자가 PATCH /api/v1/reviews/me/written/{reviewId} 요청을 전송하면, ReviewController는 ReviewService의 updateReview() 메서드를 호출한다.

- ReviewServiceImpl은 ReviewRepository의 findById() 메서드를 호출하여 리뷰 정보를 조회한 후, 권한 검증(작성자 여부)과 리뷰 상태 검증(ACTIVE 여부)을 수행한다.

- 검증이 완료되면 review.update() 메서드를 호출하여 리뷰 내용을 수정하고, ReviewRepository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 최종적으로 "리뷰 수정에 성공했습니다." 메시지와 함께 reviewId를 포함한 ApiResponse.ok()가 반환된다.


### 4.8.5 리뷰 삭제 (Delete Review)

<img width="1415" height="1022" alt="review-delete-sequence" src="https://github.com/user-attachments/assets/361f8915-cf28-4a53-b1e5-473f867ce5ab" />

이 Sequence Diagram은 사용자가 자신이 작성한 리뷰를 삭제하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/reviews/me/written/{reviewId} 요청을 전송하면, ReviewController는 ReviewService의 deleteReview() 메서드를 호출한다.

- ReviewServiceImpl은 ReviewRepository의 findById() 메서드를 호출하여 리뷰 정보를 조회한 후, 권한 검증(작성자 여부)과 리뷰 상태 검증(ACTIVE 여부)을 수행한다.

- 검증이 완료되면 review.delete() 메서드를 호출하여 리뷰 상태를 DELETED로 변경하고(소프트 삭제), ReviewRepository의 flush() 메서드를 통해 변경사항을 즉시 반영한다.

- 최종적으로 "리뷰 삭제에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.


---

## 4.9 Team

팀 및 팀 멤버 관리 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.9.1 내 팀 목록 조회 (Get My Teams)

<img width="2138" height="534" alt="team-get-my-sequence" src="https://github.com/user-attachments/assets/a2924eaf-ce03-4d0f-9834-bfa7d3093dca" />

이 Sequence Diagram은 현재 로그인한 사용자가 자신이 작성한 공고의 팀 목록을 조회하는 과정을 나타낸다.

- 사용자가 GET /api/v1/teams/me 요청을 전송하면, TeamController는 TeamService의 getByUserIdAndCategoryAndStatus() 메서드를 호출한다.

- TeamServiceImpl은 TeamRepository의 findDistinctByRecruitmentCategoryAndRecruitmentStatusAndRecruitmentUserId() 메서드를 호출하여 카테고리와 상태로 필터링된 팀 목록을 조회한다. EntityGraph를 사용하여 연관 엔티티를 즉시 로딩한다.

- 조회된 Team 엔티티들을 TeamResponseDto.fromEntity() 메서드를 통해 DTO로 변환한 후, "내 팀 목록 조회에 성공했습니다." 메시지와 함께 Page<TeamResponseDto>를 포함한 ApiResponse.ok()가 반환된다.


### 4.9.2 팀 멤버 제거 (Delete Team Member)

<img width="2054" height="1373" alt="team-member-delete-sequence" src="https://github.com/user-attachments/assets/ca160ff8-fe39-40f0-a9c3-860ad2152d60" />

이 Sequence Diagram은 팀 리더가 팀 멤버를 제거하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/teams/{teamId}/members/{memberId} 요청을 전송하면, TeamMemberController는 TeamMemberService의 removeMember() 메서드를 호출한다.

- TeamMemberServiceImpl은 TeamService의 findByIdWithMembers() 메서드를 호출하여 팀 정보와 멤버 목록을 함께 조회한다.

- 이후 team.findMember() 메서드를 호출하여 제거 대상 멤버를 찾고, RecruitmentService의 findByIdNotCanceled() 메서드를 호출하여 공고 정보를 조회한다.

- 권한 검증을 수행한다: 제거자가 리더인지 확인(ONLY_LEADER_CAN_REMOVE_MEMBER), 본인 제거 시도 여부 확인(CANNOT_REMOVE_SELF).

- 검증이 완료되면 team.removeMember() 메서드를 호출하여 팀에서 멤버를 제거하고, recruitment.decreaseCurrParticipant() 메서드를 호출하여 참여 인원을 감소시킨다.

- TeamMemberRepository의 flush() 메서드를 통해 변경사항을 즉시 반영한다. 이 과정에서 Optimistic Locking을 통해 동시성 제어를 수행하며, 실패 시 @Retryable 어노테이션에 의해 자동으로 재시도된다.

- 최종적으로 "팀 멤버 제거에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.

---

## 4.10 Bookmark

북마크 관련 기능의 처리 흐름을 보여주는 시퀀스 다이어그램들이다.

### 4.10.1 북마크 생성 (Create Bookmark)

<img width="1776" height="860" alt="bookmark-create-sequence" src="https://github.com/user-attachments/assets/98a1d71a-3333-4b78-b522-049ebdc399d4" />

이 Sequence Diagram은 사용자가 공고를 북마크 목록에 추가하는 과정을 나타낸다.

- 사용자가 POST /api/v1/bookmarks/recruitments/{recruitmentId} 요청을 전송하면, BookmarkController는 BookmarkService의 createBookmark() 메서드를 호출한다.

- BookmarkServiceImpl은 BookmarkRepository의 existsByUserIdAndRecruitmentId() 메서드를 호출하여 이미 북마크되어 있는지 확인한다. 중복인 경우 BusinessException(ALREADY_BOOKMARKED)을 발생시킨다.

- 중복이 아닌 경우, UserService의 findById()와 RecruitmentService의 findById() 메서드를 호출하여 사용자와 공고 정보를 조회한다.

- 이후 Bookmark 엔티티를 생성하고 BookmarkRepository의 save() 메서드를 호출하여 저장한다.

- 최종적으로 "북마크 생성에 성공했습니다." 메시지와 함께 생성된 bookmarkId를 포함한 ApiResponse.ok()가 201 Created 상태로 반환된다.


### 4.10.2 북마크 삭제 (Delete Bookmark)

  <img width="1368" height="819" alt="bookmark-delete-sequence" src="https://github.com/user-attachments/assets/d1326802-c7c7-498d-a53c-5e75fe61b2ee" />

이 Sequence Diagram은 사용자가 북마크 목록에서 공고를 제거하는 과정을 나타낸다.

- 사용자가 DELETE /api/v1/bookmarks/{bookmarkId} 요청을 전송하면, BookmarkController는 BookmarkService의 deleteBookmark() 메서드를 호출한다.

- BookmarkServiceImpl은 BookmarkRepository의 findById() 메서드를 호출하여 북마크 정보를 조회한 후, 권한 검증(북마크 소유자 여부)을 수행한다.

- 검증이 완료되면 BookmarkRepository의 delete() 메서드를 호출하여 북마크를 삭제한다.

- 최종적으로 "북마크 삭제에 성공했습니다." 메시지를 포함한 ApiResponse.ok()가 반환된다.
