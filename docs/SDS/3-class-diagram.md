# 3. Class Diagram

본 장에서는 Waggle 시스템의 클래스 다이어그램을 도메인별 및 기능별로 분류하여 정리한다. 각 다이어그램은 시스템의 구조를 명확히 이해할 수 있도록 구성되었으며, 엔티티 간의 관계, 계층 구조, 데이터 흐름 등을 시각적으로 표현한다.


## 3.1 시스템 전반 클래스 구조 (Overview)

<img width="1502" height="878" alt="01-overview" src="https://github.com/user-attachments/assets/daf426eb-a854-4294-8c91-678b94cb564f" />

전체 시스템 전반의 엔티티 간 상속 및 연관 관계를 종합적으로 나타낸다. 
시스템의 핵심 엔티티인 User, BaseRecruitment, 그리고 이를 상속하는 Project, Study, Assignment 등의 관계를 한눈에 파악할 수 있다. 
또한 Application, Team, TeamMember, Bookmark, Review 등 주요 엔티티들의 연관 관계를 확인할 수 있도록 구성되었다.

---

## 3.2 도메인별 클래스 상세 구조

각 도메인의 주요 엔티티, 값 객체, 열거형의 관계를 보여주는 다이어그램들이다. 
도메인별 클래스 간 연관성을 파악하고, 데이터 및 로직의 구조를 이해할 수 있도록 구성되었다.


### 3.2.1 User Domain

<img width="2182" height="1698" alt="02-user-domain" src="https://github.com/user-attachments/assets/b226501b-99d5-4277-add9-1150472f2505" />

사용자 도메인의 엔티티와 관련 열거형을 보여주는 다이어그램이다. User 엔티티의 구조와 University, UserRoleType, UserStatus, PositionType, Skill 등의 열거형과의 관계를 표현한다.


### 3.2.2 Project Domain

<img width="3639" height="1881" alt="02-project-domain" src="https://github.com/user-attachments/assets/a5fb9b25-785c-4130-bbd1-400a700d6440" />

프로젝트 공고 도메인의 구조를 보여주는 다이어그램이다. BaseRecruitment를 상속하는 Project 엔티티와 PositionParticipantInfo, ParticipantInfo, Period 등의 값 객체, 그리고 ProjectPurpose, MeetingType 등의 열거형과의 관계를 표현한다.


### 3.2.3 Assignment Domain


<img width="2417" height="1467" alt="02-assignment-domain" src="https://github.com/user-attachments/assets/2b49fce4-7453-456b-972d-5ed6639c2f90" />

과제 공고 도메인의 구조를 보여주는 다이어그램이다. BaseRecruitment를 상속하는 Assignment 엔티티와 ParticipantInfo 값 객체, 그리고 관련 열거형과의 관계를 표현한다.


### 3.2.4 Study Domain

<img width="3243" height="1467" alt="02-study-domain" src="https://github.com/user-attachments/assets/78d5c8a3-ef1d-4a3f-8a5c-ac3942f7dd53" />

스터디 공고 도메인의 구조를 보여주는 다이어그램이다. BaseRecruitment를 상속하는 Study 엔티티와 ParticipantInfo, Period 값 객체, 그리고 관련 열거형과의 관계를 표현한다.


### 3.2.5 Team Domain

<img width="2297" height="1361" alt="02-team-domain" src="https://github.com/user-attachments/assets/b96aad6a-ade4-4ef6-a2f1-7ba36052de79" />

팀 및 팀 멤버 도메인의 구조를 보여주는 다이어그램이다. Team과 TeamMember 엔티티 간의 관계, TeamRole 열거형, 그리고 BaseRecruitment와의 연관 관계를 표현한다.


### 3.2.6 Application Domain

<img width="3693" height="1239" alt="02-application-domain" src="https://github.com/user-attachments/assets/2c315c61-b08f-4aec-b107-6b654a1d8924" />

공고 지원 도메인의 구조를 보여주는 다이어그램이다. Application 엔티티와 BaseRecruitment, User 간의 관계, 그리고 ApplicationStatus, MeetingType 등의 열거형과의 관계를 표현한다.


### 3.2.7 Notification Domain

<img width="2970" height="1703" alt="02-notification-domain" src="https://github.com/user-attachments/assets/65e4d8e5-5516-4b0d-8345-1205d7aaa5cc" />

알림 도메인의 구조를 보여주는 다이어그램이다. Notification 엔티티와 Application, User 간의 관계, 그리고 NotificationType 열거형과의 관계를 표현한다.


### 3.2.8 Bookmark Domain

<img width="1475" height="1124" alt="02-bookmark-domain" src="https://github.com/user-attachments/assets/ab332d85-fe18-473a-9932-a5e515bec574" />

북마크 도메인의 구조를 보여주는 다이어그램이다. Bookmark 엔티티와 User, BaseRecruitment 간의 관계를 표현한다.


### 3.2.9 Review Domain

<img width="1383" height="805" alt="02-review-domain" src="https://github.com/user-attachments/assets/28f0fed0-7411-4c51-9bf2-a2f74143501d" />

리뷰 도메인의 구조를 보여주는 다이어그램이다. Review 엔티티와 User 간의 관계, 그리고 ReviewStatus 열거형과의 관계를 표현한다.


### 3.2.10 Auth/Security Summary

<img width="4096" height="2094" alt="02-auth-security-summary" src="https://github.com/user-attachments/assets/fb65af14-7f14-4071-97ff-f32ea8978548" />

인증 및 보안 관련 클래스들의 구조를 요약하여 보여주는 다이어그램이다. SecurityConfig, JwtFilter, JwtUtil, CustomUserDetailsService 등의 클래스와 그들의 관계를 표현한다.

---

## 3.3 기능별 계층 구조

Controller, Service, Repository, DTO 등 계층 간의 요청 흐름을 기능 단위로 시각화한 다이어그램이다.
각 기능의 처리 절차, 의존 관계, 데이터 이동 과정을 단계적으로 파악할 수 있도록 구성되었다.

### 3.3.1 Project Process Structure

<img width="4096" height="2200" alt="03-project-flow" src="https://github.com/user-attachments/assets/eea3f229-a0b3-4639-b4c5-e311e97f04da" />

프로젝트 공고 관리 기능의 계층 구조와 처리 과정을 나타낸다. ProjectController, ProjectService, ProjectRepository 간의 의존 관계와 데이터 흐름을 표현하며, QueryDSL 기반의 커스텀 쿼리 구조와 KomoranUtil을 활용한 검색 기능의 통합 방식을 보여준다.


### 3.3.2 Assignment Process Structure

<img width="4096" height="2090" alt="03-assignment-flow" src="https://github.com/user-attachments/assets/40bba47a-af99-4eaf-b026-0365614698e3" />

과제 공고 관리 기능의 계층 구조와 처리 과정을 나타낸다. AssignmentController, AssignmentService, AssignmentRepository 간의 의존 관계와 DTO 변환 과정을 표현하며, QueryDSL을 활용한 동적 검색 쿼리 구조를 보여준다.


### 3.3.3 Study Process Structure

<img width="4096" height="2090" alt="03-study-flow" src="https://github.com/user-attachments/assets/a0e5a1ae-8152-4a00-9a99-5d49fd7a5134" />

스터디 공고 관리 기능의 계층 구조와 처리 과정을 나타낸다. StudyController, StudyService, StudyRepository 간의 의존 관계와 데이터 변환 과정을 표현하며, 키워드 기반 검색을 위한 KomoranUtil 통합 구조를 보여준다.


### 3.3.4 Application & Notification Process Structure

<img width="4096" height="2205" alt="03-application-notification-flow" src="https://github.com/user-attachments/assets/f94e6ed7-e241-4181-a66e-10dc793314bb" />

공고 지원 및 알림 기능의 통합 처리 구조를 나타낸다. ApplicationController와 NotificationService 간의 비동기 연동 방식, 지원 상태 변경에 따른 알림 생성 메커니즘, 그리고 ApplicationService와 NotificationService의 의존 관계를 표현한다.


### 3.3.5 Team Process Structure

<img width="4096" height="2352" alt="03-team-flow" src="https://github.com/user-attachments/assets/0d62b703-fcc9-45ed-861d-58b77c5967ef" />

팀 및 팀 멤버 관리 기능의 계층 구조를 나타낸다. TeamController와 TeamMemberController의 역할 분리, TeamService와 TeamMemberService 간의 협력 구조, 그리고 Optimistic Locking을 통한 동시성 제어 메커니즘을 표현한다.


### 3.3.6 User Profile Process Structure

<img width="4096" height="2055" alt="03-user-profile-flow" src="https://github.com/user-attachments/assets/cae50c6e-d20a-4b94-a7ff-8e622a19505b" />

사용자 프로필 관리 기능의 계층 구조를 나타낸다. UserController와 UserService의 의존 관계, 프로필 조회 시 ReviewService와의 연동 구조, 그리고 비밀번호 변경 및 회원 탈퇴 등의 보안 처리 과정을 표현한다.


### 3.3.7 Review Process Structure

<img width="3558" height="1844" alt="03-review-flow" src="https://github.com/user-attachments/assets/557e2815-cb61-4298-b353-9f218e7d786a" />

리뷰 관리 기능의 계층 구조와 처리 과정을 나타낸다. ReviewController, ReviewService, ReviewRepository 간의 의존 관계를 표현하며, 리뷰 작성자와 수신자 간의 관계 구조와 소프트 삭제 메커니즘을 보여준다.


### 3.3.8 Authentication & Authorization Process Structure

<img width="4096" height="2315" alt="03-authentication-authorization-flow" src="https://github.com/user-attachments/assets/cd8ace21-eece-4aa4-a3cb-96d7df69bb6d" />

인증 및 인가 기능의 보안 계층 구조를 나타낸다. AuthController와 AuthService의 관계, JWT 기반 토큰 처리 구조, SecurityConfig의 필터 체인 구성, 그리고 JwtFilter를 통한 요청 검증 과정을 표현한다.


### 3.3.9 Bookmark Process Structure


<img width="4096" height="1821" alt="03-bookmark-flow" src="https://github.com/user-attachments/assets/080958e1-c8fe-40db-a7c7-32fa795cba25" />

북마크 관리 기능의 계층 구조와 연동 방식을 나타낸다. BookmarkController와 BookmarkService의 의존 관계, ProjectService, AssignmentService, StudyService와의 협력 구조를 통한 카테고리별 북마크 목록 조회 메커니즘을 표현한다.


### 3.3.10 Scheduler Recruitment Process Structure

<img width="2577" height="1241" alt="03-scheduler-recruitment-flow" src="https://github.com/user-attachments/assets/f0c9959f-19fc-406a-8257-ecdaedb4a6d5" />

공고 마감 스케줄러의 자동화 처리 구조를 나타낸다. RecruitmentClosingScheduler 컴포넌트의 스케줄링 메커니즘, RecruitmentService와 ApplicationService를 활용한 일괄 처리 구조, 그리고 마감일 기반 상태 변경 프로세스를 표현한다.

