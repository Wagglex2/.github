# 3. Class Diagram

본 장에서는 Waggle 시스템의 클래스 다이어그램을 도메인별 및 기능별로 분류하여 정리한다. 각 다이어그램은 시스템의 구조를 명확히 이해할 수 있도록 구성되었으며, 엔티티 간의 관계, 계층 구조, 데이터 흐름 등을 시각적으로 표현한다.


## 3.1 시스템 전반 클래스 구조

<img width="1502" height="878" alt="01-overview" src="https://github.com/user-attachments/assets/daf426eb-a854-4294-8c91-678b94cb564f" />

전체 시스템 전반의 엔티티 간 상속 및 연관 관계를 종합적으로 나타낸다. 
시스템의 핵심 엔티티인 User, BaseRecruitment, 그리고 이를 상속하는 Project, Study, Assignment 등의 관계를 한눈에 파악할 수 있다. 
또한 Application, Team, TeamMember, Bookmark, Review 등 주요 엔티티들의 연관 관계를 확인할 수 있도록 구성되었다.

---

## 3.2 공통 클래스 구조

여러 도메인에서 공통으로 사용되는 클래스들의 구조를 보여준다. BaseRecruitment 엔티티와 같은 공통 엔티티, ParticipantInfo, Period, PositionParticipantInfo와 같은 값 객체, 그리고 PositionType, RecruitmentCategory, RecruitmentStatus, Skill 등의 열거형을 포함한다. 또한 공통 서비스, 레포지토리, DTO, 예외 처리, 검증기, 스케줄러 등 전역적으로 활용되는 클래스들의 구조와 관계를 표현한다.

#### BaseRecruitment

프로젝트, 과제, 스터디 공고 엔티티들이 상속받는 **공통 추상 클래스**로,  
공통 속성과 상태 관리 로직을 정의하며 상속 구조에서 기반 엔티티 역할을 한다.

##### Attributes

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

##### Operations

| Name | Return Type | Visibility      | Description |
|-----|-----------|-----------------|--------------|
| `getId()` | Long | public          | 공고 식별자 반환 |
| `getUser()` | User | public          | 공고 등록자 반환 |
| `getCategory()` | RecruitmentCategory | public          | 공고 카테고리 반환 |
| `getTitle()` | String | public          | 공고 제목 반환 |
| `getContent()` | String | public          | 공고 내용 반환 |
| `getDeadline()` | LocalDateTime | public          | 모집 마감일 반환 |
| `getStatus()` | RecruitmentStatus | public          | 공고 상태 반환 |
| `getCreatedAt()` | LocalDateTime | public          | 생성 시각 반환 |
| `getUpdatedAt()` | LocalDateTime | public          | 수정 시각 반환 |
| `getViewCount()` | int | public          | 조회수 반환 |
| `update(String title, String content, LocalDateTime deadline)` | void | protected       | 제목, 내용, 마감일 수정 |
| `changeStatusByDeadline()` | void | public          | 마감일에 따라 상태를 자동으로 변경 |
| `cancel()` | void | public          | 공고를 삭제 상태(CANCELED)로 변경 |
| `decreaseCurrParticipant(PositionType positionType)` | void | public abstract | 특정 포지션의 현재 참여 인원을 1명 감소 |

---

#### RecruitmentCategory

프로젝트 내 모집 공고의 유형을 정의하는 **열거형(Enum)** 으로, 각 카테고리의 설명(`desc`)과 해당 Enum 값을 제공한다.

##### Enum Values

| Value | Description |
|-------|-------------|
| `PROJECT` | 프로젝트 |
| `ASSIGNMENT` | 과제 |
| `STUDY` | 스터디 |

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| desc | String | private | 모집 공고 카테고리 설명 (예: "프로젝트") |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getDesc()` | String | public | 카테고리 설명 반환 |
| `getName()` | String | public | Enum 이름 반환 |

---

#### RecruitmentStatus

모집 공고의 현재 상태를 정의하는 **열거형(Enum)** 으로, 각 상태의 설명(`desc`)과 해당 Enum 값을 제공한다.

##### Enum Values

| Value | Description |
|-------|-------------|
| `RECRUITING` | 모집 중 |
| `CLOSED` | 마감 |
| `CANCELED` | 취소 |

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| desc | String | private | 모집 상태 설명 (예: "모집 중") |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getDesc()` | String | public | 상태 설명 반환 |
| `getName()` | String | public | Enum 이름 반환 |

---

#### PositionType

프로젝트 내 포지션 유형을 정의하는 **열거형(Enum)** 으로, 각 포지션의 설명(desc)과 해당 Enum 값을 제공한다.

##### Enum Values

| Value | Description |
|------|-------------|
| `FULL_STACK` | 풀스택 |
| `FRONT_END` | 프론트엔드 |
| `BACK_END` | 백엔드 |
| `DATA` | 데이터 |
| `AI` | AI |
| `GAME` | 게임 |
| `PLANNER` | 기획 |
| `DESIGNER` | 디자인 |

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| desc | String | private | 포지션 설명 (예: "프론트엔드") |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getDesc()` | String | public | 포지션 설명 반환 |
| `getName()` | String | public | Enum 이름 반환 (FULL_STACK, FRONT_END 등) |

---

#### Skill

사용 가능한 기술 스택을 정의하는 **열거형(Enum)** 으로, 각 기술의 설명(`desc`)과 해당 Enum 값을 제공한다.

##### Enum Values

| Value | Description |
|-------|-------------|
| `HTML` | HTML |
| `CSS` | CSS |
| `JAVASCRIPT` | JavaScript |
| `JAVA` | Java |
| `KOTLIN` | Kotlin |
| `PYTHON` | Python |
| `SWIFT` | Swift |
| `C_CPP` | C/C++ |
| `CSHARP` | C#### |
| `TYPESCRIPT` | TypeScript |
| `REACT` | React |
| `NODE_JS` | Node.js |
| `EXPRESS` | Express |
| `VUE_JS` | Vue.js |
| `NEXT_JS` | Next.js |
| `SPRING_BOOT` | Spring Boot |
| `DJANGO` | Django |
| `PANDAS` | Pandas |
| `SCIKIT_LEARN` | scikit-learn |
| `PYTORCH` | PyTorch |
| `TENSORFLOW` | TensorFlow |
| `FLUTTER` | Flutter |
| `MYSQL` | MySQL |
| `REDIS` | Redis |
| `MONGODB` | MongoDB |
| `POSTGRESQL` | PostgreSQL |
| `GIT_GITHUB` | Git/GitHub |
| `GITHUB_ACTIONS` | GitHub Actions |
| `FIGMA` | Figma |
| `NOTION` | Notion |
| `JIRA` | Jira |
| `DOCKER` | Docker |
| `UNITY` | Unity |
| `UNREAL` | Unreal |

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| desc | String | private | 기술 이름 설명 (예: "Spring Boot") |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getDesc()` | String | public | 기술 설명 반환 |
| `getName()` | String | public | Enum 이름 반환 |

---

#### ParticipantInfo

모집 참여 인원 정보를 담는 **임베디드 클래스**로,  
최대 참여 인원과 현재 참여 인원을 관리하고, 참여 인원 증가/감소 여부를 판단한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| maxParticipants | int | private | 최대 참여 인원 |
| currParticipants | int | private | 현재 참여 인원 (기본값 0) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getMaxParticipants()` | int | public | 최대 참여 인원 반환 |
| `getCurrParticipants()` | int | public | 현재 참여 인원 반환 |
| `isFull()` | boolean | public | 현재 참여 인원이 최대 참여 인원에 도달했는지 확인 |
| `incrementCurrParticipants()` | void | public | 현재 참여 인원을 1 증가 |
| `decreaseCurrParticipants()` | void | public | 현재 참여 인원을 1 감소, 0 이하 시 예외 발생 |

---

#### PositionParticipantInfo

특정 포지션별 참여 인원 정보를 담는 **임베디드 클래스**로,  
포지션 타입과 해당 포지션의 참여 인원 정보를 관리한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| position | PositionType | private | 포지션 타입 (예: 프론트엔드, 백엔드 등) |
| participantInfo | ParticipantInfo | private | 해당 포지션의 참여 인원 정보 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getPosition()` | PositionType | public | 포지션 타입 반환 |
| `getParticipantInfo()` | ParticipantInfo | public | 해당 포지션 참여 인원 정보 반환 |

---

#### Period

기간 정보를 표현하는 **임베디드(Embeddable) 레코드 클래스**로,  
시작일(`startDate`)과 종료일(`endDate`)을 관리한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| startDate | LocalDate | private | 시작 날짜 |
| endDate | LocalDate | private | 종료 날짜 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `startDate()` | LocalDate | public | 시작 날짜 반환 |
| `endDate()` | LocalDate | public | 종료 날짜 반환 |

---

#### BaseRecruitmentRequestDto

모집 공고 작성 요청의 공통 필드를 정의하는 **추상 DTO 클래스**로, 제목, 본문, 마감일 정보를 관리한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| title | String | private final | 공고 제목 (최대 255자, 필수 입력) |
| content | String | private final | 공고 본문 내용 (최대 4096자, 필수 입력) |
| deadline | LocalDateTime | private final | 공고 마감일 (오늘 이후 날짜, 필수 입력) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getTitle()` | String | public | 제목 반환 |
| `getContent()` | String | public | 본문 내용 반환 |
| `getDeadline()` | LocalDateTime | public | 마감일 반환 |

---

#### GradeRequestDto

모집 우대 학년 정보를 전달하는 **DTO record 클래스**로, 1~4학년 범위를 검증한다.

##### Attributes

| Name | Type | Visibility | Description           |
|------|------|-------------|-----------------------|
| grade | Integer | private | 우대 학년 (필수, 1~4 범위 검증) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `grade()` | Integer | public | 학년값 반환 |

---

#### PositionInfoCreationRequestDto

포지션 생성 요청을 처리하기 위한 DTO로, 사용자가 입력한 포지션 정보와 해당 포지션의 최대 모집 인원수를 전달한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| position | PositionType | private | 포지션 (필수 입력) |
| maxParticipants | int | private | 모집 인원수 (최소 1 이상, 필수 입력) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `to(PositionInfoCreationRequestDto dto)` | PositionParticipantInfo | public static | PositionInfoCreationRequestDto 객체를 PositionParticipantInfo 객체로 변환한다. |

---

#### ParticipantInfoUpdateRequestDto

공고의 모집 인원 및 현재 인원 정보를 수정하기 위한 DTO로, 현재 인원이 모집 인원을 초과할 수 없다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| maxParticipants | int | private | 모집 인원수 (최소 1 이상, 필수 입력) |
| currParticipants | int | private | 현재 인원수 (최소 0 이상, 모집 인원수 초과 불가, 필수 입력) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `to(ParticipantInfoUpdateRequestDto dto)` | ParticipantInfo | public static | ParticipantInfoUpdateRequestDto 객체를 ParticipantInfo 객체로 변환한다. |

---

#### PositionInfoUpdateRequestDto

특정 포지션의 인원 정보를 수정하기 위한 DTO로, 포지션 유형과 해당 포지션의 인원 정보를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| position | PositionType | private | 포지션 (필수 입력) |
| participantInfo | ParticipantInfoUpdateRequestDto | private | 포지션별 인원 정보 (유효성 검사 포함, 필수 입력) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `to(PositionInfoUpdateRequestDto dto)` | PositionParticipantInfo | public static | PositionInfoUpdateRequestDto 객체를 PositionParticipantInfo 객체로 변환한다. |

---

#### PeriodRequestDto

기간 정보를 전달하기 위한 DTO로, 시작일과 종료일을 포함하며 날짜 유효성을 검사한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| startDate | LocalDate | private | 시작일 (필수 입력) |
| endDate | LocalDate | private | 종료일 (오늘 이후, 시작일 이전 불가, 필수 입력) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `to(PeriodRequestDto dto)` | Period | public static | PeriodRequestDto 객체를 Period 객체로 변환한다. |

---

#### BaseRecruitmentDetailResponseDto

공고 상세 조회 시 공통적으로 사용되는 응답 DTO로, 작성자 정보, 공고 기본 정보, 마감일 및 상태 등의 데이터를 포함한다.

##### Attributes

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

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getId()` | Long | public | 공고 ID를 반환한다. |
| `getAuthorId()` | Long | public | 작성자 ID를 반환한다. |
| `getAuthorNickname()` | String | public | 작성자 닉네임을 반환한다. |
| `getCategory()` | RecruitmentCategory | public | 공고 카테고리를 반환한다. |
| `getUniversity()` | University | public | 작성자 소속 대학교를 반환한다. |
| `getTitle()` | String | public | 공고 제목을 반환한다. |
| `getContent()` | String | public | 공고 본문 내용을 반환한다. |
| `getDeadline()` | LocalDateTime | public | 모집 마감일을 반환한다. |
| `getCreatedAt()` | LocalDateTime | public | 공고 생성일을 반환한다. |
| `getStatus()` | RecruitmentStatus | public | 공고 상태를 반환한다. |
| `getViewCount()` | int | public | 조회수를 반환한다. |

---

#### BaseRecruitmentSummaryResponseDto

공고 요약 응답 DTO의 **추상 클래스**. 공통 필드(작성자, 카테고리, 제목, 마감일, 상태 등)를 정의하며 하위 DTO가 이를 상속하여 구체화한다.

##### Attributes

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

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `getId()` | Long | public | 공고 식별자 반환 |
| `getAuthorId()` | Long | public | 작성자 식별자 반환 |
| `getAuthorNickname()` | String | public | 작성자 닉네임 반환 |
| `getUniversity()` | University | public | 소속 대학교 반환 |
| `getCategory()` | RecruitmentCategory | public | 공고 카테고리 반환 |
| `getTitle()` | String | public | 공고 제목 반환 |
| `getDeadline()` | LocalDateTime | public | 모집 마감일 반환 |
| `getStatus()` | RecruitmentStatus | public | 공고 상태 반환 |

---

#### ParticipantInfoResponseDto

포지션별 인원 정보를 반환하기 위한 응답 DTO로, 모집 인원과 현재 인원을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| maxParticipants | int | private | 모집 인원 |
| currParticipants | int | private | 현재 인원 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `from(ParticipantInfo info)` | ParticipantInfoResponseDto | public static | ParticipantInfo 객체를 ParticipantInfoResponseDto로 변환 |

---

#### PositionInfoResponseDto

포지션 정보와 해당 포지션의 인원 정보를 반환하기 위한 응답 DTO로, 포지션 유형과 ParticipantInfoResponseDto를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| position | PositionType | private | 포지션 |
| participantInfo | ParticipantInfoResponseDto | private | 포지션별 인원 정보 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `from(PositionParticipantInfo info)` | PositionInfoResponseDto | public static | PositionParticipantInfo 객체를 PositionInfoResponseDto로 변환 |

---

#### PeriodResponseDto

기간 정보를 반환하기 위한 응답 DTO로, 시작일과 종료일을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| startDate | LocalDate | private | 시작일 (`yyyy-MM-dd`) |
| endDate | LocalDate | private | 종료일 (`yyyy-MM-dd`) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `from(Period period)` | PeriodResponseDto | public static | Period 객체를 PeriodResponseDto로 변환 |

---

#### RecruitmentService

모집 공고 관련 비즈니스 로직을 정의하는 서비스 인터페이스로, 공고 조회 및 상태 관리 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|      |      |           |             |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `findById(Long recruitmentId)` | BaseRecruitment | public | 주어진 ID의 모집 공고를 조회 |
| `findByIdNotCanceled(Long recruitmentId)` | BaseRecruitment | public | 취소되지 않은 모집 공고를 ID로 조회 |
| `closeExpiredRecruitments()` | void | public | 마감일이 지난 모집 공고의 상태를 CLOSED로 변경 |

---

#### RecruitmentServiceImpl

[RecruitmentService](####recruitmentservice)를 구현한 서비스 클래스
모집 공고 조회, 마감 처리 등의 실제 비즈니스 로직을 수행하며, 트랜잭션 및 로깅을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| recruitmentRepository | RecruitmentRepository | private final | 모집 공고 데이터 접근 계층 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `findById(Long recruitmentId)` | BaseRecruitment | public | 주어진 ID의 모집 공고를 조회 |
| `findByIdNotCanceled(Long recruitmentId)` | BaseRecruitment | public | 취소되지 않은 모집 공고를 ID로 조회 |
| `closeExpiredRecruitments()` | void | public | 마감일이 지난 모집 공고를 CLOSED 상태로 변경 |

---

#### RecruitmentRepository

BaseRecruitment 엔티티의 데이터 접근 계층 인터페이스로, 공고 조회 및 상태 변경 관련 쿼리를 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|      |      |           |             |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `findByIdNotCanceled(Long id)` | Optional\<BaseRecruitment\> | public | 취소되지 않은 공고를 ID로 조회 |
| `closeExpiredRecruitments(LocalDateTime baseTime)` | int | public | 기준 시각 이전 마감 공고의 상태를 CLOSED로 변경 |

---

#### PageResponse<T>

페이지 단위로 데이터를 반환하기 위한 제네릭 DTO로, 내용, 페이지 정보, 전체 요소 수 및 마지막 페이지 여부를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-------------|--------------|
| content | List<T> | private | 페이지 내 데이터 목록 |
| pageNumber | int | private | 현재 페이지 번호 |
| pageSize | int | private | 페이지 크기 |
| totalElements | long | private | 전체 요소 수 |
| totalPages | int | private | 전체 페이지 수 |
| last | boolean | private | 마지막 페이지 여부 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|--------------|
| `from(Page<T> page)` | PageResponse<T> | public static | Page 객체를 PageResponse 객체로 변환 |

---

## 3.3 도메인별 클래스 상세 구조

각 도메인의 주요 엔티티, 값 객체, 열거형의 관계를 보여주는 다이어그램들이다. 
도메인별 클래스 간 연관성을 파악하고, 데이터 및 로직의 구조를 이해할 수 있도록 구성되었다.


### 3.3.1 User Domain

<img width="2182" height="1698" alt="02-user-domain" src="https://github.com/user-attachments/assets/b226501b-99d5-4277-add9-1150472f2505" />

사용자 도메인의 엔티티와 관련 열거형을 보여주는 다이어그램이다. User 엔티티의 구조와 University, UserRoleType, UserStatus, PositionType, Skill 등의 열거형과의 관계를 표현한다.


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


### 3.4.6 User Profile Process Structure

<img width="4096" height="2055" alt="03-user-profile-flow" src="https://github.com/user-attachments/assets/cae50c6e-d20a-4b94-a7ff-8e622a19505b" />

사용자 프로필 관리 기능의 계층 구조를 나타낸다. UserController와 UserService의 의존 관계, 프로필 조회 시 ReviewService와의 연동 구조, 그리고 비밀번호 변경 및 회원 탈퇴 등의 보안 처리 과정을 표현한다.


### 3.4.7 Review Process Structure

<img width="3558" height="1844" alt="03-review-flow" src="https://github.com/user-attachments/assets/557e2815-cb61-4298-b353-9f218e7d786a" />

리뷰 관리 기능의 계층 구조와 처리 과정을 나타낸다. ReviewController, ReviewService, ReviewRepository 간의 의존 관계를 표현하며, 리뷰 작성자와 수신자 간의 관계 구조와 소프트 삭제 메커니즘을 보여준다.


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

---

## 3.6 보안 클래스 (Security Classes)

시스템의 인증 및 인가를 담당하는 보안 관련 클래스들의 구조를 보여준다.
JWT 기반 토큰 관리를 위한 JwtUtil, 요청 검증을 위한 JwtFilter, Spring Security와의 통합을 위한 CustomUserDetails 및 CustomUserDetailsService, 그리고 인증 실패 및 접근 거부 상황을 처리하는 CustomAuthenticationEntryPoint와 CustomAccessDeniedHandler 등의 보안 클래스들의 구조와 상호작용을 표현한다.

---

## 3.7 공통 유틸리티 (Common Utilities)

시스템 전반에서 공통으로 사용되는 유틸리티 클래스들의 구조를 보여준다.
한국어 형태소 분석을 위한 KomoranUtil, String과 Enum 간의 타입 변환을 위한 StringToEnumConverterFactory, 페이지네이션 요청의 유효성을 검증하는 PageableValidator 등 다양한 기능을 지원하는 유틸리티 클래스들의 구조와 사용 방식을 표현한다.
