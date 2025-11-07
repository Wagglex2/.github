## 2. Use case analysis

### Use Case #1: 로그인한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 ‘와글와글’ 웹 서비스의 모든 기능을 이용하기 위해 로그인해야 한다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 ‘와글와글’ 서비스의 회원가입을 완료한 상태여야 하며, 사용자가 로그인 페이지에 접근한 상태여야 한다. |
| **Trigger** |사용자가 아이디와 비밀번호를 모두 입력한 후 ‘로그인 하기’ 버튼을 클릭한다. |
| **Success Post Condition** |시스템이 사용자를 인증하고 로그인 상태로 변경한다. 이후 서비스의 메인 페이지로 이동한다. |
| **Failed Post Condition** |시스템이 사용자를 인증하지 못하고 로그인 상태가 변경되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 로그인한다. |
| **1** |이 Use case는 사용자가 ‘와글와글’ 에 로그인할 때 시작된다. |
| **2** |사용자가 로그인 페이지에서 아이디와 비밀번호를 입력하고 로그인 버튼을 클릭한다. |
| **3** |시스템은 입력된 아이디와 비밀번호를 회원 데이터베이스 정보와 비교해보고 등록된 사용자라면 로그인에 성공한다. |
| **4** |이 Use case 는 로그인이 성공하면  메인 페이지로 이동한 후 종료된다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**2** |2a. 사용자가 아이디 또는 비밀번호를 입력하지 않고 ‘로그인 하기’ 버튼을 클릭한다. <br> ...2a1. 시스템은 잘못된 입력 방식임을 알리는 오류 메시지를 표시한다. <br> ...2a2. 아이디와 비밀번호를 입력하는 단계로 돌아간다. |
|**3** |3a. 아이디나 비밀번호가 잘못되어 로그인에 실패한다. <br> ...3a1. 아이디 또는 비밀번호가 잘못되었다는 오류 메시지를 표시한다. <br> ...3a2. 아이디와 비밀번호를 입력하는 단계로 돌아간다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 2 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|


---

### Use Case #2: 로그아웃한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 서비스를 로그아웃하는 기능이다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 로그인한 상태여야 한다. |
| **Trigger** |사용자가 웹 서비스 공통 헤더의 우측에 있는 '로그아웃' 버튼을 클릭한다.  |
| **Success Post Condition** |로그아웃 상태가 되고, 시스템은 사용자를 서비스의 초기 화면인 ‘로그인 페이지’로 이동한다. |
| **Failed Post Condition** |로그아웃에 실패하고, 사용자는 현재 페이지에 머무른다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 ‘와글와글’ 웹 서비스를 로그아웃한다. |
| **1** |이 Use Case는 로그인한 사용자가 헤더 우측의 '로그아웃' 버튼을 클릭하는 것으로 시작된다. |
| **2** |시스템은 현재 사용자의 세션을 종료하라는 요청을 서버로 전송한다. |
| **3** |서버는 사용자의 세션을 성공적으로 종료하고 성공 응답을 반환한다. |
| **4** |시스템은 로그아웃이 완료됐음을 알리는 팝업창을 표시한다. |
| **5** |시스템은 팝업창이 닫힌 후, 사용자를 로그인 페이지로 자동 이동시킨다. |
| **6** |이 Use Case는 로그인 페이지가 성공적으로 표시되면 종료된다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**3** |3a. 서버가 세션 종료 요청 처리에 실패한다. <br> ...3a1. 시스템은 로그아웃에 실패했음을 알리는 메시지를 표시한다. <br> ...3a2. 사용자는 현재 페이지에 머무르며 로그인 상태가 유지된다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 2 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---

### Use Case #3: 회원가입한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |시스템의 사용자가 되기 위한 절차이며 모든 사용자는 사용에 앞서 회원가입을 해야 한다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 시스템의 로그인 화면에 접근한 상태이다. |
| **Trigger** |사용자가 웹 사이트의 로그인 화면에서 회원가입 버튼을 누른다. |
| **Success Post Condition** |사용자의 계정이 생성되어 시스템에 로그인 가능한 상태가 되고 로그인 페이지로 이동한다. |
| **Failed Post Condition** |사용자의 계정이 생성되지 않으며 사용자는 회원가입 페이지에 머무른다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 회원가입을 한다. |
| **1** |사용자가 웹 사이트의 로그인 화면에서 회원가입 버튼을 누른다. |
| **2** |회원가입 화면으로 넘어간다. |
| **3** |사용자는 닉네임을 입력한 후 중복 확인 버튼을 누른다. |
| **4** |시스템은 닉네임 유효성을 검사하고 사용가능한 닉네임인지 메시지를 표시한다. |
| **5** |사용자는 학교 공식 이메일 계정을 입력한 후 인증 번호 받기 버튼을 누른다. |
| **6** |사용자가 수신한 인증번호를 입력하고 확인 버튼을 누른다. |
| **7** |시스템은 인증번호가 일치하는지 확인하고 인증되었다는 메시지를 표시한다. |
| **8** |사용자가 비밀번호를 입력하고 비밀번호 확인 칸에도 동일하게 입력한다. |
| **9** |시스템은 두 비밀번호가 일치하고 비밀번호 형식에 맞는지 검사한다. |
| **10** |사용자가 가입하기 버튼을 누른다.|
| **11** |시스템은 모든 항목이 정상적으로 완료되었는지 확인한다. |
| **12** |시스템은 사용자 정보를 데이터베이스에 저장하여 계정 생성을 완료하고 로그인 페이지로 이동한다.|

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**4** |4a. 닉네임이 중복인 경우 <br> ...4a1. 닉네임 입력 칸 밑에 “이미 사용중인 닉네임입니다.” 라고 표시한다. |
|**4** |4b. 닉네임의 형식이 기준과 맞지 않을 경우 <br> ...4b1. 시스템은 “닉네임의 형식이 올바르지 않습니다.”라고 표시한다. |
|**7**|7a. 인증번호가 틀린 경우 <br> ...7a1. 시스템은 “인증번호가 일치하지 않습니다.”라고 표시한다. |
|**9**|9a. 비밀번호의 형식이 일치하지 않는 경우 <br> ...9a1. 비밀번호 입력 칸 밑에 “영문자, 숫자, 특수문자 포함 8~20자여야 합니다.” 와 같이 형식을 표시한다.|
|**9** |9b. 비밀번호가 일치하지 않는 경우 <br> ...9b1. 시스템은 비밀번호 확인 칸 밑에 “비밀번호가 일치하지 않습니다.”라고 표시한다.|
|**11** |11a. 필수 항목이 완료되지 않은 경우 <br> ...11a1. 사용자가 가입하기 버튼을 눌렀으나 닉네임 중복 확인, 이메일 인증 등이 완료되지 않았는 경우에 시스템은 “필수 항목을 완료해주세요.”라는 팝업을 띄운다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |사용자당 1번|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #4: 회원탈퇴한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 회원탈퇴를 하기 위한 절차이다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |로그인 상태여야 한다. |
| **Trigger** |사용자가 내 프로필에서 회원탈퇴 버튼을 누른다.|
| **Success Post Condition** |사용자의 계정 정보가 삭제되고 사용자는 로그아웃되어 로그인 페이지로 이동한다. |
| **Failed Post Condition** |사용자의 계정 정보가 유지되고 사용자는 로그인 상태로 회원탈퇴 페이지에 머무른다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 회원탈퇴를 한다.|
| **1** |사용자가 내 프로필 페이지에서 회원탈퇴 버튼을 누른다. |
| **2** |시스템은 “정말로 탈퇴하시겠습니까?”라는 경고 팝업을 표시한다.|
| **3** |사용자가 경고 팝업에서 확인 버튼을 누른다 |
| **4** |시스템은 해당 사용자의 계정 정보를 데이터베이스에서 삭제 처리한다. |
| **5** |시스템은 사용자를 강제 로그아웃시킨다. |
| **6** |시스템은 “회원탈퇴가 성공적으로 완료되었습니다.”메세지를 표시하고 로그인 페이지로 리디렉션한다.|


#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**3** |3a. 사용자가 탈퇴를 취소하는 경우 <br> ...3a1. 시스템은 경고 팝업을 닫고 사용자는 내 프로필 페이지에 머무른다. |


#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |사용자당 1번|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #5: 기본 정보 입력한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |시스템의 사용자가 되기 위한 절차이며 사용자의 기본 정보를 입력해야 한다.|
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 회원가입을 완료하고 해당 계정으로 최초 로그인을 성공한 상태이다.|
| **Trigger** |사용자가 최초 로그인을 성공하면 시스템이 프로필이 비어있음을 감지하고 자동으로 기본 정보 입력 모달을 표시한다.|
| **Success Post Condition** |기본 정보 입력에 성공한다.|
| **Failed Post Condition** |기본 정보 입력에 실패한다.|

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 사용자의 기본 정보를 입력한다.|
| **1** |시스템은 메인 메뉴 위로 기본 정보 입력 모달을 표시한다. 모달에는 학년, 포지션, 기술, 한 줄 소개 입력 폼이 포함된다.|
| **2** |사용자가 폼에 맞춰 자신의 학년, 포지션, 기술, 한 줄 소개를 입력한다.|
| **3** |사용자가 완료 버튼을 누른다.|
| **4** |시스템은 입력된 정보의 유효성을 검사한다.|
| **5** |시스템은 사용자 정보를 데이터베이스에 저장한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**4** |4a. 필수 항목이 누락된 경우 <br> ...4a1. 시스템이 필수 항목이 입력되지 않았음을 확인한다. <br> ...4a2. 시스템은“필수 항목을 모두 입력해주세요.”라는 오류 팝업을 표시한다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |사용자당 1번|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---

### Use Case #6: 공고 검색한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 헤더의 검색창에 특정 키워드를 입력하여, 등록된 공고 중 원하는 정보를 찾는다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 로그인한 상태여야 하며, 메인 페이지 등의 헤더 검색창이 보이는 페이지에 접근한 상태여야 한다. |
| **Trigger** |사용자가 헤더 검색창에 검색어를 입력하고 검색을 실행한다.  |
| **Success Post Condition** |시스템이 검색 결과 페이지로 이동한 후, 입력된 키워드와 일치하는 공고 목록이 화면에 표시된다. |
| **Failed Post Condition** |일치하는 공고가 없을 경우, 검색 결과가 없음을 알리는 메시지가 표시된다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 헤더의 검색창을 통해 검색한다. |
| **1** |이 Use Case는 사용자가 헤더 검색창에서 검색할 카테고리를 선택하고 키워드를 입력하는 것으로 시작된다. |
| **2** |사용자가 검색 아이콘을 클릭하거나 Enter 키를 누른다. |
| **3** |시스템은 선택된 카테고리 내에서, 입력된 키워드를 포함하는 공고 목록을 데이터 베이스에서 조회한다. |
| **4** |시스템은 ‘검색 결과 페이지’로 이동한다. |
| **5** |시스템은 조회된 공고 목록을 카드 형태로 화면에 표시한다. |
| **6** |이 Use Case는 검색 결과가 성공적으로 화면에 표시되면 종료된다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**2** |2a. 사용자가 카테고리는 선택했으나, 키워드를 입력하지 않고 검색을 시도한다. <br> ...2a1. 시스템은 검색을 수행하지 않는다. |
|**4** |4a. 시스템이 키워드와 일치하는 공고를 하나도 찾지 못한다. <br> ...4a1. 시스템은 ‘검색 결과 페이지’로 이동하여, 일치하는 검색 결과가 없음을 알리는 메시지를 목록 영역에 표시한다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 3 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|


---

### Use Case #7: 공고 필터링한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 공고 조회 페이지에서 제공되는 여러 필터를 선택하여 원하는 조건의 공고 목록만 선별하여 조회한다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 '프로젝트', '과제', '스터디' 중 하나의 공고 내역 조회 페이지에 접근한 상태여야 하며, 화면에 최소 1개 이상의 공고 카드가 로드되어 있어야 한다. |
| **Trigger** |사용자가 페이지 상단의 필터 드롭다운 중 하나 이상을 선택/해제한다.  |
| **Success Post Condition** |공고 목록이 사용자가 선택한 필터 조건에 맞는 공고들로 즉시 갱신되어 화면에 표시된다. |
| **Failed Post Condition** |필터 적용에 실패하고, 공고 목록은 변경되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 필터 드롭다운을 통해 필터링한다. |
| **1** |이 Use Case는 사용자가 공고 내역 조회 페이지에서 필터 기준을 클릭하는 것으로 시작된다. |
| **2** |시스템은 해당 기준의 하위 옵션 목록을 표시한다. |
| **3** |사용자가 하나 이상의 옵션을 선택 또는 해제한다. |
| **4** |시스템은 선택된 모든 필터 조건을 데이터베이스에 실시간으로 공고 목록을 다시 요청한다. |
| **5** |시스템은 필터 조건과 일치하는 새로운 공고 목록을 응답받는다. |
| **6** |시스템은 현재 공고 목록 영역을 새로운 공고 목록으로 갱신하여 표시한다. |
| **7** |이 Use Case는 필터링된 결과가 성공적으로 화면에 표시되면 종료된다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**4** |4a. 사용자가 ‘마감된 공고만’ 토글 버튼을 클릭한다. <br> ...4a1. 시스템은 현재 적용된 다른 필터 조건에 ‘마감됨’ 조건을 추가 또는 해제하여 데이터베이스 요청부터 다시 수행한다. |
|**5** |5a. 시스템이 선택된 필터 조건과 일치하는 공고를 하나도 찾지 못한다. <br> ...5a1. 시스템은 공고 목록 영역에 선택한 조건과 일치하는 공고가 없음을 알리는 메시지를 표시한다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 2 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #8: 공고 내역 조회한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 헤더의 메인 카테고리(프로젝트, 과제, 스터디)를 선택하여, 해당 카테고리에 속한 전체 공고 목록을 조회한다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 로그인한 상태여야 한다. |
| **Trigger** |사용자가 헤더의 메인 내비게이션 바에서 '프로젝트', '과제', '스터디' 탭 중 하나를 클릭한다.  |
| **Success Post Condition** |시스템이 선택한 카테고리의 공고 목록 페이지로 이동한 후, 해당 카테고리의 전체 공고 목록이 화면에 카드 형태로 표시된다. |
| **Failed Post Condition** |공고 목록을 불러오지 못한다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 헤더의 카테고리 탭을 선택하여 공고 목록을 조회한다. |
| **1** |이 Use Case는 사용자가 헤더의 메인 카테고리 탭을 클릭하는 것으로 시작된다. |
| **2** |시스템은 서버에 해당 카테고리의 모든 공고 목록을 요청한다. |
| **3** |시스템은 서버로부터 공고 목록 데이터를 수신한다. |
| **4** |시스템은 ‘공고 내역 조회’ 페이지로 이동하여 수신한 공고 목록을 카드 형태로 나열한다. |
| **5** |이 Use Case는 공고 목록이 성공적으로 화면에 표시되면 종료된다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**4** |4a. 해당 카테고리에 등록된 공고가 하나도 없다. <br> ...4a1. 시스템은 목록 영역에 등록된 공고가 없음을 알리는 메시지를 표시한다. |


#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 2 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #9: 공고 찜한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 공고 목록이나 상세 페이지에서 관심 있는 공고를 '찜'한다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 로그인한 상태여야 하며, 사용자가 공고 목록이나 공고 상세 페이지를 보고 있어야 한다. |
| **Trigger** |사용자가 아직 찜하지 않은 공고의 ‘찜하기’ 아이콘을 클릭한다.  |
| **Success Post Condition** |해당 공고가 사용자의 '찜한 공고' 목록에 추가되고, '찜하기' 아이콘이 '찜 완료' 상태로 즉시 변경된다. |
| **Failed Post Condition** |공고가 찜 목록에 추가되지 않고, 아이콘이 변경되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 ‘찜하기’ 아이콘을 클릭해 공고를 찜한다. |
| **1** |이 Use Case는 사용자가 공고 목록 또는 공고 상세 페이지에서 찜하지 않은 공고의 '찜하기' 아이콘을 클릭하는 것으로 시작된다. |
| **2** |서버는 해당 공고와 사용자 정보를 데이터베이스에 저장한다. |
| **3** |시스템은 즉시 찜하기 아이콘의 시각적 상태를 ‘찜 완료’로 변경한다. |
| **4** |이 Use Case는 찜 상태가 성공적으로 반영되면 종료된다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**2** |2a. 서버가 데이터베이스 저장에 실패하는 등 기술적 오류가 발생한다. <br> ...2a1. 시스템은 서버로부터 오류 응답을 받는다. <br> ...2a2. 찜하기 아이콘은 찜하지 않은 상태로 유지된다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|


---
### Use Case #10: 찜한 공고 취소한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 이전에 '찜'했던 공고를 개인 찜 목록에서 제거한다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 해당 공고를 이미 ‘찜’한 상태여야 하며, 해당 공고의 ‘찜하기’ 아이콘이 보이는 페이지에 있어야 한다. |
| **Trigger** |사용자가 이미 '찜 완료' 상태인 공고의 '찜하기' 버튼을 클릭한다.  |
| **Success Post Condition** |해당 공고가 사용자의 ‘찜한 공고’ 목록에서 제거되며 ‘찜하기’ 아이콘이 ‘찜하지 않은’ 상태로 즉시 변경된다. 만약 찜한 공고 조회 페이지에서 취소한 경우, 해당 공고가 카드 목록에서 사라진다. |
| **Failed Post Condition** |아이콘이 변경되지 않고 공고가 찜 목록에서 제거되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 찜하기를 취소한다. |
| **1** |이 Use Case는 사용자가 이미 활성화되어 있는 ‘찜하기’ 아이콘을 클릭하는 것으로 시작된다. |
| **2** |서버는 해당 사용자의 찜 목록에서 해당 공고 정보를 삭제한다. |
| **3** |시스템은 즉시 ‘찜하기’ 아이콘의 시각적 상태를 ‘찜하지 않은’ 상태로 변경한다. |
| **4** |이 Use Case는 찜 취소가 성공적으로 반영되면 종료된다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**2** |2a. 서버가 데이터베이스 삭제에 실패하는 등 기술적 오류가 발생한다. <br> ...2a1. 시스템은 서버로부터 오류 응답을 받는다. <br> ...2a2. 찜하기 아이콘은 ‘찜 완료’ 상태로 유지된다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #11: 찜한 공고 조회한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 자신이 '찜'했던 모든 공고를 한곳에 모아보고 관리할 수 있는 페이지를 조회한다.  |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자가 로그인한 상태여야 한다. |
| **Trigger** |사용자가 마이페이지의 사이드바 메뉴에서 ‘찜한 공고’를 클릭한다.  |
| **Success Post Condition** |시스템이 찜한 공고 조회 페이지로 이동한 후, 찜한 모든 공고의 목록이 카테고리 별로 화면에 표시된다. |
| **Failed Post Condition** |찜한 공고 목록을 불러오지 못한다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 찜한 공고 목록을 조회한다. |
| **1** |이 Use Case는 사용자가 마이페이지의 사이드바 메뉴에서 ‘찜한 공고’ 메뉴를 클릭하는 것으로 시작된다. |
| **2** |시스템은 찜한 공고 조회 페이지로 이동한다. |
| **3** |시스템은 서버에 현재 로그인한 사용자의 찜한 공고 목록 전체를 요청하고 반환 받는다. |
| **4** |시스템은 수신한 찜 공고 목록을 카테고리를 나누어 카드 형태로 나열한다. |
| **5** |이 Use Case는 찜한 공고 목록이 성공적으로 화면에 표시되면 종료된다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**4** |4a. 사용자가 찜한 공고가 하나도 없다. <br> ...4a.1. 시스템은 목록 영역에 등록된 찜한 공고가 없음을 알리는 메시지를 표시한다. |
|**4** |4b. 사용자가 페이지 내의 ‘마감된 공고만’ 토글을 적용한다. <br> ...4b1. 시시스템은 현재 찜 목록 내에서 조건에 맞는 공고만 선별하여 표시한다. |
|**4** |4c.사용자가 찜한 공고 목록에 있는 공고의 ‘찜하기’ 버튼을 클릭한다. <br> ...4c1. 해당 공고는 찜한 공고 목록에서 삭제된다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 2 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|


---
### Use Case #12: 알림을 조회한다.
#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 공고 지원에 대한 결과(승인/거절)와 본인이 게시한 공고에 대한 지원 알림을 조회할 수 있다.|
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자는 로그인을 성공한 상태이다.|
| **Trigger** |사용자가 서비스 최상단의 [알림]버튼을 클릭한다.|
| **Success Post Condition** |사용자에게 알림 내역이 정상적으로 노출된다.|
| **Failed Post Condition** |사용자는 알림 내역을 조회할 수 없다.|

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자는 알림 내역을 조회할 수 있다.|
| **1** |이 use case는 사용자가 [알림]버튼을 눌렀을 때 시작된다.|
| **2** |시스템은 ‘알림’ 페이지로 화면을 전환한다. |
| **3** |사용자가 본인에게 온 알림을 확인한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**3** |3a. 사용자가 특정 알림을 선택하여 클릭할 경우</br>...3a1. 시스템은 알림의 유형(지원 결과 / 게시한 공고에 대한 지원 요청)에 따라마이페이지의 ‘내가 지원한 공고’ 또는 ‘내가 올린 공고’ 페이지로 화면을 전환한다.</br>...3a2. 시스템은 해당 알림을 ‘읽음’ 상태로 변경한다.</br></br>3b. 사용자가 알림을 카테고리(프로젝트, 과제, 스터디)별로 필터링하려는 경우,</br>...3b1. 사용자는 카테고리 필터링 옵션을 선택한다.</br>...3b2. 시스템은 선택된 카테고리에 해당하는 알림만 화면에 표시한다.</br></br>3c. 조회 결과가 없는 경우,</br>...3c1. 시스템은 "조회된 알림이 없습니다." 메시지를 화면에 표시한다.|

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #13: 알림을 개별 삭제한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 ‘알림’ 페이지에서 특정 알림을 선택하여 삭제할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자는 로그인을 성공한 상태이다.</br>조회된 알림이 1개 이상이어야 한다. |
| **Trigger** |특정 알림 카드의 [X] 버튼을 클릭한다. |
| **Success Post Condition** |삭제된 알림은 알림 목록에 표시되지 않는다. |
| **Failed Post Condition** |알림 목록이 기존 상태와 동일하게 유지된다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)
| Step | Action |
| :--- | :--- |
| **S** |사용자는 특정 알림을 선택하여 삭제할 수 있다. |
| **1** |Use case는 사용자가 특정 알림의 [X] 버튼을 클릭했을 때 시작된다. |
| **2** |시스템은 "해당 알림을 삭제하시겠습니까?"라는 확인 팝업을 띄운다. |
| **3** |사용자는 팝업의 [확인] 버튼을 클릭한다. |
| **4** |시스템은 선택된 알림을 삭제한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)
| Step | Branching Action |
| :--- | :--- |
|**3**|3a. 사용자가 [취소] 버튼을 클릭할 경우<br>…3a1. 삭제 요청은 실행되지 않고 기존 알림 목록이 유지된다. |
|**4** |4a. 알림 삭제에 실패한 경우<br>…4a1. 시스템은 "알림 삭제에 실패하였습니다. 다시 시도해 주세요." 팝업을 띄운다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #14: 모든 알림을 삭제한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 조회된 모든 알림을 일괄 삭제할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자는 로그인을 성공한 상태이다.<br>조회된 알림이 1개 이상이어야 한다.|
| **Trigger**|[전체 삭제] 버튼을 클릭한다. |
| **Success Post Condition** |모든 알림이 삭제되며, “조회된 알림이 없습니다.” 메시지가 표시된다. |
| **Failed Post Condition** |알림 목록이 기존 상태와 동일하게 유지된다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)
| Step | Action |
| :--- | :--- |
| **S** |사용자는 조회된 모든 알림을 일괄 삭제할 수 있다. |
| **1** |사용자는 ‘알림’ 페이지에서 카테고리 필터링의 [전체] 탭을 선택한다. |
| **2** |사용자는 [전체 삭제] 버튼을 클릭한다. |
| **3** |시스템은 "모든 알림을 삭제하시겠습니까?"라는 확인 팝업을 띄운다. |
| **4** |사용자는 팝업의 [확인] 버튼을 클릭한다. |
| **5** |시스템은 모든 알림을 삭제하고, “조회된 알림이 없습니다.” 메시지를 표시한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)
| Step | Branching Action |
| :--- | :--- |
| **4** |4a. 사용자가 [취소] 버튼을 클릭할 경우<br>…4a1. 삭제 요청은 실행되지 않고 기존 알림 목록이 유지된다.|
| **5** |5a. 알림 삭제에 실패한 경우<br>…5a1. 시스템은 "알림 삭제에 실패하였습니다. 다시 시도해 주세요." 팝업을 띄운다. |

#### 4. RELATED INFORMATION (관련 정보)
| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|
---
### Use Case #15: 카테고리별 알림을 일괄 삭제한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 ‘알림’ 페이지에 조회된 특정 카테고리의 알림을 일괄 삭제할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자는 로그인을 성공한 상태이다.<br>조회된 알림이 1개 이상이어야 한다.|
| **Trigger**|[전체 삭제]버튼을 클릭한다. |
| **Success Post Condition** |선택한 카테고리의 알림이 모두 삭제되며, “조회된 알림이 없습니다.” 메시지가 표시된다. |
| **Failed Post Condition** |알림 목록이 기존 상태와 동일하게 유지된다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)
| Step | Action |
| :--- | :--- |
| **S** |사용자는 특정 카테고리의 알림을 일괄 삭제할 수 있다. |
| **1** | 사용자는 ‘알림’ 페이지의 카테고리 탭 ([프로젝트], [과제], [스터디]) 중 하나를 선택한다. |
| **2** |사용자는 [전체 삭제]버튼을 클릭한다. |
| **3** |시스템은 "선택한 카테고리의 알림을 모두 삭제하시겠습니까?"라는 확인 팝업을 띄운다. |
| **4** |사용자는 팝업의 [확인] 버튼을 클릭한다. |
| **5** |시스템은 해당 카테고리의 알림을 모두 삭제하고, “조회된 알림이 없습니다.” 메시지를 표시한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)
| Step | Branching Action |
| :--- | :--- |
| **4** |4a. 사용자가 [취소] 버튼을 클릭할 경우<br>…4a1. 삭제 요청은 실행되지 않고 기존 알림 목록이 유지된다. |
| **5** |5a. 삭제에 실패한 경우<br>…5a1. 시스템은 "알림 삭제에 실패하였습니다. 다시 시도해 주세요." 팝업을 띄운다 |

#### 4. RELATED INFORMATION (관련 정보)
| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|
---
### Use Case #16: 공고를 작성한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 프로젝트, 과제, 스터디 팀원 모집 공고를 작성할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자는 로그인을 성공한 상태이다.|
| **Trigger**|[공고 등록하기]드롭다운을 클릭하여 옵션(프로젝트, 과제, 스터디)을 선택한다. |
| **Success Post Condition** |공고가 정삭적으로 등록되어 타 사용자들에게 노출된다. |
| **Failed Post Condition** |공고가 등록되지 않는다. |
#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)
| Step | Action |
| :--- | :--- |
| **S** |사용자는 팀원 모집 공고를 작성할 수 있다. |
| **1** |사용자는 [공고 등록하기]드롭다운을 클릭하여 [프로젝트] 또는 [과제] 또는 [스터디] 옵션을 선택한다. |
| **2** |시스템은 사용자가 선택한 옵션에 해당하는 공고 작성 페이지로 화면을 전환한다. |
| **3** |사용자는 작성 폼의 모든 필드을 입력하고, 사용자는 [등록하기]버튼을 클릭한다. |
| **4** |시스템은 공고를 저장한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)
| Step | Branching Action |
| :--- | :--- |
| **2** |2a. 사용자가 ‘프로젝트’옵션을 선택한 경우<br>…2a1. 시스템은 작성 폼의 입력 필드로 목적, 공고 제목, 공고 마감일, 프로젝트 진행기간, 진행 방식, 학년, 포지션, 기술, 상세 내용을 표시한다. <br>2b. 사용자가 ‘과제’옵션을 선택한 경우<br>…2b1. 시스템은 작성 폼의 입력 필드로 공고 제목, 공고 마감일, 학과, 과목코드, 학년, 포지션(모집 인원), 상세 내용을 표시한다.<br>2c. 사용자가 ‘스터디’옵션을 선택한 경우<br>…2c1. 시스템은 작성 폼의 입력 필들로 공고 제목, 공고 마감일, 스터디 진행기간, 포지션(모집인원), 기술, 상세 내용을 표시한다. |
| **3** |3a. 사용자가 [등록하기]버튼을 클릭하지 않고 페이지를 벗어나려할 경우<br>...3a1. 시스템은 "지금 나가면 작성 내용이 저장되지 않습니다. 계속 진행하시겠습니까?"라는 경고 팝업을 띄운다. |
| **4** |4a. 공고 저장에 실패할 경우<br>...4a1. 시스템은 "공고 등록에 실패하였습니다. 다시 시도해 주세요." 라는 팝업을 띄운다. |

#### 4. RELATED INFORMATION (관련 정보)
| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #17: 공고 상세 내용을 조회한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 특정 공고의 상세 내용을 조회할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자는 로그인을 성공한 상태이다.<br>서비스 사용자가 등록한 공고가 1개 이상 존재하여야 한다.|
| **Trigger**|특정 공고 카드를 클릭한다. |
| **Success Post Condition** |선택한 공고의 상세 내용이 정상적으로 화면에 표시된다. |
| **Failed Post Condition** |사용자는 선택한 공고의 상세 내용을 확인할 수 없다. |
#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)
| Step | Action |
| :--- | :--- |
| **S** |사용자는 특정 공고의 상세 내용을 조회할 수 있다. |
| **1** |사용자는 공고 내역 조회 화면에서 특정 공고 카드를 클릭한다. |
| **2** |시스템은 해당 공고의 상세 페이지로 화면을 전환한다. |
| **3** |시스템은 공고의 상세 내용을 화면에 표시한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)
| Step | Branching Action |
| :--- | :--- |
| **3** |3a. 공고 내용을 불러오지 못한 경우<br>...3a1. 시스템은 "데이터 불러오기에 실패하였습니다. 다시 시도해 주세요" 라는 팝업을 띄운다. |

#### 4. RELATED INFORMATION (관련 정보)
| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #18: 공고에 지원한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 공고 상세 페이지에서 지원서를 작성하여 지원할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자는 로그인을 성공한 상태이다.<br>공고가 1개 이상 존재해야 한다.|
| **Trigger**|지원서 작성 후 [지원하기] 버튼을 클릭한다. |
| **Success Post Condition** |지원서가 저장되고 공고 등록자가 확인할 수 있다. |
| **Failed Post Condition** |지원서 저장에 실패하며 실패 팝업이 뜬다. |
#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)
| Step | Action |
| :--- | :--- |
| **S** |사용자는 특정 공고에 지원할 수 있다. |
| **1** |사용자는 공고 상세 페이지에서 지원서 작성 폼을 확인한다. |
| **2** |사용자는 입력 항목을 작성한다. |
| **3** |사용자는 [지원하기] 버튼을 클릭한다. |
| **4** |시스템은 지원서를 저장하고 “지원이 완료되었습니다.” 팝업을 띄운다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)
| Step | Branching Action |
| :--- | :--- |
| **1** |1a. 사용자가 보고 있는 공고의 카테고리가 [프로젝트]인 경우<br>…1a1. 시스템은 지원서의 입력 필드로 /참여 가능한 진행방식/학년/지원 포지션/사용 가능한 기술/상세 설명/을 표시한다.<br>1b. 사용자가 보고 있는 공고의 카테고리가 [과제]인 경우<br>…1b1. 시스템은 지원서의 입력 필드로 /참여 가능한 진행방식/학년/상세 설명/을 표시한다.<br>1c. 사용자가 보고 있는 공고의 카테고리가 [스터디]인 경우<br>…1c1. 시스템은 지원서의 입력 필드로 /참여 가능한 진행방식/학년/상세 설명/을 표시한다.|
| **3** |3a. 사용자가 [지원하기]버튼을 클릭하지 않고 페이지를 벗어나려할 경우<br>...3a1. 시스템은 "지금 나가면 작성 내용이 저장되지 않습니다. 계속 진행하시겠습니까?"라는 경고 팝업을 띄운다. |
| **4** |4a. 지원서 저장에 실패할 경우<br>...4a1. 시스템은 "지원서 저장에 실패하였습니다. 다시 시도해 주세요." 라는 팝업을 띄운다.|

#### 4. RELATED INFORMATION (관련 정보)
| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #19: 회원정보를 조회한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 프로필 페이지에서 본인의 회원정보를 조회할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |사용자는 로그인을 성공한 상태이다.|
| **Trigger**| 마이페이지 사이드바의 [프로필] 탭 선택한다. |
| **Success Post Condition** |회원정보가 화면에 표시된다. |
| **Failed Post Condition** |회원정보를 표시할 수 없다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)
| Step | Action |
| :--- | :--- |
| **S** |사용자는 회원정보를 조회할 수 있다. |
| **1** |사용자는 마이페이지에서 [프로필] 탭을 선택한다. |
| **2** |시스템은 프로필 이미지, 인증된 학교, 이메일, 닉네임, 학년, 포지션, 기술, 한 줄 소개, 피드백 목록을 표시한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)
| Step | Branching Action |
| :--- | :--- |
| **2** |2a. 데이터를 불러오지 못한 경우<br>…2a1. "데이터 불러오기에 실패하였습니다. 다시 시도해 주세요." 팝업을 띄운다|

#### 4. RELATED INFORMATION (관련 정보)
| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #20: 회원정보를 수정한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 프로필 페이지에서 회원정보를 수정할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |자는 로그인을 성공한 상태이다.|
| **Trigger**|[수정하기] 버튼을 클릭한다. |
| **Success Post Condition** |수정된 회원정보가 저장된다. |
| **Failed Post Condition** |기존 정보가 유지된다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자는 회원정보를 수정할 수 있다. |
| **1** |사용자는 [프로필] 탭을 선택한다. |
| **2** |사용자는 수정 가능한 값(프로필 이미지, 닉네임, 학년, 포지션, 기술, 한 줄 소개)을 수정한다. |
| **3** |사용자는 [수정하기] 버튼을 클릭한다. |
| **4** |시스템은 수정된 정보를 저장한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
| **2** |2a. 비밀번호를 수정하려는 경우<br>…2a1. 사용자는 [비밀번호 변경하기] 버튼을 클릭한다.<br>…2a2. 시스템은 Use Case: 비밀번호를 수정한다 를 실행한다. |
| **4** |4a. 저장에 실패한 경우<br>…4a1. "변경 내용 저장에 실패하였습니다. 다시 시도해 주세요." 팝업을 띄운다.|


#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #21: 비밀번호를 수정한다.
| 항목 | 내용 |
| :--- | :--- |
| **Summary** | 사용자는 비밀번호 변경 페이지에서 비밀번호를 수정할 수 있다.|
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|system |
| **Preconditions** |자는 로그인을 성공한 상태이다.|
| **Trigger**|[변경하기] 버튼을 클릭한다.| |
| **Success Post Condition** |비밀번호가 변경된다. |
| **Failed Post Condition** |기존 비밀번호가 유지된다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자는 비밀번호를 수정할 수 있다. |
| **1** |사용자는 프로필에서 [비밀번호 변경하기] 버튼을 클릭한다. |
| **2** |시스템은 비밀번호 변경 페이지로 이동한다. |
| **3** |사용자는 기존 비밀번호, 새 비밀번호, 새 비밀번호 확인을 입력한다. |
| **4** |[변경하기] 버튼을 클릭한다. |
| **5** |시스템은 비밀번호를 저장하고 "비밀번호가 변경되었습니다." 팝업을 띄운다.|

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
| **4**|4a. 저장 실패한 경우<br>…4a1. "변경 내용 저장에 실패하였습니다. 다시 시도해 주세요." 팝업을 띄운다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |제한 없음|
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---

### Use Case #22: 팀 조회한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 자신이 속한 팀의 목록을 확인하고 특정 팀을 선택하여 팀원 구성을 조회한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인 상태여야 한다. |
| **Trigger** |사용자가 마이페이지에서 내 팀 관리 카테고리를 누른다. |
| **Success Post Condition** |사용자가 자신의 팀 목록 및 선택한 팀의 팀원 정보를 성공적으로 확인한다. |
| **Failed Post Condition** |팀 정보를 불러오지 못한다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 팀원의 정보를 확인한다. |
| **1** |사용자가 마이페이지에서 내 팀 관리를 누른다. |
| **2** |시스템은 사용자가 속한 모든 팀의 목록(공고 제목, 진행 상태, 리더, 인원)을 표시한다. |
| **3** |사용자가 목록에서 특정 팀의 공고 제목을 누른다. |
| **4** |시스템은 해당 팀의 팀원 목록(닉네임, 포지션, 리더/팀원 여부)을 드롭다운으로 표시한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**2** |2a. 사용자가 속한 팀이 없는 경우 <br> ...2a1. 시스템은 “참여 중인 팀이 없어요.”라는 메시지를 목록 영역에 표시한다. |
|**4** |4a. 시스템 오류로 팀 정보를 불러오지 못하는 경우 <br> ...4a1. 시스템은 “팀 정보를 불러오는 데 실패했습니다.”라는 오류 팝업을 표시한다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #23: 팀원 삭제한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 리더일 경우 팀원을 삭제한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 리더 권한을 가지고 있어야 한다. <br> 사용자가 팀원 조회 화면에 접근해 있어야 한다. |
| **Trigger** |리더가 특정 팀원 옆의 삭제하기 버튼을 누른다. |
| **Success Post Condition** |팀원 삭제에 성공하고 팀원 목록이 갱신된다. |
| **Failed Post Condition** |팀원 삭제에 실패한다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 팀원의 정보를 삭제한다. |
| **1** |리더가 삭제하기 버튼을 누른다. |
| **2** |시스템이 “정말 팀에서 삭제하시겠습니까?”라는 확인 팝업을 띄운다. |
| **3** |리더가 확인을 누른다. |
| **4** |시스템이 해당 팀원을 팀에서 삭제 처리한다. |
| **5** |시스템이 팀원 목록을 갱신한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**3** |3a. 리더가 팝업에서 취소를 누른 경우 <br> ...3a1. 팀원이 삭제되지 않고 사용자는 내 팀 관리 페이지(팀원 조회 화면)에 머무른다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음|
| **Due Date** |2025.11.15|

---
### Use Case #24: 팀원 평가한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자는 사용자의 팀원에게 리뷰를 남겨 평가할 수 있다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인한 상태이다. <br> 사용자가 해당 팀원에게 아직 리뷰를 작성하지 않았다. |
| **Trigger** |사용자가 내 팀 관리에서 리뷰쓰기 버튼을 누른다. |
| **Success Post Condition** |팀원 리뷰가 생성되어 시스템에 저장된다. <br> 리뷰쓰기 버튼이 수정하기 버튼으로 변경된다. |
| **Failed Post Condition** |리뷰가 저장되지 않고 리뷰쓰기 버튼이 유지된다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 같이 활동한 팀원에게 리뷰를 달 수 있다. |
| **1** |사용자가 내 팀 관리에서 팀원 정보를 확인한다. |
| **2** |사용자가 리뷰를 작성하지 않은 특정 팀원 옆의 리뷰쓰기 버튼을 누른다. |
| **3** |시스템은 리뷰 텍스트를 입력할 수 있는 모달을 표시한다. |
| **4** |사용자가 피드백을 작성하고 저장하기 버튼을 누른다. |
| **5** |시스템은 입력된 리뷰 내용을 데이터베이스에 저장한다. |
| **6** |시스템은 “리뷰가 성공적으로 저장되었습니다.”라는 메시지를 표시하고 모달을 닫는다. |
| **7** |시스템은 해당 팀원의 리뷰쓰기 버튼을 수정하기 버튼으로 변경한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**4** |4a. 사용자가 닫기 버튼을 누른 경우 <br> ...4a1. 리뷰가 저장되지 않고 모달을 닫는다. |
|**5** |5a. 시스템 오류로 저장에 실패한 경우 <br> ...5a1. 시스템은 "서버 오류로 리뷰 저장에 실패했습니다."라는 메시지를 표시한다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |프로젝트 완료 시 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #25: 팀원 리뷰 수정한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 팀원에게 남긴 리뷰를 수정한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인한 상태이다. <br> 이미 작성한 리뷰가 존재한다. |
| **Trigger** |사용자가 내 팀 관리에서 수정하기 버튼을 누른다. |
| **Success Post Condition** |팀원의 리뷰가 수정된다. |
| **Failed Post Condition** |리뷰가 저장되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 팀원에게 남긴 리뷰에 대해 수정할 수 있다. |
| **1** |사용자가 수정하기 버튼을 누른다. |
| **2** |사용자가 리뷰 내용을 수정 후 저장하기를 누른다. |
| **3** |시스템이 데이터베이스의 기존 리뷰를 업데이트한다. |
| **4** |시스템이 “리뷰 수정이 완료되었습니다.”라는 메시지를 표시 후 모달을 닫는다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**2** |2a. 사용자가 (수정 모달 내의) 삭제하기 버튼을 누른 경우 <br> ...2a1. 팀원 리뷰 삭제가 실행된다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #26: 팀원 리뷰 삭제한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 팀원에게 남긴 리뷰를 삭제한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인한 상태이다. <br> 이미 작성한 리뷰가 존재한다. |
| **Trigger** |사용자가 리뷰 수정 모달 내의 삭제하기 버튼을 누른다. |
| **Success Post Condition** |팀원에게 남긴 리뷰가 삭제된다. |
| **Failed Post Condition** |팀원에게 남긴 리뷰가 삭제되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 팀원에게 남긴 리뷰를 삭제할 수 있다. |
| **1** |사용자가 리뷰 수정 모달의 삭제하기 버튼을 누른다. |
| **2** |시스템이 “정말로 리뷰를 삭제하시겠습니까?”라는 확인 팝업을 표시한다. |
| **3** |사용자가 확인 버튼을 누른다. |
| **4** |시스템이 데이터베이스에서 리뷰를 삭제한다. |
| **5** |시스템이 “리뷰가 삭제되었습니다.”라는 메시지를 표시하고 모달을 닫는다. |
| **6** |시스템이 수정하기 버튼을 다시 리뷰쓰기 버튼으로 변경한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**3** |3a. 사용자가 취소 버튼을 누른 경우 <br> ...3a1. 시스템이 확인 팝업을 닫고 리뷰수정 모달로 돌아간다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #27: 올린 공고 목록 조회한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 자신이 작성한 공고의 목록과 상태를 확인한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인한 상태이다. |
| **Trigger** |사용자가 마이페이지에서 내가 올린 공고 카테고리를 누른다. |
| **Success Post Condition** |사용자가 작성한 공고 목록을 표시한다. |
| **Failed Post Condition** |공고 목록을 불러오지 못한다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 올린 공고의 목록을 조회한다. |
| **1** |사용자가 마이페이지에서 내가 올린 공고 카테고리를 누른다. |
| **2** |시스템은 사용자가 작성한 모든 공고의 목록(공고 제목, 모집 상태, 카테고리)을 표시한다. |
| **3** |시스템은 각 공고 항목 옆에 [수정], [삭제] 버튼과 [지원자 보기] 링크를 제공한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**2** |2a. 사용자가 올린 공고가 없는 경우 <br> ...2a1. “해당 카테고리의 공고가 없어요.”라는 메시지를 목록 영역에 표시한다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #28: 공고 수정한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 자신이 이전에 작성한 공고를 수정한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인한 상태이다. <br> 사용자가 '내가 올린 공고' 목록 페이지에 접근한 상태이다. |
| **Trigger** |사용자가 '내가 올린 공고' 목록에서 특정 공고의 '수정' 버튼을 누른다. |
| **Success Post Condition** |공고의 내용이 변경되어 데이터베이스에 저장된다. <br> 사용자는 '내가 올린 공고' 목록 페이지로 이동한다. |
| **Failed Post Condition** |공고의 내용이 변경되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 작성한 공고를 수정한다. |
| **1** |1. 시스템이 해당 공고의 기존 내용이 채워진 공고 작성 페이지로 이동한다.|
| **2** |1. 사용자가 내용을 수정하고 수정 완료 버튼을 누른다. |
| **3** |1. 시스템은 해당 공고의 기존 데이터가 채워진 수정 페이지를 표시한다. |
| **4** |1. 사용자가 공고의 원하는 정보를 수정한다. |
| **5** |1. 사용자가 수정 완료 버튼을 누른다. |
| **6** |1. 시스템은 수정된 내용의 유효성을 검사한다. |
| **7** |1. 시스템은 변경된 공고 내용을 데이터베이스에 업데이트한다. |
| **8** |1. 시스템은 “공고가 성공적으로 수정되었습니다.”라는 메시지를 표시하고 내가 올린 공고 목록 페이지로 이동시킨다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**1** |1a. 사용자가 올린 공고가 없는 경우 <br> ... 1a1. “해당 카테고리의 공고가 없어요.”라는 메시지를 목록 영역에 표시한다.|
|**7** |7a. 공고 저장에 실패할 경우 <br> ...7a1. 시스템은 "공고 등록에 실패하였습니다. 다시 시도해 주세요."라는 팝업을 띄운다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |N/A (공고 작성자 본인만 실행) |
| **Due Date** |2025.11.15|

---
### Use Case #29: 공고 삭제한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 자신이 이전에 작성한 공고를 삭제한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인한 상태이다. <br> 사용자가 '내가 올린 공고' 목록 페이지에 접근한 상태이다. |
| **Trigger** |사용자가 '내가 올린 공고' 목록에서 특정 공고의 '삭제' 버튼을 누른다. |
| **Success Post Condition** |공고가 데이터베이스에서 삭제된다. |
| **Failed Post Condition** |공고가 삭제되지 않고 데이터베이스에 유지된다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 작성한 공고를 삭제한다. |
| **1** |사용자가 '내가 올린 공고' 목록에서 삭제할 공고의 '삭제' 버튼을 누른다. |
| **2** |시스템은 “정말 이 공고를 삭제하시겠습니까?”라는 확인 팝업을 표시한다. |
| **3** |사용자가 확인 팝업에서 '확인' 버튼을 누른다. |
| **4** |시스템은 해당 공고를 데이터베이스에서 삭제한다. |
| **5** |삭제된 공고가 '내가 올린 공고' 목록에 보이지 않도록 갱신한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**3** |3a. 사용자가 삭제를 취소하는 경우 <br> ...3a1. 사용자가 확인 팝업에서 '취소' 버튼을 누른다. <br> ...3a2. 시스템은 팝업을 닫고 공고는 삭제되지 않는다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |N/A (공고 작성자 본인만 실행) |
| **Due Date** |2025.11.15|

---
### Use Case #30: 지원자 조회한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 자신이 올린 공고에 지원한 지원자들의 목록과 상세 지원서를 확인한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인한 상태여야 한다. <br> 사용자가 해당 공고의 작성자여야 한다. |
| **Trigger** |사용자가 '내가 올린 공고' 목록에서 특정 공고를 누른다. |
| **Success Post Condition** |사용자가 해당 공고의 지원자 목록과 상세 지원서 내용을 확인한다. |
| **Failed Post Condition** |사용자가 해당 공고의 지원자 목록과 상세 지원서 내용 확인에 실패한다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 올린 공고에 지원한 지원자를 확인하고 지원서를 확인한다. |
| **1** |사용자가 '내가 올린 공고' 목록에서 지원자를 확인할 공고를 누른다. |
| **2** |시스템은 해당 공고의 지원자 목록을 드롭다운으로 표시한다. |
| **3** |사용자가 목록에서 특정 지원자의 '지원서 보기' 버튼을 누른다. |
| **4** |시스템은 모달(팝업)을 통해 해당 지원자의 상세 정보(가능한 진행방식, 학년, 기술 스택, 상세 소개 등)를 표시한다. |
| **5** |시스템은 이 모달 내에 [수락하기]와 [거절하기] 버튼을 함께 표시한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**4** |4a. 사용자가 '지원서 보기' 모달을 닫는 경우 <br> ...4a1. 시스템은 모달을 닫고 '내가 올린 공고' 페이지에 머무른다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #31: 지원자 수락한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |공고 작성자가 지원자의 지원을 승인하여 팀원으로 확정한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 '지원자 조회' 모달을 보고 있는 상태이다. <br> 해당 지원자의 상태가 '대기 중'이다. |
| **Trigger** |사용자가 '지원서 보기' 모달에서 [수락하기] 버튼을 누른다. |
| **Success Post Condition** |사용자가 해당 공고의 지원자 목록과 상세 지원서 내용을 확인한다. |
| **Failed Post Condition** |사용자가 해당 공고의 지원자 목록과 상세 지원서 내용 확인에 실패한다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 공고 지원자의 지원을 승인한다. |
| **1** |사용자가 [수락하기] 버튼을 누른다. |
| **2** |시스템은 “지원이 수락되었습니다.”라는 팝업을 표시한다. |
| **3** |시스템은 팝업과 모달을 닫고 '지원자 관리' 페이지의 목록을 갱신한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**1** |1. 1a. 사용자가 닫기 버튼을 클릭한 경우 <br> ...1a1. 지원자는 승인되지 않고 내가 올린 공고 페이지에 머무른다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #32: 지원자 거절한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |공고 작성자가 지원자의 지원을 거절한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 해당 공고의 작성자이다. <br> 사용자가 특정 지원자의 '지원서 보기' 모달을 보고 있는 상태이다. |
| **Trigger** |사용자가 '지원서 보기' 모달에서 [거절하기] 버튼을 누른다. |
| **Success Post Condition** |지원자의 지원이 거절되고 목록이 갱신된다. |
| **Failed Post Condition** |지원자의 지원이 거절되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 공고 지원자의 지원을 거절한다. |
| **1** |시스템이 “지원이 거절 되었습니다.”라는 확인 메시지를 표시하고 지원서 보기 모달을 닫는다. |
| **2** |시스템이 “정말로 거절하시겠습니까?”라는 확인 팝업을 표시한다. |
| **3** |시스템은 지원자 목록을 갱신한다.|

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**1** |1a. 사용자가 닫기 버튼을 클릭한 경우 <br> ...1a1. 지원자는 거절되지 않고 내가 올린 공고 페이지에 머무른다.|

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #33: 지원한 공고 조회한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 자신이 지원한 공고의 목록과 제출했던 지원서의 내용을 확인한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 로그인한 상태이다. |
| **Trigger** |사용자가 마이페이지에서 '내가 지원한 공고' 카테고리를 누른다. |
| **Success Post Condition** |사용자가 자신의 지원 목록과 지원서 내용을 확인한다. |
| **Failed Post Condition** |지원 목록을 불러오지 못하여 확인하지 못한다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 지원한 공고와 지원서의 내용을 확인한다. |
| **1** |사용자가 마이페이지에서 '내가 지원한 공고' 카테고리를 누른다. |
| **2** |시스템은 사용자가 지원한 모든 공고의 목록(카테고리, 공고 제목, 마감일, 지원 상태)을 표시한다. |
| **3** |사용자가 목록에서 '공고 제목'을 누른다. |
| **4** |시스템은 해당 '공고 상세 보기' 페이지로 이동시킨다.|
| **5** |사용자가 목록에서 특정 공고의 '내 지원서 보기' 버튼을 누른다. |
| **6** |시스템은 사용자가 제출한 지원서의 내용(가능한 진행방식, 학년, 기술 스택 등)을 보여주는 모달을 표시한다. |
| **7** |시스템은 공고 항목 옆에 [지원 취소] 버튼을 표시한다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**2** |2a. 사용자가 지원한 공고가 없는 경우 <br> ...2a1. 시스템은 목록 영역에 “지원한 공고가 없어요.”라는 메시지를 목록 영역에 표시한다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 수시로 |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|

---
### Use Case #34: 지원한 공고 취소한다.

#### 1. GENERAL CHARACTERISTICS (개요)

| 항목 | 내용 |
| :--- | :--- |
| **Summary** |사용자가 지원한 공고를 취소한다. |
| **Scope** |WaggleWaggle |
| **Level** |User level |
| **Last Update** |2025.11.06 |
| **Status** |Analysis |
| **Primary Actor** |User |
| **Secondary Actors**|System |
| **Preconditions** |사용자가 '내가 지원한 공고' 목록 페이지에 접근한 상태이다. |
| **Trigger** |사용자가 '내가 지원한 공고' 목록에서 공고의 '지원 취소' 버튼을 누른다. |
| **Success Post Condition** |사용자가 지원한 공고가 삭제되고 공고 목록 페이지가 갱신된다. |
| **Failed Post Condition** |사용자가 지원한 공고가 취소되지 않는다. |

#### 2. MAIN SUCCESS SCENARIO (주요 성공 시나리오)

| Step | Action |
| :--- | :--- |
| **S** |사용자가 지원한 공고를 취소한다. |
| **1** |사용자가 해당 공고의 '지원 취소' 버튼을 누른다. |
| **2** |시스템은 “정말 지원을 취소하시겠습니까?”라는 확인 메시지를 표시한다. |
| **3** |사용자가 확인 팝업에서 '확인' 버튼을 누른다. |
| **4** |시스템은 해당 지원 정보의 상태를 '모집 취소'로 변경하여 데이터베이스에 저장한다. |
| **5** |시스템은 '내가 지원한 공고' 목록을 갱신하고 해당 공고의 상태가 '모집 취소'로 변경된 것을 보여준다. |

#### 3. EXTENSION SCENARIOS (예외 및 대안 흐름)

| Step | Branching Action |
| :--- | :--- |
|**3** |3a. 사용자가 팝업에서 취소 버튼을 누른 경우 <br> ...3a1. 시스템은 지원한 공고를 취소하지 않고 확인 팝업을 닫는다. |

#### 4. RELATED INFORMATION (관련 정보)

| 항목 | 내용 |
| :--- | :--- |
| **Performance** |≤ 1 second |
| **Frequency** |필요시 (낮음) |
| **&lt;Concurrency&gt;** |제한 없음 |
| **Due Date** |2025.11.15|
