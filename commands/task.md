---
description: "요청을 plan(시안 생성)/do(바로 구현)로 분류해 처리"
argument-hint: "<요청> [--mode plan|do] [--build]"
---

## 경로 규약

`{PROJECT_ROOT}` = `${CLAUDE_PROJECT_DIR}` → 없으면 cwd 에서 위로 올라가며 `.claude/` 가 있는 디렉토리.

## Step 0: 분류

1. 루트 `CLAUDE.md` 를 읽어 프로젝트 컨텍스트 파악.
2. `$ARGUMENTS` 에 `--mode X` 가 있으면 X 로 강제. 없으면 "UI 시안(html)으로 미리 보고 확인할 가치가 있는가?" 만 판단:

   | 모드 | 시그널 |
   |:----|:------|
   | `plan` | 새 화면/페이지, 큰 UI 변경, 리디자인, 톤·레이아웃·다크모드, 결정 옵션 다수 |
   | `do` | 텍스트 변경, 버그, 백엔드·로직, 작은 스타일, 명확히 작은 수정 |

3. 신뢰도 낮으면 AskUserQuestion 1회 (`plan / do / 취소`). 명확하면 묻지 않음.
4. 채팅창에 `[plan|do] 모드로 시작합니다: $ARGUMENTS` 출력.

## Step 1: plan 모드

### 1-1. idea vs design 자동 판정 (묻지 않음)

| 판정 | 시그널 | 템플릿 | 질문 세트 |
|:----|:------|:------|:--------|
| `idea` | 대상 화면/기능이 코드에 없음 | `.claude/templates/idea-template.html` | 1) 핵심 문제, 2) 핵심 기능 1~2가지, 3) 참고 앱/서비스 |
| `design` | 대상이 이미 존재 → 변경 | `.claude/templates/design-template.html` | 1) 현재 불만, 2) 원하는 방향, 3) 참고 디자인, 4) 변경 범위 |

### 1-2. ID 채번

`{PROJECT_ROOT}/.output/plans/` 폴더가 없으면 먼저 mkdir + `index.html` 초기 생성 (인덱스가 없는 경우만 — `<ul class="entries"><!-- AUTO-ENTRIES-START --><!-- AUTO-ENTRIES-END --></ul>` 골격 포함).
`{PROJECT_ROOT}/.output/plans/*.html` 글로빙 (`index.html` 제외) → 개수 + 1 → `[3자리]-[한국어slug].html`. slug 은 요청 핵심 키워드를 한국어 하이픈 연결.

### 1-3. Q&A 통합 라운드 (사용자 대기 1회)

먼저 텍스트로 질문 세트를 안내한 뒤 AskUserQuestion 1회:

```
[plan / idea|design 판정]
[질문]
1. ...
2. ...
한 번에 답해주세요.
```

옵션: `답변 입력` / `알아서 판단` (Other 자유 텍스트). 결정적으로 부족하면 1회만 추가 질문.

### 1-4. html 생성

**design-system** 스킬 활성화.

1. 위 표의 템플릿 Read → `{{PLACEHOLDER}}` 치환. 실제 한국어 텍스트 + 실제 색상/폰트 값. design 이면 Before/After 변경 부분 highlight.
2. `{PROJECT_ROOT}/.output/plans/[ID]-[slug].html` 으로 Write.

### 1-5. 인덱스 갱신

`{PROJECT_ROOT}/.output/plans/index.html` Read → `<!-- AUTO-ENTRIES-START -->` 직후에 아래 엔트리를 삽입 (최근이 위). 기존 `<li class="empty">…</li>` 있으면 제거.

```html
      <li class="entry"><a href="[ID]-[slug].html" target="_blank"><span class="badge badge-[idea|design]">[idea|design]</span><span class="title">[요청 한 줄 요약]</span><time>[YYYY-MM-DD]</time></a></li>
```

### 1-6. 브라우저 자동 오픈

Bash 로:

```powershell
Start-Process "{PROJECT_ROOT}/.output/plans/[ID]-[slug].html"
```

### 1-7. 승인 분기

산출물 경로 안내 후 AskUserQuestion: `승인 / 수정 필요 / 폐기`.

- **승인** → Step 2.
- **수정 필요** → 수정 사항 텍스트 받음 → Step 2 진입. html 재생성이 필요하면 1-4 부터 재실행 (인덱스는 갱신하지 않음).
- **폐기** → html 파일 삭제 + 인덱스에서 해당 `<li>` 한 줄 제거 → 종료.

## Step 2: 구현

### 범위 파악

1. CLAUDE.md 기반으로 관련 영역(프론트/백/공통) 파악. plan 진입 시 1-3 답변과 1-4 html 을 출발점으로 사용.
2. UI/스타일/레이아웃 작업이면 **design-system** 스킬 활성화. 백엔드·로직만이면 생략.
3. 대상 파일·영향 범위·재사용 자산 확정.
4. do 모드에서 정보 부족 또는 방향이 2개 이상이면 AskUserQuestion 1회 보강 (최대 3개 묶음).

### 구현

- CLAUDE.md 컨벤션 준수, 최소 변경, 기존 자산 재사용.
- 프론트·백 양쪽이면 양쪽 모두 수정.
- 중간에 멈추지 말고 완료까지. 문제 발생 시에만 멈추고 보고.

### 에러 체크

`$ARGUMENTS` 에 `--build` 있으면 build 추가. 없으면 typecheck + lint + test 만.

1. `typecheck` — CLAUDE.md Dev Commands 에 있으면 실행 (없으면 `tsc --noEmit`).
2. `lint` — 있으면 실행 (자동 수정 가능하면 `lint:fix`).
3. `test` — 기존 테스트 있으면 실행.
4. 에러 수정 후 재검증. 3회 이상 반복되면 중단하고 미해결로 보고.

### 완료 보고

채팅창에 짧게:

```
✅ 완료 — [요청 요약]
- 변경: 생성 N / 수정 M / 삭제 K
- 검증: typecheck [✓/✗], lint [✓/✗], test [✓/✗/–]
- 미해결: [있으면 한 줄, 없으면 "없음"]
```
