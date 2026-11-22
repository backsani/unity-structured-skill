# Project_Structure.md template

Use this template when creating `Project_Structure.md`. Write the document's content in Korean; keep code identifiers, paths and signatures as they are in code.

Rules for the document:
- Location: project root, next to `Assets/`. Never inside `Assets/`.
- Purpose: let Claude find the files a task needs (and the files related to them) without exploring, keep new code consistent with the existing structure, and remember why the structure is the way it is.
- Sections 1-5 are the always-read core. Keep them compact.
- Section 6 holds signatures only. Never paste method bodies.
- Mark not-yet-implemented items with `(planned)`.
- Size limit: when the document passes about 800 lines, move sections 6-9 of each system into `Docs/Structure/<SystemName>.md` (project root, not inside `Assets/`). In the main document, keep a one-line summary per system and a link to its file.

---

````markdown
# Project Structure

> Last updated: YYYY-MM-DD
> 작업 전 1~5번은 항상 읽고, 6~9번은 작업 대상 시스템과 관련된 부분만 읽는다.

## 1. 프로젝트 개요
- 장르 / 핵심 게임 루프:
- 대상 플랫폼:
- Unity 버전:
- 렌더 파이프라인:
- 주요 패키지: (Input System, UniTask, VContainer 등 + 버전)

## 2. 폴더 구조
```
Assets/
└── _Project/
    ├── Scripts/
    │   ├── Core/        # 역할 한 줄
    │   └── ...
    ├── ScriptableObjects/
    ├── Prefabs/
    └── Scenes/
```

## 3. 어셈블리·네임스페이스
| Assembly (asmdef) | 경로 | 네임스페이스 | 참조하는 어셈블리 |
|---|---|---|---|

## 4. 핵심 흐름
### 초기화 순서
1. Bootstrap 씬 로드 → ...
### 게임 루프
- ...
### 씬 전환
- ...

## 5. 시스템 맵
| 시스템 | 책임 | 주요 파일 | 의존하는 시스템 | 이 시스템을 쓰는 곳 |
|---|---|---|---|---|
| Input | 입력을 게임 명령으로 변환 | `Scripts/Input/InputReader.cs` | - | Player, UI |

## 6. 인터페이스·클래스 구조
### <SystemName>
```csharp
namespace Game.Core
{
    public interface ISaveService
    {
        void Save(SaveData data);
        SaveData Load();
    }

    public sealed class JsonSaveService : ISaveService { /* signatures only */ }
}
```

## 7. 데이터 구조
- ScriptableObject: `EnemyData` (경로, 주요 필드, 용도)
- 저장 데이터: `SaveData` (필드, 직렬화 방식, 저장 위치)
- 주요 enum/struct:

## 8. 코딩 규칙
- 이름 규칙: (클래스 PascalCase, private 필드 _camelCase 등)
- 주석 언어:
- 비동기 처리 방식:
- 의존성 주입/참조 방식:
- 금지 사항: (예: FindObjectOfType 사용 금지)

## 9. 설계 결정 기록
### D-001: <결정 제목> (YYYY-MM-DD)
- 결정:
- 이유:
- 검토한 대안: (대안과 고르지 않은 이유)
- 결정 과정: (어떤 답변과 제약에서 이 결정으로 이어졌는지)
- 다시 검토할 조건: (예: 동시 접속 기능이 생기면)

## 10. 변경 이력
- YYYY-MM-DD: 한 줄 요약
````
