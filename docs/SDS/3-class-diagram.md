## 3. Class diagram

# RecruitmentCategory

프로젝트 내 모집 공고의 유형을 정의하는 **열거형(Enum)** 으로, 각 카테고리의 설명(`desc`)과 해당 Enum 값을 제공한다.

## Enum Values

| Value | Description |
|-------|-------------|
| PROJECT | 프로젝트 |
| ASSIGNMENT | 과제 |
| STUDY | 스터디 |

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| desc | String | private | 모집 공고 카테고리 설명 (예: "프로젝트") |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getDesc() | String | public | 카테고리 설명 반환 |
| getName() | String | public | Enum 이름 반환 |

---

# RecruitmentStatus

모집 공고의 현재 상태를 정의하는 **열거형(Enum)** 으로, 각 상태의 설명(`desc`)과 해당 Enum 값을 제공한다.

## Enum Values

| Value | Description |
|-------|-------------|
| RECRUITING | 모집 중 |
| CLOSED | 마감 |
| CANCELED | 취소 |

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| desc | String | private | 모집 상태 설명 (예: "모집 중") |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getDesc() | String | public | 상태 설명 반환 |
| getName() | String | public | Enum 이름 반환 |

---

# PositionType

프로젝트 내 포지션 유형을 정의하는 열거형(Enum) 으로, 각 포지션의 설명(desc)과 해당 Enum 값을 제공한다.

## Enum Values

| Value | Description |
|------|-------------|
| FULL_STACK | 풀스택 |
| FRONT_END | 프론트엔드 |
| BACK_END | 백엔드 |
| DATA | 데이터 |
| AI | AI |
| GAME | 게임 |
| PLANNER | 기획 |
| DESIGNER | 디자인 |

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| desc | String | private | 포지션 설명 (예: "프론트엔드") |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getDesc() | String | public | 포지션 설명 반환 |
| getName() | String | public | Enum 이름 반환 (FULL_STACK, FRONT_END 등) |

---

# Skill

사용 가능한 기술 스택을 정의하는 **열거형(Enum)** 으로, 각 기술의 설명(`desc`)과 해당 Enum 값을 제공한다.

## Enum Values

| Value | Description |
|-------|-------------|
| HTML | HTML |
| CSS | CSS |
| JAVASCRIPT | JavaScript |
| JAVA | Java |
| KOTLIN | Kotlin |
| PYTHON | Python |
| SWIFT | Swift |
| C_CPP | C/C++ |
| CSHARP | C# |
| TYPESCRIPT | TypeScript |
| REACT | React |
| NODE_JS | Node.js |
| EXPRESS | Express |
| VUE_JS | Vue.js |
| NEXT_JS | Next.js |
| SPRING_BOOT | Spring Boot |
| DJANGO | Django |
| PANDAS | Pandas |
| SCIKIT_LEARN | scikit-learn |
| PYTORCH | PyTorch |
| TENSORFLOW | TensorFlow |
| FLUTTER | Flutter |
| MYSQL | MySQL |
| REDIS | Redis |
| MONGODB | MongoDB |
| POSTGRESQL | PostgreSQL |
| GIT_GITHUB | Git/GitHub |
| GITHUB_ACTIONS | GitHub Actions |
| FIGMA | Figma |
| NOTION | Notion |
| JIRA | Jira |
| DOCKER | Docker |
| UNITY | Unity |
| UNREAL | Unreal |

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| desc | String | private | 기술 이름 설명 (예: "Spring Boot") |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getDesc() | String | public | 기술 설명 반환 |
| getName() | String | public | Enum 이름 반환 |

---

# ParticipantInfo

모집 참여 인원 정보를 담는 **임베디드 클래스**로,  
최대 참여 인원과 현재 참여 인원을 관리하고, 참여 인원 증가/감소 여부를 판단한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| maxParticipants | int | private | 최대 참여 인원 |
| currParticipants | int | private | 현재 참여 인원 (기본값 0) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getMaxParticipants() | int | public | 최대 참여 인원 반환 |
| getCurrParticipants() | int | public | 현재 참여 인원 반환 |
| isFull() | boolean | public | 현재 참여 인원이 최대 참여 인원에 도달했는지 확인 |
| incrementCurrParticipants() | void | public | 현재 참여 인원을 1 증가 |
| decreaseCurrParticipants() | void | public | 현재 참여 인원을 1 감소, 0 이하 시 예외 발생 |

---

# PositionParticipantInfo

특정 포지션별 참여 인원 정보를 담는 **임베디드 클래스**로,  
포지션 타입과 해당 포지션의 참여 인원 정보를 관리한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| position | PositionType | private | 포지션 타입 (예: 프론트엔드, 백엔드 등) |
| participantInfo | ParticipantInfo | private | 해당 포지션의 참여 인원 정보 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getPosition() | PositionType | public | 포지션 타입 반환 |
| getParticipantInfo() | ParticipantInfo | public | 해당 포지션 참여 인원 정보 반환 |

---

# Period

기간 정보를 표현하는 **임베디드(Embeddable) 레코드 클래스**로,  
시작일(`startDate`)과 종료일(`endDate`)을 관리한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| startDate | LocalDate | private | 시작 날짜 |
| endDate | LocalDate | private | 종료 날짜 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| startDate() | LocalDate | public | 시작 날짜 반환 |
| endDate() | LocalDate | public | 종료 날짜 반환 |

---

# BaseRecruitment

프로젝트, 과제, 스터디 공고 엔티티들이 상속받는 **공통 추상 클래스**로,  
공통 속성과 상태 관리 로직을 정의하며 상속 구조에서 기반 엔티티 역할을 한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| id | Long | private | 공고의 고유 식별자 |
| user | User | private | 공고를 등록한 사용자 엔티티 참조 |
| category | RecruitmentCategory | private | 공고의 카테고리 (프로젝트, 과제, 스터디 등) |
| title | String | private | 공고 제목 |
| content | String | private | 공고 본문 |
| deadline | LocalDateTime | private | 모집 마감일 |
| status | RecruitmentStatus | private | 공고 상태 (모집 중, 마감, 삭제) |
| createdAt | LocalDateTime | private | 공고가 생성된 시각 |
| updatedAt | LocalDateTime | private | 공고가 마지막으로 수정된 시각 |
| viewCount | int | private | 공고 조회수 (기본값 0) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getId() | Long | public | 공고 식별자 반환 |
| getUser() | User | public | 공고 등록자 반환 |
| getCategory() | RecruitmentCategory | public | 공고 카테고리 반환 |
| getTitle() | String | public | 공고 제목 반환 |
| getContent() | String | public | 공고 내용 반환 |
| getDeadline() | LocalDateTime | public | 모집 마감일 반환 |
| getStatus() | RecruitmentStatus | public | 공고 상태 반환 |
| getCreatedAt() | LocalDateTime | public | 생성 시각 반환 |
| getUpdatedAt() | LocalDateTime | public | 수정 시각 반환 |
| getViewCount() | int | public | 조회수 반환 |
| update(String title, String content, LocalDateTime deadline) | void | protected | 제목, 내용, 마감일 수정 |
| changeStatusByDeadline() | void | public | 마감일에 따라 상태를 자동으로 변경 |
| cancel() | void | public | 공고를 삭제 상태(CANCELED)로 변경 |

---

# DTO

# BaseRecruitmentRequestDto

모집 공고 작성 요청의 공통 필드를 정의하는 **추상 DTO 클래스**로, 제목, 본문, 마감일 정보를 관리한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| title | String | private final | 공고 제목 (최대 255자, 필수 입력) |
| content | String | private final | 공고 본문 내용 (최대 4096자, 필수 입력) |
| deadline | LocalDateTime | private final | 공고 마감일 (오늘 이후 날짜, 필수 입력) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getTitle() | String | public | 제목 반환 |
| getContent() | String | public | 본문 내용 반환 |
| getDeadline() | LocalDateTime | public | 마감일 반환 |

---

# GradeRequestDto

모집 대상 학년 정보를 전달하는 **DTO record 클래스**로, 1~4학년 범위를 검증한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| grade | Integer | private | 모집 학년 (필수, 1~4 범위 검증) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| grade() | Integer | public | 학년값 반환 |

---

# PositionInfoCreationRequestDto

포지션 생성 요청을 처리하기 위한 DTO로, 사용자가 입력한 포지션 정보와 해당 포지션의 최대 모집 인원수를 전달한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| position | PositionType | private (record component) | 포지션 (필수 입력) |
| maxParticipants | int | private (record component) | 모집 인원수 (최소 1 이상, 필수 입력) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| to(PositionInfoCreationRequestDto dto) | PositionParticipantInfo | public static | PositionInfoCreationRequestDto 객체를 PositionParticipantInfo 객체로 변환한다. |

---

# ParticipantInfoUpdateRequestDto

공고의 모집 인원 및 현재 인원 정보를 수정하기 위한 DTO로, 현재 인원이 모집 인원을 초과할 수 없다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| maxParticipants | int | private (record component) | 모집 인원수 (최소 1 이상, 필수 입력) |
| currParticipants | int | private (record component) | 현재 인원수 (최소 0 이상, 모집 인원수 초과 불가, 필수 입력) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| to(ParticipantInfoUpdateRequestDto dto) | ParticipantInfo | public static | ParticipantInfoUpdateRequestDto 객체를 ParticipantInfo 객체로 변환한다. |

---

# PositionInfoUpdateRequestDto

특정 포지션의 인원 정보를 수정하기 위한 DTO로, 포지션 유형과 해당 포지션의 인원 정보를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| position | PositionType | private (record component) | 포지션 (필수 입력) |
| participantInfo | ParticipantInfoUpdateRequestDto | private (record component) | 포지션별 인원 정보 (유효성 검사 포함, 필수 입력) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| to(PositionInfoUpdateRequestDto dto) | PositionParticipantInfo | public static | PositionInfoUpdateRequestDto 객체를 PositionParticipantInfo 객체로 변환한다. |

---

# PeriodRequestDto

기간 정보를 전달하기 위한 DTO로, 시작일과 종료일을 포함하며 날짜 유효성을 검사한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| startDate | LocalDate | private (record component) | 시작일 (필수 입력) |
| endDate | LocalDate | private (record component) | 종료일 (오늘 이후, 시작일 이전 불가, 필수 입력) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| to(PeriodRequestDto dto) | Period | public static | PeriodRequestDto 객체를 Period 객체로 변환한다. |

---

# BaseRecruitmentDetailResponseDto

공고 상세 조회 시 공통적으로 사용되는 응답 DTO로, 작성자 정보, 공고 기본 정보, 마감일 및 상태 등의 데이터를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| id | Long | private final | 공고 ID |
| authorId | Long | private final | 작성자 ID |
| authorNickname | String | private final | 작성자 닉네임 |
| category | RecruitmentCategory | private final | 공고 카테고리 |
| university | University | private final | 작성자 소속 대학교 |
| title | String | private final | 공고 제목 |
| content | String | private final | 공고 본문 내용 |
| deadline | LocalDateTime | private final | 모집 마감일 (`yyyy-MM-dd HH:mm:ss`) |
| createdAt | LocalDateTime | private final | 공고 생성일 (`yyyy-MM-dd HH:mm:ss`) |
| status | RecruitmentStatus | private final | 공고 상태 |
| viewCount | int | private final | 조회수 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getId() | Long | public | 공고 ID를 반환한다. |
| getAuthorId() | Long | public | 작성자 ID를 반환한다. |
| getAuthorNickname() | String | public | 작성자 닉네임을 반환한다. |
| getCategory() | RecruitmentCategory | public | 공고 카테고리를 반환한다. |
| getUniversity() | University | public | 작성자 소속 대학교를 반환한다. |
| getTitle() | String | public | 공고 제목을 반환한다. |
| getContent() | String | public | 공고 본문 내용을 반환한다. |
| getDeadline() | LocalDateTime | public | 모집 마감일을 반환한다. |
| getCreatedAt() | LocalDateTime | public | 공고 생성일을 반환한다. |
| getStatus() | RecruitmentStatus | public | 공고 상태를 반환한다. |
| getViewCount() | int | public | 조회수를 반환한다. |

---

# BaseRecruitmentSummaryResponseDto

공고 요약 응답 DTO의 **추상 클래스**. 공통 필드(작성자, 카테고리, 제목, 마감일, 상태 등)를 정의하며 하위 DTO가 이를 상속하여 구체화한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| id | Long | private final | 공고 식별자 |
| authorId | Long | private final | 작성자 식별자 |
| authorNickname | String | private final | 작성자 닉네임 |
| university | University | private final | 소속 대학교 |
| category | RecruitmentCategory | private final | 공고 카테고리 |
| title | String | private final | 공고 제목 |
| deadline | LocalDateTime | private final | 모집 마감일 (yyyy-MM-dd) |
| status | RecruitmentStatus | private final | 공고 상태 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| getId() | Long | public | 공고 식별자 반환 |
| getAuthorId() | Long | public | 작성자 식별자 반환 |
| getAuthorNickname() | String | public | 작성자 닉네임 반환 |
| getUniversity() | University | public | 소속 대학교 반환 |
| getCategory() | RecruitmentCategory | public | 공고 카테고리 반환 |
| getTitle() | String | public | 공고 제목 반환 |
| getDeadline() | LocalDateTime | public | 모집 마감일 반환 |
| getStatus() | RecruitmentStatus | public | 공고 상태 반환 |

---

# ParticipantInfoResponseDto

포지션별 인원 정보를 반환하기 위한 응답 DTO로, 모집 인원과 현재 인원을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| maxParticipants | int | private (record component) | 모집 인원 |
| currParticipants | int | private (record component) | 현재 인원 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| from(ParticipantInfo info) | ParticipantInfoResponseDto | public static | ParticipantInfo 객체를 ParticipantInfoResponseDto로 변환 |

---

# PositionInfoResponseDto

포지션 정보와 해당 포지션의 인원 정보를 반환하기 위한 응답 DTO로, 포지션 유형과 ParticipantInfoResponseDto를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| position | PositionType | private (record component) | 포지션 |
| participantInfo | ParticipantInfoResponseDto | private (record component) | 포지션별 인원 정보 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| from(PositionParticipantInfo info) | PositionInfoResponseDto | public static | PositionParticipantInfo 객체를 PositionInfoResponseDto로 변환 |

---

# PeriodResponseDto

기간 정보를 반환하기 위한 응답 DTO로, 시작일과 종료일을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| startDate | LocalDate | private (record component) | 시작일 (`yyyy-MM-dd`) |
| endDate | LocalDate | private (record component) | 종료일 (`yyyy-MM-dd`) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| from(Period period) | PeriodResponseDto | public static | Period 객체를 PeriodResponseDto로 변환 |

---

# PageResponse<T>

페이지 단위로 데이터를 반환하기 위한 제네릭 DTO로, 내용, 페이지 정보, 전체 요소 수 및 마지막 페이지 여부를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| content | List<T> | private (record component) | 페이지 내 데이터 목록 |
| pageNumber | int | private (record component) | 현재 페이지 번호 |
| pageSize | int | private (record component) | 페이지 크기 |
| totalElements | long | private (record component) | 전체 요소 수 |
| totalPages | int | private (record component) | 전체 페이지 수 |
| last | boolean | private (record component) | 마지막 페이지 여부 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| from(Page<T> page) | PageResponse<T> | public static | Page 객체를 PageResponse 객체로 변환 |

---

# RecruitmentService

모집 공고 관련 비즈니스 로직을 정의하는 서비스 인터페이스로, 공고 조회 및 상태 관리 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|      |      |           |             |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| findById(Long recruitmentId) | BaseRecruitment | public | 주어진 ID의 모집 공고를 조회 |
| findByIdNotCanceled(Long recruitmentId) | BaseRecruitment | public | 취소되지 않은 모집 공고를 ID로 조회 |
| closeExpiredRecruitments() | void | public | 마감일이 지난 모집 공고의 상태를 CLOSED로 변경 |

---

# RecruitmentServiceImpl

[RecruitmentService](#recruitmentservice)를 구현한 서비스 클래스
모집 공고 조회, 마감 처리 등의 실제 비즈니스 로직을 수행하며, 트랜잭션 및 로깅을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| recruitmentRepository | RecruitmentRepository | private final | 모집 공고 데이터 접근 계층 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| findById(Long recruitmentId) | BaseRecruitment | public | 주어진 ID의 모집 공고를 조회 |
| findByIdNotCanceled(Long recruitmentId) | BaseRecruitment | public | 취소되지 않은 모집 공고를 ID로 조회 |
| closeExpiredRecruitments() | void | public | 마감일이 지난 모집 공고를 CLOSED 상태로 변경 |

---

# RecruitmentRepository

BaseRecruitment 엔티티의 데이터 접근 계층 인터페이스로, 공고 조회 및 상태 변경 관련 쿼리를 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|      |      |           |             |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| findByIdNotCanceled(Long id) | Optional\<BaseRecruitment\> | public | 취소되지 않은 공고를 ID로 조회 |
| closeExpiredRecruitments(LocalDateTime baseTime) | int | public | 기준 시각 이전 마감 공고의 상태를 CLOSED로 변경 |

---

# Project 관련

# Project

[BaseRecruitment](#baserecruitment)를 상속한 프로젝트 공고 엔티티로, 프로젝트 목적, 모임 유형, 포지션, 필요 스킬, 학년, 기간 등의 상세 정보를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| purpose | ProjectPurpose | private | 프로젝트 목적 |
| meetingType | MeetingType | private | 모임 유형 |
| positions | Set\<PositionParticipantInfo\> | private | 프로젝트 포지션별 참여자 정보 |
| skills | Set\<Skill\> | private | 요구 스킬 목록 |
| grades | Set\<Integer\> | private | 참여 가능한 학년 |
| period | Period | private | 프로젝트 기간 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| update(ProjectUpdateRequestDto dto) | void | public | 프로젝트 공고 정보를 업데이트 |
| getPositionInfoByRole(PositionType position) | Optional\<PositionParticipantInfo\> | public | 특정 포지션의 참여자 정보 조회 |
| decreaseCurrParticipant(PositionType positionType) | void | public | 특정 포지션의 현재 참여 인원을 1명 감소 |

---

# MeetingType

회의 또는 프로젝트 진행 방식을 정의하는 **열거형(Enum)**으로, 각 유형의 설명(`desc`)과 해당 Enum 값을 제공한다.

## Enum Values

| Value | Description |
|-------|-------------|
| ONLINE | 온라인 |
| OFFLINE | 오프라인 |
| HYBRID | 온/오프라인 |

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| desc | String | private final | 회의 유형 설명 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| getDesc() | String | public | 회의 유형 설명 반환 |
| getName() | String | public | Enum 이름 반환 |

---

# ProjectPurpose

프로젝트의 목적을 정의하는 **열거형(Enum)**으로, 각 목적의 설명(`desc`)과 해당 Enum 값을 제공한다.

## Enum Values

| Value | Description |
|-------|-------------|
| CONTEST | 공모전 |
| HACKATHON | 해커톤 |
| TOY_PROJECT | 토이 프로젝트 |
| SIDE_PROJECT | 사이드 프로젝트 |

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| desc | String | private final | 프로젝트 목적 설명 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| getDesc() | String | public | 프로젝트 목적 설명 반환 |
| getName() | String | public | Enum 이름 반환 |

---

# Project DTO 관련

# ProjectCommonRequestDto

[BaseRecruitmentRequestDto](#baserecruitmentrequestdto)를 상속한 추상 요청 DTO로, 프로젝트 공고 생성 및 수정 시 공통으로 필요한 정보를 담는다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| purpose | ProjectPurpose | private final | 프로젝트 목적 |
| meetingType | MeetingType | private final | 진행 방식 |
| skills | Set\<Skill\> | private final | 기술 스택 목록 |
| grades | Set\<GradeRequestDto\> | private final | 모집 학년 |
| period | PeriodRequestDto | private final | 프로젝트 기간 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| validate() | void | public | 마감일이 종료일 이전인지 검증 |

---

# ProjectCreationRequestDto

[ProjectCommonRequestDto](#projectcommonrequestdto)를 상속한 요청 DTO로, 프로젝트 공고 생성 시 필요한 정보를 담는다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| positions | Set\<PositionInfoCreationRequestDto\> | private final | 포지션별 인원 정보 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| toEntity(User user, ProjectCreationRequestDto dto) | Project | public static | DTO를 Project 엔티티로 변환 |

---

# ProjectUpdateRequestDto

[ProjectCommonRequestDto](#projectcommonrequestdto)를 상속한 요청 DTO로, 프로젝트 공고 수정 시 필요한 정보를 담는다.  

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| positions | Set\<PositionInfoUpdateRequestDto\> | private final | 포지션별 인원 정보 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|

---

# ProjectSearchCondition

프로젝트 공고 검색 조건을 담는 레코드(Record)로, 키워드, 목적, 포지션, 기술 스택, 모집 상태 등의 필드를 포함한다.  
컬렉션은 불변성을 보장하며, null 값이 들어오면 Empty Set으로 초기화한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| keywords | Set\<String\> | public final | 검색 키워드 목록 |
| purpose | ProjectPurpose | public final | 프로젝트 목적 |
| positions | Set\<PositionType\> | public final | 포지션 목록 |
| skills | Set\<Skill\> | public final | 기술 스택 목록 |
| status | RecruitmentStatus | public final | 모집 공고 상태 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|

---

# ProjectDetailResponseDto

[BaseRecruitmentDetailResponseDto](#baseRecruitmentdetailresponsedto)를 상속한 응답 DTO로, 프로젝트 공고의 상세 정보를 담는다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| purpose | ProjectPurpose | private final | 프로젝트 목적 |
| meetingType | MeetingType | private final | 진행 방식 |
| positions | Set\<PositionInfoResponseDto\> | private | 포지션별 인원 정보 |
| skills | Set\<Skill\> | private | 기술 스택 목록 |
| grades | Set\<Integer\> | private | 모집 학년 |
| period | PeriodResponseDto | private final | 프로젝트 기간 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| fromEntity(Project project) | ProjectDetailResponseDto | public static | Project 엔티티를 DTO로 변환 |

---

# ProjectSummaryResponseDto

[BaseRecruitmentSummaryResponseDto](#baserecruitmentsummaryresponsedto)를 상속한 응답 DTO로, 프로젝트 공고의 요약 정보를 담는다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| purpose | ProjectPurpose | private final | 프로젝트 목적 |
| meetingType | MeetingType | private final | 진행 방식 |
| positions | Set\<PositionType\> | private final | 포지션 목록 |
| skills | Set\<Skill\> | private final | 기술 스택 목록 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|

---

# Project 관련 Controller, Service, Repository

---

# ProjectController

프로젝트 공고와 관련된 REST API를 제공하는 컨트롤러로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(ProjectService)을 통해 실제 비즈니스 로직을 수행한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| projectService | ProjectService | private final | 프로젝트 관련 비즈니스 로직 서비스 |
| komoranUtil | KomoranUtil | private final | 키워드 형태소 분석 유틸리티 |

## Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| createProject(ProjectCreationRequestDto requestDto, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Long\>\> | `POST /api/v1/projects` | public | 프로젝트 공고 생성 |
| getProject(Long projectId) | ResponseEntity\<ApiResponse\<ProjectDetailResponseDto\>\> | `GET /api/v1/projects/{projectId}` | public | 프로젝트 공고 상세 조회 |
| getProjectSummaries(String keywords, ProjectPurpose purpose, List\<PositionType\> positions, List\<Skill\> skills, RecruitmentStatus status, Pageable pageable) | ResponseEntity\<ApiResponse\<Page\<ProjectSummaryResponseDto\>\>\> | `GET /api/v1/projects` | public | 프로젝트 공고 목록 조회 |
| updateProject(Long projectId, ProjectUpdateRequestDto requestDto, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Void\>\> | `PUT /api/v1/projects/{projectId}` | public | 프로젝트 공고 수정 |
| deleteProject(Long projectId, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/projects/{projectId}` | public | 프로젝트 공고 삭제 |

---

# ProjectService

프로젝트 공고와 관련된 비즈니스 로직을 정의한 서비스 인터페이스로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
실제 구현체([ProjectServiceImpl](#projectserviceimpl))가 해당 기능을 수행하며, 트랜잭션과 검증 로직을 포함할 수 있다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createProject(Long userId, ProjectCreationRequestDto projectCreationRequestDto) | Long | public | 프로젝트 공고 생성 |
| getProject(Long projectId) | ProjectDetailResponseDto | public | 프로젝트 공고 상세 조회 |
| getProjectSummaries(ProjectSearchCondition condition, Pageable pageable) | Page\<ProjectSummaryResponseDto\> | public | 프로젝트 공고 목록 조회 (조건 및 페이징 적용) |
| getProjectSummariesByIds(List\<Long\> projectIds) | List\<ProjectSummaryResponseDto\> | public | 특정 ID 목록에 해당하는 프로젝트 공고 조회 |
| updateProject(Long userId, Long projectId, ProjectUpdateRequestDto updateDto) | void | public | 프로젝트 공고 수정 |
| deleteProject(Long userId, Long projectId) | void | public | 프로젝트 공고 삭제 |

---

# ProjectServiceImpl

[ProjectService](#projectservice)의 구현체로, 프로젝트 공고 생성, 조회, 수정, 삭제 등 실제 비즈니스 로직을 수행하며, 트랜잭션 처리와 권한 검증, 조회수 증가 등을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| projectRepository | ProjectRepository | private final | 프로젝트 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| teamService | TeamService | private final | 팀 관련 비즈니스 로직 계층 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createProject(Long userId, ProjectCreationRequestDto requestDto) | Long | public | 프로젝트 공고 생성 |
| getProject(Long projectId) | ProjectDetailResponseDto | public | 프로젝트 공고 상세 조회 |
| getProjectSummaries(ProjectSearchCondition condition, Pageable pageable) | Page\<ProjectSummaryResponseDto\> | public | 프로젝트 공고 목록 조회 |
| getProjectSummariesByIds(List\<Long\> projectIds) | List\<ProjectSummaryResponseDto\> | public | 특정 ID 목록 프로젝트 요약 정보 조회 |
| updateProject(Long userId, Long projectId, ProjectUpdateRequestDto updateDto) | void | public | 프로젝트 공고 수정 |
| deleteProject(Long userId, Long projectId) | void | public | 프로젝트 공고 삭제 |

---

# ProjectRepository

Project 엔티티의 데이터 접근 계층으로, 프로젝트 공고와 관련된 User, Position, Skill, Grade 조회 및 조회수 증가 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| findByIdWithUser(Long id) | Optional\<Project\> | public | 프로젝트와 작성자 정보 조회 |
| findPositionsByProjectId(Long id) | Set\<PositionParticipantInfo\> | public | 프로젝트 포지션 조회 |
| findSkillsByProjectId(Long id) | Set\<Skill\> | public | 프로젝트 기술 스택 조회 |
| findGradesByProjectId(Long id) | Set\<Integer\> | public | 프로젝트 모집 학년 조회 |
| increaseViewCount(Long id) | int | public | 조회수 증가 |

---

# ProjectRepositoryCustom

QueryDSL을 활용한 프로젝트 데이터 접근 계층으로, 프로젝트 요약 DTO 조회 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getProjectSummaries(ProjectSearchCondition condition, Pageable pageable) | Page\<ProjectSummaryResponseDto\> | public | 검색 조건에 맞는 프로젝트 요약 DTO를 페이징 조회 |
| getProjectSummariesByIds(List\<Long\> projectIds) | List\<ProjectSummaryResponseDto\> | public | 주어진 Project ID 목록에 해당하는 프로젝트 요약 정보 조회 (입력 순서 보장) |

---

# ProjectRepositoryImpl

[ProjectRepositoryCustom](#projectrepositorycustom) 인터페이스의 구현체로, QueryDSL을 활용하여 프로젝트 요약 DTO 조회 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| queryFactory | JPAQueryFactory | private final | QueryDSL용 JPA 쿼리 팩토리 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getProjectSummaries(ProjectSearchCondition condition, Pageable pageable) | Page\<ProjectSummaryResponseDto\> | public | 조건에 맞는 프로젝트 요약 DTO를 페이징 조회 |
| getProjectSummariesByIds(List\<Long\> projectIds) | List\<ProjectSummaryResponseDto\> | public | 주어진 Project ID 목록에 해당하는 프로젝트 요약 정보 조회 (입력 순서 보장) |
| eqPurpose(ProjectPurpose purpose) | BooleanExpression | private | Project 목적(purpose)과 일치하는 조건 생성 |
| eqStatus(RecruitmentStatus status) | BooleanExpression | private | Project 상태(status)와 일치하는 조건 생성 (CANCELED 제외) |
| containsAnyKeyword(Set\<String\> keywords) | BooleanBuilder | private | 제목(title) 또는 내용(content)에 키워드 중 하나라도 포함되는 조건 생성 |
| containsAnyPosition(Set\<PositionType\> positions) | BooleanExpression | private | Project positions 컬렉션 중 하나라도 지정된 positions에 포함되는 조건 생성 |
| containsAnySkill(Set\<Skill\> skills) | BooleanExpression | private | Project skills 컬렉션 중 하나라도 지정된 skills에 포함되는 조건 생성 |

---

# Assignment 관련

---

# Assignment

[BaseRecruitment](#baserecruitment)를 상속한 과제 공고 엔티티로, 학과, 과목, 인원 정보, 모집 학년 등을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| department | String | private | 학과명 |
| lecture | String | private | 과목명 |
| lectureCode | String | private | 과목 코드 |
| participants | ParticipantInfo | private | 참여자 정보 |
| grades | Set\<Integer\> | private | 모집 학년 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getDepartment() | String | public | 학과명 반환 |
| getLecture() | String | public | 과목명 반환 |
| getLectureCode() | String | public | 과목 코드 반환 |
| getParticipants() | ParticipantInfo | public | 인원 정보 반환 |
| getGrades() | Set\<Integer\> | public | 모집 학년 반환 |
| update(AssignmentUpdateRequestDto dto) | void | public | 과제 공고 정보를 업데이트 |
| decreaseCurrParticipant(PositionType positionType) | void | public | 현재 인원을 1명 감소 |

---

# Assignment DTO

---

# AssignmentCommonRequestDto

[BaseRecruitmentRequestDto](#baserecruitmentrequestdto)를 상속한 과제 공고 공통 추상 요청 DTO로, 학과, 강의 정보, 모집 학년 등의 공통 필드를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| department | String | private | 학과명 |
| lecture | String | private | 과목명 |
| lectureCode | String | private | 과목 코드 |
| grades | Set\<GradeRequestDto\> | private | 모집 학년 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getDepartment() | String | public | 학과명 반환 |
| getLecture() | String | public | 과목명 반환 |
| getLectureCode() | String | public | 과목 코드 반환 |
| getGrades() | Set\<GradeRequestDto\> | public | 모집 학년 반환 |

---

# AssignmentCreationRequestDto

[AssignmentCommonRequestDto](#assignmentcommonrequestdto)를 상속한 과제 공고 생성 요청 DTO로, 모집 인원을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| maxParticipants | Integer | private final | 모집 인원 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getMaxParticipants() | Integer | public | 모집 인원 반환 |
| toEntity(User user, AssignmentCreationRequestDto dto) | Assignment | public | AssignmentCreationRequestDto 객체를 Assignment 객체로 변환 |

---

# AssignmentUpdateRequestDto

[AssignmentCommonRequestDto](#assignmentcommonrequestdto)를 상속한 과제 공고 수정 요청 DTO로, 인원 정보를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| participants | ParticipantInfoUpdateRequestDto | private final | 인원 정보 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getParticipants() | ParticipantInfoUpdateRequestDto | public | 인원 정보 반환 |

---

# AssignmentSearchCondition

사용자가 입력한 키워드, 학년, 모집 상태를 기준으로 과제를 검색하기 위한 조건을 담는 record이다.  
컬렉션은 불변성을 보장하며, null 값이 들어오면 Empty Set으로 초기화한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|------------|-------------|
| keywords | Set\<String\> | private | 검색 키워드 집합 |
| grades | Set\<Integer\> | private | 학년 조건 집합 |
| status | RecruitmentStatus | private | 모집 상태 |

## Operations

| Name | Return Type | Visibility | Description |
|------|------------|------------|-------------|
| keywords() | Set\<String\> | public | 키워드 반환 |
| grades() | Set\<Integer\> | public | 학년 조건 반환 |
| status() | RecruitmentStatus | public | 모집 상태 반환 |

---

# AssignmentDetailResponseDto

[BaseRecruitmentDetailResponseDto](#baseRecruitmentdetailresponsedto)를 상속한 응답 DTO로, 과제 공고의 상세 정보를 담는다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|------------|-------------|
| department | String | private | 학과 |
| lecture | String | private | 강의명 |
| lectureCode | String | private | 강의 코드 |
| grades | Set\<Integer\> | private | 모집 학년 |

## Operations

| Name | Return Type | Visibility | Description |
|------|------------|------------|-------------|
| getDepartment() | String | public | 학과 반환 |
| getLecture() | String | public | 강의명 반환 |
| getLectureCode() | String | public | 강의 코드 반환 |
| getGrades() | Set\<Integer\> | public | 모집 학년 반환 |

---

# AssignmentSummaryResponseDto

[BaseRecruitmentSummaryResponseDto](#baserecruitmentsummaryresponsedto)를 상속한 응답 DTO로, 과제 공고의 요약 정보를 담는다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|------------|-------------|
| department | String | private | 학과 |
| lecture | String | private | 강의명 |
| lectureCode | String | private | 강의 코드 |
| grades | Set\<Integer\> | private | 모집 학년 |

## Operations

| Name | Return Type | Visibility | Description |
|------|------------|------------|-------------|
| getDepartment() | String | public | 학과 반환 |
| getLecture() | String | public | 강의명 반환 |
| getLectureCode() | String | public | 강의 코드 반환 |
| getGrades() | Set\<Integer\> | public | 모집 학년 반환 |

---

# Assignment 관련 Controller, Service, Repository

---

# AssignmentController

과제 공고와 관련된 REST API를 제공하는 컨트롤러로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(AssignmentService)을 통해 실제 비즈니스 로직을 수행한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| assignmentService | AssignmentService | private final | 과제 관련 비즈니스 로직 서비스 |
| komoranUtil | KomoranUtil | private final | 키워드 형태소 분석 유틸리티 |

## Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| createAssignment(AssignmentCreationRequestDto requestDto, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Long\>\> | `POST /api/v1/assignments` | public | 과제 공고 생성 |
| getAssignment(Long assignmentId) | ResponseEntity\<ApiResponse\<AssignmentDetailResponseDto\>\> | `GET /api/v1/assignments/{assignmentId}` | public | 과제 공고 상세 조회 |
| getAssignmentSummaries(String keywords, Set\<Integer\> grades, RecruitmentStatus status, Pageable pageable) | ResponseEntity\<ApiResponse\<Page\<AssignmentSummaryResponseDto\>\>\> | `GET /api/v1/assignments` | public | 과제 공고 목록 조회 |
| updateAssignment(Long assignmentId, AssignmentUpdateRequestDto requestDto, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Void\>\> | `PUT /api/v1/assignments/{assignmentId}` | public | 과제 공고 수정 |
| deleteAssignment(Long assignmentId, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/assignments/{assignmentId}` | public | 과제 공고 삭제 |

---

# AssignmentService

과제 공고와 관련된 비즈니스 로직을 정의한 서비스 인터페이스로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
실제 구현체([AssignmentServiceImpl](#assignmentserviceimpl))가 해당 기능을 수행하며, 트랜잭션과 검증 로직을 포함할 수 있다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createAssignment(AssignmentCreationRequestDto assignmentCreationRequestDto, Long userId) | Long | public | 과제 공고 생성 |
| getAssignment(Long assignmentId) | AssignmentDetailResponseDto | public | 과제 공고 상세 조회 |
| getAssignmentSummaries(AssignmentSearchCondition condition, Pageable pageable) | Page\<AssignmentSummaryResponseDto\> | public | 과제 공고 목록 조회 |
| updateAssignment(Long userId, Long assignmentId, AssignmentUpdateRequestDto updateDto) | void | public | 과제 공고 수정 |
| deleteAssignment(Long userId, Long assignmentId) | void | public | 과제 공고 삭제 |

---

# AssignmentServiceImpl

[AssignmentService](#assignmentservice)의 구현체로, 과제 공고 생성, 조회, 수정, 삭제 등 실제 비즈니스 로직을 수행하며,  
트랜잭션 처리, 권한 검증, 조회수 증가 등을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| assignmentRepository | AssignmentRepository | private final | 과제 공고 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| teamService | TeamService | private final | 팀 관련 비즈니스 로직 계층 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createAssignment(AssignmentCreationRequestDto requestDto, Long userId) | Long | public | 과제 공고 생성 |
| getAssignment(Long assignmentId) | AssignmentDetailResponseDto | public | 과제 공고 상세 조회 |
| getAssignmentSummaries(AssignmentSearchCondition condition, Pageable pageable) | Page\<AssignmentSummaryResponseDto\> | public | 과제 공고 목록 조회 |
| updateAssignment(Long userId, Long assignmentId, AssignmentUpdateRequestDto updateDto) | void | public | 과제 공고 수정 |
| deleteAssignment(Long userId, Long assignmentId) | void | public | 과제 공고 삭제 |

---

# AssignmentRepository

Assignment 엔티티의 데이터 접근 계층으로, 조회수 증가 기능과 커스텀 쿼리 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| increaseViewCount(Long assignmentId) | int | public | 과제 공고 조회수 증가 |

---

# AssignmentRepositoryCustom

QueryDSL을 활용한 Assignment 커스텀 리포지토리로, 과제 요약 DTO 조회 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getAssignmentSummaries(AssignmentSearchCondition condition, Pageable pageable) | Page\<AssignmentSummaryResponseDto\> | public | 검색 조건에 맞는 과제 요약 DTO를 페이징 조회 |
| getAssignmentSummariesByIds(List\<Long\> assignmentIds) | List\<AssignmentSummaryResponseDto\> | public | 주어진 Assignment ID 목록에 해당하는 과제 요약 정보 조회 (입력 순서 보장) |

---

# AssignmentRepositoryImpl

[AssignmentRepositoryCustom](#assignmentrepositorycustom) 인터페이스의 구현체로, QueryDSL을 활용하여 과제 요약 DTO 조회 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| queryFactory | JPAQueryFactory | private final | QueryDSL용 JPA 쿼리 팩토리 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getAssignmentSummaries(AssignmentSearchCondition condition, Pageable pageable) | Page\<AssignmentSummaryResponseDto\> | public | 조건에 맞는 과제 요약 DTO를 페이징 조회 |
| getAssignmentSummariesByIds(List\<Long\> assignmentIds) | List\<AssignmentSummaryResponseDto\> | public | 주어진 Assignment ID 목록에 해당하는 과제 요약 정보 조회 (입력 순서 보장) |
| eqStatus(RecruitmentStatus status) | BooleanExpression | private | Assignment 상태(status)와 일치하는 조건 생성 (CANCELED 제외) |
| containsAnyKeyword(Set\<String\> keywords) | BooleanBuilder | private | 제목(title) 또는 내용(content)에 키워드 중 하나라도 포함되는 조건 생성 |
| containsAnyGrade(Set\<Integer\> grades) | BooleanExpression | private | Assignment 모집 학년(grades) 중 하나라도 지정된 학년에 포함되는 조건 생성 |

---

# Study 관련

---

# Study

[BaseRecruitment](#baserecruitment)를 상속한 스터디 공고 엔티티로, 인원 정보, 활동 기간, 기술 스택 정보를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| participants | ParticipantInfo | private | 인원 정보 |
| period | Period | private | 스터디 활동 기간 |
| skills | Set\<Skill\> | private | 스터디 기술 스택 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getParticipants() | ParticipantInfo | public | 인원 정보 반환 |
| getPeriod() | Period | public | 스터디 활동 기간 반환 |
| getSkills() | Set\<Skill\> | public | 기술 스택 반환 |
| update(StudyUpdateRequestDto dto) | void | public | 스터디 공고 정보를 업데이트 |
| decreaseCurrParticipant(PositionType positionType) | void | public | 현재 인원을 1명 감소 |

---

# Study DTO

---

# StudyCommonRequestDto

[BaseRecruitmentRequestDto](#baserecruitmentrequestdto)를 상속한 스터디 공고 공통 추상 요청 DTO로, 진행 기간과 기술 스택 정보를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| period | PeriodRequestDto | private final | 진행 기간 정보 |
| skills | Set\<Skill\> | private final | 기술 스택 정보 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getPeriod() | PeriodRequestDto | public | 진행 기간 정보 반환 |
| getSkills() | Set\<Skill\> | public | 기술 스택 정보 반환 |
| validate() | void | public | 진행 기간과 마감일의 유효성을 검증 |

---

# StudyCreationRequestDto

[StudyCommonRequestDto](#studycommonrequestdto)를 상속한 스터디 공고 생성 요청 DTO로, 모집 인원 정보를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| maxParticipants | Integer | private final | 모집 인원 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| toEntity(User user, StudyCreationRequestDto dto) | Study | public static | StudyCreationRequestDto 객체를 Study 엔티티로 변환 |
| getMaxParticipants() | Integer | public | 모집 인원 반환 |

---

# StudyUpdateRequestDto

[StudyCommonRequestDto](#studycommonrequestdto)를 상속한 스터디 공고 수정 요청 DTO로, 인원 정보를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| participants | ParticipantInfoUpdateRequestDto | private final | 인원 정보 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getParticipants() | ParticipantInfoUpdateRequestDto | public | 인원 정보 반환 |

---

# StudyDetailResponseDto

[BaseRecruitmentDetailResponseDto](#baserecruitmentdetailresponsedto)를 상속한 스터디 공고 상세 응답 DTO로, 인원 정보, 진행 기간, 기술 스택 정보를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| participants | ParticipantInfoResponseDto | private final | 인원 정보 반환 |
| period | PeriodResponseDto | private final | 진행 기간 정보 반환 |
| skills | Set\<Skill\> | private | 기술 스택 정보 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| fromEntity(Study study) | StudyResponseDto | public static | Study 엔티티를 StudyResponseDto 객체로 변환 |
| getParticipants() | ParticipantInfoResponseDto | public | 인원 정보 반환 |
| getPeriod() | PeriodResponseDto | public | 진행 기간 정보 반환 |
| getSkills() | Set\<Skill\> | public | 기술 스택 정보 반환 |
| setSkills(Set\<Skill\> skills) | void | public | 기술 스택 정보 수정 |

---

# StudySummaryResponseDto

[BaseRecruitmentSummaryResponseDto](#baserecruitmentsummaryresponsedto)를 상속한 응답 DTO로, 스터디 공고의 요약 정보를 담는다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| skills | Set\<Skill\> | private final | 기술 스택 목록 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| getSkills() | Set\<Skill\> | public | 기술 스택 목록 반환 |

---

# StudySearchCondition

스터디 공고 검색 조건을 담는 레코드(Record)로, 키워드, 기술 스택, 모집 상태 등의 필드를 포함한다.  
컬렉션은 불변성을 보장하며, null 값이 들어오면 Empty Set으로 초기화한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| keywords | Set\<String\> | public final | 검색 키워드 목록 |
| skills | Set\<Skill\> | public final | 기술 스택 목록 |
| status | RecruitmentStatus | public final | 모집 공고 상태 |

## Operations

| Name | Return Type | Visibility | Description |
|------|------------|-----------|-------------|
| keywords() | Set\<String\> | public | 키워드 반환 |
| skills() | Set\<Skill\> | public | 기술 스택 조건 반환 |
| status() | RecruitmentStatus | public | 모집 상태 반환 |

---

Study 관련 Controller, Service, Repository

---

# StudyController

스터디 공고와 관련된 REST API를 제공하는 컨트롤러로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(StudyService)을 통해 실제 비즈니스 로직을 수행한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| studyService | StudyService | private final | 스터디 관련 비즈니스 로직 서비스 |
| komoranUtil | KomoranUtil | private final | 키워드 형태소 분석 유틸리티 |

## Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| createStudy(StudyCreationRequestDto requestDto, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Long\>\> | `POST /api/v1/studies` | public | 스터디 공고 생성 |
| getStudy(Long studyId) | ResponseEntity\<ApiResponse\<StudyDetailResponseDto\>\> | `GET /api/v1/studies/{studyId}` | public | 스터디 공고 상세 조회 |
| getStudySummaries(String keywords, List\<Skill\> skills, RecruitmentStatus status, Pageable pageable) | ResponseEntity\<ApiResponse\<Page\<StudySummaryResponseDto\>\>\> | `GET /api/v1/studies` | public | 스터디 공고 목록 조회 |
| updateStudy(Long studyId, StudyUpdateRequestDto requestDto, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Void\>\> | `PUT /api/v1/studies/{studyId}` | public | 스터디 공고 수정 |
| deleteStudy(Long studyId, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/studies/{studyId}` | public | 스터디 공고 삭제 |

---

# StudyService

스터디 공고와 관련된 비즈니스 로직을 정의한 서비스 인터페이스로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
실제 구현체([StudyServiceImpl](#studyserviceimpl))가 해당 기능을 수행하며, 트랜잭션과 검증 로직을 포함할 수 있다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createStudy(Long userId, StudyCreationRequestDto studyCreationRequestDto) | Long | public | 스터디 공고 생성 |
| getStudy(Long studyId) | StudyDetailResponseDto | public | 스터디 공고 상세 조회 |
| getStudySummaries(StudySearchCondition condition, Pageable pageable) | Page\<StudySummaryResponseDto\> | public | 스터디 공고 목록 조회 (조건 및 페이징 적용) |
| getStudySummariesByIds(List\<Long\> studyIds) | List\<StudySummaryResponseDto\> | public | 특정 ID 목록에 해당하는 스터디 공고 조회 |
| updateStudy(Long userId, Long studyId, StudyUpdateRequestDto updateDto) | void | public | 스터디 공고 수정 |
| deleteStudy(Long userId, Long studyId) | void | public | 스터디 공고 삭제 |

---

# StudyServiceImpl

[StudyService](#studyservice)의 구현체로, 스터디 공고 생성, 조회, 수정, 삭제 등 실제 비즈니스 로직을 수행하며, 트랜잭션 처리와 권한 검증, 조회수 증가 등을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| studyRepository | StudyRepository | private final | 스터디 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| teamService | TeamService | private final | 팀 관련 비즈니스 로직 계층 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createStudy(Long userId, StudyCreationRequestDto requestDto) | Long | public | 스터디 공고 생성 |
| getStudy(Long studyId) | StudyDetailResponseDto | public | 스터디 공고 상세 조회 |
| getStudySummaries(StudySearchCondition condition, Pageable pageable) | Page\<StudySummaryResponseDto\> | public | 스터디 공고 목록 조회 |
| getStudySummariesByIds(List\<Long\> studyIds) | List\<StudySummaryResponseDto\> | public | 특정 ID 목록 스터디 요약 정보 조회 |
| updateStudy(Long userId, Long studyId, StudyUpdateRequestDto updateDto) | void | public | 스터디 공고 수정 |
| deleteStudy(Long userId, Long studyId) | void | public | 스터디 공고 삭제 |

---

# StudyRepository

Study 엔티티의 데이터 접근 계층으로, 조회수 증가 기능과 커스텀 쿼리 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| increaseViewCount(Long id) | int | public | 조회수 증가 |

---

# StudyRepositoryCustom

QueryDSL을 활용한 스터디 데이터 접근 계층으로, 스터디 요약 DTO 조회 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getStudySummaries(StudySearchCondition condition, Pageable pageable) | Page\<StudySummaryResponseDto\> | public | 검색 조건에 맞는 스터디 요약 DTO를 페이징 조회 |
| getStudySummariesByIds(List\<Long\> studyIds) | List\<StudySummaryResponseDto\> | public | 주어진 Study ID 목록에 해당하는 스터디 요약 정보 조회 (입력 순서 보장) |

---

# StudyRepositoryImpl

[StudyRepositoryCustom](#studyrepositorycustom) 인터페이스의 구현체로, QueryDSL을 활용하여 스터디 요약 DTO 조회 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| queryFactory | JPAQueryFactory | private final | QueryDSL용 JPA 쿼리 팩토리 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getStudySummaries(StudySearchCondition condition, Pageable pageable) | Page\<StudySummaryResponseDto\> | public | 조건에 맞는 스터디 요약 DTO를 페이징 조회 |
| getStudySummariesByIds(List\<Long\> studyIds) | List\<StudySummaryResponseDto\> | public | 주어진 Study ID 목록에 해당하는 스터디 요약 정보 조회 (입력 순서 보장) |
| eqStatus(RecruitmentStatus status) | BooleanExpression | private | Study 상태(status)와 일치하는 조건 생성 (CANCELED 제외) |
| containsAnyKeyword(Set\<String\> keywords) | BooleanBuilder | private | 제목(title) 또는 내용(content)에 키워드 중 하나라도 포함되는 조건 생성 |
| containsAnySkill(Set\<Skill\> skills) | BooleanExpression | private | Study skills 컬렉션 중 하나라도 지정된 skills에 포함되는 조건 생성 |

---

# Bookmark 관련

---

# Bookmark

사용자가 공고를 찜할 때의 정보를 담는 엔티티로, 생성일자를 자동으로 기록하며 User와 BaseRecruitment를 참조한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| id | Long | private | 찜의 고유 식별자 (PK) |
| user | User | private | 찜한 사용자 |
| recruitment | BaseRecruitment | private | 북마크 대상 공고 |
| bookmarkedAt | LocalDateTime | private | 북마크 생성일자 (자동 생성, 수정 불가) |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getId() | Long | public | 찜 ID 반환 |
| getUser() | User | public | 찜한 사용자 반환 |
| getRecruitment() | BaseRecruitment | public | 찜한 공고 반환 |
| getBookmarkedAt() | LocalDateTime | public | 찜 생성일자 반환 |

---

# BookmarkController

사용자의 북마크(찜) 관련 REST API를 제공하는 컨트롤러로, 생성, 조회, 삭제 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(BookmarkService)을 통해 실제 비즈니스 로직을 수행한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| bookmarkService | BookmarkService | private final | 북마크(찜) 관련 비즈니스 로직 서비스 |

## Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| createBookmark(Long recruitmentId, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Long\>\> | `POST /api/v1/bookmarks/recruitments/{recruitmentId}` | public | 공고를 찜 목록에 추가 |
| getBookmarkedProjectSummariesByUserId(Pageable pageable, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Page\<ProjectSummaryResponseDto\>\>\> | `GET /api/v1/bookmarks/projects` | public | 사용자가 찜한 프로젝트 공고 목록 조회 |
| getBookmarkedAssignmentSummariesByUserId(Pageable pageable, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Page\<AssignmentSummaryResponseDto\>\>\> | `GET /api/v1/bookmarks/assignments` | public | 사용자가 찜한 과제 공고 목록 조회 |
| getBookmarkedStudySummariesByUserId(Pageable pageable, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Page\<StudySummaryResponseDto\>\>\> | `GET /api/v1/bookmarks/studies` | public | 사용자가 찜한 스터디 공고 목록 조회 |
| deleteBookmark(Long bookmarkId, CustomUserDetails userDetails) | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/bookmarks/{bookmarkId}` | public | 찜 취소 |

---

# BookmarkService

사용자의 북마크(찜) 관련 비즈니스 로직을 정의한 서비스 인터페이스로, 생성, 조회, 삭제 기능을 포함한다.  
실제 구현체([BookmarkServiceImpl](#bookmarkserviceimpl))가 해당 기능을 수행하며, 트랜잭션과 검증 로직을 포함할 수 있다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createBookmark(Long userId, Long recruitmentId) | Long | public | 공고를 찜 목록에 추가 |
| getBookmarkedProjectsByUserId(Long userId, Pageable pageable) | Page\<ProjectSummaryResponseDto\> | public | 사용자가 찜한 프로젝트 공고 목록 조회 |
| getBookmarkedAssignmentsByUserId(Long userId, Pageable pageable) | Page\<AssignmentSummaryResponseDto\> | public | 사용자가 찜한 과제 공고 목록 조회 |
| getBookmarkedStudiesByUserId(Long userId, Pageable pageable) | Page\<StudySummaryResponseDto\> | public | 사용자가 찜한 스터디 공고 목록 조회 |
| deleteBookmark(Long userId, Long bookmarkId) | void | public | 찜 취소 |

---

# BookmarkServiceImpl

[BookmarkService](#bookmarkservice)의 구현체로, 사용자의 북마크(찜) 관련 실제 비즈니스 로직을 수행하며, 트랜잭션 처리와 권한 검증, 중복 체크 등을 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| bookmarkRepository | BookmarkRepository | private final | 북마크 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| recruitmentService | RecruitmentService | private final | 공고 관련 비즈니스 로직 계층 |
| projectService | ProjectService | private final | 프로젝트 공고 관련 비즈니스 로직 계층 |
| assignmentService | AssignmentService | private final | 과제 공고 관련 비즈니스 로직 계층 |
| studyService | StudyService | private final | 스터디 공고 관련 비즈니스 로직 계층 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createBookmark(Long userId, Long recruitmentId) | Long | public | 공고를 찜 목록에 추가. 이미 찜한 경우 예외 발생 |
| getBookmarkedProjectsByUserId(Long userId, Pageable pageable) | Page\<ProjectSummaryResponseDto\> | public | 사용자가 찜한 프로젝트 공고 목록 조회 |
| getBookmarkedAssignmentsByUserId(Long userId, Pageable pageable) | Page\<AssignmentSummaryResponseDto\> | public | 사용자가 찜한 과제 공고 목록 조회 |
| getBookmarkedStudiesByUserId(Long userId, Pageable pageable) | Page\<StudySummaryResponseDto\> | public | 사용자가 찜한 스터디 공고 목록 조회 |
| deleteBookmark(Long userId, Long bookmarkId) | void | public | 찜 취소 |

---

# BookmarkRepository

Bookmark 엔티티의 데이터 접근 계층으로, 사용자의 북마크 관련 CRUD 및 조회 기능을 제공한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| existsByUserIdAndRecruitmentId(Long userId, Long recruitmentId) | boolean | public | 사용자가 특정 공고를 이미 찜했는지 여부 확인 |
| findBookmarkedRecruitmentIdsByUserId(Long userId, RecruitmentCategory category, Pageable pageable) | Page\<Long\> | public | 특정 사용자가 찜한 프로젝트/과제/스터디 공고 ID 목록 조회 (최신순) |

---

# Application

사용자가 프로젝트, 과제, 스터디 공고에 지원할 때 생성되는 엔티티로, 지원 정보와 상태를 관리한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| id | Long | private | 지원 ID (PK) |
| applicant | User | private | 지원자 정보 |
| recruitment | BaseRecruitment | private | 지원 대상 공고 |
| content | String | private | 지원 내용 |
| grade | Integer | private | 지원자의 학년 |
| meetingType | MeetingType | private | 진행 방식 |
| position | PositionType | private | 프로젝트 공고 지원 시 포지션 정보 (그 외는 null) |
| skills | Set\<Skill\> | private | 프로젝트 공고 지원 시 보유 기술 스택 (그 외는 empty set) |
| status | ApplicationStatus | private | 지원 상태 (기본값 `SUBMITTED`) |
| createdAt | LocalDateTime | private | 생성일 |
| isDeleted | boolean | private | 논리적 삭제 여부 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getId() | Long | public | 지원 ID 반환 |
| getApplicant() | User | public | 지원자 정보 반환 |
| getRecruitment() | BaseRecruitment | public | 지원 대상 공고 반환 |
| getContent() | String | public | 지원 내용 반환 |
| getGrade() | Integer | public | 지원자의 학년 반환 |
| getMeetingType() | MeetingType | public | 진행 방식 반환 |
| getPosition() | PositionType | public | 포지션 반환 (프로젝트 공고 지원) |
| getSkills() | Set\<Skill\> | public | 기술 스택 반환 (프로젝트 공고 지원) |
| getStatus() | ApplicationStatus | public | 지원 상태 반환 |
| getCreatedAt() | LocalDateTime | public | 생성일 반환 |
| isDeleted() | boolean | public | 논리적 삭제 여부 반환 |
| accept() | void | public | 지원 상태를 `ACCEPTED`로 변경 |
| reject() | void | public | 지원 상태를 `REJECTED`로 변경 |
| delete() | void | public | 논리적 삭제 처리 |

---

# ApplicationStatus

지원 상태를 정의하는 **열거형(Enum)** 으로, 각 상태의 설명(`desc`)과 해당 Enum 값을 제공한다.

## Enum Values

| Value | Description |
|-------|-------------|
| SUBMITTED | 대기중 |
| ACCEPTED | 수락됨 |
| REJECTED | 거절됨 |
| CLOSED | 모집종료 |
| CANCELED | 모집취소 |

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| desc | String | private | 지원 상태 설명 (예: "대기중") |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getDesc() | String | public | 상태 설명 반환 |
| getName() | String | public | Enum 이름 반환 |

---

# Application DTO

---

# ApplicationCommonRequestDto

지원서 작성 시 공통적으로 사용되는 추상 요청 DTO 클래스
모든 공고(프로젝트, 과제, 스터디)에 공통적으로 필요한 필드를 포함하며, 실제 엔티티 변환 로직은 하위 클래스에서 구현한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| category | RecruitmentCategory | private final | 지원하는 공고의 카테고리 |
| meetingType | MeetingType | protected | 선호하는 진행 방식 |
| grade | Integer | protected | 지원자의 학년 (1~4) |
| content | String | protected | 지원서 본문 내용 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getCategory() | RecruitmentCategory | public | 지원 공고 카테고리 반환 |
| toEntity(User applicant, BaseRecruitment recruitment) | Application | public abstract | DTO를 Application 엔티티로 변환 <br> 하위 클래스에서 구현 필요 |

---

# ApplicationProjectRequestDto

[ApplicationCommonRequestDto](#applicationcommonrequestdto)를 상속한 요청 DTO로, 프로젝트 공고 지원서 작성 시 필요한 정보를 담는다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| position | PositionType | private final | 지원 포지션 |
| skills | Set\<Skill\> | private final | 지원 기술 스택 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| getPosition() | PositionType | public | 지원 포지션 반환 |
| getSkills() | Set\<Skill\> | public | 지원 기술 스택 반환 |
| toEntity(User applicant, BaseRecruitment recruitment) | Application | public | DTO를 Application 엔티티로 변환 |

---

# ApplicationSimpleRequestDto

[ApplicationCommonRequestDto](#applicationcommonrequestdto)를 상속한 요청 DTO로, 과제 및 스터디 공고 지원서 작성 시 사용된다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| toEntity(User applicant, BaseRecruitment recruitment) | Application | public | DTO를 Application 엔티티로 변환 |

---

# ApplicationCommonResponseDto

지원 공고 응답 시 공통적으로 사용되는 추상 응답 DTO로, 지원서 ID, 공고 정보, 진행 방식 등의 데이터를 포함한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| applicationId | Long | private | 지원서 ID |
| recruitmentId | Long | private | 공고 ID |
| recruitmentTitle | String | private | 공고 제목 |
| recruitmentDeadline | LocalDateTime | private | 공고 마감일 (`yyyy.MM.dd`) |
| meetingType | MeetingType | private final | 선호하는 진행 방식 |
| grade | Integer | private final | 지원 학년 |
| content | String | private final | 지원서 본문 내용 |
| status | ApplicationStatus | private final | 지원서 상태 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| getApplicationId() | Long | public | 지원서 ID 반환 |
| getRecruitmentId() | Long | public | 공고 ID 반환 |
| getRecruitmentTitle() | String | public | 공고 제목 반환 |
| getRecruitmentDeadline() | LocalDateTime | public | 공고 마감일 반환 |
| getMeetingType() | MeetingType | public | 선호 진행 방식 반환 |
| getGrade() | Integer | public | 지원 학년 반환 |
| getContent() | String | public | 지원서 본문 내용 반환 |
| getStatus() | ApplicationStatus | public | 지원서 상태 반환 |

---

# ApplicationProjectResponseDto

[ApplicationCommonResponseDto](#applicationcommonresponsedto)를 상속한 응답 DTO로, 프로젝트 공고 지원서에 사용된다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| position | PositionType | private final | 지원 포지션 |
| skills | Set\<Skill\> | private final | 지원 기술 스택 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| fromEntity(Application entity) | ApplicationProjectResponseDto | public static | Application 엔티티를 DTO로 변환 |
| getPosition() | PositionType | public | 지원 포지션 반환 |
| getSkills() | Set\<Skill\> | public | 지원 기술 스택 반환 |

---

# ApplicationSimpleResponseDto

[ApplicationCommonResponseDto](#applicationcommonresponsedto)를 상속한 응답 DTO로, 과제 및 스터디 공고 지원서에 사용된다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| fromEntity(Application entity) | ApplicationSimpleResponseDto | public static | Application 엔티티를 DTO로 변환 |

---




# Util

---

# KomoranUtil

텍스트에서 명사를 추출하기 위한 형태소 분석 유틸리티 클래스

[KOMORAN 공식 문서](https://docs.komoran.kr/)

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| USER_DIC_PATH | String | private static final | 사용자 사전 파일 경로 |
| komoran | Komoran | private static final | Komoran 형태소 분석기 인스턴스 |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| createKomoran() | Komoran | private static | Komoran 인스턴스를 생성하고 사용자 사전을 적용 |
| getNouns(String target) | Set\<String\> | public | 입력 문자열에서 명사 집합을 추출 |

---

# Converter

---

# StringToEnumConverterFactory

모든 Enum 타입에 대해 문자열(String) → Enum 변환을 지원하는 컨버터 팩토리 클래스  
Spring Controller에서 쿼리 파라미터로 전달된 문자열을 Enum으로 자동 매핑할 때 사용된다.  

Spring 인터페이스 `ConverterFactory<String, Enum>`를 구현하여 동작한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

## Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| getConverter(Class\<T\> targetType) | \<T extends Enum\> Converter\<String, T\> | public | 지정한 Enum 타입에 대한 StringToEnumConverter 반환 |

---

# StringToEnumConverterFactory.StringToEnumConverter\<T extends Enum\>

StringToEnumConverterFactory의 내부 클래스로, 실제 문자열 → Enum 변환 로직을 수행한다.

## Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| enumType | Class\<T\> | private | 변환 대상 Enum 타입 |

## Operations

| Name | Return Type | Visibility | Description |
|------|:-----------:|-----------|-------------|
| convert(String source) | T | public | 입력 문자열을 Enum 값으로 변환 <br> 소문자, 공백, 하이픈(-) 등을 Enum 형식에 맞게 처리 |
