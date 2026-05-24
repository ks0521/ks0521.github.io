# 문규성 포트폴리오
<img src="assets/img/증명사진.jpg" align="left" width="170" hspace="3" />

Unity와 C#을 기반으로 전투, 스테이지, 데이터 저장/로드, 상호작용 시스템을 직접 설계하고 구현해 온 게임 클라이언트 개발자 지원자입니다. 

해당 포트폴리오에서는 3가지 프로젝트를 개발하며, 고민하고 구현했던 핵심적인 로직에 대해서 설명하고 있습니다.

팀 프로젝트에서는 Addressables 기반 데이터 로딩 이후 매니저 초기화를 수행하는 부트스트랩 흐름, 스테이지 실행/규칙/보상 책임 분리, 저장용 데이터와 런타임 데이터 변환 자동화를 구현했습니다.
개인 프로젝트에서는 플레이어와 AI가 공통 전투 실행 계층을 재사용하도록 구조화하고, 공격 가능 판정과 무기별 실행 로직을 분리해 전투 시스템의 확장성을 높였습니다.

단순한 기능완성보다 초기화 순서, 데이터 구조, 책임 분리, 확장 단위처럼 유지보수성과 협업 효율에 영향을 주는 문제를 구조적으로 해결하는 데 집중한다는 개발 철학을
이 포트폴리오에 담아 소개해드리고자 합니다.

<br clear="left"/>

## 기술 스택

| 구분 | 기술 |
| --- | --- |
| Language | ![C#](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=csharp&logoColor=white) |
| Engine | ![Unity](https://img.shields.io/badge/Unity_2022.3.62f3_LTS-000000?style=for-the-badge&logo=unity&logoColor=white) |
| Library / SDK | ![UniTask](https://img.shields.io/badge/UniTask-512BD4?style=for-the-badge&logo=unity&logoColor=white) ![Addressables](https://img.shields.io/badge/Addressables-000000?style=for-the-badge&logo=unity&logoColor=white) |
| Data | ![JSON](https://img.shields.io/badge/JSON-000000?style=for-the-badge&logo=json&logoColor=white) ![ScriptableObject](https://img.shields.io/badge/ScriptableObject-000000?style=for-the-badge&logo=unity&logoColor=white) |
| Collaboration | ![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white) ![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white) ![Notion](https://img.shields.io/badge/Notion-000000?style=for-the-badge&logo=notion&logoColor=white) |

## 핵심 기술

- `IManager` 기반 초기화 파이프라인 및 부트스트랩 흐름 구현
- `Addressables` + `UniTask` 기반 비동기 게임 데이터 로딩
- `Attribute` / `Reflection` 기반 저장 데이터 자동 매핑 구조 구현
- `StageManager` / `Stage` / `StageRule` 기반 스테이지 책임 분리
- 플레이어 / AI 입력과 공통 전투 실행 계층 분리
- `AttackInvoker` / `WeaponParts` 기반 공격 파이프라인 책임 분리
- `IInteractable` / `IInteractAction` / `IInteractionCondition` 기반 상호작용 시스템 구현
- `Raycast` 탐지와 이벤트 기반 UI 반응 구조 분리

## 프로젝트 목차

| 구분 | 프로젝트 | 장르 | 기간 | 역할 | GitHub |
| --- | --- | --- | --- | --- | --- |
| [2차 팀 프로젝트](#team-project-2) | 귀차니즘 용사 | 방치형 RPG | 2025.03.03 - 2025.04.15 | 클라이언트 리드 | [TeamProject](https://github.com/ks0521/TeamProject) |
| [개인 프로젝트](#personal-project) | 그때 갑자기 건담이 나타났다 | TPS 로그라이크 | 2026.01.29 - 2026.02.27 | 1인 개발 | [SingleProject](https://github.com/ks0521/SingleProject) |
| [1차 팀 프로젝트](#team-project-1) | 카미카쿠시 | 공포 | 2025.12.10 - 2025.12.26 | 클라이언트 리드 | [First-Game-Project](https://github.com/ks0521/First-Game-Project) |

---
<div style="page-break-before: always;"></div>

<a id="team-project-2"></a>
# 2차 팀 프로젝트: 귀차니즘 용사

<p align="center">
  <img src="/assets/img/mainStage.png" width="49%" alt="일반 스테이지 자동 전투" />
  <img src="/assets/img/bossStage.png" width="49%" alt="보스 스테이지 전투" />
</p>

<p align="center">
  <b>일반 스테이지 파밍과 보스 스테이지 돌파로 이어지는 방치형 RPG</b><br/>
  <sub>자동 전투, 보상 획득, 성장, 스테이지 해금 흐름을 중심으로 구현했습니다.</sub>
</p>

- 개발 기간: 2025.03.03 - 2025.04.15, 총 7주
- 팀 구성: 개발 4인
- 본인 역할: 클라이언트 리드
- 개발 환경: Unity 2022.3.62f3 LTS, C#, UniTask, Addressables
- GitHub: [https://github.com/ks0521/TeamProject](https://github.com/ks0521/TeamProject)
- 시연 영상: [YouTube](https://youtu.be/7LdXl2Ow0QU)

## 핵심 구현 1. IManager 기반 초기화 파이프라인

> **문제**  
> 팀 작업물을 통합하는 과정에서 Unity 라이프사이클과 매니저 간 의존성이 얽혀 초기화 순서가 불안정해지는 문제가 발생했습니다.  
> 또한 스테이지, 장비, 아이템, 재화 같은 필수 데이터가 준비되기 전에 시스템이 먼저 초기화될 위험이 있었습니다.

### 해결

공통 진입점인 `StartBootstrap`에서 Addressables 기반 데이터를 먼저 비동기 로딩하고, 로딩 완료 후 `GameManager`가 `IManager` 구현체를 수집해 `GetOrder()` 기준으로 정렬한 뒤 `Init()`을 순차 호출하는 초기화 파이프라인을 구성했습니다.

### 효과

데이터 로딩과 시스템 초기화 순서를 명확히 분리해 초기화 관련 오류를 줄였고, 신규 매니저를 추가할 때 `IManager` 구현과 초기화 우선순위 지정만으로 기존 초기화 흐름에 편입할 수 있게 되었습니다.

### 구현 과정

1. `IGameSystem` / `IManager` 인터페이스를 정의하고 각 매니저가 `GetOrder()`와 `Init()`을 구현하도록 구성했습니다.
2. `GameManager`의 `Awake`에서 씬 내 `IGameSystem` 구현체를 수집하고 타입 기준으로 캐싱했습니다.
3. `StartBootstrap`에서 `DataLoadManager.InitAllData()`를 `await`한 뒤 `GameManager.InitAllManagers()`를 호출했습니다.
4. `InitAllManagers()`에서 수집된 시스템을 `GetOrder()` 기준으로 정렬하고 `IManager` 구현체의 `Init()`을 순차 실행했습니다.

### 관련 코드

- [GameManager.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Base/Managers/GameManager.cs)
- [StartBootstrap.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Base/Data/Script/StartBootstrap.cs)
- [DataLoadManager.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Base/Managers/DataLoadManager.cs)

<div style="page-break-before: always;"></div>

## 핵심 구현 2. Addressables + UniTask 기반 비동기 데이터 로딩

### 문제

스테이지, 장비, 아이템, 재화 데이터가 여러 ScriptableObject로 분산되어 있어, 게임 시작 시 필요한 데이터를 안정적으로 로딩하고 이후 시스템 초기화와 연결하는 흐름이 필요했습니다.

### 해결

`DataLoadManager`에서 Addressables 라벨을 기준으로 주요 게임 데이터를 로딩하고, `UniTask.WhenAll`을 이용해 Stage, Equipment, Item, Currency 데이터를 병렬로 초기화했습니다.

### 효과

초기 데이터 로딩 단계를 명확히 분리했고, 여러 데이터 그룹을 병렬로 로딩해 부트스트랩 흐름을 단순화했습니다. 이후 매니저들은 데이터 로딩이 완료된 상태를 전제로 초기화될 수 있게 되었습니다.

### 구현 과정

1. `Addressables.LoadAssetsAsync<T>()`로 데이터 타입별 ScriptableObject를 로딩했습니다.
2. `UniTask.WhenAll()`로 여러 로딩 작업을 병렬 실행했습니다.
3. 로딩이 성공하면 `GameDataDictionaries`에 결과를 저장했습니다.
4. 로딩 실패 시 예외를 발생시켜 부트스트랩 단계에서 초기화 실패를 감지하도록 구성했습니다.

### 관련 코드

- [DataLoadManager.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Base/Managers/DataLoadManager.cs)
- [StartBootstrap.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Base/Data/Script/StartBootstrap.cs)

<div style="page-break-before: always;"></div>

## 핵심 구현 3. StageManager / Stage / StageRule 기반 스테이지 구조 분리

### 문제

스테이지 전환, 몬스터 스폰, 클리어 조건 판정, 보상 지급 책임이 한 흐름에 몰리면 신규 스테이지 타입이나 도전 콘텐츠를 추가할 때 수정 범위가 커지는 문제가 있었습니다.

### 해결

`StageManager`는 스테이지 전환과 진행 상태를 관리하고, `Stage`는 몬스터 스폰과 생명주기를 담당하며, `StageRule`은 클리어 조건과 보상 규칙을 담당하도록 역할을 분리했습니다.

### 효과

스테이지 실행 로직과 클리어 규칙을 분리해 일반 스테이지, 처치 수 조건, 보스 처치, 생존형 스테이지 같은 규칙을 기존 흐름에 조합하는 방식으로 확장할 수 있게 되었습니다.

### 구현 과정

1. `StageManager`에서 기존 스테이지 정리, 신규 `Stage` 생성, `StageRule` 선택, 이벤트 연결을 담당하도록 구성했습니다.
2. `Stage`는 `SpawnType`에 따라 몬스터를 생성하고, 몬스터 사망 이벤트를 `StageRule`에 전달하도록 분리했습니다.
3. `StageRule`을 추상 클래스로 정의하고 `NormalStageRule`, `ChallengeStageRule`, `KillCount`, `BossKill`, `Survival` 등 규칙 클래스로 확장했습니다.
4. `ChallengeStageRule`에서는 제한 시간, 성공/실패 이벤트, 보상 지급 흐름을 별도로 관리했습니다.

### 관련 코드

- [StageManager.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Base/Managers/StageManager.cs)
- [Stage.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Battle/Script/Stage.cs)
- [StageRule.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Battle/Script/StageRule.cs)

<div style="page-break-before: always;"></div>

## 핵심 구현 4. Attribute / Reflection 기반 저장 데이터 자동 매핑

### 문제

Unity `JsonUtility`는 `Dictionary` 직렬화를 지원하지 않아 저장용 데이터와 런타임 데이터 타입을 분리해야 했습니다.

콘텐츠가 늘어나면서 `SaveData`와 `RuntimeData` 사이의 필드를 수동 변환할 경우 누락 위험과 유지보수 비용이 커졌습니다.

### 해결

저장 데이터는 `List` 기반 구조로, 런타임 데이터는 `Dictionary` 기반 구조로 분리했습니다.

공통으로 복사 가능한 데이터 블록은 `[CommonType]` Attribute로 표시하고, Reflection을 이용해 같은 이름과 타입을 가진 필드를 자동 매핑하도록 구현했습니다.

### 효과

저장과 런타임 각각의 목적에 맞는 자료구조를 사용할 수 있었고, 공통 필드 추가 시 반복적인 수동 복사 코드를 줄였습니다.

Reflection 메타데이터를 1회 캐싱해 저장/로드 시 반복 탐색 비용도 줄였습니다.

### 구현 과정

1. `CommonType` Attribute를 정의하고 자동 복사 가능한 공통 데이터 블록에 적용했습니다.
2. `DataConverter`에서 `Base.Save` 네임스페이스의 `CommonType` 타입을 수집했습니다.
3. `RuntimeProgressData`와 `SaveProgressData`의 루트 필드를 비교해 이름과 타입이 같은 필드만 `BlockMap`으로 캐싱했습니다.
4. 공통 블록은 Reflection으로 자동 복사하고, `Dictionary` / `List` 변환이 필요한 인벤토리, 장비, 스탯, 스킬 데이터는 명시적으로 변환했습니다.

### 관련 코드

- [DataConverter.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Base/Save/DataConverter.cs)
- [CommonTypeData.cs](https://github.com/ks0521/TeamProject/blob/main/Assets/Game/Base/Save/CommonTypeData.cs)

## 회고

### 협업/운영 측면
이번 프로젝트에서는 초기 프레임워크 설계가 이후 팀 생산성에 직접적인 영향을 준다는 점을 확인했습니다.  
초기 비용이 들더라도 확장 가능한 구조와 컨벤션을 먼저 정하고 공유하면, 기능 추가 과정의 충돌을 줄일 수 있었습니다.

### 기술적 측면
이번 프로젝트의 구조 설계는 이전 프로젝트 경험을 성격에 맞게 분리해 적용한 것이 핵심이었습니다.
먼저 초기화 흐름은 1차 팀 프로젝트에서 겪었던 **매니저 간 의존성 문제**를 직접적으로 개선하는 방향으로 접근했습니다.  
`StartBootstrap → DataLoadManager → GameManager(IManager Init)` 순서로 진입점을 고정해, 시작 시점의 의존성 문제를 줄이고 초기화 안정성을 확보했습니다.
이벤트 기반 구조는 1차 팀 프로젝트에서 확인한 이벤트 분리의 효과를 확장한 영역이었습니다.  
`SaveManager`, `StatusCalculator`처럼 이벤트를 받아 동작하는 컴포넌트 중심으로 구성해 시스템 간 결합도를 낮추고, 기능 추가 시 기존 로직 수정 범위를 최소화했습니다.
Stage 구조는 개인 프로젝트에서 검증한 책임 분리 원칙(판정/실행 분리)을 시스템 단위로 확장한 사례였습니다.  
`StageManager / Stage / StageRule`로 진행 관리·실행·규칙 책임을 분리해, 스테이지 규칙 확장과 유지보수가 쉬운 구조를 만들었습니다.
결과적으로 이번 프로젝트는 초기화 안정성, 이벤트 기반 확장성, 콘텐츠 규칙 분리를 함께 확보하는 구조 설계 경험이었습니다.

---
<div style="page-break-before: always;"></div>

<a id="personal-project"></a>
# 개인 프로젝트: 그때 갑자기 건담이 나타났다

<p align="center">
  <b>불리한 전투 상황을 돌파하며 무작위 보상으로 기체를 강화하는 TPS 로그라이크</b><br/>
  <sub>1인 개발로 플레이어/AI 공통 전투 실행 계층, 공격 파이프라인, 상태 전이 기반 NPC AI를 구현했습니다.</sub>
</p>

- 개발 기간: 2026.01.29 - 2026.02.27, 총 4주
- 팀 구성: 1인 개발
- 개발 환경: Unity 2022.3.62f3 LTS, C#, UniTask
- GitHub: [https://github.com/ks0521/SingleProject](https://github.com/ks0521/SingleProject)
- 기획서: [Notion](https://www.notion.so/2f110afad1cd80fd868de0a4df86b3fe)

## 핵심 구현 1. 플레이어 / AI 입력과 공통 전투 실행 계층 분리

### 문제

플레이어와 AI가 각각 이동, 회전, 공격 실행 로직을 따로 가지면 동일한 전투 기능이 중복되고, 기능 추가나 버그 수정 시 수정 범위가 커지는 문제가 있었습니다.

### 해결

플레이어 입력은 `PlayerController`에서, AI 판단은 `NPCController`에서 담당하고, 실제 이동/회전/공격 실행은 `MechBehavior`에서 공통 처리하도록 분리했습니다.

### 효과

입력 주체가 달라도 동일한 전투 실행 로직을 재사용할 수 있게 되었고, 이동/공격/피격 경직 같은 핵심 동작의 수정 지점을 `MechBehavior` 중심으로 모을 수 있었습니다.

### 구현 과정

1. `PlayerController`는 키 입력을 받아 이동 축과 조작 상태를 계산한 뒤 `MechBehavior.Move()`를 호출하도록 구성했습니다.
2. `NPCController`는 상태 전이와 타겟 판단을 통해 이동 방향과 공격 시점을 결정하도록 구성했습니다.
3. `MechBehavior`는 `Rigidbody` 기반 이동, 타겟 회전, 공격 요청, 피격 경직 처리를 공통 실행 계층으로 담당했습니다.
4. 공격 요청은 `MechBehavior`에서 `AttackInvoker`로 전달해 이후 공격 파이프라인과 연결했습니다.

### 관련 코드

- [PlayerController.cs](https://github.com/ks0521/SingleProject/blob/main/Assets/Gundam/Game/Script/Contents/Player/PlayerController.cs)
- [NPCController.cs](https://github.com/ks0521/SingleProject/blob/main/Assets/Gundam/Game/Script/Contents/NPC/NPCController.cs)
- [MechBehavior.cs](https://github.com/ks0521/SingleProject/blob/main/Assets/Gundam/Game/Script/Contents/Mech/MechBehavior.cs)

<div style="page-break-before: always;"></div>

## 핵심 구현 2. AttackInvoker / WeaponParts 기반 공격 파이프라인 책임 분리

### 문제

공격 입력, 발사 가능 여부 검증, 무기별 실행, 탄약/재장전/딜레이 관리가 한 클래스에 몰리면 공격 타입 추가와 디버깅이 어려워지는 문제가 있었습니다.

### 해결

공격 요청은 `MechBehavior`에서 시작하고, `AttackInvoker`가 재장전/공격 딜레이 등 발사 가능 조건을 검증한 뒤, `WeaponParts`가 무기 타입에 맞는 실제 공격 실행과 상태 관리를 담당하도록 분리했습니다.

### 효과

공격 가능 판정과 실제 무기 실행 로직이 분리되어 디버깅 지점이 명확해졌고, Raycast, Projectile, Melee 같은 공격 타입을 동일한 진입점에서 관리할 수 있게 되었습니다.

### 구현 과정

1. `MechBehavior.Attack()`에서 현재 조준 정보, 무기 파츠, 추가 스탯을 `AttackInvoker`로 전달했습니다.
2. `AttackInvoker`는 `IsReloading`, `IsDelay` 상태를 확인해 공격 가능 여부를 판정했습니다.
3. `WeaponParts`는 무기 데이터와 보너스 스탯을 기반으로 `FinalStat`을 계산했습니다.
4. 공격 타입에 따라 SphereCast 기반 Raycast 공격, Projectile Handler, `MeleeComboAttack`으로 실행을 분기했습니다.
5. 공격 후 탄약 감소, 재장전, 공격 딜레이를 UniTask 기반 비동기 루프로 처리했습니다.

### 관련 코드

- [AttackInvoker.cs](https://github.com/ks0521/SingleProject/blob/main/Assets/Gundam/Game/Script/Contents/Mech/AttackInvoker.cs)
- [WeaponParts.cs](https://github.com/ks0521/SingleProject/blob/main/Assets/Gundam/Game/Script/Contents/Weapon/WeaponParts.cs)

<div style="page-break-before: always;"></div>

## 핵심 구현 3. 상태 전이 기반 NPC AI 및 SO 파라미터 튜닝 구조

### 문제

NPC가 단순 추적/공격만 수행하면 전투 상황 변화에 대응하기 어렵고, AI 행동 값을 코드 수정 없이 조정하기 어렵습니다.

### 해결

`NPCController`에 `Seek`, `Approach`, `Attack`, `Retreat`, `Reposition`, `Stunned` 상태를 두고 거리, 시야, 안전거리, 공격거리 기준으로 상태를 전이하도록 구성했습니다.

또한 AI 파라미터를 ScriptableObject 기반 데이터로 분리해 판단 주기, 공격 거리, 선호 거리, 회피 거리 같은 값을 외부 데이터로 조정할 수 있게 했습니다.

### 효과

NPC 행동을 상태 단위로 분리해 디버깅과 튜닝이 쉬워졌고, 기체나 적 타입별 AI 성향을 파라미터 조정으로 확장할 수 있는 기반을 만들었습니다.

### 구현 과정

1. `NPCController`에서 현재 타겟, 시야 여부, 거리 조건을 계산했습니다.
2. 일정 판단 주기마다 `ChangeTransition()`을 호출해 상태를 전환했습니다.
3. `Act()`에서 현재 상태에 따라 접근, 후퇴, 측면 이동, 공격 실행을 분기했습니다.
4. 선호 거리, 공격 거리, 안전 거리, 판단 주기 같은 값을 AI 파라미터로 분리해 튜닝 가능하도록 구성했습니다.

### 관련 코드

- [NPCController.cs](https://github.com/ks0521/SingleProject/blob/main/Assets/Gundam/Game/Script/Contents/NPC/NPCController.cs)

## 회고

### 협업/운영 측면
1인 개발로 전투-보상-최종 전투 루프 MVP는 완성했지만, UI 구현에 시간이 편중되어 핵심 루프 고도화가 부족했습니다.  
이 경험으로 1인 개발에서도 기능 우선순위와 일정 통제가 필수이며, 핵심 루프 기준으로 범위를 관리해야 함을 배웠습니다.

### 기술적 측면
1차 팀 프로젝트의 책임 분리 기준을 발전시켜 입력 주체(`PlayerController`, `NPCController`)와 실행 계층(`MechBehavior`)을 분리했습니다.  
또한 `AttackInvoker`(판정)와 `WeaponParts`(실행)로 공격 로직을 단계화해 확장 지점과 디버깅 경로를 명확히 했습니다.  
이 설계 경험은 이후 2차 팀 프로젝트의 초기화/데이터/규칙 분리 설계로 확장되었습니다.

---
<div style="page-break-before: always;"></div>

<a id="team-project-1"></a>
# 1차 팀 프로젝트: 카미카쿠시

<p align="center">
  <b>마을을 탐색하며 단서를 수집하고 선택에 따라 사건의 결말이 달라지는 공포 게임</b><br/>
  <sub>클라이언트 리드로 인터페이스 기반 상호작용 시스템과 Raycast 탐지, 이벤트 기반 UI 반응 구조를 구현했습니다.</sub>
</p>

- 개발 기간: 2025.12.10 - 2025.12.26, 총 3주
- 팀 구성: 기획 1인, 아트 1인, 개발 3인
- 본인 역할: 클라이언트 리드
- 개발 환경: Unity 2022.3.62f3 LTS, C#, Coroutine
- GitHub: [https://github.com/ks0521/First-Game-Project](https://github.com/ks0521/First-Game-Project)
- 시연 영상: [YouTube](https://www.youtube.com/watch?v=v1zOEZXVuqY)

## 핵심 구현 1. 인터페이스 기반 상호작용 시스템

### 문제

오브젝트마다 상호작용 조건과 실행 로직을 개별 스크립트로 작성하면 아이템, 문, 은신 오브젝트, 단서 등 콘텐츠가 늘어날수록 중복 코드가 증가하고 추가 속도가 느려지는 문제가 있었습니다.

### 해결

`IInteractable`, `IInteractionCondition`, `IInteractAction`으로 상호작용 대상을 추상화했습니다.

오브젝트는 자신에게 부착된 조건 컴포넌트와 액션 컴포넌트를 수집하고, 플레이어는 `IInteractable` 인터페이스만 바라보도록 구성했습니다.

### 효과

상호작용 오브젝트를 추가할 때 기존 플레이어 코드를 수정하지 않고 조건과 액션 컴포넌트를 조합하는 방식으로 확장할 수 있게 되었습니다.

상호작용 가능 여부, 실행 결과, 실제 상태 변화 책임이 분리되어 유지보수 부담도 줄었습니다.

### 구현 과정

1. `IInteractable`에서 `Interact()`와 `GetContext()`를 정의해 모든 상호작용 오브젝트의 공통 진입점을 만들었습니다.
2. `InteractItems`에서 `GetComponents<IInteractionCondition>()`과 `GetComponents<IInteractAction>()`으로 조건과 액션을 수집했습니다.
3. `CanInteract()`에서 모든 조건을 검사하고, 성공 시 `InteractResult`에 실행할 액션 목록을 담아 반환했습니다.
4. `PlayerInteract`는 반환된 `IInteractAction` 목록을 순회하며 `Execute()`를 호출했습니다.

### 관련 코드

- [IInteractable.cs](https://github.com/ks0521/First-Game-Project/blob/main/Assets/_Kamikakushi/Scripts/Utills/Interfaces/IInteractable.cs)
- [IInteractionCondition.cs](https://github.com/ks0521/First-Game-Project/blob/main/Assets/_Kamikakushi/Scripts/Utills/Interfaces/IInteractionCondition.cs)
- [IInteractAction.cs](https://github.com/ks0521/First-Game-Project/blob/main/Assets/_Kamikakushi/Scripts/Utills/Interfaces/IInteractAction.cs)
- [InteractItems.cs](https://github.com/ks0521/First-Game-Project/blob/main/Assets/_Kamikakushi/Scripts/Contents/Object/Interective%20Object/InteractItems.cs)
- [PlayerInteract.cs](https://github.com/ks0521/First-Game-Project/blob/main/Assets/_Kamikakushi/Scripts/Contents/Player/PlayerInteract.cs)

<div style="page-break-before: always;"></div>

## 핵심 구현 2. Raycast 탐지 + PlayerEvents 기반 UI/상호작용 분리

### 문제

한 컴포넌트가 아이템 탐지, 상호작용 실행, UI 표시까지 모두 담당하면 기능이 늘어날수록 의존성이 커지고 UI나 상호작용 로직을 독립적으로 수정하기 어려웠습니다.

### 해결

`CameraCrosshair`는 Raycast 탐지만 담당하고, 탐지된 `IInteractable`과 `InteractContext`를 `PlayerEvents`로 발행하도록 구성했습니다.

`PlayerInteract`는 이벤트로 받은 대상에 대해 상호작용을 시도하고, UI는 `PlayerEvents`를 구독해 크로스헤어와 안내 텍스트를 갱신하도록 분리했습니다.

### 효과

탐지, 실행, UI 표시 흐름이 이벤트를 기준으로 느슨하게 연결되어 각 파트를 독립적으로 수정할 수 있게 되었습니다.

상호작용 대상이 바뀌어도 UI와 플레이어 실행 로직의 결합을 줄일 수 있었습니다.

### 구현 과정

1. `CameraCrosshair`에서 카메라 전방으로 Raycast를 수행하고 충돌 지점에 크로스헤어 위치를 보정했습니다.
2. `IInteractable`을 감지하면 `PlayerEvents.OnFindInteractable()`과 `OnRaycastEnter()`를 호출했습니다.
3. 대상에서 벗어나면 `OnRaycastOut()`을 발행해 상호작용 가능 상태를 해제했습니다.
4. `PlayerInteract`는 `GetInteractable`, `GetInteractContext`, `RaycastOut` 이벤트를 구독해 현재 대상과 상호작용 가능 여부를 관리했습니다.
5. 상호작용 결과는 `OnInteract()` 이벤트로 발행해 UI가 결과 메시지를 표시하도록 구성했습니다.

### 관련 코드

- [CameraCrosshair.cs](https://github.com/ks0521/First-Game-Project/blob/main/Assets/_Kamikakushi/Scripts/Contents/Player/CameraCrosshair.cs)
- [PlayerInteract.cs](https://github.com/ks0521/First-Game-Project/blob/main/Assets/_Kamikakushi/Scripts/Contents/Player/PlayerInteract.cs)
- [PlayerEvents.cs](https://github.com/ks0521/First-Game-Project/blob/main/Assets/_Kamikakushi/Scripts/Contents/Player/PlayerEvents.cs)

## 회고

### 협업/운영 측면
팀장으로서 팀원 상황에 맞춰 일정을 유동적으로 조정했습니다.  
일부 인원 이탈로 기존 대형 맵/다수 오브젝트 구조를 튜토리얼-단서 탐색-선택 분기-멀티 엔딩 중심으로 재구성했습니다.  
이 경험을 통해 제한된 시간과 인원 변동 속에서는 범위 조정과 우선순위 합의가 완성도를 좌우한다는 점을 배웠습니다.

### 기술적 측면
상호작용을 `IInteractable / IInteractionCondition / IInteractAction`으로 분리하고,  
`PlayerEvents` 기반으로 탐지·실행·UI를 느슨하게 연결해 결합도를 낮췄습니다.  
이 과정에서 **빠른 기능 추가**보다 **책임 경계 선정**이 확장성과 유지보수에 유리하다는 기준을 확보했고, 이는 이후 개인 프로젝트 설계로 이어졌습니다.
추가적으로, 프로젝트 후반 통합 과정에서 매니저 초기화 시점이 명확히 고정되지 않아 매니저의 의존 기능의 실행 타이밍 이슈를 겪었습니다. 이후 초기화 순서를 구조적으로 보장하는 설계의 필요성을 인식했습니다.

