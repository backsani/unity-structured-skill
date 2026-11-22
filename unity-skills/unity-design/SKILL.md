---
name: unity-design
description: Initial architecture design for a new Unity project. Interviews the user relentlessly in rounds until the project's base structure is fully decided, writes Project_Structure.md, then builds only the skeleton (folders, asmdefs, interfaces, core class stubs). Invoke with /unity-design.
disable-model-invocation: true
---

# unity-design

Design the base structure of a Unity project with the user, record it in `Project_Structure.md`, and build the skeleton. Feature implementation is out of scope; it belongs to `/unity-dev`.

Respond in Korean. No preamble ("좋아요", "시작하겠습니다"), no recaps, no closers ("도움이 되었길"). Start with the content, end when it is done.

## Phases

1. Explore
2. Interview
3. Confirm
4. Document
5. Build the skeleton
6. Hand off

Do not write any project file before the user confirms in phase 3.

## 1. Explore

Read the project yourself before asking anything. Facts are your job, never the user's.

- `ProjectSettings/ProjectVersion.txt`: Unity version
- `Packages/manifest.json`: installed packages (render pipeline, Input System, UniTask, DI containers, test framework)
- `Assets/`: existing folders, scripts, scenes
- `Project_Structure.md` at the project root: if it exists, ask whether to redesign from scratch or extend it
- If the project already has substantial code, say so and suggest `/unity-dev` (it can generate the document from existing code) before continuing
- Check whether Unity MCP tools are available in this session. Do not depend on any specific MCP's tool names; use whatever Unity MCP tools exist.

Answers found here are settled facts: do not ask the user about them.

## 2. Interview

Interview the user until every branch of the design is decided.

### Method

- Model the design as a **decision tree**: every decision branches into the decisions that depend on it.
- The **frontier** is every decision whose prerequisites are already settled.
- Ask in **rounds**. One round = the whole frontier, then wait for the answers.
- A question that depends on another question still open in this round belongs to a later round.
- After each round, recompute the frontier from the answers and ask the next round.
- Ask only for **decisions**. If a question needs a fact from the project or environment, look it up yourself.
- Never answer a decision yourself. Put it to the user and wait.

### Question format

```
❓ **Q1** - **<question title>**: <question body, choices if any>

➡️ <your recommended answer, with a one-line reason>

---

❓ **Q2** - ...
```

- Number questions continuously across rounds (Q1, Q2, ... Q14, Q15).
- Word each question so "예" accepts your recommendation.
- The user may answer by number ("1 예, 2 두 번째, 3 아니요").
- If the user says "모르겠다": give 2-3 options with a one-line trade-off each, recommend one, and ask again.
- If a question can't be answered by talking (how something should look or feel), record it as an open question with a provisional default and move on.
- No cap on the number of questions. If the scope is clearly too large for one session, suggest splitting it.
- If the user says "마무리" or "이제 됐어": stop asking, record every unvisited branch as an open question with your recommended default, and go to phase 3.

### Topics (root branches of the tree)

Cover all of these. Skip any that phase 1 already settled.

1. Genre, core game loop, target platforms, scale (prototype / small release / long-term live)
2. Unity version, render pipeline (Built-in / URP / HDRP)
3. Architecture pattern (MVC/MVP, ScriptableObject-driven, manager singletons, component-based, ECS/DOTS)
4. Dependency management (direct references / singleton / service locator / DI such as VContainer or Zenject)
5. Scene composition and transitions (bootstrap scene, additive loading, Addressables)
6. Data: static game data (ScriptableObject / JSON / CSV), save data (format, location, encryption)
7. UI (UGUI / UI Toolkit), UI architecture
8. Input (Input System / legacy Input Manager), rebinding
9. Async (coroutines / UniTask / async-await)
10. Folder structure, asmdef split, namespaces
11. Coding conventions (naming, comment language, forbidden APIs such as `FindObjectOfType`)
12. Testing (none / EditMode / PlayMode)

Each answer opens deeper branches (e.g. "VContainer" → "LifetimeScope per scene or one root?"). Follow them until nothing is silently assumed.

### Done

The interview is done when the frontier is empty: every branch visited, nothing left as an unstated assumption.

## 3. Confirm

Present the full design summary grouped by topic, plus open questions with their defaults. Ask the user to reply "확인" or to correct items. Apply corrections (ask follow-up rounds if a correction opens new branches) and present again. Proceed only after "확인".

## 4. Document

Read `PROJECT_STRUCTURE_TEMPLATE.md` in this skill's folder now, and create `Project_Structure.md` at the project root (next to `Assets/`, never inside it) following it.

- Fill every section from the confirmed design. Mark files and systems that don't exist yet as `(planned)`.
- Section 5 (system map) must list, for each system, its files, the systems it depends on, and the systems that use it. This is what lets later work read only the related files.
- Section 9 (decision record): write one `D-xxx` entry per significant decision from the interview: the decision, the reason, the alternatives considered and why they lost, and how the conversation got there (which answers and constraints led to it). Record open questions here too, marked as provisional.
- Section 10: add the first entry ("초기 설계").

## 5. Build the skeleton

Scope: structure only, no gameplay features.

- Folders as designed
- asmdef files with the designed references
- Interfaces with full signatures
- Core class stubs: correct namespace, class declaration, fields, method signatures with empty bodies and `// TODO(<System>): ...` comments
- ScriptableObject class definitions
- Base scenes and bootstrap objects, created through Unity MCP if the design includes them

Rules:
- Never create or edit `.meta` files. Unity generates them.
- Never edit `.unity` or `.prefab` files as text. Do editor work (scenes, GameObjects, components, Inspector references) through Unity MCP tools.
- After writing scripts, let Unity refresh/compile through MCP and read the console. Fix every compile error before moving on.
- If Unity MCP is unavailable, finish the code part and list the editor steps for the user in phase 6. State that compilation was not verified.
- When done, update `Project_Structure.md`: remove `(planned)` from what now exists.

## 6. Hand off

Answer in this shape and nothing else:

```
## 생성 결과
<folder tree of what was created, short>

## Unity 에디터 작업          ← only if something could not be done through MCP
1. ...

## 다음 단계
`/unity-dev`로 아래 순서로 구현을 추천합니다.
1. <first feature> (예상 시간)
2. ...
3. ...
```
