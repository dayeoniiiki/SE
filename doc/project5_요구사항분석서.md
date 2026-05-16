# DevTrack 요구사항 분석서

**문서번호:** DevTrack-REQ-001  
**버전:** v1.1  
**파일 경로:** doc/project5_요구사항분석서.md  

| 버전 | 날짜 | 작성자 | 변경사항 |
|------|------|--------|----------|
| v1.0 | 2025-05-09 | 팀원 | 최초 작성 |
| v1.1 | 2025-05-16 | 팀원 | 리뷰 반영: U_06/U_08/U_09 예외 흐름 추가, 클래스 다이어그램 Requirement-Member 관계 보완 |

---

## 목차

1. [서론](#1-서론)
2. [시스템 개요](#2-시스템-개요)
3. [요구사항 명세](#3-요구사항-명세)
4. [인터페이스 분석](#4-인터페이스-분석)
5. [제약사항](#5-제약사항)
6. [요구사항 추적표](#6-요구사항-추적표)
7. [참고문헌 및 부록](#7-참고문헌-및-부록)

---

## 1. 서론

### 1.1 목적 및 범위

이 문서는 소프트웨어 개발 조직에서 요구사항, 일정, 산출물을 통합적으로 관리하는 **DevTrack** 시스템의 요구사항 분석서이다.  
UML 기반 객체지향 분석 방법론을 적용하여 기능 모델링(유스케이스), 구조 모델링(클래스 다이어그램), 행위 모델링(순차 다이어그램)을 수행하고, 각 산출물 간의 일관성을 검증한다.

### 1.2 용어 정의

| 용어 | 설명 |
|------|------|
| DevTrack | 소프트웨어 개발 프로젝트 통합 관리 시스템 |
| 유스케이스 (Use Case) | 시스템이 액터에게 제공하는 기능 단위 |
| 스프린트 (Sprint) | Agile-Scrum에서 반복적인 개발 주기 단위 (1~4주) |
| 백로그 (Backlog) | 요구사항 및 작업 목록 (제품 백로그, 스프린트 백로그) |
| 이슈 (Issue) | 프로젝트 진행 중 발생하는 작업, 버그, 기능 요청 등의 단위 |
| 산출물 (Artifact) | 프로젝트 과정에서 생성되는 문서, 코드, 설계서 등 |

### 1.3 참조 문서

- `doc/project1_정의서.md` – 프로젝트 정의서 (v0.1)
- `doc/project2_품질요소.md` – 대상 시스템 품질 요소 추정
- `doc/project3_프로젝트관리계획서.md` – 프로젝트 관리 계획서
- `doc/project4_요구사항정의서.md` – 요구사항 정의서

---

## 2. 시스템 개요

### 2.1 Actor Table

| Actor | Role |
|-------|------|
| 프로젝트 관리자 (PM) | 프로젝트 전체를 총괄하며 일정, 요구사항, 팀원을 관리하는 주체 |
| 팀원 (Developer) | 이슈를 처리하고 산출물을 등록하며 진행 상황을 업데이트하는 주체 |
| 시스템 (System) | 알림, 통계 생성, 대시보드 갱신 등 자동화된 동작을 수행하는 주체 |

### 2.2 UseCase Diagram

```
<<DevTrack System Boundary>>
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  프로젝트 생성/수정/삭제하기 (U_01)                            │
│  팀원 초대하기 (U_02)                                         │
│  요구사항 등록/수정하기 (U_03)                                 │
│  스프린트 계획하기 (U_04) ──«include»──→ 이슈 할당하기 (U_05)  │
│  이슈 처리 상태 업데이트하기 (U_06)                            │
│  산출물 등록하기 (U_07)                                       │
│  대시보드/통계 조회하기 (U_08)                                 │
│  알림 발송하기 (U_09) ←──«extend»── 이슈 기한 초과 감지 (U_10) │
│                                                             │
└─────────────────────────────────────────────────────────────┘
       ↑                          ↑                   ↑
    [PM]                    [Developer]            [System]
```

### 2.3 UseCase Description

---

#### U_01 – 프로젝트 생성/수정/삭제하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 프로젝트 생성/수정/삭제하기 |
| **ID** | U_01 |
| **Importance Level** | High |
| **Primary Actor** | 프로젝트 관리자 (PM) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | PM이 DevTrack에서 새 프로젝트를 생성하거나 기존 프로젝트 정보를 수정·삭제한다. |
| **Trigger** | PM이 [새 프로젝트] 버튼 또는 [프로젝트 설정] 메뉴를 선택한다. |
| **Relationships** | Association: PM / Include: - / Extend: - |

**Normal Flow of Events**

1. PM은 프로젝트 이름, 설명, 시작일, 종료일을 입력하고 [생성] 버튼을 누른다.
2. 시스템은 입력값을 검증하고 프로젝트를 저장한 뒤 프로젝트 대시보드로 이동한다.
3. PM은 프로젝트 설정 화면에서 정보를 수정하거나 [삭제] 버튼을 누를 수 있다.
4. 삭제 시 시스템은 확인 다이얼로그를 표시하고 승인 후 프로젝트를 제거한다.

**Subflows**
- S-1 (수정): PM이 변경 사항 입력 → [저장] 클릭 → 시스템이 갱신 후 성공 메시지 표시

**Alternate / Exceptional Flows**
- 1.a1: 필수 항목이 공란인 경우 시스템은 오류 메시지를 표시하고 입력 화면을 유지한다.
- 1.a2: 종료일이 시작일보다 이전인 경우 유효성 오류를 표시한다.

---

#### U_02 – 팀원 초대하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 팀원 초대하기 |
| **ID** | U_02 |
| **Importance Level** | High |
| **Primary Actor** | 프로젝트 관리자 (PM) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | PM이 이메일 주소 또는 계정 ID를 이용하여 팀원을 프로젝트에 초대한다. |
| **Trigger** | PM이 [팀원 관리] 메뉴에서 [초대] 버튼을 클릭한다. |
| **Relationships** | Association: PM / Include: - / Extend: - |

**Normal Flow of Events**

1. PM은 팀원의 이메일 주소를 입력하고 역할(Developer/Viewer)을 선택한다.
2. [초대 전송] 버튼을 누르면 시스템이 해당 이메일로 초대 링크를 발송한다.
3. 초대받은 사용자가 링크를 클릭하면 프로젝트 멤버로 등록된다.

**Alternate / Exceptional Flows**
- 2.a1: 이미 멤버인 사용자를 초대할 경우 '이미 참여 중인 팀원입니다' 메시지를 출력한다.

---

#### U_03 – 요구사항 등록/수정하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 요구사항 등록/수정하기 |
| **ID** | U_03 |
| **Importance Level** | High |
| **Primary Actor** | 프로젝트 관리자 (PM) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | PM이 프로젝트의 기능/비기능 요구사항을 등록하고 우선순위, 상태를 관리한다. |
| **Trigger** | PM이 [요구사항] 탭에서 [새 요구사항 추가] 버튼을 클릭한다. |
| **Relationships** | Association: PM / Include: - / Extend: - |

**Normal Flow of Events**

1. PM은 요구사항 ID, 제목, 설명, 유형(기능/비기능), 우선순위를 입력한다.
2. [저장] 버튼을 누르면 시스템이 요구사항을 저장하고 목록에 표시한다.
3. PM은 목록에서 요구사항을 선택하여 내용을 수정하거나 상태를 변경할 수 있다.

**Alternate / Exceptional Flows**
- 1.a1: 제목이 공란인 경우 시스템은 저장을 차단하고 경고 메시지를 출력한다.

---

#### U_04 – 스프린트 계획하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 스프린트 계획하기 |
| **ID** | U_04 |
| **Importance Level** | High |
| **Primary Actor** | 프로젝트 관리자 (PM) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | PM이 스프린트를 생성하고 기간을 설정한다. 이슈 할당(U_05)을 포함하여 수행한다. |
| **Trigger** | PM이 [스프린트] 탭에서 [새 스프린트 생성] 버튼을 클릭한다. |
| **Relationships** | Association: PM / Include: 이슈 할당하기 (U_05) / Extend: - |

**Normal Flow of Events**

1. PM은 스프린트 이름, 목표, 시작일, 종료일을 입력하고 [생성]을 누른다.
2. 시스템은 스프린트를 생성하고 이슈 할당 화면(U_05)으로 이동한다.
3. PM은 스프린트를 [시작] 또는 [완료] 처리할 수 있다.

**Alternate / Exceptional Flows**
- 1.a1: 이미 진행 중인 스프린트가 있는 경우 경고 메시지를 표시한다.

---

#### U_05 – 이슈 할당하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 이슈 할당하기 |
| **ID** | U_05 |
| **Importance Level** | High |
| **Primary Actor** | 프로젝트 관리자 (PM) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | PM이 백로그에서 이슈를 선택하여 스프린트에 할당하고 담당 팀원을 지정한다. |
| **Trigger** | U_04 스프린트 계획하기에서 포함 호출됨. |
| **Relationships** | Association: PM / Include: - / Extend: - |

**Normal Flow of Events**

1. 시스템은 제품 백로그 이슈 목록을 표시한다.
2. PM은 이슈를 선택하고 스프린트 및 담당자를 지정 후 [할당]을 누른다.
3. 시스템은 할당된 이슈를 스프린트 백로그에 추가한다.

**Alternate / Exceptional Flows**
- 2.a1: 이미 다른 스프린트에 할당된 이슈를 중복 할당 시 경고 메시지를 출력한다.

---

#### U_06 – 이슈 처리 상태 업데이트하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 이슈 처리 상태 업데이트하기 |
| **ID** | U_06 |
| **Importance Level** | High |
| **Primary Actor** | 팀원 (Developer) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | 팀원이 자신에게 할당된 이슈의 상태를 To Do → In Progress → Done으로 변경한다. |
| **Trigger** | 팀원이 칸반 보드 또는 이슈 상세 페이지에서 상태를 변경한다. |
| **Relationships** | Association: Developer / Include: - / Extend: 알림 발송하기 (U_09) |

**Normal Flow of Events**

1. 팀원은 자신의 이슈를 선택하고 상태 드롭다운에서 새 상태를 선택한다.
2. [저장] 또는 드래그-앤-드롭으로 상태를 변경한다.
3. 시스템은 상태를 갱신하고 PM에게 알림(U_09)을 발송한다.

**Alternate / Exceptional Flows** *(v1.1 추가)*
- 2.a1: 자신에게 할당되지 않은 이슈의 상태를 변경하려 할 경우 시스템은 '권한이 없습니다' 메시지를 출력하고 변경을 차단한다.
- 2.a2: 이미 Done 상태인 이슈를 To Do로 되돌리려 할 경우 PM 승인 요청 알림을 발송한다.
- 3.a1: 알림 발송(U_09) 중 이메일 서버 오류가 발생한 경우 인앱 알림만 발송하고 이메일 재시도 큐에 등록한다.

---

#### U_07 – 산출물 등록하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 산출물 등록하기 |
| **ID** | U_07 |
| **Importance Level** | Mid |
| **Primary Actor** | 팀원 (Developer) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | 팀원이 이슈 또는 스프린트에 연결된 산출물(문서, 링크, 파일)을 등록한다. |
| **Trigger** | 팀원이 이슈 상세 페이지에서 [산출물 추가] 버튼을 클릭한다. |
| **Relationships** | Association: Developer / Include: - / Extend: - |

**Normal Flow of Events**

1. 팀원은 산출물 유형(파일/URL), 이름, 설명을 입력하고 파일을 첨부하거나 URL을 입력한다.
2. [등록] 버튼을 누르면 시스템이 산출물을 저장하고 이슈에 연결한다.

**Alternate / Exceptional Flows**
- 1.a1: 파일 크기가 50MB를 초과하는 경우 업로드를 차단하고 오류 메시지를 출력한다.

---

#### U_08 – 대시보드/통계 조회하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 대시보드/통계 조회하기 |
| **ID** | U_08 |
| **Importance Level** | High |
| **Primary Actor** | PM / Developer |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | 사용자가 프로젝트의 진행 현황, 번다운 차트, 이슈 통계를 대시보드에서 조회한다. |
| **Trigger** | 사용자가 [대시보드] 메뉴를 클릭한다. |
| **Relationships** | Association: PM, Developer / Include: - / Extend: - |

**Normal Flow of Events**

1. 시스템은 현재 스프린트의 완료율, 이슈 상태 분포, 번다운 차트를 렌더링한다.
2. 사용자는 기간 필터를 변경하여 과거 스프린트 통계를 조회할 수 있다.

**Alternate / Exceptional Flows** *(v1.1 추가)*
- 1.a1: 아직 스프린트가 생성되지 않은 프로젝트의 경우 '진행 중인 스프린트가 없습니다' 안내 메시지를 표시한다.
- 1.a2: 데이터 집계 중 서버 오류가 발생한 경우 마지막으로 캐시된 통계를 표시하고 '일부 데이터가 최신이 아닐 수 있습니다' 경고를 출력한다.
- 2.a1: 조회 기간에 해당하는 스프린트 데이터가 없는 경우 '해당 기간의 데이터가 없습니다' 메시지를 표시한다.

---

#### U_09 – 알림 발송하기

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 알림 발송하기 |
| **ID** | U_09 |
| **Importance Level** | Mid |
| **Primary Actor** | 시스템 (System) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | 이슈 상태 변경, 기한 초과 등 이벤트 발생 시 관련 사용자에게 알림을 발송한다. |
| **Trigger** | U_06 상태 변경, U_10 기한 초과 감지 이벤트 발생 시 |
| **Relationships** | Association: System / Include: - / Extend: - |

**Normal Flow of Events**

1. 시스템은 이벤트를 감지하면 해당 프로젝트의 관련 멤버를 조회한다.
2. 시스템은 인앱 알림과 이메일 알림을 생성하여 발송한다.

**Alternate / Exceptional Flows** *(v1.1 추가)*
- 1.a1: 알림 대상 멤버가 프로젝트에서 이미 탈퇴한 경우 해당 멤버는 수신 목록에서 제외하고 나머지 멤버에게만 발송한다.
- 2.a1: 이메일 서버 연결 실패 시 인앱 알림만 즉시 발송하고, 이메일은 최대 3회 재시도 후 실패 로그를 기록한다.
- 2.a2: 알림 대상 멤버가 알림 수신을 비활성화한 경우 해당 유형의 알림 발송을 건너뛴다.

---

#### U_10 – 이슈 기한 초과 감지

| 항목 | 내용 |
|------|------|
| **Use Case Name** | 이슈 기한 초과 감지 |
| **ID** | U_10 |
| **Importance Level** | Mid |
| **Primary Actor** | 시스템 (System) |
| **Use Case Type** | Detail, Essential |
| **Brief Description** | 시스템이 주기적으로 이슈 마감일을 확인하여 기한 초과 이슈를 감지하고 알림을 트리거한다. |
| **Trigger** | 시스템 스케줄러가 매일 자정에 실행됨. |
| **Relationships** | Association: System / Include: - / Extend: 알림 발송하기 (U_09) |

**Normal Flow of Events**

1. 시스템은 현재 날짜와 이슈 마감일을 비교한다.
2. 기한이 지났고 상태가 Done이 아닌 이슈를 '기한 초과' 상태로 변경한다.
3. 해당 이슈에 대한 알림 발송(U_09)을 트리거한다.

---

## 3. 요구사항 명세

### 3.1 정적 분석 (클래스 다이어그램)

> **v1.1 변경:** Requirement ↔ Member, Member ↔ Project 관계 명시적으로 보완

```
Class DevTrack

┌──────────────┐  1    0..*  ┌──────────────┐  1    0..*  ┌──────────────┐
│   Project    │────────────│    Sprint    │────────────│    Issue     │
│──────────────│            │──────────────│            │──────────────│
│ -id: String  │            │ -id: String  │            │ -id: String  │
│ -name        │            │ -name        │            │ -title       │
│ -startDate   │            │ -startDate   │            │ -status      │
│ -endDate     │            │ -endDate     │            │ -priority    │
│ -status      │            │ -status      │            │ -dueDate     │
│──────────────│            │──────────────│            │ -assignee    │
│ +create()    │            │ +create()    │            │──────────────│
│ +update()    │            │ +start()     │            │ +update()    │
│ +delete()    │            │ +complete()  │            │ +assign()    │
└──────┬───────┘            └──────────────┘            └──────┬───────┘
       │ 1                                                      │ 0..*
       │ 0..*  [has-parts]                                      │
┌──────┴───────┐                                       ┌────────┴─────┐
│ Requirement  │──────────────(연관)───────────────────│   Artifact   │
│──────────────│                                       │──────────────│
│ -id          │                                       │ -id          │
│ -title       │                                       │ -name        │
│ -type        │                                       │ -type        │
│ -priority    │                                       │ -path        │
│ -status      │                                       │──────────────│
│ -reviewer    │ ← [검토 담당 Member, 0..1]             │ +upload()    │
│──────────────│                                       │ +delete()    │
│ +add()       │                                       └──────────────┘
│ +update()    │
│ +linkIssue() │
└──────┬───────┘
       │ 0..1  (assignedReviewer)
       │
┌──────┴───────┐            ┌──────────────┐
│    Member    │────────────│ Notification │
│──────────────│ 1    0..*  │──────────────│
│ -userId      │            │ -id          │
│ -name        │            │ -type        │
│ -email       │            │ -message     │
│ -role        │            │ -isRead      │
│ -notifEnabled│            │──────────────│
│──────────────│            │ +send()      │
│ +invite()    │            │ +markRead()  │
│ +updateRole()│            └──────────────┘
└──────┬───────┘
       │ 다대다
       │ [한 Member는 여러 Project에 참여 가능]
    Project

※ 관계 보완 설명 (v1.1)
- Requirement ↔ Member : Requirement는 PM 역할의 Member가 등록·승인하며,
  assignedReviewer(0..1) 속성으로 검토 담당 멤버를 선택적으로 지정할 수 있음.
- Member ↔ Project : 한 명의 Member는 여러 Project에 참여 가능 (다대다 연관).
  Project는 반드시 1명 이상의 Member(PM)를 보유해야 함.
```

### 3.2 CRC 카드

---

**Class Name: Project** | ID: 01 | Type: Concrete, Domain

> Description: DevTrack에서 관리되는 소프트웨어 개발 프로젝트를 나타낸다.  
> Associated Use Cases: U_01, U_04, U_08

| Responsibilities | Collaborators |
|-----------------|---------------|
| createProject() : void | PM |
| updateProject() : void | PM |
| deleteProject() : void | PM |
| getStatistics() : Dashboard | PM, Developer |

**Attributes**
- id : String
- name : String
- description : String
- startDate : Date
- endDate : Date
- status : Enum {Active, Archived}

**Relationships**
- Aggregation (has-parts): Sprint, Requirement, Member
- Other Associations: Issue

---

**Class Name: Sprint** | ID: 02 | Type: Concrete, Domain

> Description: 프로젝트 내 반복 개발 주기를 나타낸다.  
> Associated Use Cases: U_04, U_08

| Responsibilities | Collaborators |
|-----------------|---------------|
| createSprint() : void | PM |
| startSprint() : void | PM |
| completeSprint() : void | PM |

**Attributes**
- id : String
- name : String
- goal : String
- startDate : Date
- endDate : Date
- status : Enum {Planned, Active, Completed}

**Relationships**
- Aggregation (has-parts): Issue
- Other Associations: Project

---

**Class Name: Issue** | ID: 03 | Type: Concrete, Domain

> Description: 스프린트 내에서 처리되는 작업 단위를 나타낸다.  
> Associated Use Cases: U_05, U_06, U_09, U_10

| Responsibilities | Collaborators |
|-----------------|---------------|
| updateStatus() : void | Developer |
| assignMember() : void | PM |
| checkOverdue() : bool | System |
| attachArtifact() : void | Developer |

**Attributes**
- id : String
- title : String
- description : String
- status : Enum {ToDo, InProgress, Done, Overdue}
- priority : Enum {High, Mid, Low}
- dueDate : Date
- assignee : Member

**Relationships**
- Aggregation (has-parts): Artifact
- Other Associations: Sprint, Requirement

---

**Class Name: Requirement** | ID: 04 | Type: Concrete, Domain

> Description: 프로젝트의 기능/비기능 요구사항을 나타낸다.  
> Associated Use Cases: U_03

| Responsibilities | Collaborators |
|-----------------|---------------|
| addRequirement() : void | PM |
| updateRequirement() : void | PM |
| linkToIssue() : void | PM |

**Attributes**
- id : String
- title : String
- description : String
- type : Enum {Functional, NonFunctional}
- priority : Enum {High, Mid, Low}
- status : Enum {Proposed, Approved, Implemented}
- assignedReviewer : Member (0..1) ← *v1.1 추가*

**Relationships**
- Other Associations: Project, Issue
- Other Associations: Member (검토 담당 PM과의 연관) ← *v1.1 추가*

---

**Class Name: Member** | ID: 05 | Type: Concrete, Domain

> Description: 프로젝트에 참여하는 사용자 및 역할을 나타낸다.  
> Associated Use Cases: U_02

| Responsibilities | Collaborators |
|-----------------|---------------|
| invite() : void | PM |
| updateRole() : void | PM |
| removeMember() : void | PM |

**Attributes**
- userId : String
- name : String
- email : String
- role : Enum {PM, Developer, Viewer}
- notificationEnabled : bool ← *v1.1 추가*

**Relationships**
- Other Associations: Project (다대다 — 한 멤버가 여러 프로젝트에 참여 가능) ← *v1.1 보완*
- Other Associations: Issue, Requirement

---

**Class Name: Artifact** | ID: 06 | Type: Concrete, Domain

> Description: 이슈에 첨부된 산출물(파일, URL 등)을 나타낸다.  
> Associated Use Cases: U_07

| Responsibilities | Collaborators |
|-----------------|---------------|
| uploadArtifact() : void | Developer |
| deleteArtifact() : void | Developer |

**Attributes**
- id : String
- name : String
- type : Enum {File, URL}
- path : String
- uploadedAt : DateTime

**Relationships**
- Other Associations: Issue

---

**Class Name: Notification** | ID: 07 | Type: Concrete, Domain

> Description: 시스템이 발송하는 알림 메시지를 나타낸다.  
> Associated Use Cases: U_09, U_10

| Responsibilities | Collaborators |
|-----------------|---------------|
| sendNotification() : void | System |
| markAsRead() : void | Member |

**Attributes**
- id : String
- type : Enum {IssueUpdate, Overdue, SprintEvent}
- message : String
- isRead : bool
- createdAt : DateTime

**Relationships**
- Other Associations: Member, Issue

---

### 3.3 동적 분석 (순차 다이어그램)

#### 3.3.1 프로젝트 생성하기 (U_01)

```
sd 프로젝트_생성 (U_01)

  :PM          :ProjectUI       :ProjectService     :ProjectDB
    |               |                  |                  |
    |──프로젝트정보입력──>|                  |                  |
    |               |──createProject()──>|                  |
    |               |                  |──save()──────────>|
    |               |                  |<────success───────|
    |               |<─────success──────|                  |
    |<──대시보드 이동──|                  |                  |
```

#### 3.3.2 스프린트 계획하기 + 이슈 할당 (U_04, U_05)

```
sd 스프린트_계획 (U_04 + U_05)

  :PM      :SprintUI    :SprintService   :IssueService    :SprintDB
    |          |              |                |               |
    |──스프린트정보입력──>|              |                |               |
    |          |──createSprint()──>|             |               |
    |          |              |──save()──────────────────────>|
    |          |              |<───success────────────────────|
    |  ┌──────────────────────────────────────────────┐
    |  │  <<include>> U_05 이슈 할당하기               │
    |──백로그이슈선택──>|              |                |
    |          |──assignIssue()─────────>|               |
    |          |              |──updateIssue()──>|        |
    |          |              |<────success───────|        |
    |  └──────────────────────────────────────────────┘
    |<──완료────|              |                |               |
```

#### 3.3.3 이슈 상태 업데이트 + 알림 발송 (U_06, U_09)

```
sd 이슈_상태_업데이트 (U_06 + U_09)

  :Developer  :IssueUI   :IssueService  :NotifService  :IssueDB
    |           |              |               |            |
    |──상태변경──>|              |               |            |
    |           |──updateStatus()──>|           |            |
    |           |              |──save()──────────────────>|
    |           |              |<───success───────────────|
    |           |              |──sendNotif()──>|           |
    |           |              |               |──알림발송──>|
    |           |<────success──|               |            |
    |<──완료─────|              |               |            |
```

#### 3.3.4 기한 초과 감지 + 알림 (U_10, U_09)

```
sd 기한초과_감지 (U_10 + U_09)

  :Scheduler  :IssueService  :NotifService  :IssueDB
    |               |               |            |
    |──(매일 자정)────>|               |            |
    |               |──queryOverdue()───────────>|
    |               |<───overdue list────────────|
    |  ┌───────────────────────────────────────────┐
    |  │  <<extend>> U_09 알림 발송                  │
    |               |──sendNotif()──>|              |
    |               |               |──알림저장────>|
    |  └───────────────────────────────────────────┘
    |<──완료──────────|               |            |
```

---

## 4. 인터페이스 분석

| 인터페이스 | 유형 | 설명 |
|-----------|------|------|
| 웹 브라우저 UI | 사용자 인터페이스 | React 기반 SPA. PM과 개발자가 브라우저로 접근. |
| REST API | 소프트웨어 인터페이스 | 프론트엔드와 백엔드 간 JSON 기반 HTTP API. |
| 이메일 서버 | 외부 인터페이스 | SMTP를 통한 초대 및 알림 이메일 발송. |
| GitHub / GitLab | 외부 인터페이스 | 커밋 연동 및 산출물 링크 연결 (옵션). |

---

## 5. 제약사항

- 시스템은 최신 버전의 Chrome, Firefox, Edge 브라우저를 지원해야 한다.
- 파일 산출물의 단건 업로드 용량은 50MB를 초과할 수 없다.
- 데이터는 관계형 데이터베이스(PostgreSQL)에 저장되어야 한다.
- REST API 응답 시간은 일반 조회 기준 2초 이내여야 한다.
- 모든 사용자 비밀번호는 bcrypt 알고리즘으로 해시 저장되어야 한다.

---

## 6. 요구사항 추적표

| 요구사항 ID | U_01 | U_02 | U_03 | U_04 | U_05 | U_06 | U_07 | U_08 | U_09 | U_10 |
|------------|------|------|------|------|------|------|------|------|------|------|
| FR_001 (프로젝트 생성) | O | | | | | | | | | |
| FR_002 (프로젝트 수정/삭제) | O | | | | | | | | | |
| FR_003 (팀원 초대) | | O | | | | | | | | |
| FR_004 (요구사항 등록) | | | O | | | | | | | |
| FR_005 (스프린트 생성) | | | | O | | | | | | |
| FR_006 (이슈 할당) | | | | O | O | | | | | |
| FR_007 (이슈 상태 변경) | | | | | | O | | | | |
| FR_008 (산출물 등록) | | | | | | | O | | | |
| FR_009 (대시보드 조회) | | | | | | | | O | | |
| FR_010 (알림 발송) | | | | | | O | | | O | |
| FR_011 (기한 초과 감지) | | | | | | | | | O | O |

---

## 7. 참고문헌 및 부록

- ch08_객체지향_분석 강의 자료 (소프트웨어공학)
- 샘플_요구사항분석서 (open CCTV 프로젝트)
- `doc/project4_요구사항정의서.md` – DevTrack 요구사항 정의서
- OMG UML Specification v2.5, Object Management Group
