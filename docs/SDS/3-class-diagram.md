# 3. Class Diagram

본 장에서는 Waggle 시스템의 클래스 다이어그램을 도메인별 및 기능별로 분류하여 정리한다. 각 다이어그램은 시스템의 구조를 명확히 이해할 수 있도록 구성되었으며, 엔티티 간의 관계, 계층 구조, 데이터 흐름 등을 시각적으로 표현한다.


## 3.1 시스템 전반 클래스 구조 (Overview)

<img width="1502" height="878" alt="01-overview" src="https://github.com/user-attachments/assets/daf426eb-a854-4294-8c91-678b94cb564f" />

전체 시스템 전반의 엔티티 간 상속 및 연관 관계를 종합적으로 나타낸다. 
시스템의 핵심 엔티티인 User, BaseRecruitment, 그리고 이를 상속하는 Project, Study, Assignment 등의 관계를 한눈에 파악할 수 있다. 
또한 Application, Team, TeamMember, Bookmark, Review 등 주요 엔티티들의 연관 관계를 확인할 수 있도록 구성되었다.

---

## 3.2 공통 클래스 구조 (Common Class Structure)

여러 도메인에서 공통으로 사용되는 클래스들의 구조를 보여준다. BaseRecruitment 엔티티와 같은 공통 엔티티, ParticipantInfo, Period, PositionParticipantInfo와 같은 값 객체, 그리고 PositionType, RecruitmentCategory, RecruitmentStatus, Skill 등의 열거형을 포함한다. 또한 공통 서비스, 레포지토리, DTO, 예외 처리, 검증기, 스케줄러 등 전역적으로 활용되는 클래스들의 구조와 관계를 표현한다.

---

## 3.3 도메인별 클래스 상세 구조

각 도메인의 주요 엔티티, 값 객체, 열거형의 관계를 보여주는 다이어그램들이다. 
도메인별 클래스 간 연관성을 파악하고, 데이터 및 로직의 구조를 이해할 수 있도록 구성되었다.


### 3.3.1 User Domain

<img width="2182" height="1698" alt="02-user-domain" src="https://github.com/user-attachments/assets/b226501b-99d5-4277-add9-1150472f2505" />

사용자 도메인의 엔티티와 관련 열거형을 보여주는 다이어그램이다. User 엔티티의 구조와 University, UserRoleType, UserStatus, PositionType, Skill 등의 열거형과의 관계를 표현한다.

#### User
와글와글 서비스에 가입한 사용자의 기본 정보를 관리하는 엔티티 클래스.
회원의 로그인 정보, 프로필, 기술 스택, 상태, 권한 등을 관리한다.

##### Attributes
| Name       | Type          | Visibility | Description                       |
| ---------- |---------------| ---------- |-----------------------------------|
| id         | Long          | private    | 사용자 식별자 (PK, 자동 증가)               |
| username   | String        | private    | 로그인용 사용자 ID (고유값)                 |
| password   | String        | private    | 비밀번호 (BCrypt 해시, 60자)             |
| email      | String        | private    | 학교 이메일 (인증 및 고유값)                 |
| university | University    | private    | 사용자의 대학교 정보                       |
| nickname   | String        | private    | 서비스 내 표시 이름 (고유값)                 |
| grade      | Integer       | private    | 학년 (1~4)                          |
| position   | PositionType  | private    | 희망 포지션 (예: BACK_END, FRONT_END)   |
| skills     | Set\<Skill>   | private    | 보유 기술 스택 (복수 선택 가능)               |
| shortIntro | String        | private    | 짧은 자기소개 (Markdown 지원)             |
| role       | UserRoleType  | private    | 사용자 권한 (ROLE_USER / ROLE_ADMIN 등) |
| createdAt  | LocalDateTime | private    | 생성 시각 (Auditing 자동 기록)            |
| updatedAt  | LocalDateTime | private    | 수정 시각 (Auditing 자동 기록)            |
| status     | UserStatus    | private    | 사용자 상태 (ACTIVE, WITHDRAWN 등)      |

##### Operations
| Name                                     | Return Type | Visibility | Description                      |
| ---------------------------------------- | ----------- | ---------- | -------------------------------- |
| `changePassword(String encodedPassword)` | void        | public     | 사용자의 비밀번호를 암호화된 새 비밀번호로 변경       |
| `updateNickname(String nickname)`        | void        | public     | 사용자의 닉네임을 변경                     |
| `updateGrade(Integer grade)`             | void        | public     | 사용자의 학년 정보를 수정                   |
| `updatePosition(PositionType position)`  | void        | public     | 희망 포지션을 변경                       |
| `updateSkills(Set<Skill> skills)`        | void        | public     | 보유 기술 스택을 갱신                     |
| `updateShortIntro(String shortIntro)`    | void        | public     | 자기소개 문구를 수정                      |
| `withdraw()`                             | void        | public     | 사용자 상태를 `WITHDRAWN`으로 변경 (탈퇴 처리) |

#### University
와글와글 서비스에서 사용자의 소속 대학교 정보를 표현하는 열거형(Enum) 타입.
각 상수는 학교의 한글명(desc) 과 이메일 도메인(domain) 을 매핑하며, 이메일 주소를 기반으로 소속 대학교를 판별하는 기능을 제공한다.

##### Enum Values
| Enum                      | Description | Domain          |
|---------------------------| ----------- | --------------- |
| `YOUNGNAM_UNIV`           | 영남대학교       | `yu.ac.kr`      |
| `KYUNGBUK_UNIV`           | 경북대학교       | `knu.ac.kr`     |
| `KUMOH_UNIV`              | 금오공과대학교     | `kumoh.ac.kr`   |
| `GYEONGGUK_NATIONAL_UNIV` | 국립경국대학교     | `gknu.ac.kr`    |
| `POSTECH`                 | 포항공과대학교     | `postech.ac.kr` |
| `DAEGU_UNIV`              | 대구대학교       | `deagu.ac.kr`   |
| `KEIMYUNG_UNIV`           | 계명대학교       | `stu.kmu.ac.kr` |


##### Attributes
| Name   | Type   | Visibility    | Description                |
| ------ | ------ | ------------- | -------------------------- |
| desc   | String | private final | 대학교 한글명                    |
| domain | String | private final | 대학교 이메일 도메인 (ex. yu.ac.kr) |

##### Operations
| Name                    | Return Type | Visibility    | Description                                                            |
| ----------------------- | ----------- | ------------- | ---------------------------------------------------------------------- |
| `getName()`               | String      | public        | Enum 상수명 반환 (`name()` 호출)                                              |
| `fromEmail(String email)` | University  | public static | 이메일 주소의 도메인 부분을 분석하여 해당 대학교 Enum 반환. 일치하지 않을 경우 `BusinessException` 발생 |

#### UserRoleType
와글와글 서비스 내 사용자 권한(Role)을 정의하는 열거형(Enum) 타입.
사용자의 접근 수준과 기능 권한을 구분하기 위해 사용된다.

##### Enum Values
| Enum            | Description                                    |
|----------------| ---------------------------------------------- |
| `ROLE_USER`    | 일반 사용자 권한 — 기본 회원 권한으로, 일반적인 서비스 이용 가능         |

##### Attributes
| Name   | Type   | Visibility    | Description                |
| ------ | ------ | ------------- | -------------------------- |

##### Operations
| Name                    | Return Type | Visibility    | Description                                                            |
| ----------------------- | ----------- | ------------- | ---------------------------------------- |

#### UserStatus
사용자 계정의 활성 상태 및 탈퇴 여부를 나타내는 열거형(Enum).
회원 계정의 유효성, 탈퇴 처리(Soft Delete) 등의 상태를 관리한다.

##### Enum Values
| Enum        | Description                                 |
|-------------| ------------------------------------------- |
| `ACTIVE`    | 정상적으로 활동 중인 상태. 로그인, 서비스 이용 가능.             |
| `WITHDRAWN` | 사용자가 자발적으로 탈퇴한 상태 (Soft Delete). 서비스 이용 불가. |


##### Attributes
| Name   | Type   | Visibility    | Description                |
| ------ | ------ | ------------- | -------------------------- |

##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |

### 3.3.2 Project Domain

<img width="3639" height="1881" alt="02-project-domain" src="https://github.com/user-attachments/assets/a5fb9b25-785c-4130-bbd1-400a700d6440" />

프로젝트 공고 도메인의 구조를 보여주는 다이어그램이다. BaseRecruitment를 상속하는 Project 엔티티와 PositionParticipantInfo, ParticipantInfo, Period 등의 값 객체, 그리고 ProjectPurpose, MeetingType 등의 열거형과의 관계를 표현한다.


### 3.3.3 Assignment Domain


<img width="2417" height="1467" alt="02-assignment-domain" src="https://github.com/user-attachments/assets/2b49fce4-7453-456b-972d-5ed6639c2f90" />

과제 공고 도메인의 구조를 보여주는 다이어그램이다. BaseRecruitment를 상속하는 Assignment 엔티티와 ParticipantInfo 값 객체, 그리고 관련 열거형과의 관계를 표현한다.


### 3.3.4 Study Domain

<img width="3243" height="1467" alt="02-study-domain" src="https://github.com/user-attachments/assets/78d5c8a3-ef1d-4a3f-8a5c-ac3942f7dd53" />

스터디 공고 도메인의 구조를 보여주는 다이어그램이다. BaseRecruitment를 상속하는 Study 엔티티와 ParticipantInfo, Period 값 객체, 그리고 관련 열거형과의 관계를 표현한다.


### 3.3.5 Team Domain

<img width="2297" height="1361" alt="02-team-domain" src="https://github.com/user-attachments/assets/b96aad6a-ade4-4ef6-a2f1-7ba36052de79" />

팀 및 팀 멤버 도메인의 구조를 보여주는 다이어그램이다. Team과 TeamMember 엔티티 간의 관계, TeamRole 열거형, 그리고 BaseRecruitment와의 연관 관계를 표현한다.

#### Team
각 공고([BaseRecruitment](#baserecruitment))와 1:1로 연결되어 팀 정보를 관리하는 엔티티.
팀은 여러 명의 팀원([TeamMember](#teammember))을 포함하며, 팀 생성 및 삭제 시 팀원과의 연결관계를 일관되게 유지한다.

##### Attributes
| Name        | Type              | Visibility | Description                                               |
| ----------- |-------------------| ---------- | --------------------------------------------------------- |
| id          | Long              | private    | 팀 식별자 (PK, 자동 증가)                                         |
| recruitment | BaseRecruitment   | private    | 연결된 모집 공고 (1:1 관계, Not Null, Unique)                      |
| members     | List\<TeamMember> | private    | 팀에 속한 팀원 목록 (1:N 관계, cascade = ALL, orphanRemoval = true) |
| createdAt   | LocalDateTime     | private    | 팀 생성 시각 (Auditing 자동 기록)                                  |
| updatedAt   | LocalDateTime     | private    | 마지막 수정 시각 (Auditing 자동 기록)                                |

##### Operations
| Name                              | Return Type          | Visibility | Description                                      |
| --------------------------------- | -------------------- | ---------- | ------------------------------------------------ |
| `addMember(TeamMember member)`    | void                 | public     | 팀원 추가 및 양방향 연관관계 설정 (`member.setTeam(this)`)     |
| `findMember(Long userId)`         | Optional\<TeamMember> | public     | 사용자 ID를 통해 특정 팀원을 조회                             |
| `removeMember(TeamMember member)` | void                 | public     | 팀원 삭제 및 연관관계 해제. 팀원 미존재 시 `BusinessException` 발생 |


#### TeamMember
팀([Team](#team))에 소속된 개별 팀원 정보를 나타내는 엔티티.
팀과 사용자 간의 관계를 연결하며, 팀 내 역할 및 포지션 정보를 함께 관리한다.
[Project](#project) 팀의 경우에만 포지션(position) 정보가 필수이며, [Study](#study)나 [Assignment](#assignment) 팀에서는 null이 허용된다.

##### Attributes
| Name      | Type          | Visibility | Description                                    |
| --------- | ------------- | ---------- |------------------------------------------------|
| id        | Long          | private    | 팀 멤버 식별자 (PK, 자동 증가)                           |
| team      | Team          | private    | 소속 팀 (N:1 관계, 필수)                              |
| user      | User          | private    | 연결된 사용자 (N:1 관계, 필수)                           |
| role      | TeamRole      | private    | 팀 내 역할 (LEADER, MEMBER 등)                      |
| position  | PositionType  | private    | 팀 내 포지션 (Project 팀 한정, 예: BACK_END, FRONT_END) |
| createdAt | LocalDateTime | private    | 등록 일시 (Auditing 자동 관리)                         |
| updatedAt | LocalDateTime | private    | 수정 일시 (Auditing 자동 관리)                         |

##### Operations
| Name                                            | Return Type | Visibility | Description                    |
| ----------------------------------------------- | ----------- | ---------- | ------------------------------ |
| `setTeam(Team team)`                              | void        | public     | 소속 팀 정보를 변경 (양방향 연관관계 설정 시 사용) |


#### TeamRole
팀 내 역할을 정의하는 Enum 클래스.
[TeamMember](#teammember) 엔티티에서 팀원의 권한을 구분하기 위해 사용된다.
리더(LEADER)와 일반 멤버(MEMBER) 두 가지 역할을 가진다.

##### Enum Values
| Enum     | Description              |
|----------| ------------------------ |
| `LEADER` | 팀의 생성자이자 리더 (팀 관리 권한 보유) |
| `MEMBER` | 일반 팀원 (리더의 관리하에 활동)      |


##### Attributes
| Name | Type   | Visibility    | Description                |
| ---- | ------ | ------------- | -------------------------- |
| desc | String | private final | 역할의 한글 설명 (예: “리더”, “멤버”). |

##### Operations
| Name      | Return Type | Visibility | Description                       |
| --------- | ----------- | ---------- | --------------------------------- |
| `getDesc()` | String      | public     | 역할의 설명(한글)을 반환.                   |
| `getName()` | String      | public     | Enum 이름(LEADER, MEMBER)을 문자열로 반환. |

### 3.3.6 Application Domain

<img width="3693" height="1239" alt="02-application-domain" src="https://github.com/user-attachments/assets/2c315c61-b08f-4aec-b107-6b654a1d8924" />

공고 지원 도메인의 구조를 보여주는 다이어그램이다. Application 엔티티와 BaseRecruitment, User 간의 관계, 그리고 ApplicationStatus, MeetingType 등의 열거형과의 관계를 표현한다.


### 3.3.7 Notification Domain

<img width="2970" height="1703" alt="02-notification-domain" src="https://github.com/user-attachments/assets/65e4d8e5-5516-4b0d-8345-1205d7aaa5cc" />

알림 도메인의 구조를 보여주는 다이어그램이다. Notification 엔티티와 Application, User 간의 관계, 그리고 NotificationType 열거형과의 관계를 표현한다.


### 3.3.8 Bookmark Domain

<img width="1475" height="1124" alt="02-bookmark-domain" src="https://github.com/user-attachments/assets/ab332d85-fe18-473a-9932-a5e515bec574" />

북마크 도메인의 구조를 보여주는 다이어그램이다. Bookmark 엔티티와 User, BaseRecruitment 간의 관계를 표현한다.


### 3.3.9 Review Domain

<img width="1383" height="805" alt="02-review-domain" src="https://github.com/user-attachments/assets/28f0fed0-7411-4c51-9bf2-a2f74143501d" />

리뷰 도메인의 구조를 보여주는 다이어그램이다. Review 엔티티와 User 간의 관계, 그리고 ReviewStatus 열거형과의 관계를 표현한다.

#### Review
사용자 간 후기(리뷰) 정보를 저장하는 엔티티.
리뷰 작성자(`reviewer`)와 리뷰 대상자(`reviewee`) 간의 관계를 표현하며, 내용(`content`)과 상태(`status`)를 포함한다.
작성일(`createdAt`), 수정일(`updatedAt`)은 JPA Auditing으로 자동 관리된다.

##### Attributes
| Name      | Type          | Visibility | Description             |
| --------- | ------------- | ---------- | ----------------------- |
| id        | Long          | private    | 리뷰 식별자 (PK, 자동 증가)      |
| reviewer  | User          | private    | 리뷰 작성자 (N:1 관계)         |
| reviewee  | User          | private    | 리뷰 대상자 (N:1 관계)         |
| content   | String        | private    | 리뷰 내용 (최대 100자)         |
| status    | ReviewStatus  | private    | 리뷰 상태 (ACTIVE, DELETED) |
| createdAt | LocalDateTime | private    | 작성일 (Auditing 자동 관리)    |
| updatedAt | LocalDateTime | private    | 수정일 (Auditing 자동 관리)    |


##### Operations
| Name                                                 | Return Type | Visibility | Description                      |
| ---------------------------------------------------- | ----------- | ---------- | -------------------------------- |
| `update(ReviewUpdateRequestDto dto)`                   | void        | public     | 리뷰 내용을 수정 (`dto.content()` 반영)   |
| `delete()`                                             | void        | public     | 리뷰 상태를 `DELETED`로 변경 (소프트 삭제 처리) |


#### ReviewStatus
리뷰의 상태(활성/삭제)를 관리하는 Enum 클래스.
Soft Delete 정책에 따라 DB에서는 실제 삭제되지 않으며, `ReviewStatus` 필드를 통해 사용자 노출 여부를 제어한다.

##### Enum Values
| Enum      | Description                             |
|-----------| --------------------------------------- |
| `ACTIVE`  | 활성 상태 — 사용자에게 정상적으로 노출되는 리뷰             |
| `DELETED` | 삭제 상태 — 사용자가 삭제한 리뷰 (비노출 처리됨, DB에는 유지됨) |


##### Attributes
| Name | Type   | Visibility    | Description                |
| ---- | ------ | ------------- | -------------------------- |
| desc | String | private final | 상태의 한글 설명 (예: “활성”, “삭제”). |

##### Operations
| Name      | Return Type | Visibility | Description                        |
| --------- | ----------- | ---------- | ---------------------------------- |
| `getDesc()` | String      | public     | 상태 설명 반환.                          |
| `getName()` | String      | public     | Enum 이름(ACTIVE, DELETED)을 문자열로 반환. |


### 3.3.10 Auth/Security Summary

<img width="4096" height="2094" alt="02-auth-security-summary" src="https://github.com/user-attachments/assets/fb65af14-7f14-4071-97ff-f32ea8978548" />

인증 및 보안 관련 클래스들의 구조를 요약하여 보여주는 다이어그램이다. SecurityConfig, JwtFilter, JwtUtil, CustomUserDetailsService 등의 클래스와 그들의 관계를 표현한다.

---

## 3.4 기능별 계층 구조

Controller, Service, Repository, DTO 등 계층 간의 요청 흐름을 기능 단위로 시각화한 다이어그램이다.
각 기능의 처리 절차, 의존 관계, 데이터 이동 과정을 단계적으로 파악할 수 있도록 구성되었다.

### 3.4.1 Project Process Structure

<img width="4096" height="2200" alt="03-project-flow" src="https://github.com/user-attachments/assets/eea3f229-a0b3-4639-b4c5-e311e97f04da" />

프로젝트 공고 관리 기능의 계층 구조와 처리 과정을 나타낸다. ProjectController, ProjectService, ProjectRepository 간의 의존 관계와 데이터 흐름을 표현하며, QueryDSL 기반의 커스텀 쿼리 구조와 KomoranUtil을 활용한 검색 기능의 통합 방식을 보여준다.


### 3.4.2 Assignment Process Structure

<img width="4096" height="2090" alt="03-assignment-flow" src="https://github.com/user-attachments/assets/40bba47a-af99-4eaf-b026-0365614698e3" />

과제 공고 관리 기능의 계층 구조와 처리 과정을 나타낸다. AssignmentController, AssignmentService, AssignmentRepository 간의 의존 관계와 DTO 변환 과정을 표현하며, QueryDSL을 활용한 동적 검색 쿼리 구조를 보여준다.


### 3.4.3 Study Process Structure

<img width="4096" height="2090" alt="03-study-flow" src="https://github.com/user-attachments/assets/a0e5a1ae-8152-4a00-9a99-5d49fd7a5134" />

스터디 공고 관리 기능의 계층 구조와 처리 과정을 나타낸다. StudyController, StudyService, StudyRepository 간의 의존 관계와 데이터 변환 과정을 표현하며, 키워드 기반 검색을 위한 KomoranUtil 통합 구조를 보여준다.


### 3.4.4 Application & Notification Process Structure

<img width="4096" height="2205" alt="03-application-notification-flow" src="https://github.com/user-attachments/assets/f94e6ed7-e241-4181-a66e-10dc793314bb" />

공고 지원 및 알림 기능의 통합 처리 구조를 나타낸다. ApplicationController와 NotificationService 간의 비동기 연동 방식, 지원 상태 변경에 따른 알림 생성 메커니즘, 그리고 ApplicationService와 NotificationService의 의존 관계를 표현한다.


### 3.4.5 Team Process Structure

<img width="4096" height="2352" alt="03-team-flow" src="https://github.com/user-attachments/assets/0d62b703-fcc9-45ed-861d-58b77c5967ef" />

팀 및 팀 멤버 관리 기능의 계층 구조를 나타낸다. TeamController와 TeamMemberController의 역할 분리, TeamService와 TeamMemberService 간의 협력 구조, 그리고 Optimistic Locking을 통한 동시성 제어 메커니즘을 표현한다.

#### TeamResponseDto
팀([Team](#team)) 엔티티 및 관련 모집공고 정보를 통합하여 API 응답으로 전달하기 위한 DTO.
[Project](#project), [Study](#study), [Assignment](#assignment) 등의 모집 유형에 관계없이 공통 구조로 팀 정보를 제공한다.
또한 각 팀 멤버의 상세 정보([TeamMemberResponseDto](#teammemberresponsedto))를 포함한다.

##### Attributes
| Name             | Type                        | Visibility                 | Description                            |
| ---------------- | --------------------------- | -------------------------- | -------------------------------------- |
| id               | Long                        | private  | 팀 식별자 ID                               |
| recruitmentId    | Long                        | private  | 연결된 모집공고(`BaseRecruitment`)의 ID        |
| recruitmentTitle | String                      | private  | 모집공고 제목                                |
| category         | RecruitmentCategory         | private  | 모집 유형 (PROJECT/STUDY/ASSIGNMENT 등)     |
| status           | RecruitmentStatus           | private  | 모집 상태 (RECRUITING, CLOSED, CANCELED 등) |
| period           | PeriodResponseDto           | private  | 모집 기간 정보 (Project/Study만 해당)           |
| durationDays     | Long                        | private  | 모집 기간 일수 (`endDate - startDate`)       |
| leaderNickname   | String                      | private  | 팀 리더(공고 작성자)의 닉네임                      |
| memberCount      | int                         | private  | 팀 멤버 수                                 |
| members          | List\<TeamMemberResponseDto> | private  | 팀 멤버 정보 리스트                            |

##### Operations
| Name                  | Return Type     | Visibility    | Description                                                     |
| --------------------- | --------------- | ------------- | --------------------------------------------------------------- |
| `fromEntity(Team team)` | TeamResponseDto | public static | `Team` 엔티티와 연결된 `BaseRecruitment`, `TeamMember` 정보를 DTO 형태로 변환. |



#### TeamMemberResponseDto
팀 멤버([TeamMember](#teammember)) 정보를 API 응답 형태로 표현하는 DTO.
`TeamMember` 엔티티에서 필요한 최소 필드만 추출하며,
팀 내 역할(`TeamRole`), 포지션(`PositionType`), 사용자 기본 정보 등을 포함한다.

##### Attributes
| Name     | Type         | Visibility                 | Description                        |
| -------- | ------------ | -------------------------- |------------------------------------|
| userId   | Long         | private  | 팀 멤버(User)의 고유 식별자 ID              |
| nickname | String       | private  | 팀 멤버의 닉네임                          |
| role     | TeamRole     | private  | 팀 내 역할 (리더 / 일반 멤버 등)              |
| position | PositionType | private  | 사용자 포지션 (예: BACK_END, FRONT_END 등) |

##### Operations
| Name                              | Return Type           | Visibility    | Description                              |
| --------------------------------- | --------------------- | ------------- | ---------------------------------------- |
| `fromEntity(TeamMember teamMember)` | TeamMemberResponseDto | public static | `TeamMember` 엔티티를 DTO로 변환하는 정적 팩토리 메서드.  |


#### TeamController
팀([Team](#team)) 관련 REST API 요청을 처리하는 컨트롤러 계층 클래스.
사용자의 인증 정보를 바탕으로 본인의 팀 목록을 카테고리와 상태별로 페이징 조회할 수 있도록 한다.

##### Attributes
| Name        | Type        | Visibility    | Description                   |
| ----------- | ----------- | ------------- | ----------------------------- |
| teamService | TeamService | private final | 팀 관련 비즈니스 로직을 수행하는 서비스 계층 의존성 |


##### Operations
| Name                                                                                                                            | Return Type                                        | Mapping              | Visibility | Description                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |----------------------|------------| ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `getMyTeamByCategory(RecruitmentCategory category, RecruitmentStatus status, CustomUserDetails userDetails, Pageable pageable)` | ResponseEntity\<ApiResponse\<Page\<TeamResponseDto>>> | `GET api/v1/teams/me` | public     | 현재 로그인한 사용자의 팀 목록을 카테고리 및 상태별로 조회한다.<br>인증 사용자(`CustomUserDetails`)의 ID를 기반으로 `TeamService`를 호출한다.<br>기본 정렬은 `createdAt DESC`, 페이지 크기는 3으로 설정된다. |

#### TeamService
팀([Team](#team)) 도메인과 관련된 핵심 비즈니스 로직의 인터페이스 계층.
서비스 구현체([TeamServiceImpl](#teamserviceimpl))가 실제 로직을 담당하며,
팀 생성, 조회, 페이징 조회 등 주요 기능의 계약(Contract)을 정의한다.

##### Attributes
| Name                 | Type                          | Visibility           | Description                                   |
| -------------------- | ----------------------------- | -------------------- | --------------------------------------------- |

##### Operations
| Name                                                                                                                      | Return Type  | Visibility | Description                                      |
| ------------------------------------------------------------------------------------------------------------------------- | ------------ | ---------- | ------------------------------------------------ |
| `save(Team team)`                                                                                                         | void         | public     | 새 팀을 저장한다.                                       |
| `findById(Long id)`                                                                                                       | Team         | public     | 팀 ID로 팀 엔티티를 조회한다.<br>존재하지 않을 경우 예외 발생.          |
| `findByIdWithMembers(Long id)`                                                                                            | Team         | public     | 팀 ID로 조회하며, 팀 멤버(`members`, `user`)를 함께 로딩한다.    |
| `existsById(Long id)`                                                                                                     | boolean      | public     | 특정 팀 ID의 존재 여부를 반환한다.                            |
| `getByUserIdAndCategoryAndStatus(Long userId, RecruitmentCategory category, RecruitmentStatus status, Pageable pageable)` | Page\<TeamResponseDto> | public     | 특정 사용자가 작성한 모집공고 중 지정된 카테고리와 상태의 팀 목록을 페이징 조회한다. |

#### TeamServiceImpl
[TeamService](#teamservice) 인터페이스의 구현체로,
[Team](#team) 엔티티의 조회 및 저장, 사용자별 팀 목록 조회 기능을 담당하는 서비스 클래스.

##### Attributes
| Name                | Type            | Visibility           | Description                          |
|---------------------|-----------------| -------------------- | ------------------------------------ |
| teamRepository      | TeamRepository  | private final        | Team 엔티티에 대한 데이터 접근을 담당하는 Repository |
| pageableValidator   | PageableValidator | private final        | 페이지 및 정렬 요청을 검증하는 유틸리티 클래스           |
| MY_TEAM_SORT_FIELDS | Set\<String>     | private static final | 허용된 정렬 기준 필드 목록 (현재 `createdAt`만 허용) |

##### Operations
| Name                                                                                                                      | Return Type  | Visibility | Description                                                                                |
| ------------------------------------------------------------------------------------------------------------------------- | ------------ | ---------- | ------------------------------------------------------------------------------------------ |
| `save(Team team)`                                                                                                         | void         | public     | 새 팀 정보를 데이터베이스에 저장한다.                                                                      |
| `findById(Long id)`                                                                                                       | Team         | public     | 팀 ID로 팀을 조회하고, 없을 경우 `TEAM_NOT_FOUND` 예외를 발생시킨다.                                           |
| `findByIdWithMembers(Long id)`                                                                                            | Team         | public     | 팀 ID로 팀을 조회하며, 팀 멤버(`members` 및 `user`)를 함께 로딩한다.                                          |
| `existsById(Long id)`                                                                                                     | boolean      | public     | 특정 ID의 팀 존재 여부를 반환한다.                                                                      |
| `getByUserIdAndCategoryAndStatus(Long userId, RecruitmentCategory category, RecruitmentStatus status, Pageable pageable)` | Page\<TeamResponseDto> | public     | 특정 사용자가 생성한 모집공고 중 지정된 카테고리 및 상태의 팀 목록을 페이지 단위로 조회한다.<br>조회된 엔티티는 `TeamResponseDto`로 변환된다. |

#### TeamRepository
Spring Data JPA를 활용하여 Team 엔티티의 데이터 접근을 담당하는 Repository 인터페이스.
팀 정보 조회, 모집 공고별 팀 목록 조회 등의 기능을 제공하며,
`@EntityGraph`를 사용하여 연관 엔티티 로딩 시 N+1 문제를 방지한다.

##### Attributes
| Name                 | Type                          | Visibility           | Description                                   |
| -------------------- | ----------------------------- | -------------------- | --------------------------------------------- |

##### Operations
| Name                                                                                                                                                                | Return Type   | Visibility | Description                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ---------- | ------------------------------------------------------------------------------------------------------ |
| `findDistinctByRecruitmentCategoryAndRecruitmentStatusAndRecruitmentUserId(RecruitmentCategory category, RecruitmentStatus status, Long userId, Pageable pageable)` | Page\<Team>    | public     | 특정 사용자가 작성한 모집공고 중 지정된 카테고리 및 상태의 팀 목록을 페이징 조회한다.<br>연관된 `Recruitment` 및 `User`를 `EntityGraph`로 함께 로딩. |
| `findByIdWithMembers(Long id)`                                                                                                                                      | Optional\<Team> | public     | 팀 ID로 팀 정보를 조회하며, `members`와 각 `member.user`를 함께 로딩한다.<br>N+1 문제 방지를 위해 `EntityGraph`를 사용한다.           |


#### TeamMemberController
팀 멤버([TeamMember](#teammember)) 관련 요청을 처리하는 REST API 컨트롤러 계층 클래스.
팀 리더가 특정 멤버를 팀에서 강제 탈퇴시키는 기능을 제공한다.

##### Attributes
| Name              | Type              | Visibility    | Description                 |
| ----------------- | ----------------- | ------------- | --------------------------- |
| teamMemberService | TeamMemberService | private final | 팀 멤버 관리 로직을 수행하는 서비스 계층 의존성 |

##### Operations
| Name                                                                      | Return Type                       | Mapping                                           | Visibility | Description                                                                                                                                                                                                             |
| ------------------------------------------------------------------------- | --------------------------------- |---------------------------------------------------|------------| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deleteMember(Long teamId, Long memberId, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void>> | `DELETE api/v1/teams/{teamId}/members/{memberId}` | public     | **팀 리더 권한으로 특정 팀 멤버를 삭제하는 엔드포인트.**<br><ul><li>`teamId`: 삭제 대상이 속한 팀 ID</li><li>`memberId`: 삭제할 멤버의 ID</li><li>`userDetails`: 현재 로그인한 사용자 정보 (리더 권한 검증용)</li></ul><br>성공 시 `"팀 멤버 삭제에 성공했습니다."` 메시지와 함께 200 OK 응답을 반환한다. |


#### TeamMemberService
팀 멤버 관리 기능을 정의하는 서비스 계층 인터페이스.
[TeamMemberServiceImpl](#teammemberserviceimpl)에서 구현되며, 팀 내 멤버 삭제(리더 권한 기반)와 같은 비즈니스 로직의 계약(Contract)을 명시한다.

##### Attributes
| Name          | Type          | Visibility    | Description                   |
| ------------- | ------------- | ------------- | ----------------------------- |

##### Operations
| Name                                                       | Return Type | Visibility | Description                                                                                                 |
| ---------------------------------------------------------- | --------- | ---------- | ----------------------------------------------------------------------------------------------------------- |
| `removeMember(Long teamId, Long removerId, Long targetId)` | void      | public     | 특정 팀(`teamId`)에서 리더(`removerId`)가 특정 멤버(`targetId`)를 삭제(강제 탈퇴)한다.<br>실제 구현은 `TeamMemberServiceImpl`에서 수행된다. |


#### TeamMemberServiceImpl
[TeamMemberService](#teammemberservice) 인터페이스의 구현체로,
팀 멤버 관리 및 리더 권한 기반 삭제 로직을 수행하는 서비스 클래스.
동시성 제어를 위해 `@Version` 기반 Optimistic Lock과 Spring Retry를 활용하며, 비즈니스 예외를 통한 명확한 검증 로직을 갖는다.

##### Attributes
| Name                 | Type                 | Visibility    | Description                     |
| -------------------- | -------------------- | ------------- | ------------------------------- |
| teamMemberRepository | TeamMemberRepository | private final | 팀 멤버 엔티티에 대한 데이터 접근 계층          |
| teamService          | TeamService          | private final | 팀 엔티티 조회 및 검증을 담당하는 서비스         |
| recruitmentService   | RecruitmentService   | private final | 모집공고(BaseRecruitment) 관련 서비스 계층 |


##### Operations
| Name                                                                                             | Return Type | Visibility | Description                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------ | ------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `removeMember(Long teamId, Long removerId, Long targetId)`                                       | void    | public     | 리더 권한으로 특정 팀 멤버를 강제 탈퇴시킨다.<br>다음 검증 및 처리 단계를 수행한다:<ul><li>리더 본인은 자신을 삭제할 수 없음</li><li>리더 권한 여부 검증</li><li>존재하지 않는 팀/멤버 시 예외 발생</li><li>모든 조건 충족 시 `team.removeMember()` 수행 및 모집 인원 감소</li></ul>동시성 충돌 발생 시 최대 3회 재시도한다. |
| `recover(ObjectOptimisticLockingFailureException e, Long teamId, Long removerId, Long targetId)` | void    | protected  | 재시도 실패 시 호출되는 복구 메서드.<br>`TOO_MANY_REQUESTS` 예외를 발생시켜 클라이언트에 알린다.                                                                                                                                                       |

#### TeamMemberRepository
[TeamMember](#teammember) 엔티티에 대한 데이터 접근 계층(Repository).
Spring Data JPA의 JpaRepository를 상속받아 기본 CRUD 기능을 제공한다.

##### Attributes
| Name          | Type          | Visibility    | Description                   |
| ------------- | ------------- | ------------- | ----------------------------- |

##### Operations
| Name                                              | Return Type          | Visibility | Description                                                   |
| ------------------------------------------------- | -------------------- | ---------- | ------------------------------------------------------------- |
| `findByTeamIdAndUserId(Long teamId, Long userId)` | Optional\<TeamMember> | public     | 특정 팀 ID와 사용자 ID로 팀 멤버를 조회한다.<br>결과가 없을 경우 빈 `Optional`을 반환한다. |


### 3.4.6 User Profile Process Structure

<img width="4096" height="2055" alt="03-user-profile-flow" src="https://github.com/user-attachments/assets/cae50c6e-d20a-4b94-a7ff-8e622a19505b" />

사용자 프로필 관리 기능의 계층 구조를 나타낸다. UserController와 UserService의 의존 관계, 프로필 조회 시 ReviewService와의 연동 구조, 그리고 비밀번호 변경 및 회원 탈퇴 등의 보안 처리 과정을 표현한다.

#### EmailRequestDto

이메일 인증, 로그인, 회원가입 등에서 사용자의 이메일 입력값을 전달하기 위한 요청 DTO 클래스.
입력값이 유효한 이메일 형식인지 `@Email` 어노테이션으로 검증한다.

##### Attributes
| Name  | Type   | Visibility                 | Description                         |
| ----- | ------ | -------------------------- | ----------------------------------- |
| email | String | private  | 사용자 이메일 주소. `@Email` 제약으로 형식 검증 수행. |

##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |

#### EmailVerificationRequestDto
사용자가 입력한 이메일과 인증번호(6자리)를 서버로 전달하여
이메일 인증 유효성을 검증하기 위한 요청 DTO.

##### Attributes
| Name      | Type   | Visibility                 | Description                                                         |
| --------- | ------ | -------------------------- | ------------------------------------------------------------------- |
| email     | String | private  | 사용자의 이메일 주소. `@NotBlank`, `@Email` 제약으로 비어 있지 않으며 올바른 형식인지 검증.      |
| inputCode | String | private  | 사용자가 입력한 인증번호. `@NotBlank`, `@Pattern("^[0-9]{6}")`으로 6자리 숫자 형식 검증. |

##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |

#### SignUpRequestDto
회원가입 요청을 처리하기 위한 DTO.
사용자 입력값을 검증한 뒤, 이를 기반으로 [User](#user) 엔티티를 생성한다.
비밀번호 검증 및 암호화, 학년·포지션·기술스택 등의 도메인 속성을 포함한다.

##### Attributes
| Name            | Type         | Visibility                 | Description                                |
| --------------- | ------------ | -------------------------- |--------------------------------------------|
| username        | String       | private  | 사용자 아이디. 영문, 숫자, 언더스코어 4~20자 (`@Pattern`). |
| password        | String       | private  | 비밀번호. 영문, 숫자, 특수문자 포함 8~72자 (`@Pattern`).  |
| passwordConfirm | String       | private  | 비밀번호 확인. password와 일치 여부 검증.               |
| nickname        | String       | private  | 닉네임. 한글/영문/숫자 2~10자 (`@Pattern`).          |
| email           | String       | private  | 이메일 주소. `@Email` 형식 검증.                    |
| grade           | Integer      | private  | 학년 (1~4 범위, `@Min`, `@Max`).               |
| position        | PositionType | private  | 포지션(enum). 예: BACK_END, FRONT_END 등.       |
| skills          | Set\<Skill>   | private  | 기술 스택 목록. 최대 10개 (`@Size(max=10)`).        |
| shortIntro      | String       | private  | 한 줄 소개. 최대 100자 제한.                        |

##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |
| `toEntity(PasswordEncoder passwordEncoder)` | User        | public     | 입력값을 기반으로 `User` 엔티티를 생성하고 비밀번호를 암호화하여 매핑.    |

#### SignInRequestDto
사용자의 로그인 요청 시 전달되는 DTO.
입력받은 아이디(`username`)와 비밀번호(`password`)를 검증하여 인증 절차에 사용된다.

##### Attributes
| Name     | Type   | Visibility                 | Description                 |
| -------- | ------ | -------------------------- | --------------------------- |
| username | String | private  | 사용자 아이디. `@NotBlank` 제약 적용. |
| password | String | private  | 비밀번호. `@NotBlank` 제약 적용.    |

##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |

#### TokenPair
JWT 기반 인증 시스템에서 Access Token과 Refresh Token을 함께 반환하기 위한 응답 DTO.
인증 성공 후 클라이언트가 인증 상태를 유지하도록 두 종류의 토큰을 제공한다.

##### Attributes
| Name         | Type   | Visibility                 | Description                                 |
| ------------ | ------ | -------------------------- | ------------------------------------------- |
| accessToken  | String | private  | 클라이언트의 인증 요청 시 사용되는 짧은 수명의 JWT 액세스 토큰.      |
| refreshToken | String | private  | 액세스 토큰 만료 시 재발급을 위해 사용되는 장기 유효 JWT 리프레시 토큰. |

##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |

#### PasswordRequestDto
비밀번호 변경 요청 시 사용되는 DTO 클래스.
사용자가 입력한 기존 비밀번호, 새로운 비밀번호, 비밀번호 확인 값을 검증하여 서버로 전달한다.
요청 검증 실패 시 BusinessException이 발생한다.

##### Attributes

| Name            | Type   | Visibility                 | Description                                               |
| --------------- | ------ | -------------------------- | --------------------------------------------------------- |
| old             | String | private  | 기존 비밀번호. `@NotBlank`, `@Size(max=72)` 제약 적용.              |
| newPassword     | String | private  | 새로운 비밀번호. `@NotBlank`, `@Pattern`(영문, 숫자, 특수문자 포함 8~72자). |
| passwordConfirm | String | private  | 비밀번호 확인 입력값. `@NotBlank`.                                 |

##### Operations
| Name                                                                       | Return Type | Visibility | Description                                                                                                          |
| -------------------------------------------------------------------------- | ----------- | ---------- | -------------------------------------------------------------------------------------------------------------------- |

#### UserUpdateRequestDto
사용자 프로필 수정 요청을 처리하기 위한 DTO.
클라이언트의 `/me` (PATCH) 요청 바디로 전달되며,
닉네임·학년·포지션·기술 스택·한 줄 소개 등의 정보를 수정한다.

##### Attributes
| Name       | Type         | Visibility                 | Description                                         |
| ---------- | ------------ | -------------------------- |-----------------------------------------------------|
| nickname   | String       | private  | 닉네임. 선택 입력. 2~10자의 한글, 영문, 숫자만 허용 (`@Pattern`).     |
| grade      | Integer      | private  | 학년. 필수 입력, 1~4 범위 (`@Min`, `@Max`).                 |
| position   | PositionType | private  | 포지션(enum). 예: FRONT_END, BACK_END 등.                |
| skills     | Set\<Skill>   | private  | 보유 기술 스택. 최대 10개 제한 (`@Size(max=10)`).              |
| shortIntro | String       | private  | 한 줄 소개. 최대 100자 제한 (`@Size(max=100)`, `@NotBlank`). |


##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |

#### WithdrawRequestDto
회원 탈퇴 요청 시 비밀번호를 입력받아 본인 여부를 확인하기 위한 요청 DTO.
BCrypt 해시 알고리즘 제약에 맞춘 최대 72자 제한을 적용하며, 비밀번호가 누락되거나 공백만 입력되는 경우 유효성 검증에 실패한다.

##### Attributes
| Name     | Type   | Visibility                 | Description                                   |
| -------- | ------ | -------------------------- | --------------------------------------------- |
| password | String | private  | 비밀번호 입력값. `@NotBlank`, `@Size(max=72)` 제약 적용. |

##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |

#### UserResponseDto
사용자 정보를 API 응답으로 전달하기 위한 DTO.
[User](#user) 엔티티의 핵심 속성을 변환하여 노출하며, 보안 및 캡슐화를 위해 엔티티 자체를 직접 반환하지 않는다.

##### Attributes
| Name       | Type         | Visibility                 | Description                          |
| ---------- | ------------ | -------------------------- |--------------------------------------|
| username   | String       | private  | 사용자 아이디.                             |
| email      | String       | private  | 이메일 주소.                              |
| university | University   | private  | 이메일 도메인 기반의 소속 대학교(enum).            |
| nickname   | String       | private  | 사용자 닉네임.                             |
| grade      | Integer      | private  | 사용자 학년.                              |
| position   | PositionType | private  | 포지션(enum). 예: FRONT_END, BACK_END 등. |
| skills     | Set\<Skill>   | private  | 보유 기술 스택 목록.                         |
| shortIntro | String       | private  | 사용자 한 줄 소개.                          |

##### Operations
| Name                 | Return Type     | Visibility    | Description                                |
| -------------------- | --------------- | ------------- | ------------------------------------------ |
| `from(User user)`      | UserResponseDto | public static | `User` 엔티티를 DTO로 변환하는 정적 팩토리 메서드.          |


#### UserController
사용자 도메인([User](#user)) 관련 API 요청을 처리하는 REST Controller.
회원정보 조회, 수정, 비밀번호 변경, 회원 탈퇴, 중복검사, 리뷰 조회 등의 기능을 제공한다.

##### Attributes
| Name                      | Type          | Visibility           | Description                                    |
| ------------------------- | ------------- | -------------------- | ---------------------------------------------- |
| REFRESH_TOKEN_COOKIE_NAME | String        | private static final | Refresh Token을 저장/삭제할 쿠키 이름 (`refresh_token`). |
| userService               | UserService   | private final        | 사용자 관련 비즈니스 로직을 처리하는 서비스 계층.                   |
| reviewService             | ReviewService | private final        | 리뷰 조회 로직을 담당하는 서비스 계층.                         |

##### Operations
| Name                                                                                            | Return Type                                                  | Mapping                                      | Visibility | Description                              |
| ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------- | ---------- | ---------------------------------------- |
| `existsByUsername(String username)`                                                             | ResponseEntity\<ApiResponse\<Boolean>>                         | `GET api/v1/users/username/check`            | public     | 아이디 중복 여부 검사                             |
| `existsByEmail(String email)`                                                                   | ResponseEntity\<ApiResponse\<Boolean>>                         | `GET api/v1/users/email/check`               | public     | 이메일 중복 여부 검사                             |
| `existsByNickname(String nickname)`                                                             | ResponseEntity\<ApiResponse\<Boolean>>                         | `GET api/v1/users/nickname/check`            | public     | 닉네임 중복 여부 검사                             |
| `passwordChange(PasswordRequestDto dto, CustomUserDetails userDetails)`                         | ResponseEntity\<ApiResponse\<Void>>                            | `POST api/v1/users/me/password-change`       | public     | 인증된 사용자의 비밀번호 변경                         |
| `getMe(CustomUserDetails userDetails)`                                                          | ResponseEntity\<ApiResponse\<UserResponseDto>>                 | `GET api/v1/users/me`                        | public     | 현재 로그인한 사용자의 프로필 조회                      |
| `updateMe(CustomUserDetails userDetails, UserUpdateRequestDto dto)`                             | ResponseEntity\<ApiResponse\<UserResponseDto>>                 | `PATCH api/v1/users/me`                      | public     | 현재 로그인한 사용자의 프로필 수정                      |
| `withdraw(CustomUserDetails userDetails, WithdrawRequestDto dto, HttpServletResponse response)` | ResponseEntity\<ApiResponse\<Void>>                            | `DELETE api/v1/users/me/withdraw`            | public     | 회원 탈퇴 (`Soft Delete` + Refresh Token 제거) |
| `getReviews(Long userId, Pageable pageable)`                                                    | ResponseEntity\<ApiResponse\<PageResponse\<ReviewResponseDto>>> | `GET api/v1/users/{userId}/reviews/received` | public     | 특정 사용자가 받은 리뷰 목록 조회                      |
| `addCookie(HttpServletResponse response, String token, String cookieName, long maxAge)`         | void                                                         | -                                            | private    | Refresh Token 쿠키 생성 또는 만료 처리             |



#### AuthController
인증 관련 API 요청을 처리하는 REST Controller.
회원가입, 로그인, 이메일 인증, 토큰 재발급/로그아웃 등의 요청을 받아 [AuthService](#authservice) 및 [UserService](#userservice)를 호출하여 비즈니스 로직을 수행한다.

##### Attributes
| Name                      | Type        | Visibility           | Description                                 |
| ------------------------- | ----------- | -------------------- | ------------------------------------------- |
| REFRESH_TOKEN_COOKIE_NAME | String      | private static final | Refresh Token을 저장할 쿠키 이름 (`refresh_token`). |
| jwtUtil                   | JwtUtil     | private final        | JWT 생성 및 검증 유틸리티.                           |
| authService               | AuthService | private final        | 인증 관련 비즈니스 로직 서비스.                          |
| userService               | UserService | private final        | 사용자 회원가입 및 사용자 관련 서비스.                      |

##### Operations
| Name                                                                                    | Return Type                       | Mapping                          | Visibility | Description                                                     |
| --------------------------------------------------------------------------------------- | --------------------------------- | -------------------------------- | ---------- | --------------------------------------------------------------- |
| `sendEmailAuthCode(EmailRequestDto dto)`                                                | ResponseEntity\<ApiResponse\<Void>> | `POST /api/v1/auth/email/code`   | public     | 회원가입 이메일 인증번호 발송                                                |
| `verifyAuthCode(EmailVerificationRequestDto dto)`                                       | ResponseEntity\<ApiResponse\<Void>> | `POST /api/v1/auth/email/verify` | public     | 사용자가 입력한 이메일 인증번호 검증                                            |
| `signUp(SignUpRequestDto dto)`                                                          | ResponseEntity\<ApiResponse\<Long>> | `POST /api/v1/auth/sign-up`      | public     | 회원가입 요청 처리 — 성공 시 생성된 `userId` 반환                               |
| `signIn(SignInRequestDto dto, HttpServletResponse response)`                            | ResponseEntity\<ApiResponse\<Void>> | `POST /api/v1/auth/sign-in`      | public     | 로그인 처리 및 Access/Refresh Token 발급                                |
| `signOut(HttpServletResponse response, CustomUserDetails userDetails)`                  | ResponseEntity\<ApiResponse\<Void>> | `POST /api/v1/auth/sign-out`     | public     | 로그아웃 처리 및 Redis 토큰 삭제                                           |
| `refreshToken(String refreshToken, HttpServletResponse response)`                       | ResponseEntity\<ApiResponse\<Void>> | `POST /api/v1/auth/refresh`      | public     | Refresh Token 기반 Access/Refresh Token 재발급                       |
| `addCookie(HttpServletResponse response, String token, String cookieName, long maxAge)` | void                              | -                                | private    | Refresh Token을 응답 쿠키에 설정 (`HttpOnly`, `Secure`, `SameSite=Lax`) |

#### UserService
사용자([User](#user)) 관련 핵심 비즈니스 로직을 정의하는 서비스 인터페이스.
회원가입, 비밀번호 변경, 사용자 정보 조회 및 수정, 탈퇴 등 주요 기능을 제공한다.

##### Attributes
| Name    | Type   | Visibility                 | Description |
| ------- | ------ | -------------------------- | ----------- |

##### Operations
| Name                                                    | Return Type     | Description                          |
| ------------------------------------------------------- | --------------- | ------------------------------------ |
| `findByUsername(String username)`                       | User            | 사용자 이름으로 사용자 조회 — 존재하지 않으면 예외 발생     |
| `findById(Long id)`                                     | User            | 사용자 ID로 조회 — 존재하지 않으면 예외 발생          |
| `findByIdWithSkills(Long id)`                           | User            | 사용자 및 보유 기술 스택을 함께 조회 (`Fetch Join`) |
| `existsById(Long id)`                                   | boolean         | 해당 ID의 사용자가 존재하는지 확인                 |
| `existsByEmail(String email)`                           | boolean         | 이메일 중복 여부 확인                         |
| `existsByUsername(String username)`                     | boolean         | 아이디 중복 여부 확인                         |
| `existsByNickname(String nickname)`                     | boolean         | 닉네임 중복 여부 확인                         |
| `signUp(SignUpRequestDto dto)`                          | Long            | 신규 사용자 등록 후 생성된 사용자 ID 반환            |
| `changePassword(Long userId, PasswordRequestDto dto)`   | void            | 기존 비밀번호 검증 후 새 비밀번호로 변경              |
| `getUserInfo(Long userId)`                              | UserResponseDto | 사용자 프로필 정보 조회                        |
| `updateUserInfo(Long userId, UserUpdateRequestDto dto)` | UserResponseDto | 사용자 정보 수정 후 갱신된 데이터 반환               |
| `withdraw(Long userId, String rawPassword)`             | void            | 비밀번호 검증 후 회원 탈퇴 (`Soft Delete`)      |


#### AuthService
인증(Authentication) 관련 핵심 비즈니스 로직을 정의하는 서비스 인터페이스.
이메일 인증, 로그인, 리프레시 토큰 삭제 및 재발급 등의 기능을 포함한다.
구체 구현체([AuthServiceImpl](#authserviceimpl))가 실제 로직을 담당한다.

##### Attributes
| Name                 | Type                          | Visibility           | Description                                   |
| -------------------- | ----------------------------- | -------------------- | --------------------------------------------- |

##### Operations
| Name                                                         | Return Type | Visibility | Description                      |
| ------------------------------------------------------------ | ----------- | ---------- | -------------------------------- |
| `sendAuthCode(String toEmail)`                               | void        | public     | 입력된 이메일 주소로 인증 코드를 발송            |
| `sendEmailAuthCode(String toEmail, String verificationCode)` | void        | public     | 이메일과 인증번호를 기반으로 인증 메일을 전송        |
| `verifyCode(String toEmail, String inputCode)`               | void        | public     | 사용자가 입력한 인증번호를 검증                |
| `login(SignInRequestDto dto)`                                | TokenPair   | public     | 로그인 요청 DTO를 기반으로 인증 수행 및 JWT 발급  |
| `deleteRefreshToken(Long userId)`                            | void        | public     | 로그아웃 시 사용자의 리프레시 토큰을 삭제          |
| `reissueTokens(String refreshToken)`                         | TokenPair   | public     | 리프레시 토큰을 검증 후 새로운 액세스/리프레시 토큰 발급 |


#### UserServiceImpl
[UserService](#userservice)의 실제 구현체로,
사용자 관리 도메인([User](#user))에 대한 모든 핵심 비즈니스 로직을 수행한다.
회원가입, 비밀번호 변경, 프로필 조회 및 수정, 회원 탈퇴를 처리한다.

##### Attributes
| Name                 | Type                          | Visibility           | Description                                   |
| -------------------- | ----------------------------- | -------------------- | --------------------------------------------- |
| REFRESH_TOKEN_PREFIX | String                        | private static final | Redis에 저장된 Refresh Token의 key prefix (`RT:`). |
| userRepository       | UserRepository                | private final        | 사용자 데이터 접근 계층.                                |
| passwordEncoder      | PasswordEncoder               | private final        | 비밀번호 암호화/검증을 담당하는 Spring Security 컴포넌트.       |
| redisTemplate        | RedisTemplate\<String, String> | private final        | Refresh Token 캐시 저장 및 삭제용 Redis 클라이언트.        |

##### Operations
| Name                                                    | Return Type     | Visibility | Description                                                   |
| ------------------------------------------------------- | --------------- | ---------- | ------------------------------------------------------------- |
| `findByUsername(String username)`                       | User            | public     | 사용자 이름으로 조회 — 없을 시 `USER_NOT_FOUND` 예외 발생                     |
| `findById(Long id)`                                     | User            | public     | 사용자 ID로 조회 — 없을 시 `USER_NOT_FOUND` 예외 발생                      |
| `findByIdWithSkills(Long id)`                           | User            | public     | 사용자 및 기술 스택(`fetch join`) 조회                                  |
| `existsById(Long id)`                                   | boolean         | public     | ID 존재 여부 반환                                                   |
| `existsByEmail(String email)`                           | boolean         | public     | 이메일 중복 여부 반환                                                  |
| `existsByUsername(String username)`                     | boolean         | public     | 아이디 중복 여부 반환                                                  |
| `existsByNickname(String nickname)`                     | boolean         | public     | 닉네임 중복 여부 반환                                                  |
| `signUp(SignUpRequestDto dto)`                          | Long            | public     | 회원가입 처리 — 중복 체크 후 비밀번호 암호화 및 엔티티 저장                           |
| `changePassword(Long userId, PasswordRequestDto dto)`   | void            | public     | 기존 비밀번호 검증 후 새 비밀번호로 변경                                       |
| `getUserInfo(Long userId)`                              | UserResponseDto | public     | 사용자 상세 정보 조회 후 DTO 변환                                         |
| `updateUserInfo(Long userId, UserUpdateRequestDto dto)` | UserResponseDto | public     | 사용자 프로필 정보 수정 (`grade`, `position`, `skills`, `shortIntro` 등) |
| `withdraw(Long userId, String rawPassword)`             | void            | public     | 비밀번호 검증 후 Soft Delete(`WITHDRAWN`) 처리 및 Redis 토큰 삭제           |


#### AuthServiceImpl
인증 서비스([AuthService](#authservice))의 구현체로,
이메일 인증, 로그인/로그아웃, JWT 토큰 발급 및 재발급을 담당한다.
`Spring Security`, `Redis`, `JavaMailSender`를 활용하여 인증 절차와 세션 관리를 수행한다.

##### Attributes
| Name                      | Type                          | Visibility           | Description                                  |
| ------------------------- | ----------------------------- | -------------------- | -------------------------------------------- |
| REFRESH_TOKEN_PREFIX      | String                        | private static final | Redis에 저장될 Refresh Token Key prefix (`RT:`). |
| EMAIL_VERIFICATION_PREFIX | String                        | private static final | Redis에 저장될 이메일 인증번호 Key prefix (`EMAIL:`).   |
| EXPIRATION_MINUTES        | int                           | private static final | 이메일 인증번호 TTL(3분).                            |
| fromEmail                 | String                        | private              | 인증 메일 발신자 주소 (application.yml에서 주입).         |
| jwtUtil                   | JwtUtil                       | private final        | JWT 생성 및 검증 유틸리티 클래스.                        |
| userService               | UserService                   | private final        | 사용자 조회 및 유효성 검증 서비스.                         |
| mailSender                | JavaMailSender                | private final        | 인증 메일 전송을 위한 메일 발송 컴포넌트.                     |
| redisTemplate             | RedisTemplate\<String, String> | private final        | 이메일 인증번호 및 토큰 저장소.                           |
| authenticationManager     | AuthenticationManager         | private final        | Spring Security 인증 처리 매니저.                   |

##### Operations
| Name                                                         | Return Type | Visibility | Description                                        |
| ------------------------------------------------------------ | ----------- | ---------- | -------------------------------------------------- |
| `sendAuthCode(String toEmail)`                               | void        | public     | 랜덤 6자리 인증번호를 생성 후 Redis에 저장하고, 해당 이메일로 발송          |
| `sendEmailAuthCode(String toEmail, String verificationCode)` | void        | public     | HTML 이메일 템플릿을 생성하여 인증번호 발송                         |
| `verifyCode(String toEmail, String inputCode)`               | void        | public     | Redis에 저장된 인증번호와 입력값을 비교하여 검증                      |
| `login(SignInRequestDto dto)`                                | TokenPair   | public     | 사용자 로그인 인증 수행 후 Access/Refresh Token 발급            |
| `deleteRefreshToken(Long userId)`                            | void        | public     | Redis에서 특정 사용자 Refresh Token 삭제 (로그아웃 처리)          |
| `reissueTokens(String refreshToken)`                         | TokenPair   | public     | 유효한 Refresh Token을 기반으로 새 Access/Refresh Token 재발급 |
| `generateVerificationCode()`                                 | String      | private    | 6자리 랜덤 인증번호 생성                                     |
| `createEmailTemplate(String verificationCode)`               | String      | private    | 인증 이메일 HTML 템플릿 문자열 생성                             |


#### UserRepository
사용자([User](#user)) 엔티티에 대한 데이터 접근을 담당하는 JPA Repository.
Spring Data JPA를 기반으로 기본 CRUD 기능을 상속받으며,
이메일, 닉네임, 아이디 중복 검증 및 Fetch Join을 통한 `skills` 컬렉션 로딩 기능을 추가로 제공한다.

##### Attributes
| Name    | Type   | Visibility                 | Description |
| ------- | ------ | -------------------------- | ----------- |

##### Operations
| Name                                | Return Type    | Description                               |
| ----------------------------------- | -------------- | ----------------------------------------- |
| `findByUsername(String username)`   | Optional\<User> | 사용자 아이디(`username`)로 조회                   |
| `existsByEmail(String email)`       | boolean        | 이메일 중복 여부 확인                              |
| `existsByUsername(String username)` | boolean        | 아이디 중복 여부 확인                              |
| `existsByNickname(String nickname)` | boolean        | 닉네임 중복 여부 확인                              |
| `findByIdWithSkills(Long id)`       | Optional\<User> | Fetch Join으로 `skills` 컬렉션을 함께 로딩하여 사용자 조회 |

### 3.4.7 Review Process Structure

<img width="3558" height="1844" alt="03-review-flow" src="https://github.com/user-attachments/assets/557e2815-cb61-4298-b353-9f218e7d786a" />

리뷰 관리 기능의 계층 구조와 처리 과정을 나타낸다. ReviewController, ReviewService, ReviewRepository 간의 의존 관계를 표현하며, 리뷰 작성자와 수신자 간의 관계 구조와 소프트 삭제 메커니즘을 보여준다.


#### ReviewCreationRequestDto
리뷰 생성 요청을 처리하기 위한 DTO.
사용자가 다른 사용자에 대한 후기를 작성할 때 전달되는 요청 데이터 구조이다.

##### Attributes
| Name       | Type   | Visibility                 | Description                                 |
| ---------- | ------ | -------------------------- | ------------------------------------------- |
| revieweeId | Long   | private  | 후기 대상 사용자 ID. `@NotNull` 제약 적용.             |
| content    | String | private  | 리뷰 내용. `@NotBlank`, `@Size(max=100)` 검증 적용. |

##### Operations
| Name                                                      | Return Type | Visibility | Description                                 |
| --------------------------------------------------------- | ----------- | ---------- | ------------------------------------------- |
| `toEntity(User reviewer, User reviewee, String content)`    | Review      | public     | 리뷰 작성자와 대상 유저 정보를 받아 `Review` 엔티티로 변환.      |


#### ReviewUpdateRequestDto
리뷰 수정 요청 시 사용되는 DTO.
사용자가 작성한 리뷰의 내용을 변경하기 위해 전달하는 요청 데이터 구조이다.

##### Attributes
| Name    | Type   | Visibility                 | Description                                     |
| ------- | ------ | -------------------------- | ----------------------------------------------- |
| content | String | private  | 수정된 리뷰 내용. `@NotBlank`, `@Size(max=100)` 검증 적용. |

##### Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |

#### ReviewResponseDto
리뷰 정보를 클라이언트에게 응답하기 위한 DTO.
[Review](#review) 엔티티의 데이터를 안전하게 변환하여 외부에 노출한다.

##### Attributes
| Name    | Type   | Visibility                 | Description |
| ------- | ------ | -------------------------- | ----------- |
| content | String | private  | 리뷰 본문 내용.   |

##### Operations
| Name                              | Return Type       | Visibility    | Description                                         |
| --------------------------------- | ----------------- | ------------- | --------------------------------------------------- |
| `from(Review review)`               | ReviewResponseDto | public static | `Review` 엔티티를 `ReviewResponseDto`로 변환하는 정적 팩토리 메서드. |


#### ReviewController
리뷰([Review](#review)) 관련 요청을 처리하는 REST API 컨트롤러 계층 클래스.
리뷰 작성, 조회(받은/작성한), 수정, 삭제(Soft Delete) 기능을 담당하며, [ReviewService](#reviewservice)를 호출해 비즈니스 로직을 수행한다.

##### Attributes
| Name          | Type          | Visibility    | Description                    |
| ------------- | ------------- | ------------- | ------------------------------ |
| reviewService | ReviewService | private final | 리뷰 관련 비즈니스 로직을 담당하는 서비스 계층 의존성 |

##### Operations
| Name                                                                                     | Return Type                                            | Mapping                                       | Visibility | Description                                                                     |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------ |-----------------------------------------------|------------| ------------------------------------------------------------------------------- |
| `createReview(ReviewCreationRequestDto dto, CustomUserDetails userDetails)`              | ResponseEntity\<ApiResponse\<Long>>                      | `POST api/v1/reviews`                         | public     | 인증된 사용자가 새로운 리뷰를 작성한다.<br>성공 시 생성된 리뷰 ID를 반환한다.                                 |
| `getMyWrittenReviews(CustomUserDetails userDetails, Pageable pageable)`                  | ResponseEntity\<ApiResponse\<PageResponse\<ReviewResponseDto>>> | `GET api/v1/reviews/me/written`               | public     | 현재 로그인한 사용자가 **작성한 리뷰 목록**을 페이지 단위로 조회한다.<br>기본 정렬: `createdAt DESC`, 페이지 크기: 5 |
| `getMyReceivedReviews(CustomUserDetails userDetails, Pageable pageable)`                 | ResponseEntity\<ApiResponse\<PageResponse\<ReviewResponseDto>>> | `GET api/v1/reviews/me/received`              | public     | 현재 로그인한 사용자가 **받은 리뷰 목록**을 페이지 단위로 조회한다.<br>기본 정렬: `createdAt DESC`, 페이지 크기: 5 |
| `updateReview(Long reviewId, ReviewUpdateRequestDto dto, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Long>>                      | `PATCH api/v1/reviews/me/written/{reviewId}`  | public     | 인증된 사용자가 **본인이 작성한 리뷰를 수정**한다.<br>PATCH 메서드를 통해 부분 업데이트 수행.                  |
| `deleteReview(Long reviewId, CustomUserDetails userDetails)`                             | ResponseEntity\<ApiResponse\<Void>>                      | `DELETE api/v1/reviews/me/written/{reviewId}` | public     | 인증된 사용자가 **본인이 작성한 리뷰를 Soft Delete** 처리한다.<br>`ReviewStatus.DELETED` 상태로 변경. |


#### ReviewService
리뷰([Review](#review)) 도메인의 핵심 비즈니스 로직 계약(Contract)을 정의하는 서비스 인터페이스.
[ReviewServiceImpl](#reviewserviceimpl)에서 구현되며, 리뷰의 생성(Create), 조회(Read), 수정(Update), 삭제(Delete) 기능을 수행한다.

##### Attributes
| Name          | Type          | Visibility    | Description                   |
| ------------- | ------------- | ------------- | ----------------------------- |

##### Operations
| Name                                                                   | Return Type               | Visibility | Description                                                       |
| ---------------------------------------------------------------------- | ------------------------- | ---------- | ----------------------------------------------------------------- |
| `findById(Long id)`                                                    | Review                    | public     | 리뷰 ID로 리뷰 엔티티를 조회한다. 존재하지 않을 경우 예외 발생.                            |
| `createReview(Long reviewerId, ReviewCreationRequestDto dto)`          | Long                      | public     | 리뷰 작성자 ID와 요청 DTO를 기반으로 리뷰를 생성하고, 생성된 리뷰의 ID를 반환한다.               |
| `getReviewsByRevieweeId(Long revieweeId, Pageable pageable)`           | PageResponse\<ReviewResponseDto> | public     | 특정 사용자가 **받은 리뷰 목록**을 페이징 조건에 맞게 조회한다.                            |
| `getReviewsByReviewerId(Long reviewerId, Pageable pageable)`           | PageResponse\<ReviewResponseDto> | public     | 특정 사용자가 **작성한 리뷰 목록**을 페이징 조건에 맞게 조회한다.                           |
| `updateReview(Long userId, Long reviewId, ReviewUpdateRequestDto dto)` | Long                      | public     | 본인이 작성한 활성 상태의 리뷰만 수정할 수 있다.                                      |
| `deleteReview(Long userId, Long reviewId)`                             | void                      | public     | 본인이 작성한 활성 상태의 리뷰만 **Soft Delete** 처리한다. (`ReviewStatus.DELETED`) |


#### ReviewServiceImpl
[ReviewService](#reviewservice) 인터페이스의 구현체로,
리뷰 생성, 조회, 수정, 삭제(Soft Delete) 기능을 제공하는 서비스 클래스.
리뷰 작성자·피평가자 간의 관계 및 상태([ReviewStatus](#reviewstatus))를 검증한다.

##### Attributes
| Name           | Type           | Visibility    | Description                     |
| -------------- |----------------| ------------- | ------------------------------- |
| reviewRepository | ReviewRepository | private final | 리뷰 엔티티에 대한 데이터 접근 계층            |
| userService    | UserService    | private final | 리뷰 작성자 및 피평가자 검증을 위한 사용자 조회 서비스 |

##### Operations
| Name                                                                   | Return Type               | Visibility | Description                                                                                          |
| ---------------------------------------------------------------------- |---------------------------| ---------- | ---------------------------------------------------------------------------------------------------- |
| `findById(Long id)`                                                    | Review                    | public     | 리뷰 ID로 리뷰를 조회하고, 존재하지 않을 경우 `REVIEW_NOT_FOUND` 예외를 발생시킨다.                                            |
| `createReview(Long reviewerId, ReviewCreationRequestDto dto)`          | Long                      | public     | 리뷰 작성자와 피평가자 정보를 조회 후, 새로운 리뷰를 생성 및 저장한다.<br>자기 자신에게 리뷰를 남기는 경우 `SELF_REVIEW_NOT_ALLOWED` 예외 발생.     |
| `getReviewsByRevieweeId(Long revieweeId, Pageable pageable)`           | PageResponse\<ReviewResponseDto> | public     | 특정 사용자가 **받은 리뷰 목록**을 페이지 단위로 조회한다.<br>`ReviewStatus.ACTIVE` 상태만 조회하며, DTO 변환 후 `PageResponse`로 반환.  |
| `getReviewsByReviewerId(Long reviewerId, Pageable pageable)`           | PageResponse\<ReviewResponseDto> | public     | 특정 사용자가 **작성한 리뷰 목록**을 페이지 단위로 조회한다.<br>`ReviewStatus.ACTIVE` 상태만 조회하며, DTO 변환 후 `PageResponse`로 반환. |
| `updateReview(Long userId, Long reviewId, ReviewUpdateRequestDto dto)` | Long                      | public     | 본인이 작성한 활성 상태 리뷰만 수정할 수 있다.<br>리뷰 소유자 불일치 또는 비활성 리뷰일 경우 예외 발생.                                       |
| `deleteReview(Long userId, Long reviewId)`                             | void                      | public     | 본인이 작성한 활성 상태 리뷰만 **Soft Delete** 처리(`ReviewStatus.DELETED`) 가능.                                     |


#### ReviewRepository
[Review](#review) 엔티티에 대한 데이터 접근 계층(Repository).
Spring Data JPA의 JpaRepository를 상속받아 기본 CRUD 기능을 제공하며,
리뷰 대상자(피평가자) 및 리뷰 작성자 기준으로 리뷰를 조회하는 기능을 제공한다.

##### Attributes
| Name          | Type          | Visibility    | Description                   |
| ------------- | ------------- | ------------- | ----------------------------- |

##### Operations
| Name                                                                                 | Return Type  | Visibility | Description                                                      |
| ------------------------------------------------------------------------------------ | ------------ | ---------- | ---------------------------------------------------------------- |
| `findByRevieweeIdAndStatus(Long revieweeId, ReviewStatus status, Pageable pageable)` | Page\<Review> | public     | 특정 피평가자(`revieweeId`)와 상태(`status`)를 기준으로 리뷰 목록을 페이지 단위로 조회한다.   |
| `findByReviewerIdAndStatus(Long reviewerId, ReviewStatus status, Pageable pageable)` | Page\<Review> | public     | 특정 리뷰 작성자(`reviewerId`)와 상태(`status`)를 기준으로 리뷰 목록을 페이지 단위로 조회한다. |


### 3.4.8 Authentication & Authorization Process Structure

<img width="4096" height="2315" alt="03-authentication-authorization-flow" src="https://github.com/user-attachments/assets/cd8ace21-eece-4aa4-a3cb-96d7df69bb6d" />

인증 및 인가 기능의 보안 계층 구조를 나타낸다. AuthController와 AuthService의 관계, JWT 기반 토큰 처리 구조, SecurityConfig의 필터 체인 구성, 그리고 JwtFilter를 통한 요청 검증 과정을 표현한다.


### 3.4.9 Bookmark Process Structure


<img width="4096" height="1821" alt="03-bookmark-flow" src="https://github.com/user-attachments/assets/080958e1-c8fe-40db-a7c7-32fa795cba25" />

북마크 관리 기능의 계층 구조와 연동 방식을 나타낸다. BookmarkController와 BookmarkService의 의존 관계, ProjectService, AssignmentService, StudyService와의 협력 구조를 통한 카테고리별 북마크 목록 조회 메커니즘을 표현한다.


### 3.4.10 Scheduler Recruitment Process Structure

<img width="2577" height="1241" alt="03-scheduler-recruitment-flow" src="https://github.com/user-attachments/assets/f0c9959f-19fc-406a-8257-ecdaedb4a6d5" />

공고 마감 스케줄러의 자동화 처리 구조를 나타낸다. RecruitmentClosingScheduler 컴포넌트의 스케줄링 메커니즘, RecruitmentService와 ApplicationService를 활용한 일괄 처리 구조, 그리고 마감일 기반 상태 변경 프로세스를 표현한다.

---

## 3.5 설정 클래스 (Configuration Classes)

Spring Framework 기반 애플리케이션의 전역 설정을 담당하는 클래스들의 구조를 보여준다.
SecurityConfig를 통한 보안 설정, WebConfig를 통한 웹 관련 설정, JpaAuditingConfig를 통한 JPA Auditing 설정, QueryDslConfig를 통한 QueryDSL 설정, 그리고 PaginationProperties를 통한 페이지네이션 프로퍼티 설정 등 애플리케이션 전반의 설정 관리 클래스들의 구조와 관계를 표현한다.

#### JpaAuditingConfig
Spring Data JPA의 Auditing(감사 기능) 을 활성화하기 위한 환경 설정 클래스(Configuration Class).
`@EnableJpaAuditing`을 통해 엔티티의 생성일(`createdAt`), 수정일(`updatedAt`) 필드를 자동으로 관리한다.

##### Attributes
| Name | Type | Visibility | Description |
|------|------|-------------|--------------|

##### Operations
| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|


#### PaginationProperties
애플리케이션 전역에서 사용할 페이지네이션(Pagination) 관련 설정 클래스.
Spring Boot의 `@ConfigurationProperties`를 사용하여
`application.properties`의 `"pagination"` prefix 설정값을 자동 주입받는다.

##### Attributes
| Name        | Type | Visibility |  Description                                   |
| ----------- |-----| ---------- |  --------------------------------------------- |
| maxSize     | int | private    |  클라이언트가 요청할 수 있는 **최대 페이지 크기** (DoS 방지 목적)     |
| defaultSize | int | private    |  클라이언트가 `size` 파라미터를 지정하지 않았을 때의 **기본 페이지 크기** |
| maxPageNumber | int | private    |  허용되는 **최대 페이지 번호** (너무 깊은 페이지 요청 방지)          |

##### Operations
| Name                 | Return Type | Visibility | Description        |
| -------------------- | ---- | ---------- | ------------------ |
| `getMaxSize()`       | int  | public     | 최대 페이지 크기 값을 반환한다. |
| `getDefaultSize()`   | int  | public     | 기본 페이지 크기 값을 반환한다. |
| `getMaxPageNumber()` | int  | public     | 최대 페이지 번호 값을 반환한다. |


#### QueryDslConfig
`QueryDSL`을 사용하기 위한 Spring JPA 설정 클래스(Configuration Class).
`EntityManager`를 기반으로 `JPAQueryFactory` Bean을 등록하여
`QueryDSL`을 활용한 타입 안전한 동적 쿼리 작성이 가능하도록 지원한다.

##### Attributes
| Name          | Type          | Visibility | Description                                                                |
| ------------- | ------------- | ---------- | -------------------------------------------------------------------------- |
| entityManager | EntityManager | private    | JPA가 관리하는 엔티티 매니저. `@PersistenceContext`로 주입받아 QueryDSL의 쿼리 수행 기반 객체로 사용됨. |

##### Operations
| Name                | Return Type     | Visibility | Description                                                                                 |
| ------------------- | --------------- | ---------- | ------------------------------------------------------------------------------------------- |
| `jpaQueryFactory()` | JPAQueryFactory | public     | `EntityManager`를 주입받아 `JPAQueryFactory` Bean을 생성 및 등록한다.<br>QueryDSL 기반 리포지토리에서 주입받아 사용 가능. |


#### SecurityConfig
Spring Security를 기반으로 한 전역 보안 설정 클래스.
JWT 인증 방식을 사용하며, 세션을 생성하지 않는 Stateless 구조로 설정되어 있다.
[JwtFilter](#jwtfilter)를 통해 요청 헤더의 토큰을 검증하고, [CustomAccessDeniedHandler](#customaccessdeniedhandler) 및 [CustomAuthenticationEntryPoint](#customauthenticationentrypoint)를 통해
권한 및 인증 예외 상황을 처리한다.

##### Attributes
| Name                           | Type                           | Visibility    | Description                            |
| ------------------------------ | ------------------------------ | ------------- | -------------------------------------- |
| jwtFilter                      | JwtFilter                      | private final | JWT 토큰을 검증하고 인증 정보를 설정하는 커스텀 필터        |
| authenticationConfiguration    | AuthenticationConfiguration    | private final | Spring Security의 인증 관리 설정 객체           |
| customAccessDeniedHandler      | CustomAccessDeniedHandler      | private final | 인가(Authorization) 실패 시 실행되는 예외 처리 핸들러  |
| customAuthenticationEntryPoint | CustomAuthenticationEntryPoint | private final | 인증(Authentication) 실패 시 실행되는 예외 처리 핸들러 |


##### Operations
| Name                             | Return Type         | Visibility | Description                                                      |
| -------------------------------- |---------------------| ---------- | ---------------------------------------------------------------- |
| `passwordEncoder()`              | PasswordEncoder     | public     | `BCryptPasswordEncoder`를 Bean으로 등록하여 비밀번호 암호화 기능 제공              |
| `authenticationManager()`        | AuthenticationManager | public     | Spring Security의 인증 매니저 Bean을 생성 및 주입                            |
| `filterChain(HttpSecurity http)` | SecurityFilterChain | public     | 보안 필터 체인(Security Filter Chain) 설정 및 구성 반환<br>JWT 기반 인증/인가 흐름 정의 |


#### WebConfig
Spring MVC 환경에서 전역 웹 설정(Web Configuration) 을 정의하는 클래스.
`StringToEnumConverterFactory`를 등록하여 HTTP 요청 파라미터로 전달된 문자열을 Enum 타입으로 자동 변환하도록 설정한다.

##### Attributes
| Name     | Type | Visibility | Description              |
| -------- | ---- | ---------- | ------------------------ |

##### Operations
| Name                                        | Return Type | Visibility | Description                                                                                      |
| ------------------------------------------- | --------- | ---------- | ------------------------------------------------------------------------------------------------ |
| `addFormatters(FormatterRegistry registry)` | void      | public     | Spring MVC의 `FormatterRegistry`에 `StringToEnumConverterFactory`를 등록하여 문자열 → Enum 자동 변환 기능을 추가한다. |

---

## 3.6 보안 클래스 (Security Classes)

시스템의 인증 및 인가를 담당하는 보안 관련 클래스들의 구조를 보여준다.
JWT 기반 토큰 관리를 위한 JwtUtil, 요청 검증을 위한 JwtFilter, Spring Security와의 통합을 위한 CustomUserDetails 및 CustomUserDetailsService, 그리고 인증 실패 및 접근 거부 상황을 처리하는 CustomAuthenticationEntryPoint와 CustomAccessDeniedHandler 등의 보안 클래스들의 구조와 상호작용을 표현한다.


#### JwtUtil
JWT(JSON Web Token)를 생성하고, 검증 및 파싱하는 유틸리티 클래스.
AccessToken과 RefreshToken을 각각 생성하며, 토큰 내 사용자 정보(`uid`, `username`, `nickname`, `role`)를 Claims에 저장한다.
서명은 `HMAC-SHA` 알고리즘 기반 `SecretKey`를 사용하여 보안 무결성을 보장한다.

##### Attributes
| Name           | Type      | Visibility           | Description                       |
| -------------- | --------- | -------------------- | --------------------------------- |
| CLAIM_UID      | String    | private static final | 사용자 ID Claim 키                    |
| CLAIM_USERNAME | String    | private static final | 사용자명 Claim 키                      |
| CLAIM_NICKNAME | String    | private static final | 닉네임 Claim 키                       |
| CLAIM_ROLE     | String    | private static final | 권한 Claim 키                        |
| CLAIM_TYPE     | String    | private static final | 토큰 종류 Claim 키                     |
| TYPE_ACCESS    | String    | private static final | `"access"` (AccessToken 타입 식별자)   |
| TYPE_REFRESH   | String    | private static final | `"refresh"` (RefreshToken 타입 식별자) |
| secretKey      | SecretKey | private final        | JWT 서명 검증용 시크릿 키 (`HMAC-SHA`)     |
| accessExp      | long      | private final        | AccessToken 만료 시간 (ms 단위)         |
| refreshExp     | long      | private final        | RefreshToken 만료 시간 (ms 단위)        |

##### Operations
| Name                                                                            | Return Type | Visibility | Description                                               |
| ------------------------------------------------------------------------------- | ----------- | ---------- | --------------------------------------------------------- |
| `createAccessToken(Long userId, String username, String nickname, String role)` | String      | public     | 사용자 정보(UID, username, nickname, role)를 포함한 AccessToken 생성 |
| `createRefreshToken(Long userId)`                                               | String      | public     | 사용자 ID만 포함된 RefreshToken 생성                               |
| `parseToken(String token)`                                                      | Claims      | public     | 서명 검증 및 Claims 추출. 만료·위조 시 `JwtException` 발생              |
| `validateToken(String token)`                                                   | boolean     | public     | 파싱 성공 여부로 유효성 검증                                          |
| `isAccessToken(String token)`                                                   | boolean     | public     | Claims의 `"token_type"`이 `"access"`인지 확인                   |
| `isRefreshToken(String token)`                                                  | boolean     | public     | Claims의 `"token_type"`이 `"refresh"`인지 확인                  |
| `getUserId(String token)`                                                       | Long        | public     | Claims에서 `uid` 추출                                         |
| `getUsername(String token)`                                                     | String      | public     | Claims에서 `username` 추출                                    |
| `getNickname(String token)`                                                     | String      | public     | Claims에서 `nickname` 추출                                    |
| `isTokenExpired(String token)`                                                  | boolean     | public     | 만료일이 현재 시각 이전인지 여부 반환                                     |
| `getTimeToExpiration(String token)`                                             | long        | public     | 토큰 만료까지 남은 시간(ms) 반환                                      |
| `getAccessExpMills()`                                                           | long        | public     | AccessToken 만료 기간(ms) 반환                                  |
| `getRefreshExpMills()`                                                          | long        | public     | RefreshToken 만료 기간(ms) 반환                                 |

#### JwtFilter
모든 HTTP 요청마다 한 번씩 실행되는 JWT 인증 필터.
`Authorization` 헤더의 Access Token을 검증하고, 유효한 경우 `SecurityContext`에 인증 정보를 설정한다.
화이트리스트(`security.whitelist`) 경로 및 preflight(OPTIONS) 요청은 필터링에서 제외된다.

##### Attributes
| Name         | Type           | Visibility           | Description                                              |
| ------------ | -------------- | -------------------- | -------------------------------------------------------- |
| whiteList    | String[]       | private              | 인증 제외 경로 목록 (application.yml의 `security.whitelist`에서 주입) |
| jwtUtil      | JwtUtil        | private final        | JWT 생성, 파싱, 검증 유틸리티 클래스                                  |
| PATH_MATCHER | AntPathMatcher | private static final | URI 패턴 매칭 유틸리티 (`/api/**` 등 지원)                          |

##### Operations
| Name                                                                   | Return Type | Visibility | Description                                                                            |
| ---------------------------------------------------------------------- | ----------- | ---------- | -------------------------------------------------------------------------------------- |
| `doFilterInternal(HttpServletRequest, HttpServletResponse, FilterChain)` | void        | protected  | 요청 단위로 JWT 토큰을 검사하고, 유효 시 `SecurityContext`에 인증 정보를 설정                                 |
| `isWhiteListed(String uri)`                                              | boolean     | private    | 요청 URI가 화이트리스트 패턴에 해당하는지 검사                                                            |
| `extractAccessToken(HttpServletRequest request)`                         | String      | private    | `Authorization: Bearer ...` 헤더에서 Access Token 추출                                       |
| `setAuthenticationFromClaims(Claims claims)`                             | void        | private    | JWT Claims를 기반으로 `CustomUserDetails` 및 `Authentication` 객체를 생성 후 `SecurityContext`에 설정 |

#### CustomUserDetails
Spring Security에서 인증된 사용자 정보를 저장하는 커스텀 구현체.
JWT에서 추출한 사용자 정보 또는 `User` 엔티티 기반으로 생성되며, `SecurityContextHolder`를 통해 전역적으로 참조된다.
비밀번호는 인증 이후 보안상 이유로 `eraseCredentials()`에 의해 제거된다.

##### Attributes
| Name     | Type   | Visibility    | Description                          |
| -------- | ------ | ------------- | ------------------------------------ |
| userId   | Long   | private final | 사용자 고유 ID                            |
| username | String | private final | 로그인 ID (또는 이메일 등)                    |
| password | String | private       | 로그인 시 비밀번호 (JWT 기반 인증 시 null)        |
| nickname | String | private final | 사용자 닉네임                              |
| role     | String | private final | 사용자 권한 (`ROLE_USER`, `ROLE_ADMIN` 등) |

##### Operations
| Name                        | Return Type                            | Visibility | Description                                  |
| --------------------------- | -------------------------------------- | ---------- | -------------------------------------------- |
| `getAuthorities()`          | Collection\<? extends GrantedAuthority> | public     | 사용자의 권한 목록 반환 (`SimpleGrantedAuthority`로 래핑) |
| `getPassword()`             | String                                 | public     | 사용자 비밀번호 반환 (JWT 인증 시 null)                  |
| `getUsername()`             | String                                 | public     | 로그인 ID 반환                                    |
| `isAccountNonExpired()`     | boolean                                | public     | 계정 만료 여부 (`true` → 항상 활성)                    |
| `isAccountNonLocked()`      | boolean                                | public     | 계정 잠금 여부 (`true` → 항상 활성)                    |
| `isCredentialsNonExpired()` | boolean                                | public     | 비밀번호 만료 여부 (`true` → 항상 활성)                  |
| `isEnabled()`               | boolean                                | public     | 계정 활성 여부 (`true` → 항상 활성)                    |
| `eraseCredentials()`        | void                                   | public     | 인증 완료 후 비밀번호를 메모리에서 제거 (보안 목적)               |



#### CustomUserDetailsService
Spring Security 인증 과정에서 사용자 이름(username)을 기반으로 DB에 저장된 [User](#user) 엔티티를 조회하고, 이를 [CustomUserDetails](#customuserdetails) 객체로 변환하여 반환한다.

##### Attributes
| Name        | Type        | Visibility    | Description             |
| ----------- | ----------- | ------------- | ----------------------- |
| userService | UserService | private final | 사용자 조회 로직을 담당하는 도메인 서비스 |

##### Operations
| Name                                | Return Type | Visibility | Description                                                                                      |
| ----------------------------------- | ----------- | ---------- | ------------------------------------------------------------------------------------------------ |
| `loadUserByUsername(String username)` | UserDetails | public     | 사용자 이름으로 `User` 엔티티를 조회하고, `CustomUserDetails`로 변환하여 반환. 존재하지 않으면 `UsernameNotFoundException` 발생 |


#### CustomAccessDeniedHandler
Spring Security 인가 과정에서 권한이 없거나(403), 인증되지 않은(401) 사용자의 접근을 감지하고, 표준화된 JSON 응답(`ApiResponse`) 형태로 클라이언트에 반환하는 핸들러.
기본 HTML 오류 페이지 대신 JSON API 규격에 맞는 응답을 제공한다.

##### Attributes
| Name         | Type         | Visibility    | Description                                                    |
| ------------ | ------------ | ------------- | -------------------------------------------------------------- |
| objectMapper | ObjectMapper | private final | JSON 직렬화를 위한 Jackson 객체. 응답 객체(`ApiResponse`)를 JSON 문자열로 변환한다. |

##### Operations
| Name                                                                                       | Return Type | Visibility | Description                                                        |
| ------------------------------------------------------------------------------------------ | ----------- | ---------- | ------------------------------------------------------------------ |
| `handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException ex)` | void        | public     | 인가 실패 발생 시 호출되어, 요청 상태(익명/권한 부족)에 따라 401 또는 403 응답을 JSON 형태로 반환한다. |


#### CustomAuthenticationEntryPoint
Spring Security에서 인증(Authentication)되지 않은 사용자가 보호된 리소스에 접근할 때 `401 Unauthorized` 응답을 반환하는 커스텀 엔트리 포인트.
[AccessDeniedHandler](#customaccessdeniedhandler)가 인가(`Authorization`) 실패를 담당한다면, `AuthenticationEntryPoint`는 인증 자체가 되지 않은 상태의 접근을 처리한다.

##### Attributes
| Name         | Type         | Visibility    | Description                                  |
| ------------ | ------------ | ------------- | -------------------------------------------- |
| objectMapper | ObjectMapper | private final | `ApiResponse` 객체를 JSON 문자열로 직렬화하는 Jackson 객체 |

##### Operations
| Name                                                                                                      | Return Type | Visibility | Description                                    |
| --------------------------------------------------------------------------------------------------------- | ----------- | ---------- | ---------------------------------------------- |
| `commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException)` | void        | public     | 인증되지 않은 사용자가 접근 시 401 상태 코드와 JSON 에러 응답을 반환한다. |

---

## 3.7 공통 유틸리티 (Common Utilities)

시스템 전반에서 공통으로 사용되는 유틸리티 클래스들의 구조를 보여준다.
한국어 형태소 분석을 위한 KomoranUtil, String과 Enum 간의 타입 변환을 위한 StringToEnumConverterFactory, 페이지네이션 요청의 유효성을 검증하는 PageableValidator 등 다양한 기능을 지원하는 유틸리티 클래스들의 구조와 사용 방식을 표현한다.


#### ErrorCode
애플리케이션 전역에서 사용되는 표준 에러 코드 정의 Enum.
각 예외 상황에 대한 HTTP 상태 코드(HttpStatus), 에러 식별 코드(String), 사용자 메시지(String) 를 포함한다.
모든 예외([BusinessException](#businessexception), [GlobalExceptionHandler](#globalexceptionhandler))는 이 Enum을 기반으로 일관된 에러 응답을 생성한다.

##### Enum Values
| 구분                                         | Enum Name | HttpStatus                              | Code                                       | Message |
| ------------------------------------------ | --------- | --------------------------------------- | ------------------------------------------ | ------- |
| **`400 BAD_REQUEST (잘못된 요청)`**             |           |                                         |                                            |         |
| `INVALID_REQUEST`                          | 400       | INVALID_REQUEST                         | 잘못된 요청입니다.                                 |         |
| `VALIDATION_FAILED`                        | 400       | VALIDATION_FAILED                       | 요청 값이 유효하지 않습니다.                           |         |
| `REQUIRED_FIELD_MISSING`                   | 400       | REQUIRED_FIELD_MISSING                  | 필수 값이 누락되었습니다.                             |         |
| `INVALID_EMAIL_FORMAT`                     | 400       | INVALID_EMAIL_FORMAT                    | 이메일 형식이 올바르지 않습니다.                         |         |
| `INVALID_NICKNAME_FORMAT`                  | 400       | INVALID_NICKNAME_FORMAT                 | 닉네임 형식이 올바르지 않습니다.                         |         |
| `INVALID_PASSWORD_FORMAT`                  | 400       | INVALID_PASSWORD_FORMAT                 | 비밀번호 형식이 올바르지 않습니다.                        |         |
| `MISMATCHED_PASSWORD`                      | 400       | MISMATCHED_PASSWORD                     | 비밀번호와 비밀번호 확인이 일치하지 않습니다.                  |         |
| `PASSWORD_SAME_AS_OLD`                     | 400       | PASSWORD_SAME_AS_OLD                    | 기존 비밀번호와 새로운 비밀번호가 일치합니다.                  |         |
| `OLD_PASSWORD_INCORRECT`                   | 400       | OLD_PASSWORD_INCORRECT                  | 기존 비밀번호가 일치하지 않습니다.                        |         |
| `UNSUPPORTED_UNIVERSITY_DOMAIN`            | 400       | UNSUPPORTED_UNIVERSITY_DOMAIN           | 지원하지 않는 학교 도메인입니다.                         |         |
| `VERIFICATION_CODE_EXPIRED`                | 400       | VERIFICATION_CODE_EXPIRED               | 인증번호가 만료되었습니다.                             |         |
| `INVALID_VERIFICATION_CODE`                | 400       | INVALID_VERIFICATION_CODE               | 인증번호가 일치하지 않습니다.                           |         |
| `INVALID_DATE_RANGE`                       | 400       | INVALID_DATE_RANGE                      | 유효하지 않은 날짜 범위입니다.                          |         |
| `MAX_PARTICIPANTS_EXCEEDED`                | 400       | MAX_PARTICIPANTS_EXCEEDED               | 참가 인원이 최대 모집 인원을 초과했습니다.                   |         |
| `SELF_REVIEW_NOT_ALLOWED`                  | 400       | SELF_REVIEW_NOT_ALLOWED                 | 자기 자신에 대한 리뷰는 작성할 수 없습니다.                  |         |
| `INVALID_PAGE_NUMBER`                      | 400       | INVALID_PAGE_NUMBER                     | 페이지 번호는 1 이상이어야 합니다.                       |         |
| `INVALID_ENUM_VALUE`                       | 400       | INVALID_ENUM_VALUE                      | 쿼리 파라미터 값이 유효하지 않습니다. 허용 가능한 값 목록을 확인해주세요. |         |
| `MISMATCHED_RECRUITMENT_CATEGORY`          | 400       | MISMATCHED_RECRUITMENT_CATEGORY         | 지원하려는 공고의 카테고리가 요청한 카테고리와 일치하지 않습니다.       |         |
| `INVALID_SORT_PROPERTY`                    | 400       | INVALID_SORT_PROPERTY                   | 잘못된 정렬 기준입니다.                              |         |
| `INVALID_SORT_DIRECTION`                   | 400       | INVALID_SORT_DIRECTION                  | 정렬 방향은 asc 또는 desc만 가능합니다.                 |         |
| `PAGE_SIZE_OUT_OF_RANGE`                   | 400       | PAGE_SIZE_OUT_OF_RANGE                  | 페이지 크기는 1 이상이어야 합니다.                       |         |
| `PAGE_INDEX_OUT_OF_RANGE`                  | 400       | PAGE_INDEX_OUT_OF_RANGE                 | 페이지 번호는 0 이상이어야 합니다.                       |         |
| `INVALID_MEMBER_COUNT`                     | 400       | INVALID_MEMBER_COUNT                    | 현재 인원이 0명 이하일 때는 감소할 수 없습니다.               |         |
| **`401 UNAUTHORIZED (인증 실패)`**             |           |                                         |                                            |         |
| `UNAUTHORIZED`                             | 401       | UNAUTHORIZED                            | 인증이 필요합니다.                                 |         |
| `INVALID_CREDENTIALS`                      | 401       | INVALID_CREDENTIALS                     | 아이디 또는 비밀번호가 올바르지 않습니다.                    |         |
| `REFRESH_TOKEN_EXPIRED`                    | 401       | REFRESH_TOKEN_EXPIRED                   | 리프레시 토큰이 만료되었습니다.                          |         |
| `REFRESH_TOKEN_INVALID`                    | 401       | REFRESH_TOKEN_INVALID                   | 유효하지 않은 리프레시 토큰입니다.                        |         |
| `REFRESH_TOKEN_TYPE_INVALID`               | 401       | REFRESH_TOKEN_TYPE_INVALID              | 리프레시 토큰 타입이 일치하지 않습니다.                     |         |
| `REFRESH_TOKEN_MISMATCH`                   | 401       | REFRESH_TOKEN_MISMATCH                  | 리프레시 토큰이 일치하지 않습니다.                        |         |
| `REFRESH_TOKEN_NOT_FOUND`                  | 401       | REFRESH_TOKEN_NOT_FOUND                 | 리프레시 토큰을 찾을 수 없습니다.                        |         |
| `ACCESS_TOKEN_INVALID`                     | 401       | ACCESS_TOKEN_INVALID                    | 유효하지 않은 액세스 토큰입니다.                         |         |
| **`403 FORBIDDEN (권한 없음)`**                |           |                                         |                                            |         |
| `FORBIDDEN`                                | 403       | FORBIDDEN                               | 접근 권한이 없습니다.                               |         |
| `ACCESS_DENIED`                            | 403       | ACCESS_DENIED                           | 요청한 리소스에 접근할 수 없습니다.                       |         |
| `NOT_UPDATE_NOT_ACTIVE_REVIEW`             | 403       | NOT_UPDATE_NOT_ACTIVE_REVIEW            | 비활성화된 리뷰는 수정할 수 없습니다.                      |         |
| `NOT_DELETE_NOT_ACTIVE_REVIEW`             | 403       | NOT_DELETE_NOT_ACTIVE_REVIEW            | 비활성화된 리뷰는 삭제할 수 없습니다.                      |         |
| `NOT_UPDATE_ANOTHER_USER_REVIEW`           | 403       | NOT_UPDATE_ANOTHER_USER_REVIEW          | 본인 리뷰만 수정할 수 있습니다.                         |         |
| `NOT_DELETE_ANOTHER_USER_REVIEW`           | 403       | NOT_DELETE_ANOTHER_USER_REVIEW          | 본인 리뷰만 삭제할 수 있습니다.                         |         |
| `CANNOT_DELETE_ANOTHER_USER_BOOKMARK`      | 403       | CANNOT_DELETE_ANOTHER_USER_BOOKMARK     | 다른 사용자의 찜은 취소할 수 없습니다.                     |         |
| `CANNOT_DELETE_ANOTHER_USER_ASSIGNMENT`    | 403       | CANNOT_DELETE_ANOTHER_USER_ASSIGNMENT   | 다른 사용자의 과제는 삭제할 수 없습니다.                    |         |
| `CANNOT_APPLY_OWN_RECRUITMENT`             | 403       | CANNOT_APPLY_OWN_RECRUITMENT            | 본인 공고에는 지원할 수 없습니다.                        |         |
| `FORBIDDEN_CROSS_UNIVERSITY_RECRUITMENT`   | 403       | FORBIDDEN_CROSS_UNIVERSITY_RECRUITMENT  | 타 대학의 공고입니다.                               |         |
| `FORBIDDEN_DECIDE_APPLICATION`             | 403       | FORBIDDEN_DECIDE_APPLICATION            | 지원 수락/거절 권한이 없습니다.                         |         |
| `CANNOT_DELETE_ANOTHER_USER_APPLICATION`   | 403       | CANNOT_DELETE_ANOTHER_USER_APPLICATION  | 다른 사용자의 지원은 취소할 수 없습니다.                    |         |
| `CANNOT_REMOVE_NOT_LEADER`                 | 403       | CANNOT_REMOVE_NOT_LEADER                | 리더만 멤버를 삭제할 수 있습니다.                        |         |
| `CANNOT_REMOVE_SELF`                       | 403       | CANNOT_REMOVE_SELF                      | 자기 자신은 삭제할 수 없습니다.                         |         |
| `CANNOT_DELETE_ANOTHER_USER_NOTIFICATION`  | 403       | CANNOT_DELETE_ANOTHER_USER_NOTIFICATION | 다른 사용자의 알림은 삭제할 수 없습니다.                    |         |
| **`404 NOT_FOUND (리소스 없음)`**               |           |                                         |                                            |         |
| `USER_NOT_FOUND`                           | 404       | USER_NOT_FOUND                          | 사용자를 찾을 수 없습니다.                            |         |
| `IMAGE_NOT_FOUND`                          | 404       | IMAGE_NOT_FOUND                         | 파일을 찾을 수 없습니다.                             |         |
| `RECRUITMENT_NOT_FOUND`                    | 404       | RECRUITMENT_NOT_FOUND                   | 공고를 찾을 수 없습니다.                             |         |
| `PROJECT_NOT_FOUND`                        | 404       | PROJECT_NOT_FOUND                       | 프로젝트 공고를 찾을 수 없습니다.                        |         |
| `ASSIGNMENT_NOT_FOUND`                     | 404       | ASSIGNMENT_NOT_FOUND                    | 과제 공고를 찾을 수 없습니다.                          |         |
| `STUDY_NOT_FOUND`                          | 404       | STUDY_NOT_FOUND                         | 스터디 공고를 찾을 수 없습니다.                         |         |
| `POSITION_NOT_FOUND`                       | 404       | POSITION_NOT_FOUND                      | 역할을 찾을 수 없습니다.                             |         |
| `SKILL_NOT_FOUND`                          | 404       | SKILL_NOT_FOUND                         | 기술 스택을 찾을 수 없습니다.                          |         |
| `REVIEW_NOT_FOUND`                         | 404       | REVIEW_NOT_FOUND                        | 리뷰를 찾을 수 없습니다.                             |         |
| `BOOKMARK_NOT_FOUND`                       | 404       | BOOKMARK_NOT_FOUND                      | 찜 정보를 찾을 수 없습니다.                           |         |
| `APPLICATION_NOT_FOUND`                    | 404       | APPLICATION_NOT_FOUND                   | 지원 정보를 찾을 수 없습니다.                          |         |
| `TEAM_NOT_FOUND`                           | 404       | TEAM_NOT_FOUND                          | 팀을 찾을 수 없습니다.                              |         |
| `LEADER_NOT_FOUND`                         | 404       | LEADER_NOT_FOUND                        | 리더를 찾을 수 없습니다.                             |         |
| `TARGET_MEMBER_NOT_FOUND`                  | 404       | TARGET_MEMBER_NOT_FOUND                 | 삭제할 멤버를 찾을 수 없습니다.                         |         |
| `NOTIFICATION_NOT_FOUND`                   | 404       | NOTIFICATION_NOT_FOUND                  | 알림을 찾을 수 없습니다.                             |         |
| **`406 NOT_ACCEPTABLE`**                   |           |                                         |                                            |         |
| `NOT_ACCEPTABLE`                           | 406       | NOT_ACCEPTABLE                          | 응답 가능한 미디어 타입이 없습니다.                       |         |
| **`409 CONFLICT (중복 또는 상태 충돌)`**           |           |                                         |                                            |         |
| `DUPLICATED_USERNAME`                      | 409       | DUPLICATED_USERNAME                     | 이미 가입된 아이디입니다.                             |         |
| `DUPLICATED_EMAIL`                         | 409       | DUPLICATED_EMAIL                        | 이미 가입된 이메일입니다.                             |         |
| `DUPLICATED_NICKNAME`                      | 409       | DUPLICATED_NICKNAME                     | 이미 존재하는 닉네임입니다.                            |         |
| `ALREADY_WITHDRAWN_USER`                   | 409       | ALREADY_WITHDRAWN_USER                  | 이미 탈퇴한 회원입니다.                              |         |
| `ALREADY_BOOKMARKED`                       | 409       | ALREADY_BOOKMARKED                      | 이미 찜한 공고입니다.                               |         |
| `ALREADY_APPLIED_RECRUITMENT`              | 409       | ALREADY_APPLIED_RECRUITMENT             | 이미 지원한 공고입니다.                              |         |
| `RECRUITMENT_CLOSED`                       | 409       | RECRUITMENT_CLOSED                      | 모집 기간이 종료된 공고입니다.                          |         |
| `RECRUITMENT_FULL`                         | 409       | RECRUITMENT_FULL                        | 이미 모집이 완료된 공고입니다.                          |         |
| `NOT_RECRUITING_POSITION`                  | 409       | NOT_RECRUITING_POSITION                 | 해당 포지션은 모집 대상이 아닙니다.                       |         |
| `POSITION_FULL`                            | 409       | POSITION_FULL                           | 이미 모집이 완료된 포지션입니다.                         |         |
| `ALREADY_PROCESSED_APPLICATION`            | 409       | ALREADY_PROCESSED_APPLICATION           | 이미 처리된 지원서입니다.                             |         |
| `TEAM_FULL`                                | 409       | TEAM_FULL                               | 이미 모집이 완료되었습니다.                            |         |
| **`413 PAYLOAD_TOO_LARGE`**                |           |                                         |                                            |         |
| `PAYLOAD_TOO_LARGE`                        | 413       | PAYLOAD_TOO_LARGE                       | 요청 또는 파일 크기가 너무 큽니다.                       |         |
| **`415 UNSUPPORTED_MEDIA_TYPE`**           |           |                                         |                                            |         |
| `UNSUPPORTED_MEDIA_TYPE`                   | 415       | UNSUPPORTED_MEDIA_TYPE                  | 지원하지 않는 Content-Type 입니다.                  |         |
| **`429 TOO_MANY_REQUESTS`**                |           |                                         |                                            |         |
| `TOO_MANY_REQUESTS`                        | 429       | TOO_MANY_REQUESTS                       | 요청이 너무 많습니다. 잠시 후 다시 시도해주세요.               |         |
| **`500 INTERNAL_SERVER_ERROR (서버 내부 오류)`** |           |                                         |                                            |         |
| `INTERNAL_SERVER_ERROR`                    | 500       | INTERNAL_ERROR                          | 서버 오류가 발생했습니다.                             |         |
| `DATABASE_ERROR`                           | 500       | DATABASE_ERROR                          | 데이터베이스 오류가 발생했습니다.                         |         |
| `REDIS_CONNECTION_ERROR`                   | 500       | REDIS_CONNECTION_ERROR                  | Redis 연결에 실패했습니다.                          |         |
| `EMAIL_CREATE_MESSAGE_FAILED`              | 500       | EMAIL_CREATE_MESSAGE_FAILED             | 이메일 메시지를 생성하는데 실패했습니다.                     |         |
| `EMAIL_SEND_FAILED`                        | 500       | EMAIL_SEND_FAILED                       | 이메일 발송에 실패했습니다.                            |         |

##### Attributes
| Name    | Type   | Visibility    | Description                                            |
| ------- | ------ | ------------- | ------------------------------------------------------ |
| httpStatus | HttpStatus | private final | 예외에 해당하는 HTTP 응답 상태 코드 (예: `BAD_REQUEST`, `FORBIDDEN`) |
| code    | String | private final | 클라이언트 및 로그 식별용 에러 코드 문자열 (예: `"USER_NOT_FOUND"`)       |
| message | String | private final | 클라이언트에게 전달할 한글 에러 메시지                                  |

##### Operations
| Name              | Return Type | Visibility | Description       |
| ----------------- | ------ | ---------- | ----------------- |
| `getHttpStatus()` | HttpStatus | public     | HTTP 상태 코드를 반환한다. |
| `getCode()`       | String | public     | 에러 코드 문자열을 반환한다.  |
| `getMessage()`    | String | public     | 에러 메시지를 반환한다.     |


#### BusinessException
도메인 및 서비스 계층에서 발생하는 비즈니스 로직 예외의 표준 클래스.
모든 커스텀 예외는 이 클래스를 상속하거나 이 클래스를 직접 발생시켜야 한다.
[ErrorCode](#errorcode)와 메시지를 함께 전달하여 일관된 예외 처리 및 응답 구조를 보장한다.

##### Attributes
| Name      | Type      | Visibility    | Description                |
| --------- | --------- | ------------- | -------------------------- |
| errorCode | ErrorCode | private final | 예외의 원인을 나타내는 표준 에러 코드 Enum |

##### Operations
| Name             | Return Type | Visibility | Description                    |
| ---------------- | --------- | ---------- | ------------------------------ |
| `getErrorCode()` | ErrorCode | public     | 예외에 해당하는 `ErrorCode` 객체를 반환한다. |


#### ApiResponse\<T>
모든 REST API 응답의 표준 응답 포맷을 정의하는 제네릭 클래스.
성공(`ok`)과 실패(`error`) 응답을 구분하며, `code`, `message`, `data` 세 가지 필드를 통해 일관된 응답 구조를 제공한다.

##### Attributes
| Name  | Type  | Visibility    | Description                                   |
| ----- |-------| ------------- | --------------------------------------------- |
| code  | String | private final | 응답 상태 코드 (예: `"SUCCESS"`, `"USER_NOT_FOUND"`) |
| message | String | private final | 사용자에게 전달되는 응답 메시지                             |
| data  | T     | private final | 응답 본문 데이터 (null일 수도 있음)                       |

##### Operations
| Name                                      | Return Type    | Visibility    | Description                          |
| ----------------------------------------- | -------------- | ------------- | ------------------------------------ |
| `of(String code, String message, T data)` | ApiResponse\<T> | public static | 정적 팩토리 메서드로 새로운 응답 객체 생성             |
| `ok()`                                    | ApiResponse\<T> | public static | 데이터 없이 기본 성공 응답 ("SUCCESS", "성공") 반환 |
| `ok(String message)`                      | ApiResponse\<T> | public static | 커스텀 메시지를 포함한 성공 응답 반환                |
| `ok(T data)`                              | ApiResponse\<T> | public static | 데이터 포함 기본 성공 응답 반환                   |
| `ok(String message, T data)`              | ApiResponse\<T> | public static | 메시지와 데이터 모두 포함한 성공 응답 반환             |
| `error(ErrorCode errorCode)`              | ApiResponse\<T> | public static | 지정된 에러 코드 기반 실패 응답 반환                |
| `error(String code, String message)`      | ApiResponse\<T> | public static | 커스텀 코드와 메시지를 포함한 실패 응답 반환            |
| `error(ErrorCode errorCode, T data)`      | ApiResponse\<T> | public static | 실패 응답과 함께 추가 데이터 포함                  |


#### GlobalExceptionHandler
애플리케이션 전역에서 발생하는 예외를 처리하는 전역 예외 처리 클래스 (Global Exception Handler).
각종 예외([BusinessException](#businessexception), `Validation`, `TypeMismatch` 등)를 잡아 일관된 [ApiResponse](#apiresponset) 형식으로 클라이언트에게 응답한다.
Spring MVC의 `@RestControllerAdvice`를 통해 모든 컨트롤러에 전역 적용된다.

##### Attributes
| Name     | Type | Visibility | Description               |
| -------- | ---- | ---------- | ------------------------- |

##### Operations
| Name                                                                                        | Return Type                                    | Visibility | Description                                                                                                                |
| ------------------------------------------------------------------------------------------- |------------------------------------------------| ---------- | -------------------------------------------------------------------------------------------------------------------------- |
| `handleBusinessException(BusinessException ex)`                                             | ResponseEntity\<ApiResponse\<Void>>              | public     | 비즈니스 로직(`Service`, `Domain`)에서 발생한 `BusinessException`을 처리한다.<br>응답에는 `ErrorCode`와 예외 메시지가 포함된다.                           |
| `handleMethodArgumentNotValidException(MethodArgumentNotValidException ex)`                 | ResponseEntity\<ApiResponse\<List\<ValidationError>>> | public     | DTO 검증 실패(`@Valid`, `@Validated`) 시 발생하는 `MethodArgumentNotValidException`을 처리하고<br>모든 필드 에러를 `ValidationError` 리스트로 반환한다. |
| `handleMissingServletRequestParameterException(MissingServletRequestParameterException ex)` | ResponseEntity\<ApiResponse\<String>>            | public     | 필수 `@RequestParam`이 누락된 경우 발생하는 예외를 처리한다.<br>누락된 파라미터 이름과 함께 `REQUIRED_FIELD_MISSING` 에러코드 반환.                             |
| `handleMethodArgumentTypeMismatchException(MethodArgumentTypeMismatchException ex)`         | ResponseEntity\<ApiResponse\<String>>            | public     | 요청 파라미터의 타입이 예상과 다를 때 발생하는 예외 처리.<br>특히 Enum 타입에 잘못된 값이 전달될 때(`INVALID_ENUM_VALUE`) 메시지를 반환한다.                             |


#### ValidationError
유효성 검증(Validation) 실패 시 발생한 단일 필드 오류 정보를 표현하는 불변 데이터 객체.
각 필드의 이름(`field`)과 오류 메시지(`message`)를 한 쌍으로 관리한다.
`record`로 정의되어 불변성(immutability)과 간결한 코드 구조를 제공한다.

##### Attributes
| Name    | Type   | Visibility               | Description           |
| ------- | ------ |--------------------------| --------------------- |
| field   | String | private final (implicit) | 오류가 발생한 필드명 |
| message | String | private final (implicit) | 해당 필드에 대한 구체적인 오류 메시지 |

##### Operations
| Name      | Return Type | Visibility | Description |
| --------- | ------- | ------ | ----------- |
| `field()`   | String | public | 오류 필드명을 반환 |
| `message()` | String | public | 오류 메시지를 반환 |


#### PageableValidator
클라이언트의 잘못된 페이징 요청(너무 큰 페이지 번호, 음수, 비허용 필드 정렬 등)을 차단하여 서버 성능 저하 및 보안 이슈를 예방한다.

##### Attributes
| Name                 | Type                 | Visibility    | Description                                                 |
| -------------------- | -------------------- | ------------- | ----------------------------------------------------------- |
| paginationProperties | PaginationProperties | private final | 페이지 관련 설정값을 보유하는 설정 클래스 (`maxPageNumber`, `defaultSize`, 등) |

##### Operations
| Name                                                             | Return Type | Visibility | Description                                         |
| ---------------------------------------------------------------- | ----------- | ---------- | --------------------------------------------------- |
| `validate(Pageable pageable)`                                    | Pageable    | public     | `page`, `size` 값을 검증한 후 동일 객체를 반환                   |
| `validateSort(Pageable pageable, Set<String> allowedProperties)` | void        | public     | 요청된 정렬 필드가 허용된 필드 목록에 포함되는지 검증                      |
| `validatePageSize(int pageSize)`                                 | void        | private    | 페이지 크기(`size`)가 1 이상인지 검증                           |
| `validatePageNumber(int pageNumber)`                             | void        | private    | 페이지 번호(`page`)가 0 이상이며 `maxPageNumber`를 초과하지 않는지 검증 |
