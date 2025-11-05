## 3. Class diagram

# 3.1 엔티티

# User

와글와글 서비스에 가입한 사용자의 기본 정보를 관리하는 엔티티 클래스.
회원의 로그인 정보, 프로필, 기술 스택, 상태, 권한 등을 관리한다.

## Attributes

| Name       | Type          | Visibility | Description                       |
| ---------- | ------------- | ---------- | --------------------------------- |
| id         | Long          | private    | 사용자 식별자 (PK, 자동 증가)               |
| username   | String        | private    | 로그인용 사용자 ID (고유값)                 |
| password   | String        | private    | 비밀번호 (BCrypt 해시, 60자)             |
| email      | String        | private    | 학교 이메일 (인증 및 고유값)                 |
| university | University    | private    | 사용자의 대학교 정보                       |
| nickname   | String        | private    | 서비스 내 표시 이름 (고유값)                 |
| grade      | Integer       | private    | 학년 (1~4)                          |
| position   | PositionType  | private    | 희망 포지션 (예: BACKEND, FRONTEND)     |
| skills     | Set<Skill>    | private    | 보유 기술 스택 (복수 선택 가능)               |
| shortIntro | String        | private    | 짧은 자기소개 (Markdown 지원)             |
| role       | UserRoleType  | private    | 사용자 권한 (ROLE_USER / ROLE_ADMIN 등) |
| createdAt  | LocalDateTime | private    | 생성 시각 (Auditing 자동 기록)            |
| updatedAt  | LocalDateTime | private    | 수정 시각 (Auditing 자동 기록)            |
| status     | UserStatus    | private    | 사용자 상태 (ACTIVE, WITHDRAWN 등)      |

## Operations

| Name                                   | Return Type | Visibility | Description                      |
| -------------------------------------- | ----------- | ---------- | -------------------------------- |
| changePassword(String encodedPassword) | void        | public     | 사용자의 비밀번호를 암호화된 새 비밀번호로 변경       |
| updateNickname(String nickname)        | void        | public     | 사용자의 닉네임을 변경                     |
| updateGrade(Integer grade)             | void        | public     | 사용자의 학년 정보를 수정                   |
| updatePosition(PositionType position)  | void        | public     | 희망 포지션을 변경                       |
| updateSkills(Set<Skill> skills)        | void        | public     | 보유 기술 스택을 갱신                     |
| updateShortIntro(String shortIntro)    | void        | public     | 자기소개 문구를 수정                      |
| withdraw()                             | void        | public     | 사용자 상태를 `WITHDRAWN`으로 변경 (탈퇴 처리) |


# Team

각 공고(`BaseRecruitment`)와 1:1로 연결되어 팀 정보를 관리하는 엔티티.
팀은 여러 명의 팀원(`TeamMember`)을 포함하며, 팀 생성 및 삭제 시 팀원과의 연결관계를 일관되게 유지한다.

## Attributes
| Name        | Type             | Visibility | Description                                               |
| ----------- | ---------------- | ---------- | --------------------------------------------------------- |
| id          | Long             | private    | 팀 식별자 (PK, 자동 증가)                                         |
| recruitment | BaseRecruitment  | private    | 연결된 모집 공고 (1:1 관계, Not Null, Unique)                      |
| members     | List<TeamMember> | private    | 팀에 속한 팀원 목록 (1:N 관계, cascade = ALL, orphanRemoval = true) |
| createdAt   | LocalDateTime    | private    | 팀 생성 시각 (Auditing 자동 기록)                                  |
| updatedAt   | LocalDateTime    | private    | 마지막 수정 시각 (Auditing 자동 기록)                                |

## Operations

| Name                              | Return Type          | Visibility | Description                                      |
| --------------------------------- | -------------------- | ---------- | ------------------------------------------------ |
| Team(BaseRecruitment recruitment) | Constructor          | public     | 특정 공고에 속하는 팀을 생성                                 |
| addMember(TeamMember member)      | void                 | public     | 팀원 추가 및 양방향 연관관계 설정 (`member.setTeam(this)`)     |
| findMember(Long userId)           | Optional<TeamMember> | public     | 사용자 ID를 통해 특정 팀원을 조회                             |
| removeMember(TeamMember member)   | void                 | public     | 팀원 삭제 및 연관관계 해제. 팀원 미존재 시 `BusinessException` 발생 |


# TeamMember

팀(Team)에 소속된 개별 팀원 정보를 나타내는 엔티티.
팀과 사용자 간의 관계를 연결하며, 팀 내 역할 및 포지션 정보를 함께 관리한다.
Project 팀의 경우에만 포지션(position) 정보가 필수이며, Study나 Assignment 팀에서는 null이 허용된다.

## Attributes

| Name      | Type          | Visibility | Description                                    |
| --------- | ------------- | ---------- |------------------------------------------------|
| id        | Long          | private    | 팀 멤버 식별자 (PK, 자동 증가)                           |
| team      | Team          | private    | 소속 팀 (N:1 관계, 필수)                              |
| user      | User          | private    | 연결된 사용자 (N:1 관계, 필수)                           |
| role      | TeamRole      | private    | 팀 내 역할 (LEADER, MEMBER 등)                      |
| position  | PositionType  | private    | 팀 내 포지션 (Project 팀 한정, 예: BACK_END, FRONT_END) |
| createdAt | LocalDateTime | private    | 등록 일시 (Auditing 자동 관리)                         |
| updatedAt | LocalDateTime | private    | 수정 일시 (Auditing 자동 관리)                         |


## Operations

| Name                                            | Return Type | Visibility | Description                    |
| ----------------------------------------------- | ----------- | ---------- | ------------------------------ |
| TeamMember(Team team, User user, TeamRole role) | Constructor | public     | 팀, 사용자, 역할 정보를 기반으로 새로운 팀원 생성  |
| setTeam(Team team)                              | void        | public     | 소속 팀 정보를 변경 (양방향 연관관계 설정 시 사용) |


# Review

사용자 간 후기(리뷰) 정보를 저장하는 엔티티.
리뷰 작성자(`reviewer`)와 리뷰 대상자(`reviewee`) 간의 관계를 표현하며, 내용(`content`)과 상태(`status`)를 포함한다.
작성일(`createdAt`), 수정일(`updatedAt`)은 JPA Auditing으로 자동 관리된다.

## Attributes

| Name      | Type          | Visibility | Description             |
| --------- | ------------- | ---------- | ----------------------- |
| id        | Long          | private    | 리뷰 식별자 (PK, 자동 증가)      |
| reviewer  | User          | private    | 리뷰 작성자 (N:1 관계)         |
| reviewee  | User          | private    | 리뷰 대상자 (N:1 관계)         |
| content   | String        | private    | 리뷰 내용 (최대 100자)         |
| status    | ReviewStatus  | private    | 리뷰 상태 (ACTIVE, DELETED) |
| createdAt | LocalDateTime | private    | 작성일 (Auditing 자동 관리)    |
| updatedAt | LocalDateTime | private    | 수정일 (Auditing 자동 관리)    |


## Operations

| Name                                                 | Return Type | Visibility | Description                      |
| ---------------------------------------------------- | ----------- | ---------- | -------------------------------- |
| Review(User reviewer, User reviewee, String content) | Constructor | public     | 리뷰 작성자, 대상자, 내용을 기반으로 객체 생성      |
| update(ReviewUpdateRequestDto dto)                   | void        | public     | 리뷰 내용을 수정 (`dto.content()` 반영)   |
| delete()                                             | void        | public     | 리뷰 상태를 `DELETED`로 변경 (소프트 삭제 처리) |

# 3.2 VO

## User 관련

# University

와글와글 서비스에서 사용자의 소속 대학교 정보를 표현하는 열거형(Enum) 타입.
각 상수는 학교의 한글명(desc) 과 이메일 도메인(domain) 을 매핑하며, 이메일 주소를 기반으로 소속 대학교를 판별하는 기능을 제공한다.

## Attributes

| Name   | Type   | Visibility    | Description                |
| ------ | ------ | ------------- | -------------------------- |
| desc   | String | private final | 대학교 한글명                    |
| domain | String | private final | 대학교 이메일 도메인 (ex. yu.ac.kr) |

## Operations
| Name                    | Return Type | Visibility    | Description                                                            |
| ----------------------- | ----------- | ------------- | ---------------------------------------------------------------------- |
| getName()               | String      | public        | Enum 상수명 반환 (`name()` 호출)                                              |
| fromEmail(String email) | University  | public static | 이메일 주소의 도메인 부분을 분석하여 해당 대학교 Enum 반환. 일치하지 않을 경우 `BusinessException` 발생 |


# UserRoleType
와글와글 서비스 내 사용자 권한(Role)을 정의하는 열거형(Enum) 타입.
사용자의 접근 수준과 기능 권한을 구분하기 위해 사용된다.

## Attributes
## Operations


# UserStatus

사용자 계정의 활성 상태 및 탈퇴 여부를 나타내는 열거형(Enum).
회원 계정의 유효성, 탈퇴 처리(Soft Delete) 등의 상태를 관리한다.

## Attributes
## Operations

# EmailRequestDto

이메일 인증, 로그인, 회원가입 등에서 사용자의 이메일 입력값을 전달하기 위한 요청 DTO 클래스.
입력값이 유효한 이메일 형식인지 `@Email` 어노테이션으로 검증한다.

## Attributes
| Name  | Type   | Visibility                 | Description                         |
| ----- | ------ | -------------------------- | ----------------------------------- |
| email | String | private (record component) | 사용자 이메일 주소. `@Email` 제약으로 형식 검증 수행. |

## Operations


# EmailVerificationRequestDto
사용자가 입력한 이메일과 인증번호(6자리)를 서버로 전달하여
이메일 인증 유효성을 검증하기 위한 요청 DTO.

## Attributes
| Name      | Type   | Visibility                 | Description                                                         |
| --------- | ------ | -------------------------- | ------------------------------------------------------------------- |
| email     | String | private (record component) | 사용자의 이메일 주소. `@NotBlank`, `@Email` 제약으로 비어 있지 않으며 올바른 형식인지 검증.      |
| inputCode | String | private (record component) | 사용자가 입력한 인증번호. `@NotBlank`, `@Pattern("^[0-9]{6}")`으로 6자리 숫자 형식 검증. |

## Operations


# SignUpRequestDto
회원가입 요청을 처리하기 위한 DTO.
사용자 입력값을 검증한 뒤, 이를 기반으로 `User` 엔티티를 생성한다.
비밀번호 검증 및 암호화, 학년·포지션·기술스택 등의 도메인 속성을 포함한다.

## Attributes
| Name            | Type         | Visibility                 | Description                                |
| --------------- | ------------ | -------------------------- | ------------------------------------------ |
| username        | String       | private (record component) | 사용자 아이디. 영문, 숫자, 언더스코어 4~20자 (`@Pattern`). |
| password        | String       | private (record component) | 비밀번호. 영문, 숫자, 특수문자 포함 8~72자 (`@Pattern`).  |
| passwordConfirm | String       | private (record component) | 비밀번호 확인. password와 일치 여부 검증.               |
| nickname        | String       | private (record component) | 닉네임. 한글/영문/숫자 2~10자 (`@Pattern`).          |
| email           | String       | private (record component) | 이메일 주소. `@Email` 형식 검증.                    |
| grade           | Integer      | private (record component) | 학년 (1~4 범위, `@Min`, `@Max`).               |
| position        | PositionType | private (record component) | 포지션(enum). 예: BACKEND, FRONTEND 등.         |
| skills          | Set<Skill>   | private (record component) | 기술 스택 목록. 최대 10개 (`@Size(max=10)`).        |
| shortIntro      | String       | private (record component) | 한 줄 소개. 최대 100자 제한.                        |

## Operations
| Name                                      | Return Type | Visibility | Description                                   |
| ----------------------------------------- | ----------- | ---------- | --------------------------------------------- |
| toEntity(PasswordEncoder passwordEncoder) | User        | public     | 입력값을 기반으로 `User` 엔티티를 생성하고 비밀번호를 암호화하여 매핑.    |


# SignInRequestDto
사용자의 로그인 요청 시 전달되는 DTO.
입력받은 아이디(`username`)와 비밀번호(`password`)를 검증하여 인증 절차에 사용된다.

## Attributes
| Name     | Type   | Visibility                 | Description                 |
| -------- | ------ | -------------------------- | --------------------------- |
| username | String | private (record component) | 사용자 아이디. `@NotBlank` 제약 적용. |
| password | String | private (record component) | 비밀번호. `@NotBlank` 제약 적용.    |

## Operations


# TokenPair
JWT 기반 인증 시스템에서 Access Token과 Refresh Token을 함께 반환하기 위한 응답 DTO.
인증 성공 후 클라이언트가 인증 상태를 유지하도록 두 종류의 토큰을 제공한다.

## Attributes
| Name         | Type   | Visibility                 | Description                                 |
| ------------ | ------ | -------------------------- | ------------------------------------------- |
| accessToken  | String | private (record component) | 클라이언트의 인증 요청 시 사용되는 짧은 수명의 JWT 액세스 토큰.      |
| refreshToken | String | private (record component) | 액세스 토큰 만료 시 재발급을 위해 사용되는 장기 유효 JWT 리프레시 토큰. |

## Operations


# PasswordRequestDto

비밀번호 변경 요청 시 사용되는 DTO 클래스.
사용자가 입력한 기존 비밀번호, 새로운 비밀번호, 비밀번호 확인 값을 검증하여 서버로 전달한다.
요청 검증 실패 시 BusinessException이 발생한다.

## Attributes

| Name            | Type   | Visibility                 | Description                                               |
| --------------- | ------ | -------------------------- | --------------------------------------------------------- |
| old             | String | private (record component) | 기존 비밀번호. `@NotBlank`, `@Size(max=72)` 제약 적용.              |
| newPassword     | String | private (record component) | 새로운 비밀번호. `@NotBlank`, `@Pattern`(영문, 숫자, 특수문자 포함 8~72자). |
| passwordConfirm | String | private (record component) | 비밀번호 확인 입력값. `@NotBlank`.                                 |

## Operations

| Name                                                                       | Return Type | Visibility | Description                                                                                                          |
| -------------------------------------------------------------------------- | ----------- | ---------- | -------------------------------------------------------------------------------------------------------------------- |
| PasswordRequestDto(String old, String newPassword, String passwordConfirm) | —           | public     | Record canonical constructor. 입력값 유효성 검증 수행.                                                                         |


# UserUpdateRequestDto
사용자 프로필 수정 요청을 처리하기 위한 DTO.
클라이언트의 `/me` (PATCH) 요청 바디로 전달되며,
닉네임·학년·포지션·기술 스택·한 줄 소개 등의 정보를 수정한다.

## Attributes
| Name       | Type         | Visibility                 | Description                                         |
| ---------- | ------------ | -------------------------- | --------------------------------------------------- |
| nickname   | String       | private (record component) | 닉네임. 선택 입력. 2~10자의 한글, 영문, 숫자만 허용 (`@Pattern`).     |
| grade      | Integer      | private (record component) | 학년. 필수 입력, 1~4 범위 (`@Min`, `@Max`).                 |
| position   | PositionType | private (record component) | 포지션(enum). 예: FRONTEND, BACKEND 등.                  |
| skills     | Set<Skill>   | private (record component) | 보유 기술 스택. 최대 10개 제한 (`@Size(max=10)`).              |
| shortIntro | String       | private (record component) | 한 줄 소개. 최대 100자 제한 (`@Size(max=100)`, `@NotBlank`). |


## Operations


# WithdrawRequestDto
회원 탈퇴 요청 시 비밀번호를 입력받아 본인 여부를 확인하기 위한 요청 DTO.
BCrypt 해시 알고리즘 제약에 맞춘 최대 72자 제한을 적용하며,
비밀번호가 누락되거나 공백만 입력되는 경우 유효성 검증에 실패한다.

## Attributes
| Name     | Type   | Visibility                 | Description                                   |
| -------- | ------ | -------------------------- | --------------------------------------------- |
| password | String | private (record component) | 비밀번호 입력값. `@NotBlank`, `@Size(max=72)` 제약 적용. |

## Operations


# UserResponseDto
사용자 정보를 API 응답으로 전달하기 위한 DTO.
`User` 엔티티의 핵심 속성을 변환하여 노출하며, 보안 및 캡슐화를 위해 엔티티 자체를 직접 반환하지 않는다.

## Attributes
| Name       | Type         | Visibility                 | Description                          |
| ---------- | ------------ | -------------------------- |--------------------------------------|
| username   | String       | private (record component) | 사용자 아이디.                             |
| email      | String       | private (record component) | 이메일 주소.                              |
| university | University   | private (record component) | 이메일 도메인 기반의 소속 대학교(enum).            |
| nickname   | String       | private (record component) | 사용자 닉네임.                             |
| grade      | Integer      | private (record component) | 사용자 학년.                              |
| position   | PositionType | private (record component) | 포지션(enum). 예: FRONT_END, BACK_END 등. |
| skills     | Set<Skill>   | private (record component) | 보유 기술 스택 목록.                         |
| shortIntro | String       | private (record component) | 사용자 한 줄 소개.                          |

## Operations
| Name                 | Return Type     | Visibility    | Description                                |
| -------------------- | --------------- | ------------- | ------------------------------------------ |
| from(User user)      | UserResponseDto | public static | `User` 엔티티를 DTO로 변환하는 정적 팩토리 메서드.          |


# Team 관련 VO

# TeamResponseDto
팀(Team) 엔티티 및 관련 모집공고 정보를 통합하여 API 응답으로 전달하기 위한 DTO.
`Project`, `Study`, `Assignment` 등의 모집 유형에 관계없이 공통 구조로 팀 정보를 제공한다.
또한 각 팀 멤버의 상세 정보(`TeamMemberResponseDto`)를 포함한다.

## Attributes
| Name             | Type                        | Visibility                 | Description                            |
| ---------------- | --------------------------- | -------------------------- | -------------------------------------- |
| id               | Long                        | private (record component) | 팀 식별자 ID                               |
| recruitmentId    | Long                        | private (record component) | 연결된 모집공고(`BaseRecruitment`)의 ID        |
| recruitmentTitle | String                      | private (record component) | 모집공고 제목                                |
| category         | RecruitmentCategory         | private (record component) | 모집 유형 (PROJECT/STUDY/ASSIGNMENT 등)     |
| status           | RecruitmentStatus           | private (record component) | 모집 상태 (RECRUITING, CLOSED, CANCELED 등) |
| period           | PeriodResponseDto           | private (record component) | 모집 기간 정보 (Project/Study만 해당)           |
| durationDays     | Long                        | private (record component) | 모집 기간 일수 (`endDate - startDate`)       |
| leaderNickname   | String                      | private (record component) | 팀 리더(공고 작성자)의 닉네임                      |
| memberCount      | int                         | private (record component) | 팀 멤버 수                                 |
| members          | List<TeamMemberResponseDto> | private (record component) | 팀 멤버 정보 리스트                            |

## Operations
| Name                  | Return Type     | Visibility    | Description                                                     |
| --------------------- | --------------- | ------------- | --------------------------------------------------------------- |
| fromEntity(Team team) | TeamResponseDto | public static | `Team` 엔티티와 연결된 `BaseRecruitment`, `TeamMember` 정보를 DTO 형태로 변환. |


# TeamMember VO

# TeamRole
팀 내 역할을 정의하는 Enum 클래스.
`TeamMember` 엔티티에서 팀원의 권한을 구분하기 위해 사용된다.
리더(LEADER)와 일반 멤버(MEMBER) 두 가지 역할을 가진다.

## Attributes
| Name | Type   | Visibility    | Description                |
| ---- | ------ | ------------- | -------------------------- |
| desc | String | private final | 역할의 한글 설명 (예: “리더”, “멤버”). |

## Operations
| Name      | Return Type | Visibility | Description                       |
| --------- | ----------- | ---------- | --------------------------------- |
| getDesc() | String      | public     | 역할의 설명(한글)을 반환.                   |
| getName() | String      | public     | Enum 이름(LEADER, MEMBER)을 문자열로 반환. |


# TeamMemberResponseDto
팀 멤버(`TeamMember`) 정보를 API 응답 형태로 표현하는 DTO.
`TeamMember` 엔티티에서 필요한 최소 필드만 추출하며,
팀 내 역할(`TeamRole`), 포지션(`PositionType`), 사용자 기본 정보 등을 포함한다.

## Attributes
| Name     | Type         | Visibility                 | Description                      |
| -------- | ------------ | -------------------------- | -------------------------------- |
| userId   | Long         | private (record component) | 팀 멤버(User)의 고유 식별자 ID            |
| nickname | String       | private (record component) | 팀 멤버의 닉네임                        |
| role     | TeamRole     | private (record component) | 팀 내 역할 (리더 / 일반 멤버 등)            |
| position | PositionType | private (record component) | 사용자 포지션 (예: BACKEND, FRONTEND 등) |

## Operations
| Name                              | Return Type           | Visibility    | Description                              |
| --------------------------------- | --------------------- | ------------- | ---------------------------------------- |
| fromEntity(TeamMember teamMember) | TeamMemberResponseDto | public static | `TeamMember` 엔티티를 DTO로 변환하는 정적 팩토리 메서드.  |


# Review 관련 VO

# ReviewStatus
리뷰의 상태(활성/삭제)를 관리하는 Enum 클래스.
Soft Delete 정책에 따라 DB에서는 실제 삭제되지 않으며, `ReviewStatus` 필드를 통해 사용자 노출 여부를 제어한다.

## Attributes
| Name | Type   | Visibility    | Description                |
| ---- | ------ | ------------- | -------------------------- |
| desc | String | private final | 상태의 한글 설명 (예: “활성”, “삭제”). |

## Operations
| Name      | Return Type | Visibility | Description                        |
| --------- | ----------- | ---------- | ---------------------------------- |
| getDesc() | String      | public     | 상태 설명 반환.                          |
| getName() | String      | public     | Enum 이름(ACTIVE, DELETED)을 문자열로 반환. |


# ReviewCreationRequestDto
리뷰 생성 요청을 처리하기 위한 DTO.
사용자가 다른 사용자에 대한 후기를 작성할 때 전달되는 요청 데이터 구조이다.

## Attributes
| Name       | Type   | Visibility                 | Description                                 |
| ---------- | ------ | -------------------------- | ------------------------------------------- |
| revieweeId | Long   | private (record component) | 후기 대상 사용자 ID. `@NotNull` 제약 적용.             |
| content    | String | private (record component) | 리뷰 내용. `@NotBlank`, `@Size(max=100)` 검증 적용. |

## Operations
| Name                                                      | Return Type | Visibility | Description                                 |
| --------------------------------------------------------- | ----------- | ---------- | ------------------------------------------- |
| toEntity(User reviewer, User reviewee, String content)    | Review      | public     | 리뷰 작성자와 대상 유저 정보를 받아 `Review` 엔티티로 변환.      |


# ReviewUpdateRequestDto
리뷰 수정 요청 시 사용되는 DTO.
사용자가 작성한 리뷰의 내용을 변경하기 위해 전달하는 요청 데이터 구조이다.

## Attributes
| Name    | Type   | Visibility                 | Description                                     |
| ------- | ------ | -------------------------- | ----------------------------------------------- |
| content | String | private (record component) | 수정된 리뷰 내용. `@NotBlank`, `@Size(max=100)` 검증 적용. |

## Operations


# ReviewResponseDto
리뷰 정보를 클라이언트에게 응답하기 위한 DTO.
`Review` 엔티티의 데이터를 안전하게 변환하여 외부에 노출한다.

## Attributes
| Name    | Type   | Visibility                 | Description |
| ------- | ------ | -------------------------- | ----------- |
| content | String | private (record component) | 리뷰 본문 내용.   |

## Operations
| Name                              | Return Type       | Visibility    | Description                                         |
| --------------------------------- | ----------------- | ------------- | --------------------------------------------------- |
| from(Review review)               | ReviewResponseDto | public static | `Review` 엔티티를 `ReviewResponseDto`로 변환하는 정적 팩토리 메서드. |


# User 관련 Controller-Service-Repository

# UserRepository
사용자(`User`) 엔티티에 대한 데이터 접근을 담당하는 JPA Repository.
Spring Data JPA를 기반으로 기본 CRUD 기능을 상속받으며,
이메일, 닉네임, 아이디 중복 검증 및 Fetch Join을 통한 `skills` 컬렉션 로딩 기능을 추가로 제공한다.

## Attributes

## Operations
| Name                              | Return Type    | Description                              |
| --------------------------------- | -------------- | ---------------------------------------- |
| findByUsername(String username)   | Optional<User> | 사용자 아이디(username)로 조회.                   |
| existsByEmail(String email)       | boolean        | 이메일 중복 여부 확인.                            |
| existsByUsername(String username) | boolean        | 아이디 중복 여부 확인.                            |
| existsByNickname(String nickname) | boolean        | 닉네임 중복 여부 확인.                            |
| findByIdWithSkills(Long id)       | Optional<User> | Fetch Join으로 skills 컬렉션을 함께 로딩하여 사용자 조회. |


# UserService
사용자(`User`) 관련 핵심 비즈니스 로직을 정의하는 서비스 인터페이스.
회원가입, 비밀번호 변경, 사용자 정보 조회 및 수정, 탈퇴 등 주요 기능을 제공한다.

## Attributes

## Operations
| Name                                                  | Return Type     | Description                                  |
| ----------------------------------------------------- | --------------- | -------------------------------------------- |
| findByUsername(String username)                       | User            | 사용자 이름으로 사용자 조회. 존재하지 않으면 예외 발생.             |
| findById(Long id)                                     | User            | 사용자 ID로 조회. 존재하지 않으면 예외 발생.                  |
| findByIdWithSkills(Long id)                           | User            | 사용자 및 보유 기술 스택을 함께 조회. (Fetch Join)          |
| existsById(Long id)                                   | boolean         | 해당 ID의 사용자가 존재하는지 확인.                        |
| existsByEmail(String email)                           | boolean         | 이메일 중복 여부 확인.                                |
| existsByUsername(String username)                     | boolean         | 아이디 중복 여부 확인.                                |
| existsByNickname(String nickname)                     | boolean         | 닉네임 중복 여부 확인.                                |
| signUp(SignUpRequestDto dto)                          | Long            | 신규 사용자 등록 후 생성된 사용자 ID 반환.                   |
| changePassword(Long userId, PasswordRequestDto dto)   | void            | 기존 비밀번호 검증 후 새 비밀번호로 변경.                     |
| getUserInfo(Long userId)                              | UserResponseDto | 사용자 프로필 정보 조회.                               |
| updateUserInfo(Long userId, UserUpdateRequestDto dto) | UserResponseDto | 사용자 정보 수정 후 갱신된 데이터 반환.                      |
| withdraw(Long userId, String rawPassword)             | void            | 비밀번호 검증 후 회원 탈퇴(Soft Delete). |


# UserServiceImpl
`UserService`의 실제 구현체로,
사용자 관리 도메인(`User`)에 대한 모든 핵심 비즈니스 로직을 수행한다.
회원가입, 비밀번호 변경, 프로필 조회 및 수정, 회원 탈퇴를 처리한다.

## Attributes
| Name                 | Type                          | Visibility           | Description                                   |
| -------------------- | ----------------------------- | -------------------- | --------------------------------------------- |
| REFRESH_TOKEN_PREFIX | String                        | private static final | Redis에 저장된 Refresh Token의 key prefix (`RT:`). |
| userRepository       | UserRepository                | private final        | 사용자 데이터 접근 계층.                                |
| passwordEncoder      | PasswordEncoder               | private final        | 비밀번호 암호화/검증을 담당하는 Spring Security 컴포넌트.       |
| redisTemplate        | RedisTemplate<String, String> | private final        | Refresh Token 캐시 저장 및 삭제용 Redis 클라이언트.        |

## Operations
| Name                                                  | Return Type     | Description                                            |
| ----------------------------------------------------- | --------------- | ------------------------------------------------------ |
| findByUsername(String username)                       | User            | 사용자 이름으로 조회. 없을 시 `USER_NOT_FOUND` 예외 발생.              |
| findById(Long id)                                     | User            | 사용자 ID로 조회. 없을 시 `USER_NOT_FOUND`.                     |
| findByIdWithSkills(Long id)                           | User            | 사용자 및 기술 스택(fetch join) 조회.                            |
| existsById(Long id)                                   | boolean         | ID 존재 여부 반환.                                           |
| existsByEmail(String email)                           | boolean         | 이메일 중복 여부 반환.                                          |
| existsByUsername(String username)                     | boolean         | 아이디 중복 여부 반환.                                          |
| existsByNickname(String nickname)                     | boolean         | 닉네임 중복 여부 반환.                                          |
| signUp(SignUpRequestDto dto)                          | Long            | 회원가입 처리. 중복 체크 후 비밀번호 암호화 및 엔티티 저장.                    |
| changePassword(Long userId, PasswordRequestDto dto)   | void            | 기존 비밀번호 검증 후 새 비밀번호로 변경.                               |
| getUserInfo(Long userId)                              | UserResponseDto | 사용자 상세 정보 조회 후 DTO 변환.                                 |
| updateUserInfo(Long userId, UserUpdateRequestDto dto) | UserResponseDto | 사용자 프로필 정보 수정. (grade, position, skills, shortIntro 등) |
| withdraw(Long userId, String rawPassword)             | void            | 비밀번호 검증 후 Soft Delete(`WITHDRAWN`) 처리 및 Redis 토큰 삭제.   |


# AuthService
인증(Authentication) 관련 핵심 비즈니스 로직을 정의하는 서비스 인터페이스.
이메일 인증, 로그인, 리프레시 토큰 삭제 및 재발급 등의 기능을 포함한다.
구체 구현체(`AuthServiceImpl`)가 실제 로직을 담당한다.

## Attributes

## Operations
| Name                                                       | Return Type | Visibility | Description                       |
| ---------------------------------------------------------- | ----------- | ---------- | --------------------------------- |
| sendAuthCode(String toEmail)                               | void        | public     | 입력된 이메일 주소로 인증 코드를 발송.            |
| sendEmailAuthCode(String toEmail, String verificationCode) | void        | public     | 이메일과 인증번호를 기반으로 인증 메일을 전송.        |
| verifyCode(String toEmail, String inputCode)               | void        | public     | 사용자가 입력한 인증번호를 검증.                |
| login(SignInRequestDto dto)                                | TokenPair   | public     | 로그인 요청 DTO를 기반으로 인증 수행 및 JWT 발급.  |
| deleteRefreshToken(Long userId)                            | void        | public     | 로그아웃 시 사용자의 리프레시 토큰을 삭제.          |
| reissueTokens(String refreshToken)                         | TokenPair   | public     | 리프레시 토큰을 검증 후 새로운 액세스/리프레시 토큰 발급. |


# AuthServiceImpl
인증 서비스(`AuthService`)의 구현체로,
이메일 인증, 로그인/로그아웃, JWT 토큰 발급 및 재발급을 담당한다.
`Spring Security`, `Redis`, `JavaMailSender`를 활용하여 인증 절차와 세션 관리를 수행한다.

## Attributes
| Name                      | Type                          | Visibility           | Description                                  |
| ------------------------- | ----------------------------- | -------------------- | -------------------------------------------- |
| REFRESH_TOKEN_PREFIX      | String                        | private static final | Redis에 저장될 Refresh Token Key prefix (`RT:`). |
| EMAIL_VERIFICATION_PREFIX | String                        | private static final | Redis에 저장될 이메일 인증번호 Key prefix (`EMAIL:`).   |
| EXPIRATION_MINUTES        | int                           | private static final | 이메일 인증번호 TTL(3분).                            |
| fromEmail                 | String                        | private              | 인증 메일 발신자 주소 (application.yml에서 주입).         |
| jwtUtil                   | JwtUtil                       | private final        | JWT 생성 및 검증 유틸리티 클래스.                        |
| userService               | UserService                   | private final        | 사용자 조회 및 유효성 검증 서비스.                         |
| mailSender                | JavaMailSender                | private final        | 인증 메일 전송을 위한 메일 발송 컴포넌트.                     |
| redisTemplate             | RedisTemplate<String, String> | private final        | 이메일 인증번호 및 토큰 저장소.                           |
| authenticationManager     | AuthenticationManager         | private final        | Spring Security 인증 처리 매니저.                   |

## Operations
| Name                                                       | Return Type | Description                                         |
| ---------------------------------------------------------- | ----------- | --------------------------------------------------- |
| sendAuthCode(String toEmail)                               | void        | 랜덤 6자리 인증번호를 생성 후 Redis에 저장하고, 해당 이메일로 발송.          |
| sendEmailAuthCode(String toEmail, String verificationCode) | void        | HTML 이메일 템플릿을 생성하여 인증번호 발송.                         |
| verifyCode(String toEmail, String inputCode)               | void        | Redis에 저장된 인증번호와 입력값을 비교하여 검증.                      |
| login(SignInRequestDto dto)                                | TokenPair   | 사용자 로그인 인증 수행 후 Access/Refresh Token 발급.            |
| deleteRefreshToken(Long userId)                            | void        | Redis에서 특정 사용자 Refresh Token 삭제 (로그아웃 처리).          |
| reissueTokens(String refreshToken)                         | TokenPair   | 유효한 Refresh Token을 기반으로 새 Access/Refresh Token 재발급. |
| generateVerificationCode()                                 | String      | 6자리 랜덤 인증번호 생성.                                     |
| createEmailTemplate(String verificationCode)               | String      | 인증 이메일 HTML 템플릿 문자열 생성.                             |


## UserController
사용자 도메인(`User`) 관련 API 요청을 처리하는 REST Controller.
회원정보 조회, 수정, 비밀번호 변경, 회원 탈퇴, 중복검사, 리뷰 조회 등의 기능을 제공한다.

## Attributes
| Name                      | Type          | Visibility           | Description                                    |
| ------------------------- | ------------- | -------------------- | ---------------------------------------------- |
| REFRESH_TOKEN_COOKIE_NAME | String        | private static final | Refresh Token을 저장/삭제할 쿠키 이름 (`refresh_token`). |
| userService               | UserService   | private final        | 사용자 관련 비즈니스 로직을 처리하는 서비스 계층.                   |
| reviewService             | ReviewService | private final        | 리뷰 조회 로직을 담당하는 서비스 계층.                         |

## Operations
| Name                                                                                          | Return Type                                                  | Mapping                                | Description                             |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------ | -------------------------------------- | --------------------------------------- |
| existsByUsername(String username)                                                             | ResponseEntity<ApiResponse<Boolean>>                         | `GET /users/username/check`            | 아이디 중복 여부 검사.                           |
| existsByEmail(String email)                                                                   | ResponseEntity<ApiResponse<Boolean>>                         | `GET /users/email/check`               | 이메일 중복 여부 검사.                           |
| existsByNickname(String nickname)                                                             | ResponseEntity<ApiResponse<Boolean>>                         | `GET /users/nickname/check`            | 닉네임 중복 여부 검사.                           |
| passwordChange(PasswordRequestDto dto, CustomUserDetails userDetails)                         | ResponseEntity<ApiResponse<Void>>                            | `POST /users/me/password-change`       | 인증된 사용자의 비밀번호 변경.                       |
| getMe(CustomUserDetails userDetails)                                                          | ResponseEntity<ApiResponse<UserResponseDto>>                 | `GET /users/me`                        | 현재 로그인한 사용자의 프로필 조회.                    |
| updateMe(CustomUserDetails userDetails, UserUpdateRequestDto dto)                             | ResponseEntity<ApiResponse<UserResponseDto>>                 | `PATCH /users/me`                      | 현재 로그인한 사용자의 프로필 수정.                    |
| withdraw(CustomUserDetails userDetails, WithdrawRequestDto dto, HttpServletResponse response) | ResponseEntity<ApiResponse<Void>>                            | `DELETE /users/me/withdraw`            | 회원 탈퇴 (Soft Delete + Refresh Token 제거). |
| getReviews(Long userId, Pageable pageable)                                                    | ResponseEntity<ApiResponse<PageResponse<ReviewResponseDto>>> | `GET /users/{userId}/reviews/received` | 특정 사용자가 받은 리뷰 목록 조회.                    |
| addCookie(HttpServletResponse response, String token, String cookieName, long maxAge)         | void                                                         | private                                | Refresh Token 쿠키 생성 또는 만료 처리.           |


# AuthController
인증 관련 API 요청을 처리하는 REST Controller.
회원가입, 로그인, 이메일 인증, 토큰 재발급/로그아웃 등의 요청을 받아 `AuthService` 및 `UserService`를 호출하여 비즈니스 로직을 수행한다.

## Attributes
| Name                      | Type        | Visibility           | Description                                 |
| ------------------------- | ----------- | -------------------- | ------------------------------------------- |
| REFRESH_TOKEN_COOKIE_NAME | String      | private static final | Refresh Token을 저장할 쿠키 이름 (`refresh_token`). |
| jwtUtil                   | JwtUtil     | private final        | JWT 생성 및 검증 유틸리티.                           |
| authService               | AuthService | private final        | 인증 관련 비즈니스 로직 서비스.                          |
| userService               | UserService | private final        | 사용자 회원가입 및 사용자 관련 서비스.                      |

## Operations
| Name                                                                                  | Return Type                       | Mapping                          | Description                                                |
| ------------------------------------------------------------------------------------- | --------------------------------- | -------------------------------- | ---------------------------------------------------------- |
| sendEmailAuthCode(EmailRequestDto dto)                                                | ResponseEntity<ApiResponse<Void>> | `POST /api/v1/auth/email/code`   | 회원가입 이메일 인증번호 발송.                                          |
| verifyAuthCode(EmailVerificationRequestDto dto)                                       | ResponseEntity<ApiResponse<Void>> | `POST /api/v1/auth/email/verify` | 사용자가 입력한 이메일 인증번호 검증.                                      |
| signUp(SignUpRequestDto dto)                                                          | ResponseEntity<ApiResponse<Long>> | `POST /api/v1/auth/sign-up`      | 회원가입 요청 처리. 성공 시 생성된 `userId` 반환.                          |
| signIn(SignInRequestDto dto, HttpServletResponse response)                            | ResponseEntity<ApiResponse<Void>> | `POST /api/v1/auth/sign-in`      | 로그인 처리 및 Access/Refresh Token 발급.                          |
| signOut(HttpServletResponse response, CustomUserDetails userDetails)                  | ResponseEntity<ApiResponse<Void>> | `POST /api/v1/auth/sign-out`     | 로그아웃 처리 및 Redis 토큰 삭제.                                     |
| refreshToken(String refreshToken, HttpServletResponse response)                       | ResponseEntity<ApiResponse<Void>> | `POST /api/v1/auth/refresh`      | Refresh Token 기반 Access/Refresh Token 재발급.                 |
| addCookie(HttpServletResponse response, String token, String cookieName, long maxAge) | void                              | private                          | Refresh Token을 응답 쿠키에 설정 (HttpOnly, Secure, SameSite=Lax). |


# Team 관련 Repository-Service-Controller

# TeamRepository
Spring Data JPA를 활용하여 Team 엔티티의 데이터 접근을 담당하는 Repository 인터페이스.
팀 정보 조회, 모집 공고별 팀 목록 조회 등의 기능을 제공하며,
`@EntityGraph`를 사용하여 연관 엔티티 로딩 시 N+1 문제를 방지한다.

## Attributes

## Operations
| Name                                                                                                                                                                | Return Type      | Visibility | Description                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---------- | ------------------------------------------------------------------------------------------------------ |
| `findDistinctByRecruitmentCategoryAndRecruitmentStatusAndRecruitmentUserId(RecruitmentCategory category, RecruitmentStatus status, Long userId, Pageable pageable)` | `Page<Team>`     | public     | 특정 사용자가 작성한 모집공고 중 지정된 카테고리 및 상태의 팀 목록을 페이징 조회한다.<br>연관된 `Recruitment` 및 `User`를 `EntityGraph`로 함께 로딩. |
| `findByIdWithMembers(Long id)`                                                                                                                                      | `Optional<Team>` | public     | 팀 ID로 팀 정보를 조회하며, `members`와 각 `member.user`를 함께 로딩한다.<br>N+1 문제 방지를 위해 `EntityGraph`를 사용한다.           |

# TeamService
팀(Team) 도메인과 관련된 핵심 비즈니스 로직의 인터페이스 계층.
서비스 구현체(`TeamServiceImpl`)가 실제 로직을 담당하며,
팀 생성, 조회, 페이징 조회 등 주요 기능의 계약(Contract)을 정의한다.

## Attributes

## Operations
| Name                                                                                                                      | Return Type             | Visibility | Description                                      |
| ------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ---------- | ------------------------------------------------ |
| `save(Team team)`                                                                                                         | `void`                  | public     | 새 팀을 저장한다.                                       |
| `findById(Long id)`                                                                                                       | `Team`                  | public     | 팀 ID로 팀 엔티티를 조회한다.<br>존재하지 않을 경우 예외 발생.          |
| `findByIdWithMembers(Long id)`                                                                                            | `Team`                  | public     | 팀 ID로 조회하며, 팀 멤버(`members`, `user`)를 함께 로딩한다.    |
| `existsById(Long id)`                                                                                                     | `boolean`               | public     | 특정 팀 ID의 존재 여부를 반환한다.                            |
| `getByUserIdAndCategoryAndStatus(Long userId, RecruitmentCategory category, RecruitmentStatus status, Pageable pageable)` | `Page<TeamResponseDto>` | public     | 특정 사용자가 작성한 모집공고 중 지정된 카테고리와 상태의 팀 목록을 페이징 조회한다. |


# TeamServiceImpl
`TeamService` 인터페이스의 구현체로,
`Team` 엔티티의 조회 및 저장, 사용자별 팀 목록 조회 기능을 담당하는 서비스 클래스.

## Attributes
| Name                  | Type                | Visibility           | Description                          |
| --------------------- | ------------------- | -------------------- | ------------------------------------ |
| `teamRepository`      | `TeamRepository`    | private final        | Team 엔티티에 대한 데이터 접근을 담당하는 Repository |
| `pageableValidator`   | `PageableValidator` | private final        | 페이지 및 정렬 요청을 검증하는 유틸리티 클래스           |
| `MY_TEAM_SORT_FIELDS` | `Set<String>`       | private static final | 허용된 정렬 기준 필드 목록 (현재 `createdAt`만 허용) |

## Operations
| Name                                                                                                                      | Return Type             | Visibility | Description                                                                                |
| ------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ---------- | ------------------------------------------------------------------------------------------ |
| `save(Team team)`                                                                                                         | `void`                  | public     | 새 팀 정보를 데이터베이스에 저장한다.                                                                      |
| `findById(Long id)`                                                                                                       | `Team`                  | public     | 팀 ID로 팀을 조회하고, 없을 경우 `TEAM_NOT_FOUND` 예외를 발생시킨다.                                           |
| `findByIdWithMembers(Long id)`                                                                                            | `Team`                  | public     | 팀 ID로 팀을 조회하며, 팀 멤버(`members` 및 `user`)를 함께 로딩한다.                                          |
| `existsById(Long id)`                                                                                                     | `boolean`               | public     | 특정 ID의 팀 존재 여부를 반환한다.                                                                      |
| `getByUserIdAndCategoryAndStatus(Long userId, RecruitmentCategory category, RecruitmentStatus status, Pageable pageable)` | `Page<TeamResponseDto>` | public     | 특정 사용자가 생성한 모집공고 중 지정된 카테고리 및 상태의 팀 목록을 페이지 단위로 조회한다.<br>조회된 엔티티는 `TeamResponseDto`로 변환된다. |


# TeamController
팀(Team) 관련 REST API 요청을 처리하는 컨트롤러 계층 클래스.
사용자의 인증 정보를 바탕으로 본인의 팀 목록을 카테고리와 상태별로 페이징 조회할 수 있도록 한다.

## Attributes
| Name          | Type          | Visibility    | Description                   |
| ------------- | ------------- | ------------- | ----------------------------- |
| `teamService` | `TeamService` | private final | 팀 관련 비즈니스 로직을 수행하는 서비스 계층 의존성 |


## Operations
| Name                                                                                                                            | Return Type                                          | Visibility | Description                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `getMyTeamByCategory(RecruitmentCategory category, RecruitmentStatus status, CustomUserDetails userDetails, Pageable pageable)` | `ResponseEntity<ApiResponse<Page<TeamResponseDto>>>` | public     | 현재 로그인한 사용자의 팀 목록을 카테고리 및 상태별로 조회한다.<br>인증 사용자(`CustomUserDetails`)의 ID를 기반으로 `TeamService`를 호출한다.<br>기본 정렬은 `createdAt DESC`, 페이지 크기는 3으로 설정된다. |


# TeamMember 관련 Repository-Service-Controller

# TeamMemberRepository
`TeamMember` 엔티티에 대한 데이터 접근 계층(Repository).
Spring Data JPA의 JpaRepository를 상속받아 기본 CRUD 기능을 제공한다.

## Attributes

## Operations
| Name                                              | Return Type            | Visibility | Description                                                   |
| ------------------------------------------------- | ---------------------- | ---------- | ------------------------------------------------------------- |
| `findByTeamIdAndUserId(Long teamId, Long userId)` | `Optional<TeamMember>` | public     | 특정 팀 ID와 사용자 ID로 팀 멤버를 조회한다.<br>결과가 없을 경우 빈 `Optional`을 반환한다. |


# TeamMemberService
팀 멤버 관리 기능을 정의하는 서비스 계층 인터페이스.
`TeamMemberServiceImpl`에서 구현되며, 팀 내 멤버 삭제(리더 권한 기반)와 같은 비즈니스 로직의 계약(Contract)을 명시한다.

## Attributes

## Operations
| Name                                                       | Return Type | Visibility | Description                                                                                                 |
| ---------------------------------------------------------- | ----------- | ---------- | ----------------------------------------------------------------------------------------------------------- |
| `removeMember(Long teamId, Long removerId, Long targetId)` | `void`      | public     | 특정 팀(`teamId`)에서 리더(`removerId`)가 특정 멤버(`targetId`)를 삭제(강제 탈퇴)한다.<br>실제 구현은 `TeamMemberServiceImpl`에서 수행된다. |



# TeamMemberServiceImpl
`TeamMemberService` 인터페이스의 구현체로,
팀 멤버 관리 및 리더 권한 기반 삭제 로직을 수행하는 서비스 클래스.
동시성 제어를 위해 `@Version` 기반 Optimistic Lock과 Spring Retry를 활용하며, 비즈니스 예외를 통한 명확한 검증 로직을 갖는다.

## Attributes
| Name                   | Type                   | Visibility    | Description                     |
| ---------------------- | ---------------------- | ------------- | ------------------------------- |
| `teamMemberRepository` | `TeamMemberRepository` | private final | 팀 멤버 엔티티에 대한 데이터 접근 계층          |
| `teamService`          | `TeamService`          | private final | 팀 엔티티 조회 및 검증을 담당하는 서비스         |
| `recruitmentService`   | `RecruitmentService`   | private final | 모집공고(BaseRecruitment) 관련 서비스 계층 |

## Operations
| Name                                                                                             | Return Type | Visibility | Description                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------ | ----------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `removeMember(Long teamId, Long removerId, Long targetId)`                                       | `void`      | public     | 리더 권한으로 특정 팀 멤버를 강제 탈퇴시킨다.<br>다음 검증 및 처리 단계를 수행한다:<ul><li>리더 본인은 자신을 삭제할 수 없음</li><li>리더 권한 여부 검증</li><li>존재하지 않는 팀/멤버 시 예외 발생</li><li>모든 조건 충족 시 `team.removeMember()` 수행 및 모집 인원 감소</li></ul>동시성 충돌 발생 시 최대 3회 재시도한다. |
| `recover(ObjectOptimisticLockingFailureException e, Long teamId, Long removerId, Long targetId)` | `void`      | protected  | 재시도 실패 시 호출되는 복구 메서드.<br>`TOO_MANY_REQUESTS` 예외를 발생시켜 클라이언트에 알린다.                                                                                                                                                       |


# TeamMemberController
팀 멤버(TeamMember) 관련 요청을 처리하는 REST API 컨트롤러 계층 클래스.
팀 리더가 특정 멤버를 팀에서 강제 탈퇴시키는 기능을 제공한다.

## Attributes
| Name                | Type                | Visibility    | Description                 |
| ------------------- | ------------------- | ------------- | --------------------------- |
| `teamMemberService` | `TeamMemberService` | private final | 팀 멤버 관리 로직을 수행하는 서비스 계층 의존성 |

## Operations
| Name                                                                      | Return Type                         | Visibility | Description                                                                                                                                                                                                             |
| ------------------------------------------------------------------------- | ----------------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deleteMember(Long teamId, Long memberId, CustomUserDetails userDetails)` | `ResponseEntity<ApiResponse<Void>>` | public     | **팀 리더 권한으로 특정 팀 멤버를 삭제하는 엔드포인트.**<br><ul><li>`teamId`: 삭제 대상이 속한 팀 ID</li><li>`memberId`: 삭제할 멤버의 ID</li><li>`userDetails`: 현재 로그인한 사용자 정보 (리더 권한 검증용)</li></ul><br>성공 시 `"팀 멤버 삭제에 성공했습니다."` 메시지와 함께 200 OK 응답을 반환한다. |


# Review 관련 Repository-Service-Controller

# ReviewRepository
`Review` 엔티티에 대한 데이터 접근 계층(Repository).
Spring Data JPA의 JpaRepository를 상속받아 기본 CRUD 기능을 제공하며,
리뷰 대상자(피평가자) 및 리뷰 작성자 기준으로 리뷰를 조회하는 기능을 제공한다.

## Attributes

## Operations
| Name                                                                                 | Return Type    | Visibility | Description                                                      |
| ------------------------------------------------------------------------------------ | -------------- | ---------- | ---------------------------------------------------------------- |
| `findByRevieweeIdAndStatus(Long revieweeId, ReviewStatus status, Pageable pageable)` | `Page<Review>` | public     | 특정 피평가자(`revieweeId`)와 상태(`status`)를 기준으로 리뷰 목록을 페이지 단위로 조회한다.   |
| `findByReviewerIdAndStatus(Long reviewerId, ReviewStatus status, Pageable pageable)` | `Page<Review>` | public     | 특정 리뷰 작성자(`reviewerId`)와 상태(`status`)를 기준으로 리뷰 목록을 페이지 단위로 조회한다. |

# ReviewService
리뷰(Review) 도메인의 핵심 비즈니스 로직 계약(Contract)을 정의하는 서비스 인터페이스.
`ReviewServiceImpl`에서 구현되며, 리뷰의 생성(Create), 조회(Read), 수정(Update), 삭제(Delete) 기능을 수행한다.

## Attributes

## Operations
| Name                                                                   | Return Type                       | Visibility | Description                                                       |
| ---------------------------------------------------------------------- | --------------------------------- | ---------- | ----------------------------------------------------------------- |
| `findById(Long id)`                                                    | `Review`                          | public     | 리뷰 ID로 리뷰 엔티티를 조회한다. 존재하지 않을 경우 예외 발생.                            |
| `createReview(Long reviewerId, ReviewCreationRequestDto dto)`          | `Long`                            | public     | 리뷰 작성자 ID와 요청 DTO를 기반으로 리뷰를 생성하고, 생성된 리뷰의 ID를 반환한다.               |
| `getReviewsByRevieweeId(Long revieweeId, Pageable pageable)`           | `PageResponse<ReviewResponseDto>` | public     | 특정 사용자가 **받은 리뷰 목록**을 페이징 조건에 맞게 조회한다.                            |
| `getReviewsByReviewerId(Long reviewerId, Pageable pageable)`           | `PageResponse<ReviewResponseDto>` | public     | 특정 사용자가 **작성한 리뷰 목록**을 페이징 조건에 맞게 조회한다.                           |
| `updateReview(Long userId, Long reviewId, ReviewUpdateRequestDto dto)` | `Long`                            | public     | 본인이 작성한 활성 상태의 리뷰만 수정할 수 있다.                                      |
| `deleteReview(Long userId, Long reviewId)`                             | `void`                            | public     | 본인이 작성한 활성 상태의 리뷰만 **Soft Delete** 처리한다. (`ReviewStatus.DELETED`) |


# ReviewServiceImpl
ReviewService 인터페이스의 구현체로,
리뷰 생성, 조회, 수정, 삭제(Soft Delete) 기능을 제공하는 서비스 클래스.
리뷰 작성자·피평가자 간의 관계 및 상태(ReviewStatus)를 검증한다.

## Attributes
| Name               | Type               | Visibility    | Description                     |
| ------------------ | ------------------ | ------------- | ------------------------------- |
| `reviewRepository` | `ReviewRepository` | private final | 리뷰 엔티티에 대한 데이터 접근 계층            |
| `userService`      | `UserService`      | private final | 리뷰 작성자 및 피평가자 검증을 위한 사용자 조회 서비스 |

## Operations
| Name                                                                   | Return Type                       | Visibility | Description                                                                                          |
| ---------------------------------------------------------------------- | --------------------------------- | ---------- | ---------------------------------------------------------------------------------------------------- |
| `findById(Long id)`                                                    | `Review`                          | public     | 리뷰 ID로 리뷰를 조회하고, 존재하지 않을 경우 `REVIEW_NOT_FOUND` 예외를 발생시킨다.                                            |
| `createReview(Long reviewerId, ReviewCreationRequestDto dto)`          | `Long`                            | public     | 리뷰 작성자와 피평가자 정보를 조회 후, 새로운 리뷰를 생성 및 저장한다.<br>자기 자신에게 리뷰를 남기는 경우 `SELF_REVIEW_NOT_ALLOWED` 예외 발생.     |
| `getReviewsByRevieweeId(Long revieweeId, Pageable pageable)`           | `PageResponse<ReviewResponseDto>` | public     | 특정 사용자가 **받은 리뷰 목록**을 페이지 단위로 조회한다.<br>`ReviewStatus.ACTIVE` 상태만 조회하며, DTO 변환 후 `PageResponse`로 반환.  |
| `getReviewsByReviewerId(Long reviewerId, Pageable pageable)`           | `PageResponse<ReviewResponseDto>` | public     | 특정 사용자가 **작성한 리뷰 목록**을 페이지 단위로 조회한다.<br>`ReviewStatus.ACTIVE` 상태만 조회하며, DTO 변환 후 `PageResponse`로 반환. |
| `updateReview(Long userId, Long reviewId, ReviewUpdateRequestDto dto)` | `Long`                            | public     | 본인이 작성한 활성 상태 리뷰만 수정할 수 있다.<br>리뷰 소유자 불일치 또는 비활성 리뷰일 경우 예외 발생.                                       |
| `deleteReview(Long userId, Long reviewId)`                             | `void`                            | public     | 본인이 작성한 활성 상태 리뷰만 **Soft Delete** 처리(`ReviewStatus.DELETED`) 가능.                                     |



# ReviewController
리뷰(Review) 관련 요청을 처리하는 REST API 컨트롤러 계층 클래스.
리뷰 작성, 조회(받은/작성한), 수정, 삭제(Soft Delete) 기능을 담당하며, `ReviewService`를 호출해 비즈니스 로직을 수행한다.

## Attributes
| Name            | Type            | Visibility    | Description                    |
| --------------- | --------------- | ------------- | ------------------------------ |
| `reviewService` | `ReviewService` | private final | 리뷰 관련 비즈니스 로직을 담당하는 서비스 계층 의존성 |

## Operations
| Name                                                                                     | Return Type                                                    | Visibility | Description                                                                     |
| ---------------------------------------------------------------------------------------- | -------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------- |
| `createReview(ReviewCreationRequestDto dto, CustomUserDetails userDetails)`              | `ResponseEntity<ApiResponse<Long>>`                            | public     | 인증된 사용자가 새로운 리뷰를 작성한다.<br>성공 시 생성된 리뷰 ID를 반환한다.                                 |
| `getMyWrittenReviews(CustomUserDetails userDetails, Pageable pageable)`                  | `ResponseEntity<ApiResponse<PageResponse<ReviewResponseDto>>>` | public     | 현재 로그인한 사용자가 **작성한 리뷰 목록**을 페이지 단위로 조회한다.<br>기본 정렬: `createdAt DESC`, 페이지 크기: 5 |
| `getMyReceivedReviews(CustomUserDetails userDetails, Pageable pageable)`                 | `ResponseEntity<ApiResponse<PageResponse<ReviewResponseDto>>>` | public     | 현재 로그인한 사용자가 **받은 리뷰 목록**을 페이지 단위로 조회한다.<br>기본 정렬: `createdAt DESC`, 페이지 크기: 5  |
| `updateReview(Long reviewId, ReviewUpdateRequestDto dto, CustomUserDetails userDetails)` | `ResponseEntity<ApiResponse<Long>>`                            | public     | 인증된 사용자가 **본인이 작성한 리뷰를 수정**한다.<br>PATCH 메서드를 통해 부분 업데이트 수행.                     |
| `deleteReview(Long reviewId, CustomUserDetails userDetails)`                             | `ResponseEntity<ApiResponse<Void>>`                            | public     | 인증된 사용자가 **본인이 작성한 리뷰를 Soft Delete** 처리한다.<br>`ReviewStatus.DELETED` 상태로 변경.    |
