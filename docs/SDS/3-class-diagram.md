## 3. Class diagram

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
