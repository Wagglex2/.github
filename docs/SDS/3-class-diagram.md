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

