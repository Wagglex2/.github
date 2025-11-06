## 2. Use case analysis

### Use Case #1:
내용 입력

---

### Use Case #2:
내용 입력

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
| **Due Date** |3025.11.15|

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
| **Due Date** |3025.11.15|

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
| **Due Date** |3025.11.15|

---

### Use Case #6:
내용 입력

---

### Use Case #7:
내용 입력

---
### Use Case #8:
내용 입력

---
### Use Case #9:
내용 입력

---
### Use Case #10:
내용 입력

---
### Use Case #11:
내용 입력

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
| **Due Date** |3025.11.15|

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
| **Due Date** |3025.11.15|

---
### Use Case #14: 모든 알림을 삭제한다.

---
### Use Case #15: 카테고리별 알림을 일괄 삭제한다.
내용 입력

---
### Use Case #16: 공고를 작성한다.
내용 입력

---
### Use Case #17: 공고 상세 내용을 조회한다.
내용 입력

---
### Use Case #18: 공고에 지원한다.
내용 입력

---
### Use Case #19: 회원정보를 조회한다.
내용 입력

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
| **Due Date** |3025.11.15|

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
| **Due Date** |3025.11.15|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|

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
| **Due Date** |2025.11.20|
