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

[RecruitmentService](#recruitmentservice)를 구현한 서비스 클래스
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

#### Project

[BaseRecruitment](#baserecruitment)를 상속한 프로젝트 공고 엔티티로, 프로젝트 목적, 모임 유형, 포지션, 필요 스킬, 학년, 기간 등의 상세 정보를 포함한다.

##### Attributes

| Name | Type | Visibility | Description      |
|------|------|-----------|------------------|
| purpose | ProjectPurpose | private | 프로젝트 목적          |
| meetingType | MeetingType | private | 진행 방식            |
| positions | Set\<PositionParticipantInfo\> | private | 프로젝트 포지션별 참여자 정보 |
| skills | Set\<Skill\> | private | 요구 기술 스택 목록      |
| grades | Set\<Integer\> | private | 우대 학년            |
| period | Period | private | 프로젝트 기간          |

##### Operations

| Name             | Return Type    | Visibility | Description             |
|------------------|----------------|------------|-------------------------|
| `getPurpose()`     | ProjectPurpose | public     | 프로젝트 목적 반환              |
| `getMeetingType()` | MeetingType | public     | 진행 방식 반환                |
| `getPositions()`   | Set\<PositionParticipantInfo\> | public     | 프로젝트 포지션별 참여자 정보 반환     |
| `getSkills()`      | Set\<Skill\> | public     | 요구 기술 스택 목록 반환          |
| `getGrades()`      | Set\<Integer\> | public     | 우대 학년 반환                |
| `getPeriod()`      | Period | public     | 프로젝트 기간 반환              |
| `update(ProjectUpdateRequestDto dto)` | void | public | 프로젝트 공고 정보를 업데이트        |
| `getPositionInfoByRole(PositionType position)` | Optional\<PositionParticipantInfo\> | public | 특정 포지션의 참여자 정보 조회       |
| `decreaseCurrParticipant(PositionType positionType)` | void | public | 특정 포지션의 현재 참여 인원을 1명 감소 |

---

#### MeetingType

회의 또는 프로젝트 진행 방식을 정의하는 **열거형(Enum)** 으로, 각 유형의 설명(`desc`)과 해당 Enum 값을 제공한다.

##### Enum Values

| Value | Description |
|-------|-------------|
| `ONLINE` | 온라인 |
| `OFFLINE` | 오프라인 |
| `HYBRID` | 온/오프라인 |

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| desc | String | private final | 진행 방식 설명    |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `getDesc()` | String | public | 진행 방식 설명 반환 |
| `getName()` | String | public | Enum 이름 반환  |

---

#### ProjectPurpose

프로젝트의 목적을 정의하는 **열거형(Enum)** 으로, 각 목적의 설명(`desc`)과 해당 Enum 값을 제공한다.

##### Enum Values

| Value | Description |
|-------|-------------|
| `CONTEST` | 공모전 |
| `HACKATHON` | 해커톤 |
| `TOY_PROJECT` | 토이 프로젝트 |
| `SIDE_PROJECT` | 사이드 프로젝트 |

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| desc | String | private final | 프로젝트 목적 설명 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `getDesc()` | String | public | 프로젝트 목적 설명 반환 |
| `getName()` | String | public | Enum 이름 반환 |

---

#### ProjectCommonRequestDto

[BaseRecruitmentRequestDto](#baserecruitmentrequestdto)를 상속한 추상 요청 DTO로, 프로젝트 공고 생성 및 수정 시 공통으로 필요한 정보를 담는다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| purpose | ProjectPurpose | private final | 프로젝트 목적     |
| meetingType | MeetingType | private final | 진행 방식       |
| skills | Set\<Skill\> | private final | 기술 스택 목록    |
| grades | Set\<GradeRequestDto\> | private final | 우대 학년       |
| period | PeriodRequestDto | private final | 프로젝트 기간     |

##### Operations

| Name | Return Type | Visibility | Description   |
|------|-----------|----------|---------------|
| `getPurpose()`     | ProjectPurpose | public     | 프로젝트 목적 반환    |
| `getMeetingType()` | MeetingType | public     | 진행 방식 반환      |
| `getSkills()`      | Set\<Skill\> | public     | 기술 스택 목록 반환   |
| `getGrades()`      | Set\<Integer\> | public     | 우대 학년 반환      |
| `getPeriod()`      | Period | public     | 프로젝트 기간 반환    |
| `validate()` | void | public | 마감일이 종료일 이전인지 검증 |

---

#### ProjectCreationRequestDto

[ProjectCommonRequestDto](#projectcommonrequestdto)를 상속한 요청 DTO로, 프로젝트 공고 생성 시 필요한 정보를 담는다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| positions | Set\<PositionInfoCreationRequestDto\> | private final | 포지션별 인원 정보 |

##### Operations

| Name | Return Type | Visibility | Description          |
|------|-----------|----------|----------------------|
| `toEntity(User user, ProjectCreationRequestDto dto)` | Project | public static | DTO를 Project 엔티티로 변환 |
| `getPositions()`   | Set\<PositionParticipantInfo\> | public     | 프로젝트 포지션별 인원 정보 반환   |

---

#### ProjectUpdateRequestDto

[ProjectCommonRequestDto](#projectcommonrequestdto)를 상속한 요청 DTO로, 프로젝트 공고 수정 시 필요한 정보를 담는다.  

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| positions | Set\<PositionInfoUpdateRequestDto\> | private final | 포지션별 인원 정보 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `getPositions()`   | Set\<PositionParticipantInfo\> | public     | 프로젝트 포지션별 인원 정보 반환   |

---

#### ProjectSearchCondition

프로젝트 공고 검색 조건을 담는 레코드(Record)로, 키워드, 목적, 포지션, 기술 스택, 모집 상태 등의 필드를 포함한다.  

컬렉션은 불변성을 보장하며, null 값이 들어오면 Empty Set으로 초기화한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| keywords | Set\<String\> | public final | 검색 키워드 목록 |
| purpose | ProjectPurpose | public final | 프로젝트 목적 |
| positions | Set\<PositionType\> | public final | 포지션 목록 |
| skills | Set\<Skill\> | public final | 기술 스택 목록 |
| status | RecruitmentStatus | public final | 모집 공고 상태 |

##### Operations

| Name        | Return Type | Visibility | Description |
|-------------|-----------|----------|-------------|
| `keywords()`  | Set\<String\> | public | 검색 키워드 목록 반환 |
| `purpose()`   | ProjectPurpose | public | 프로젝트 목적 반환 |
| `positions()` | Set\<PositionType\> | public | 포지션 목록 반환 |
| `skills()`    | Set\<Skill\> | public | 기술 스택 목록 반환 |
| `status()`    | RecruitmentStatus | public | 모집 공고 상태 반환 |

---

#### ProjectDetailResponseDto

[BaseRecruitmentDetailResponseDto](#baseRecruitmentdetailresponsedto)를 상속한 응답 DTO로, 프로젝트 공고의 상세 정보를 담는다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| purpose | ProjectPurpose | private final | 프로젝트 목적     |
| meetingType | MeetingType | private final | 진행 방식       |
| positions | Set\<PositionInfoResponseDto\> | private | 포지션별 인원 정보  |
| skills | Set\<Skill\> | private | 기술 스택 목록    |
| grades | Set\<Integer\> | private | 우대 학년       |
| period | PeriodResponseDto | private final | 프로젝트 기간     |

##### Operations

| Name                        | Return Type | Visibility | Description |
|-----------------------------|-----------|----------|-------------|
| `fromEntity(Project project)` | ProjectDetailResponseDto | public static | Project 엔티티를 DTO로 변환 |
| `getPurpose()`                | ProjectPurpose | public | 프로젝트 목적 반환 |
| `getMeetingType()`            | MeetingType | public | 진행 방식 반환 |
| `getPositions()`              | Set\<PositionType\> | public | 포지션 목록 반환 |
| `getSkills()`                 | Set\<Skill\> | public | 기술 스택 목록 반환 |
| `getGrades()`                 | Set\<Integer\> | public | 우대 학년 반환 |
| `getPeriod()`                 | PeriodResponseDto | public | 프로젝트 기간 반환 |

---

#### ProjectSummaryResponseDto

[BaseRecruitmentSummaryResponseDto](#baserecruitmentsummaryresponsedto)를 상속한 응답 DTO로, 프로젝트 공고의 요약 정보를 담는다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| purpose | ProjectPurpose | private final | 프로젝트 목적 |
| meetingType | MeetingType | private final | 진행 방식 |
| positions | Set\<PositionType\> | private final | 포지션 목록 |
| skills | Set\<Skill\> | private final | 기술 스택 목록 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `getPurpose()`                | ProjectPurpose | public | 프로젝트 목적 반환 |
| `getMeetingType()`            | MeetingType | public | 진행 방식 반환 |
| `getPositions()`              | Set\<PositionType\> | public | 포지션 목록 반환 |
| `getSkills()`                 | Set\<Skill\> | public | 기술 스택 목록 반환 |

---

#### ProjectController

프로젝트 공고와 관련된 REST API를 제공하는 컨트롤러로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(ProjectService)을 통해 실제 비즈니스 로직을 수행한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| projectService | ProjectService | private final | 프로젝트 관련 비즈니스 로직 서비스 |
| komoranUtil | KomoranUtil | private final | 키워드 형태소 분석 유틸리티 |

##### Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| `createProject(ProjectCreationRequestDto requestDto, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Long\>\> | `POST /api/v1/projects` | public | 프로젝트 공고 생성 |
| `getProject(Long projectId)` | ResponseEntity\<ApiResponse\<ProjectDetailResponseDto\>\> | `GET /api/v1/projects/{projectId}` | public | 프로젝트 공고 상세 조회 |
| `getProjectSummaries(String keywords, ProjectPurpose purpose, List\<PositionType\> positions, List\<Skill\> skills, RecruitmentStatus status, Pageable pageable)` | ResponseEntity\<ApiResponse\<Page\<ProjectSummaryResponseDto\>\>\> | `GET /api/v1/projects` | public | 프로젝트 공고 목록 조회 |
| `updateProject(Long projectId, ProjectUpdateRequestDto requestDto, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `PUT /api/v1/projects/{projectId}` | public | 프로젝트 공고 수정 |
| `deleteProject(Long projectId, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/projects/{projectId}` | public | 프로젝트 공고 삭제 |

---

#### ProjectService

프로젝트 공고와 관련된 비즈니스 로직을 정의한 서비스 인터페이스로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
실제 구현체([ProjectServiceImpl](#projectserviceimpl))가 해당 기능을 수행하며, 트랜잭션과 검증 로직을 포함할 수 있다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `createProject(Long userId, ProjectCreationRequestDto projectCreationRequestDto)` | Long | public | 프로젝트 공고 생성 |
| `getProject(Long projectId)` | ProjectDetailResponseDto | public | 프로젝트 공고 상세 조회 |
| `getProjectSummaries(ProjectSearchCondition condition, Pageable pageable)` | Page\<ProjectSummaryResponseDto\> | public | 프로젝트 공고 목록 조회 (조건 및 페이징 적용) |
| `getProjectSummariesByIds(List\<Long\> projectIds)` | List\<ProjectSummaryResponseDto\> | public | 특정 ID 목록에 해당하는 프로젝트 공고 조회 |
| `updateProject(Long userId, Long projectId, ProjectUpdateRequestDto updateDto)` | void | public | 프로젝트 공고 수정 |
| `deleteProject(Long userId, Long projectId)` | void | public | 프로젝트 공고 삭제 |

---

#### ProjectServiceImpl

[ProjectService](#projectservice)의 구현체로, 프로젝트 공고 생성, 조회, 수정, 삭제 등 실제 비즈니스 로직을 수행하며, 트랜잭션 처리와 권한 검증, 조회수 증가 등을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| projectRepository | ProjectRepository | private final | 프로젝트 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| teamService | TeamService | private final | 팀 관련 비즈니스 로직 계층 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `createProject(Long userId, ProjectCreationRequestDto requestDto)` | Long | public | 프로젝트 공고 생성 |
| `getProject(Long projectId)` | ProjectDetailResponseDto | public | 프로젝트 공고 상세 조회 |
| `getProjectSummaries(ProjectSearchCondition condition, Pageable pageable)` | Page\<ProjectSummaryResponseDto\> | public | 프로젝트 공고 목록 조회 |
| `getProjectSummariesByIds(List\<Long\> projectIds)` | List\<ProjectSummaryResponseDto\> | public | 특정 ID 목록 프로젝트 요약 정보 조회 |
| `updateProject(Long userId, Long projectId, ProjectUpdateRequestDto updateDto)` | void | public | 프로젝트 공고 수정 |
| `deleteProject(Long userId, Long projectId)` | void | public | 프로젝트 공고 삭제 |

---

#### ProjectRepository

Project 엔티티의 데이터 접근 계층으로, 프로젝트 공고와 관련된 User, Position, Skill, Grade 조회 및 조회수 증가 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `findByIdWithUser(Long id)` | Optional\<Project\> | public | 프로젝트와 작성자 정보 조회 |
| `findPositionsByProjectId(Long id)` | Set\<PositionParticipantInfo\> | public | 프로젝트 포지션 조회 |
| `findSkillsByProjectId(Long id)` | Set\<Skill\> | public | 프로젝트 기술 스택 조회 |
| `findGradesByProjectId(Long id)` | Set\<Integer\> | public | 프로젝트 우대 학년 조회 |
| `increaseViewCount(Long id)` | int | public | 조회수 증가 |

---

#### ProjectRepositoryCustom

QueryDSL을 활용한 프로젝트 데이터 접근 계층으로, 프로젝트 요약 DTO 조회 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getProjectSummaries(ProjectSearchCondition condition, Pageable pageable)` | Page\<ProjectSummaryResponseDto\> | public | 검색 조건에 맞는 프로젝트 요약 DTO를 페이징 조회 |
| `getProjectSummariesByIds(List\<Long\> projectIds)` | List\<ProjectSummaryResponseDto\> | public | 주어진 Project ID 목록에 해당하는 프로젝트 요약 정보 조회 (입력 순서 보장) |

---

#### ProjectRepositoryImpl

[ProjectRepositoryCustom](#projectrepositorycustom) 인터페이스의 구현체로, QueryDSL을 활용하여 프로젝트 요약 DTO 조회 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| queryFactory | JPAQueryFactory | private final | QueryDSL용 JPA 쿼리 팩토리 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getProjectSummaries(ProjectSearchCondition condition, Pageable pageable)` | Page\<ProjectSummaryResponseDto\> | public | 조건에 맞는 프로젝트 요약 DTO를 페이징 조회 |
| `getProjectSummariesByIds(List\<Long\> projectIds)` | List\<ProjectSummaryResponseDto\> | public | 주어진 Project ID 목록에 해당하는 프로젝트 요약 정보 조회 (입력 순서 보장) |
| `eqPurpose(ProjectPurpose purpose)` | BooleanExpression | private | Project 목적(purpose)과 일치하는 조건 생성 |
| `eqStatus(RecruitmentStatus status)` | BooleanExpression | private | Project 상태(status)와 일치하는 조건 생성 (CANCELED 제외) |
| `containsAnyKeyword(Set\<String\> keywords)` | BooleanBuilder | private | 제목(title) 또는 내용(content)에 키워드 중 하나라도 포함되는 조건 생성 |
| `containsAnyPosition(Set\<PositionType\> positions)` | BooleanExpression | private | Project positions 컬렉션 중 하나라도 지정된 positions에 포함되는 조건 생성 |
| `containsAnySkill(Set\<Skill\> skills)` | BooleanExpression | private | Project skills 컬렉션 중 하나라도 지정된 skills에 포함되는 조건 생성 |

---

### 3.3.3 Assignment Domain

<img width="2417" height="1467" alt="02-assignment-domain" src="https://github.com/user-attachments/assets/2b49fce4-7453-456b-972d-5ed6639c2f90" />

과제 공고 도메인의 구조를 보여주는 다이어그램이다. BaseRecruitment를 상속하는 Assignment 엔티티와 ParticipantInfo 값 객체, 그리고 관련 열거형과의 관계를 표현한다.

#### Assignment

[BaseRecruitment](#baserecruitment)를 상속한 과제 공고 엔티티로, 학과, 과목, 인원 정보, 우대 학년 등을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| department | String | private | 학과명         |
| lecture | String | private | 과목명         |
| lectureCode | String | private | 과목 코드       |
| participants | ParticipantInfo | private | 참여자 정보      |
| grades | Set\<Integer\> | private | 우대 학년       |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getDepartment()` | String | public | 학과명 반환 |
| `getLecture()` | String | public | 과목명 반환 |
| `getLectureCode()` | String | public | 과목 코드 반환 |
| `getParticipants()` | ParticipantInfo | public | 인원 정보 반환 |
| `getGrades()` | Set\<Integer\> | public | 우대 학년 반환 |
| `update(AssignmentUpdateRequestDto dto)` | void | public | 과제 공고 정보를 업데이트 |
| `decreaseCurrParticipant(PositionType positionType)` | void | public | 현재 인원을 1명 감소 |

---

#### AssignmentCommonRequestDto

[BaseRecruitmentRequestDto](#baserecruitmentrequestdto)를 상속한 과제 공고 공통 추상 요청 DTO로, 학과, 강의 정보, 우대 학년 등의 공통 필드를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| department | String | private | 학과명 |
| lecture | String | private | 과목명 |
| lectureCode | String | private | 과목 코드 |
| grades | Set\<GradeRequestDto\> | private | 우대 학년 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getDepartment()` | String | public | 학과명 반환 |
| `getLecture()` | String | public | 과목명 반환 |
| `getLectureCode()` | String | public | 과목 코드 반환 |
| `getGrades()` | Set\<GradeRequestDto\> | public | 우대 학년 반환 |

---

#### AssignmentCreationRequestDto

[AssignmentCommonRequestDto](#assignmentcommonrequestdto)를 상속한 과제 공고 생성 요청 DTO로, 모집 인원을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| maxParticipants | Integer | private final | 모집 인원 |

##### Operations

| Name | Return Type | Visibility    | Description |
|------|-----------|---------------|-------------|
| `toEntity(User user, AssignmentCreationRequestDto dto)` | Assignment | public static | AssignmentCreationRequestDto 객체를 Assignment 객체로 변환 |
| `getMaxParticipants()` | Integer | public        | 모집 인원 반환 |

---

#### AssignmentUpdateRequestDto

[AssignmentCommonRequestDto](#assignmentcommonrequestdto)를 상속한 과제 공고 수정 요청 DTO로, 인원 정보를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| participants | ParticipantInfoUpdateRequestDto | private final | 인원 정보 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getParticipants()` | ParticipantInfoUpdateRequestDto | public | 인원 정보 반환 |

---

#### AssignmentSearchCondition

사용자가 입력한 키워드, 학년, 모집 상태를 기준으로 과제를 검색하기 위한 조건을 담는 record이다.  
컬렉션은 불변성을 보장하며, null 값이 들어오면 Empty Set으로 초기화한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|------------|-------------|
| keywords | Set\<String\> | private | 검색 키워드 집합 |
| grades | Set\<Integer\> | private | 학년 조건 집합 |
| status | RecruitmentStatus | private | 모집 상태 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|------------|------------|-------------|
| `keywords()` | Set\<String\> | public | 키워드 반환 |
| `grades()` | Set\<Integer\> | public | 학년 조건 반환 |
| `status()` | RecruitmentStatus | public | 모집 상태 반환 |

---

#### AssignmentDetailResponseDto

[BaseRecruitmentDetailResponseDto](#baseRecruitmentdetailresponsedto)를 상속한 응답 DTO로, 과제 공고의 상세 정보를 담는다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|------------|-------------|
| department | String | private | 학과          |
| lecture | String | private | 강의명         |
| lectureCode | String | private | 강의 코드       |
| participants | ParticipantInfoResponseDto | private | 인원 정보       |
| grades | Set\<Integer\> | private | 우대 학년       |

##### Operations

| Name              | Return Type | Visibility | Description |
|-------------------|------------|------------|-------------|
| `getDepartment()`   | String | public | 학과 반환       |
| `getLecture()`      | String | public | 강의명 반환      |
| `getLectureCode()`  | String | public | 강의 코드 반환    |
| `getParticipants()` | ParticipantInfoResponseDto | public | 인원 정보 반환    |
| `getGrades()`       | Set\<Integer\> | public | 우대 학년 반환    |

---

#### AssignmentSummaryResponseDto

[BaseRecruitmentSummaryResponseDto](#baserecruitmentsummaryresponsedto)를 상속한 응답 DTO로, 과제 공고의 요약 정보를 담는다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|------------|-------------|
| department | String | private | 학과 |
| lecture | String | private | 강의명 |
| lectureCode | String | private | 강의 코드 |
| grades | Set\<Integer\> | private | 우대 학년 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|------------|------------|-------------|
| `getDepartment()` | String | public | 학과 반환 |
| `getLecture()` | String | public | 강의명 반환 |
| `getLectureCode()` | String | public | 강의 코드 반환 |
| `getGrades()` | Set\<Integer\> | public | 우대 학년 반환 |

---

#### AssignmentController

과제 공고와 관련된 REST API를 제공하는 컨트롤러로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(AssignmentService)을 통해 실제 비즈니스 로직을 수행한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| assignmentService | AssignmentService | private final | 과제 관련 비즈니스 로직 서비스 |
| komoranUtil | KomoranUtil | private final | 키워드 형태소 분석 유틸리티 |

##### Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| `createAssignment(AssignmentCreationRequestDto requestDto, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Long\>\> | `POST /api/v1/assignments` | public | 과제 공고 생성 |
| `getAssignment(Long assignmentId)` | ResponseEntity\<ApiResponse\<AssignmentDetailResponseDto\>\> | `GET /api/v1/assignments/{assignmentId}` | public | 과제 공고 상세 조회 |
| `getAssignmentSummaries(String keywords, Set\<Integer\> grades, RecruitmentStatus status, Pageable pageable)` | ResponseEntity\<ApiResponse\<Page\<AssignmentSummaryResponseDto\>\>\> | `GET /api/v1/assignments` | public | 과제 공고 목록 조회 |
| `updateAssignment(Long assignmentId, AssignmentUpdateRequestDto requestDto, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `PUT /api/v1/assignments/{assignmentId}` | public | 과제 공고 수정 |
| `deleteAssignment(Long assignmentId, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/assignments/{assignmentId}` | public | 과제 공고 삭제 |

---

#### AssignmentService

과제 공고와 관련된 비즈니스 로직을 정의한 서비스 인터페이스로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
실제 구현체([AssignmentServiceImpl](#assignmentserviceimpl))가 해당 기능을 수행하며, 트랜잭션과 검증 로직을 포함할 수 있다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `createAssignment(AssignmentCreationRequestDto assignmentCreationRequestDto, Long userId)` | Long | public | 과제 공고 생성 |
| `getAssignment(Long assignmentId)` | AssignmentDetailResponseDto | public | 과제 공고 상세 조회 |
| `getAssignmentSummaries(AssignmentSearchCondition condition, Pageable pageable)` | Page\<AssignmentSummaryResponseDto\> | public | 과제 공고 목록 조회 |
| `updateAssignment(Long userId, Long assignmentId, AssignmentUpdateRequestDto updateDto)` | void | public | 과제 공고 수정 |
| `deleteAssignment(Long userId, Long assignmentId)` | void | public | 과제 공고 삭제 |

---

#### AssignmentServiceImpl

[AssignmentService](#assignmentservice)의 구현체로, 과제 공고 생성, 조회, 수정, 삭제 등 실제 비즈니스 로직을 수행하며,  
트랜잭션 처리, 권한 검증, 조회수 증가 등을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| assignmentRepository | AssignmentRepository | private final | 과제 공고 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| teamService | TeamService | private final | 팀 관련 비즈니스 로직 계층 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `createAssignment(AssignmentCreationRequestDto requestDto, Long userId)` | Long | public | 과제 공고 생성 |
| `getAssignment(Long assignmentId)` | AssignmentDetailResponseDto | public | 과제 공고 상세 조회 |
| `getAssignmentSummaries(AssignmentSearchCondition condition, Pageable pageable)` | Page\<AssignmentSummaryResponseDto\> | public | 과제 공고 목록 조회 |
| `updateAssignment(Long userId, Long assignmentId, AssignmentUpdateRequestDto updateDto)` | void | public | 과제 공고 수정 |
| `deleteAssignment(Long userId, Long assignmentId)` | void | public | 과제 공고 삭제 |

---

#### AssignmentRepository

Assignment 엔티티의 데이터 접근 계층으로, 조회수 증가 기능과 커스텀 쿼리 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `increaseViewCount(Long assignmentId)` | int | public | 과제 공고 조회수 증가 |

---

#### AssignmentRepositoryCustom

QueryDSL을 활용한 Assignment 커스텀 리포지토리로, 과제 요약 DTO 조회 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getAssignmentSummaries(AssignmentSearchCondition condition, Pageable pageable)` | Page\<AssignmentSummaryResponseDto\> | public | 검색 조건에 맞는 과제 요약 DTO를 페이징 조회 |
| `getAssignmentSummariesByIds(List\<Long\> assignmentIds)` | List\<AssignmentSummaryResponseDto\> | public | 주어진 Assignment ID 목록에 해당하는 과제 요약 정보 조회 (입력 순서 보장) |

---

#### AssignmentRepositoryImpl

[AssignmentRepositoryCustom](#assignmentrepositorycustom) 인터페이스의 구현체로, QueryDSL을 활용하여 과제 요약 DTO 조회 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| queryFactory | JPAQueryFactory | private final | QueryDSL용 JPA 쿼리 팩토리 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getAssignmentSummaries(AssignmentSearchCondition condition, Pageable pageable)` | Page\<AssignmentSummaryResponseDto\> | public | 조건에 맞는 과제 요약 DTO를 페이징 조회 |
| `getAssignmentSummariesByIds(List\<Long\> assignmentIds)` | List\<AssignmentSummaryResponseDto\> | public | 주어진 Assignment ID 목록에 해당하는 과제 요약 정보 조회 (입력 순서 보장) |
| `eqStatus(RecruitmentStatus status)` | BooleanExpression | private | Assignment 상태(status)와 일치하는 조건 생성 (CANCELED 제외) |
| `containsAnyKeyword(Set\<String\> keywords)` | BooleanBuilder | private | 제목(title) 또는 내용(content)에 키워드 중 하나라도 포함되는 조건 생성 |
| `containsAnyGrade(Set\<Integer\> grades)` | BooleanExpression | private | Assignment 우대 학년(grades) 중 하나라도 지정된 학년에 포함되는 조건 생성 |

---

### 3.3.4 Study Domain

<img width="3243" height="1467" alt="02-study-domain" src="https://github.com/user-attachments/assets/78d5c8a3-ef1d-4a3f-8a5c-ac3942f7dd53" />

스터디 공고 도메인의 구조를 보여주는 다이어그램이다. BaseRecruitment를 상속하는 Study 엔티티와 ParticipantInfo, Period 값 객체, 그리고 관련 열거형과의 관계를 표현한다.

#### Study

[BaseRecruitment](#baserecruitment)를 상속한 스터디 공고 엔티티로, 인원 정보, 기간, 기술 스택 정보를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| participants | ParticipantInfo | private | 인원 정보 |
| period | Period | private | 스터디 기간 |
| skills | Set\<Skill\> | private | 스터디 기술 스택 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getParticipants()` | ParticipantInfo | public | 인원 정보 반환 |
| `getPeriod()` | Period | public | 스터디 기간 반환 |
| `getSkills()` | Set\<Skill\> | public | 기술 스택 반환 |
| `update(StudyUpdateRequestDto dto)` | void | public | 스터디 공고 정보를 업데이트 |
| `decreaseCurrParticipant(PositionType positionType)` | void | public | 현재 인원을 1명 감소 |

---

#### StudyCommonRequestDto

[BaseRecruitmentRequestDto](#baserecruitmentrequestdto)를 상속한 스터디 공고 공통 추상 요청 DTO로, 진행 기간과 기술 스택 정보를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| period | PeriodRequestDto | private final | 진행 기간 정보 |
| skills | Set\<Skill\> | private final | 기술 스택 정보 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getPeriod()` | PeriodRequestDto | public | 진행 기간 정보 반환 |
| `getSkills()` | Set\<Skill\> | public | 기술 스택 정보 반환 |
| `validate()` | void | public | 진행 기간과 마감일의 유효성을 검증 |

---

#### StudyCreationRequestDto

[StudyCommonRequestDto](#studycommonrequestdto)를 상속한 스터디 공고 생성 요청 DTO로, 모집 인원 정보를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| maxParticipants | Integer | private final | 모집 인원 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `toEntity(User user, StudyCreationRequestDto dto)` | Study | public static | StudyCreationRequestDto 객체를 Study 엔티티로 변환 |
| `getMaxParticipants()` | Integer | public | 모집 인원 반환 |

---

#### StudyUpdateRequestDto

[StudyCommonRequestDto](#studycommonrequestdto)를 상속한 스터디 공고 수정 요청 DTO로, 인원 정보를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| participants | ParticipantInfoUpdateRequestDto | private final | 인원 정보 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getParticipants()` | ParticipantInfoUpdateRequestDto | public | 인원 정보 반환 |

---

#### StudySearchCondition

스터디 공고 검색 조건을 담는 레코드(Record)로, 키워드, 기술 스택, 모집 상태 등의 필드를 포함한다.  
컬렉션은 불변성을 보장하며, null 값이 들어오면 Empty Set으로 초기화한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|--------|-------------|
| keywords | Set\<String\> | public | 검색 키워드 목록 |
| skills | Set\<Skill\> | public | 기술 스택 목록 |
| status | RecruitmentStatus | public | 모집 공고 상태 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|------------|-----------|-------------|
| `keywords()` | Set\<String\> | public | 키워드 반환 |
| `skills()` | Set\<Skill\> | public | 기술 스택 조건 반환 |
| `status()` | RecruitmentStatus | public | 모집 상태 반환 |

---

#### StudyDetailResponseDto

[BaseRecruitmentDetailResponseDto](#baserecruitmentdetailresponsedto)를 상속한 스터디 공고 상세 응답 DTO로, 인원 정보, 진행 기간, 기술 스택 정보를 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| participants | ParticipantInfoResponseDto | private final | 인원 정보 반환 |
| period | PeriodResponseDto | private final | 진행 기간 정보 반환 |
| skills | Set\<Skill\> | private | 기술 스택 정보 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `fromEntity(Study study)` | StudyResponseDto | public static | Study 엔티티를 StudyResponseDto 객체로 변환 |
| `getParticipants()` | ParticipantInfoResponseDto | public | 인원 정보 반환 |
| `getPeriod()` | PeriodResponseDto | public | 진행 기간 정보 반환 |
| `getSkills()` | Set\<Skill\> | public | 기술 스택 정보 반환 |
| `setSkills(Set\<Skill\> skills)` | void | public | 기술 스택 정보 수정 |

---

#### StudySummaryResponseDto

[BaseRecruitmentSummaryResponseDto](#baserecruitmentsummaryresponsedto)를 상속한 응답 DTO로, 스터디 공고의 요약 정보를 담는다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| skills | Set\<Skill\> | private final | 기술 스택 목록 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `getSkills()` | Set\<Skill\> | public | 기술 스택 목록 반환 |

---

#### StudyController

스터디 공고와 관련된 REST API를 제공하는 컨트롤러로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(StudyService)을 통해 실제 비즈니스 로직을 수행한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| studyService | StudyService | private final | 스터디 관련 비즈니스 로직 서비스 |
| komoranUtil | KomoranUtil | private final | 키워드 형태소 분석 유틸리티 |

##### Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| `createStudy(StudyCreationRequestDto requestDto, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Long\>\> | `POST /api/v1/studies` | public | 스터디 공고 생성 |
| `getStudy(Long studyId)` | ResponseEntity\<ApiResponse\<StudyDetailResponseDto\>\> | `GET /api/v1/studies/{studyId}` | public | 스터디 공고 상세 조회 |
| `getStudySummaries(String keywords, List\<Skill\> skills, RecruitmentStatus status, Pageable pageable)` | ResponseEntity\<ApiResponse\<Page\<StudySummaryResponseDto\>\>\> | `GET /api/v1/studies` | public | 스터디 공고 목록 조회 |
| `updateStudy(Long studyId, StudyUpdateRequestDto requestDto, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `PUT /api/v1/studies/{studyId}` | public | 스터디 공고 수정 |
| `deleteStudy(Long studyId, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/studies/{studyId}` | public | 스터디 공고 삭제 |

---

#### StudyService

스터디 공고와 관련된 비즈니스 로직을 정의한 서비스 인터페이스로, 생성, 조회, 수정, 삭제 기능을 포함한다.  
실제 구현체([StudyServiceImpl](#studyserviceimpl))가 해당 기능을 수행하며, 트랜잭션과 검증 로직을 포함할 수 있다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `createStudy(Long userId, StudyCreationRequestDto studyCreationRequestDto)` | Long | public | 스터디 공고 생성 |
| `getStudy(Long studyId)` | StudyDetailResponseDto | public | 스터디 공고 상세 조회 |
| `getStudySummaries(StudySearchCondition condition, Pageable pageable)` | Page\<StudySummaryResponseDto\> | public | 스터디 공고 목록 조회 (조건 및 페이징 적용) |
| `getStudySummariesByIds(List\<Long\> studyIds)` | List\<StudySummaryResponseDto\> | public | 특정 ID 목록에 해당하는 스터디 공고 조회 |
| `updateStudy(Long userId, Long studyId, StudyUpdateRequestDto updateDto)` | void | public | 스터디 공고 수정 |
| `deleteStudy(Long userId, Long studyId)` | void | public | 스터디 공고 삭제 |

---

#### StudyServiceImpl

[StudyService](#studyservice)의 구현체로, 스터디 공고 생성, 조회, 수정, 삭제 등 실제 비즈니스 로직을 수행하며, 트랜잭션 처리와 권한 검증, 조회수 증가 등을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| studyRepository | StudyRepository | private final | 스터디 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| teamService | TeamService | private final | 팀 관련 비즈니스 로직 계층 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `createStudy(Long userId, StudyCreationRequestDto requestDto)` | Long | public | 스터디 공고 생성 |
| `getStudy(Long studyId)` | StudyDetailResponseDto | public | 스터디 공고 상세 조회 |
| `getStudySummaries(StudySearchCondition condition, Pageable pageable)` | Page\<StudySummaryResponseDto\> | public | 스터디 공고 목록 조회 |
| `getStudySummariesByIds(List\<Long\> studyIds)` | List\<StudySummaryResponseDto\> | public | 특정 ID 목록 스터디 요약 정보 조회 |
| `updateStudy(Long userId, Long studyId, StudyUpdateRequestDto updateDto)` | void | public | 스터디 공고 수정 |
| `deleteStudy(Long userId, Long studyId)` | void | public | 스터디 공고 삭제 |

---

#### StudyRepository

Study 엔티티의 데이터 접근 계층으로, 조회수 증가 기능과 커스텀 쿼리 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `increaseViewCount(Long id)` | int | public | 조회수 증가 |

---

#### StudyRepositoryCustom

QueryDSL을 활용한 스터디 데이터 접근 계층으로, 스터디 요약 DTO 조회 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getStudySummaries(StudySearchCondition condition, Pageable pageable)` | Page\<StudySummaryResponseDto\> | public | 검색 조건에 맞는 스터디 요약 DTO를 페이징 조회 |
| `getStudySummariesByIds(List\<Long\> studyIds)` | List\<StudySummaryResponseDto\> | public | 주어진 Study ID 목록에 해당하는 스터디 요약 정보 조회 (입력 순서 보장) |

---

#### StudyRepositoryImpl

[StudyRepositoryCustom](#studyrepositorycustom) 인터페이스의 구현체로, QueryDSL을 활용하여 스터디 요약 DTO 조회 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| queryFactory | JPAQueryFactory | private final | QueryDSL용 JPA 쿼리 팩토리 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getStudySummaries(StudySearchCondition condition, Pageable pageable)` | Page\<StudySummaryResponseDto\> | public | 조건에 맞는 스터디 요약 DTO를 페이징 조회 |
| `getStudySummariesByIds(List\<Long\> studyIds)` | List\<StudySummaryResponseDto\> | public | 주어진 Study ID 목록에 해당하는 스터디 요약 정보 조회 (입력 순서 보장) |
| `eqStatus(RecruitmentStatus status)` | BooleanExpression | private | Study 상태(status)와 일치하는 조건 생성 (CANCELED 제외) |
| `containsAnyKeyword(Set\<String\> keywords)` | BooleanBuilder | private | 제목(title) 또는 내용(content)에 키워드 중 하나라도 포함되는 조건 생성 |
| `containsAnySkill(Set\<Skill\> skills)` | BooleanExpression | private | Study skills 컬렉션 중 하나라도 지정된 skills에 포함되는 조건 생성 |

---

### 3.3.5 Team Domain

<img width="2297" height="1361" alt="02-team-domain" src="https://github.com/user-attachments/assets/b96aad6a-ade4-4ef6-a2f1-7ba36052de79" />

팀 및 팀 멤버 도메인의 구조를 보여주는 다이어그램이다. Team과 TeamMember 엔티티 간의 관계, TeamRole 열거형, 그리고 BaseRecruitment와의 연관 관계를 표현한다.


### 3.3.6 Application Domain

<img width="3693" height="1239" alt="02-application-domain" src="https://github.com/user-attachments/assets/2c315c61-b08f-4aec-b107-6b654a1d8924" />

공고 지원 도메인의 구조를 보여주는 다이어그램이다. Application 엔티티와 BaseRecruitment, User 간의 관계, 그리고 ApplicationStatus, MeetingType 등의 열거형과의 관계를 표현한다.

#### Application

사용자가 프로젝트, 과제, 스터디 공고에 지원할 때 생성되는 엔티티로, 지원 정보와 상태를 관리한다.

##### Attributes

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

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getId()` | Long | public | 지원 ID 반환 |
| `getApplicant()` | User | public | 지원자 정보 반환 |
| `getRecruitment()` | BaseRecruitment | public | 지원 대상 공고 반환 |
| `getContent()` | String | public | 지원 내용 반환 |
| `getGrade()` | Integer | public | 지원자의 학년 반환 |
| `getMeetingType()` | MeetingType | public | 진행 방식 반환 |
| `getPosition()` | PositionType | public | 포지션 반환 (프로젝트 공고 지원) |
| `getSkills()` | Set\<Skill\> | public | 기술 스택 반환 (프로젝트 공고 지원) |
| `getStatus()` | ApplicationStatus | public | 지원 상태 반환 |
| `getCreatedAt()` | LocalDateTime | public | 생성일 반환 |
| `isDeleted()` | boolean | public | 논리적 삭제 여부 반환 |
| `accept()` | void | public | 지원 상태를 `ACCEPTED`로 변경 |
| `reject()` | void | public | 지원 상태를 `REJECTED`로 변경 |
| `delete()` | void | public | 논리적 삭제 처리 |

---

#### ApplicationStatus

지원 상태를 정의하는 **열거형(Enum)** 으로, 각 상태의 설명(`desc`)과 해당 Enum 값을 제공한다.

##### Enum Values

| Value | Description |
|-------|-------------|
| `SUBMITTED` | 대기중 |
| `ACCEPTED` | 수락됨 |
| `REJECTED` | 거절됨 |
| `CLOSED` | 모집종료 |
| `CANCELED` | 모집취소 |

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| desc | String | private | 지원 상태 설명 (예: "대기중") |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getDesc()` | String | public | 상태 설명 반환 |
| `getName()` | String | public | Enum 이름 반환 |

---

#### ApplicationCommonRequestDto

지원서 작성 시 공통적으로 사용되는 추상 요청 DTO 클래스
모든 공고(프로젝트, 과제, 스터디)에 공통적으로 필요한 필드를 포함하며, 실제 엔티티 변환 로직은 하위 클래스에서 구현한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| category | RecruitmentCategory | private final | 지원하는 공고의 카테고리 |
| meetingType | MeetingType | protected | 선호하는 진행 방식 |
| grade | Integer | protected | 지원자의 학년 (1~4) |
| content | String | protected | 지원서 본문 내용 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getCategory()` | RecruitmentCategory | public | 지원 공고 카테고리 반환 |
| `toEntity(User applicant, BaseRecruitment recruitment)` | Application | public abstract | DTO를 Application 엔티티로 변환 <br> 하위 클래스에서 구현 필요 |

---

#### ApplicationProjectRequestDto

[ApplicationCommonRequestDto](#applicationcommonrequestdto)를 상속한 요청 DTO로, 프로젝트 공고 지원서 작성 시 필요한 정보를 담는다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| position | PositionType | private final | 지원 포지션 |
| skills | Set\<Skill\> | private final | 지원 기술 스택 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `getPosition()` | PositionType | public | 지원 포지션 반환 |
| `toEntity(User applicant, BaseRecruitment recruitment)` | Application | public | DTO를 Application 엔티티로 변환 |

---

#### ApplicationSimpleRequestDto

[ApplicationCommonRequestDto](#applicationcommonrequestdto)를 상속한 요청 DTO로, 과제 및 스터디 공고 지원서 작성 시 사용된다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `toEntity(User applicant, BaseRecruitment recruitment)` | Application | public | DTO를 Application 엔티티로 변환 |

---

#### ApplicationCommonResponseDto

지원 공고 응답 시 공통적으로 사용되는 추상 응답 DTO로, 지원서 ID, 공고 정보, 진행 방식 등의 데이터를 포함한다.

##### Attributes

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

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `getApplicationId()` | Long | public | 지원서 ID 반환 |
| `getRecruitmentId()` | Long | public | 공고 ID 반환 |
| `getRecruitmentTitle()` | String | public | 공고 제목 반환 |
| `getRecruitmentDeadline()` | LocalDateTime | public | 공고 마감일 반환 |
| `getMeetingType()` | MeetingType | public | 선호 진행 방식 반환 |
| `getGrade()` | Integer | public | 지원 학년 반환 |
| `getContent()` | String | public | 지원서 본문 내용 반환 |
| `getStatus()` | ApplicationStatus | public | 지원서 상태 반환 |

---

#### ApplicationProjectResponseDto

[ApplicationCommonResponseDto](#applicationcommonresponsedto)를 상속한 응답 DTO로, 프로젝트 공고 지원서에 사용된다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| position | PositionType | private final | 지원 포지션 |
| skills | Set\<Skill\> | private final | 지원 기술 스택 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `fromEntity(Application entity)` | ApplicationProjectResponseDto | public static | Application 엔티티를 DTO로 변환 |
| `getPosition()` | PositionType | public | 지원 포지션 반환 |
| `getSkills()` | Set\<Skill\> | public | 지원 기술 스택 반환 |

---

#### ApplicationSimpleResponseDto

[ApplicationCommonResponseDto](#applicationcommonresponsedto)를 상속한 응답 DTO로, 과제 및 스터디 공고 지원서에 사용된다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
| `fromEntity(Application entity)` | ApplicationSimpleResponseDto | public static | Application 엔티티를 DTO로 변환 |

---

#### ApplicationController

공고 지원(프로젝트, 과제, 스터디)과 관련된 REST API를 제공하는 컨트롤러로, 지원서 제출, 조회, 수락, 거절, 취소 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(ApplicationService)을 통해 실제 비즈니스 로직을 수행한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| applicationService | ApplicationService | private final | 공고 지원 관련 비즈니스 로직 서비스 |

##### Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| `submitProjectApplication(Long recruitmentId, ApplicationCommonRequestDto requestDto, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Long\>\> | `POST /api/v1/applications/recruitments/{recruitmentId}` | public | 프로젝트, 과제, 스터디 지원서 제출 |
| `getMyApplicationByCategory(RecruitmentCategory category, Pageable pageable, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Page\<ApplicationCommonResponseDto\>\>\> | `GET /api/v1/applications/me` | public | 로그인 사용자 기준, 카테고리별 지원 내역 조회 |
| `acceptApplication(Long applicationId, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `POST /api/v1/applications/{applicationId}/accept` | public | 특정 지원서 수락 |
| `rejectApplication(Long applicationId, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `POST /api/v1/applications/{applicationId}/reject` | public | 특정 지원서 거절 |
| `cancelApplication(Long applicationId, CustomUserDetails userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/applications/{applicationId}` | public | 특정 지원서 취소/삭제 |

---

#### ApplicationService

공고 지원과 관련된 비즈니스 로직을 정의한 서비스 인터페이스로, 제출, 조회, 수락/거절, 취소 기능을 포함한다.  
실제 구현체([ApplicationServiceImpl](#applicationserviceimpl))가 해당 기능을 수행하며, 트랜잭션과 권한 검증 로직을 포함할 수 있다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `submitApplication(Long userId, Long recruitmentId, ApplicationCommonRequestDto requestDto)` | Long | public | 공고 지원서 제출 |
| `findById(Long id)` | Application | public | ID로 지원서 조회 |
| `getAllByUserIdAndRecruitmentCategory(Long userId, RecruitmentCategory category, Pageable pageable)` | Page\<ApplicationCommonResponseDto\> | public | 사용자별, 카테고리별 지원서 목록 조회 |
| `acceptApplication(Long deciderId, Long applicationId)` | void | public | 지원서 수락 |
| `rejectApplication(Long deciderId, Long applicationId)` | void | public | 지원서 거절 |
| `cancelApplication(Long userId, Long applicationId)` | void | public | 지원서 취소 |
| `closeApplicationsForClosedRecruitments()` | void | public | 마감된 공고에 대한 모든 지원 상태를 CLOSED로 변경 |

---

#### ApplicationServiceImpl

[ApplicationService](#applicationservice)의 구현체로, 공고 지원과 관련된 실제 비즈니스 로직을 수행한다.  
지원서 제출, 조회, 수락/거절, 취소, 마감 공고 처리 등 다양한 기능을 포함하며, 트랜잭션 관리와 권한 검증, 낙관적 락 재시도 로직을 포함한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| applicationRepository | ApplicationRepository | private final | 지원서 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| recruitmentService | RecruitmentService | private final | 공고 관련 비즈니스 로직 계층 |
| teamService | TeamService | private final | 팀 관련 비즈니스 로직 계층 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `submitApplication(Long userId, Long recruitmentId, ApplicationCommonRequestDto requestDto)` | Long | public | 지원서 제출 |
| `findById(Long id)` | Application | public | ID로 지원서 조회 |
| `getAllByUserIdAndRecruitmentCategory(Long userId, RecruitmentCategory category, Pageable pageable)` | Page\<ApplicationCommonResponseDto\> | public | 사용자별, 카테고리별 지원서 목록 조회 |
| `acceptApplication(Long deciderId, Long applicationId)` | void | public | 지원서 수락, 참여 인원 업데이트 및 팀에 추가 |
| `rejectApplication(Long deciderId, Long applicationId)` | void | public | 지원서 거절 |
| `cancelApplication(Long userId, Long applicationId)` | void | public | 지원서 취소 (논리적 삭제) |
| `closeApplicationsForClosedRecruitments()` | void | public | 마감된 공고에 대한 모든 지원 상태를 CLOSED로 변경 |
| `applyProject(User applicant, Project project, ApplicationProjectRequestDto requestDto)` | Long | private | 프로젝트 지원 처리 |
| `applyAssignment(User applicant, Assignment assignment, ApplicationCommonRequestDto requestDto)` | Long | private | 과제 지원 처리 |
| `applyStudy(User applicant, Study study, ApplicationCommonRequestDto requestDto)` | Long | private | 스터디 지원 처리 |
| `recover(ObjectOptimisticLockingFailureException e, Long deciderId, Long applicationId)` | void | protected | 낙관적 락 재시도 실패 시 처리 |

---

#### ApplicationRepository

Application 엔티티의 데이터 접근 계층으로, 지원서 조회, 존재 여부 확인, 마감 공고 상태 업데이트 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `existsByApplicantIdAndRecruitmentId(Long applicantId, Long recruitmentId)` | boolean | public | 특정 사용자가 특정 공고에 이미 지원했는지 확인 |
| `findByIdAndNotDeletedWithRecruitmentAndAuthor(Long id)` | Optional\<Application\> | public | 지원서 조회 시 Recruitment 및 작성자(User)를 즉시 로딩 |
| `findAllByApplicantIdAndRecruitmentCategoryAndIsDeletedFalse(Long applicantId, RecruitmentCategory category, Pageable pageable)` | Page\<Application\> | public | 삭제되지 않은 특정 사용자의 지원서 목록을 카테고리별로 페이지 단위 조회 |
| `closeApplicationsForClosedRecruitments()` | int | public | 마감된 공고에 대한 모든 지원 상태를 CLOSED로 변경하고 업데이트된 건수 반환 |

---

### 3.3.7 Notification Domain

<img width="2970" height="1703" alt="02-notification-domain" src="https://github.com/user-attachments/assets/65e4d8e5-5516-4b0d-8345-1205d7aaa5cc" />

알림 도메인의 구조를 보여주는 다이어그램이다. Notification 엔티티와 Application, User 간의 관계, 그리고 NotificationType 열거형과의 관계를 표현한다.

#### Notification

사용자 간 알림 정보를 담는 엔티티로, 발신자, 수신자, 관련 지원서(Application), 알림 유형, 생성일 및 읽음 여부를 관리한다.  

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| id | Long | private | 알림 ID (PK) |
| sender | User | private | 알림 발신자 |
| receiver | User | private | 알림 수신자 |
| application | Application | private | 알림과 관련된 지원서 |
| type | NotificationType | private | 알림 유형 |
| createdAt | LocalDateTime | private | 알림 생성일 (자동 생성) |
| isRead | boolean | private | 알림 읽음 여부 (기본값 false) |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getId()` | Long | public | 알림 ID 반환 |
| `getSender()` | User | public | 알림 발신자 반환 |
| `getReceiver()` | User | public | 알림 수신자 반환 |
| `getApplication()` | Application | public | 알림 관련 지원서 반환 |
| `getType()` | NotificationType | public | 알림 유형 반환 |
| `getCreatedAt()` | LocalDateTime | public | 알림 생성일 반환 |
| `isRead()` | boolean | public | 알림 읽음 여부 반환 |

---

#### NotificationType

지원 관련 알림 유형을 정의하는 **열거형(Enum)** 으로, 지원 발생, 수락, 거절 상황에 따라 알림 대상이 달라진다.

##### Enum Values

| Value | Description |
|-------|-------------|
| `APPLICATION_SUBMITTED` | 지원 발생 → 공고 작성자에게 알림 |
| `APPLICATION_ACCEPTED`  | 지원 수락 → 지원자에게 알림 |
| `APPLICATION_REJECTED`  | 지원 거절 → 지원자에게 알림 |

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|----------|-------------|
|  |  |  |  |

---

#### NotificationResponseDto

사용자에게 전달되는 알림 정보를 담는 응답 DTO(Record)

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| notificationId | Long | private | 알림 ID |
| applicationId | Long | private | 관련 지원서 ID |
| category | RecruitmentCategory | private | 관련 모집 공고 카테고리 |
| senderNickname | String | private | 알림 발신자 닉네임 |
| type | NotificationType | private | 알림 유형 |
| createdAt | LocalDateTime | private | 알림 생성일 |
| isRead | boolean | private | 알림 읽음 상태 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `notificationId()` | Long | public | 알림 ID 반환 |
| `applicationId()` | Long | public | 관련 지원서 ID 반환 |
| `category()` | RecruitmentCategory | public | 관련 모집 공고 카테고리 반환 |
| `senderNickname()` | String | public | 알림 발신자 닉네임 반환 |
| `type()` | NotificationType | public | 알림 유형 반환 |
| `createdAt()` | LocalDateTime | public | 알림 생성일 반환 |
| `isRead()` | boolean | public | 알림 읽음 상태 반환 |

---

#### NotificationController

알림과 관련된 REST API를 제공하는 컨트롤러로, 사용자의 알림 조회, 삭제 기능을 포함한다.  
인증된 사용자만 접근 가능하며, 서비스 계층(NotificationService)을 통해 실제 비즈니스 로직을 수행한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| notificationService | NotificationService | private final | 알림 관련 비즈니스 로직 서비스 |

##### Operations

| Name | Return Type | Mapping | Visibility | Description |
|------|-----------|---------|-----------|-------------|
| `getMyNotificationsByCategory(category, pageable, userDetails)` | ResponseEntity\<ApiResponse\<Page\<NotificationResponseDto\>\>\> | `GET /api/v1/notifications` | public | 사용자의 알림 목록 조회 (카테고리 선택 가능) |
| `deleteById(notificationId, userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/notifications/{notificationId}` | public | 특정 알림 삭제 |
| `deleteAll(category, userDetails)` | ResponseEntity\<ApiResponse\<Void\>\> | `DELETE /api/v1/notifications` | public | 전체 또는 카테고리별 알림 삭제 |

---

#### NotificationService

알림과 관련된 비즈니스 로직을 정의한 서비스 인터페이스로, 알림 생성, 조회, 삭제 기능을 포함한다.  
실제 구현체([NotificationServiceImpl](#notificationserviceimpl))가 해당 기능을 수행하며, 다른 서비스 로직 내부에서 호출될 수 있다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `createNotification(Long senderId, Long receiverId, Long applicationId, NotificationType type)` | void | public | 알림 생성, 컨트롤러가 아닌 서비스 로직 내부에서 호출 |
| `getAllByUserIdAndCategory(Long receiverId, RecruitmentCategory category, Pageable pageable)` | Page\<NotificationResponseDto\> | public | 사용자별, 카테고리별 알림 목록 조회 |
| `deleteById(Long userId, Long notificationId)` | void | public | 특정 알림 삭제 |
| `deleteAll(Long receiverId, RecruitmentCategory category)` | void | public | 지정된 사용자의 알림 전체 또는 카테고리별 삭제 |

---

#### NotificationServiceImpl

[NotificationService](#notificationservice)의 구현체로, 알림 생성, 조회, 삭제와 관련된 실제 비즈니스 로직을 수행한다.  
권한 검증과 페이지 유효성 검사, 정렬 검증을 포함하며, 트랜잭션을 통해 데이터 변경을 안전하게 처리한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| NOTIFICATION_SORT_FIELDS | Set\<String\> | private static final | 알림 정렬 시 허용되는 필드 목록 |
| notificationRepository | NotificationRepository | private final | 알림 데이터 접근 계층 |
| userService | UserService | private final | 사용자 관련 비즈니스 로직 계층 |
| applicationService | ApplicationService | private final | 공고 지원 관련 비즈니스 로직 계층 |
| pageableValidator | PageableValidator | private final | 페이징 및 정렬 검증 유틸리티 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `createNotification(Long senderId, Long receiverId, Long applicationId, NotificationType type)` | void | public | 알림 생성, 다른 서비스 로직에서 호출 |
| `getAllByUserIdAndCategory(Long receiverId, RecruitmentCategory category, Pageable pageable)` | Page\<NotificationResponseDto\> | public | 사용자별, 카테고리별 알림 목록 조회 |
| `deleteById(Long userId, Long notificationId)` | void | public | 특정 알림 삭제, 권한 검증 포함 |
| `deleteAll(Long receiverId, RecruitmentCategory category)` | void | public | 지정된 사용자의 알림 전체 또는 카테고리별 삭제 |

---

#### NotificationRepository

Notification 엔티티의 데이터 접근 계층으로, 알림 삭제 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `deleteAllByReceiverId(Long receiverId)` | void | public | 특정 사용자의 전체 알림 삭제 |
| `deleteAllByReceiverIdAndCategory(Long receiverId, RecruitmentCategory category)` | void | public | 특정 사용자의 특정 카테고리 알림 삭제 |

---

#### NotificationRepositoryCustom

QueryDSL을 활용한 Notification 데이터 접근 계층 인터페이스로, 사용자별 알림 목록 조회 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
|  |  |  |  |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getAllByUserIdAndCategory(Long receiverId, RecruitmentCategory category, Pageable pageable)` | Page\<NotificationResponseDto\> | public | 지정된 사용자 ID와 카테고리에 해당하는 알림 목록을 페이지 단위로 조회 (category가 null이면 전체 알림 포함) |

---

#### NotificationRepositoryImpl

[NotificationRepositoryCustom](#notificationrepositorycustom) 인터페이스의 구현체로, QueryDSL을 활용하여 사용자별 알림 목록 조회 기능을 제공한다.

##### Attributes

| Name | Type | Visibility | Description |
|------|------|-----------|-------------|
| queryFactory | JPAQueryFactory | private final | QueryDSL용 JPA 쿼리 팩토리 |

##### Operations

| Name | Return Type | Visibility | Description |
|------|-----------|-----------|-------------|
| `getAllByUserIdAndCategory(Long receiverId, RecruitmentCategory category, Pageable pageable)` | Page\<NotificationResponseDto\> | public | 지정된 사용자 ID와 카테고리에 해당하는 알림 목록을 페이지 단위로 조회 (category가 null이면 전체 알림 포함) |

---

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
