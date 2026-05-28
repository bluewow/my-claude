# 디자인 규칙: Fit-day

> **토큰 소스(값의 단일 진실):** `web/src/app/globals.css` — `:root` 변수 + `@theme inline` 매핑. (Tailwind v4, `tailwind.config` 없음 / shadcn `components.json` 기반)
> **기준 화면(디자인 레퍼런스):** `web/src/features/landing/components/care-panel.tsx` — "담당 PM 케어" 패널. 이 화면의 톤·합성을 다른 화면의 기준으로 삼는다.
> **마지막 업데이트:** 2026-05-28
> **이 문서의 규칙:** 값(hex/oklch/px)을 적지 않는다. **토큰명·유틸 클래스명만** 참조한다. 실제 값이 필요하면 항상 `globals.css` 를 본다. 코드와 충돌하면 **코드가 승리**한다.

---

## 0. 한 줄 방향성

**화이트 캔버스 + Forest Green(`--primary`) 단색 브랜드.** 성장기 스타트업을 위한 B2B 채용 대행 — "AI 아님, 사람이 정성껏 검증한다"는 신뢰·차분함이 톤의 핵심. 화려함 대신 여백·정렬·단일 브랜드 컬러로 신뢰를 만든다. 시멘틱 컬러(red/emerald/amber/orange)는 **수치의 의미를 말할 때만** 쓰고 장식에는 절대 쓰지 않는다.

---

## 1. 색상 사용 규칙

### 브랜드 / 액션
- **`--primary`** (Forest Green, 로고 톤) — 브랜드 강조, CTA, 섹션 eyebrow 배지(`bg-primary/10` + `border-primary/20` + `text-primary`), 내비, 타임라인 기본 점·라벨. 한 화면에서 가장 강한 색이므로 남발 금지.
- **`--accent`** (mid-sage) — hover/링크 등 인터랙션 강조. primary 보다 한 단계 밝은 보조 브랜드 톤.
- **`.bg-gradient-brand` / `.text-gradient-brand`** (forest green → sage) — hero·카드 헤드라인의 **강조 라인 1줄**, 그리고 핵심 CTA 버튼·아바타에만. 본문 카드 배경에는 금지.
- **`--primary-foreground`** — 그라데이션/primary 면 위의 텍스트·아이콘.

### 시멘틱 (수치·상태의 의미 전용 — `globals.css` 주석 규약과 동일)
- **`fd-emerald-400` / `fd-green-400`** — 절감·이득·Fit-day 성과("+30일 안착", "연 +N일", 평균 응답시간 등 긍정 수치).
- **`fd-red-400`** — 손실·문제·매몰 비용("잃고 있는 시간", 적자 막대 등 부정 수치).
- **`fd-amber-400`** — 주의 / "높음" 직전 단계(게이지 중간 구간).
- **`fd-orange-500`** — 긴급 / "매우 높음"(게이지 상위 구간).
- **`fd-blue-400` / `fd-purple-400`** — info·차트 보조 계열(`--chart-3/4`). 주요 메시지에는 쓰지 않음.
- **`--destructive`** — 폼 검증 에러·삭제 액션(시멘틱 red 와 역할 구분: destructive=조작, fd-red=수치).

### 텍스트 위계
- **`text-foreground`** — 헤드라인·본문 핵심.
- **`text-foreground/80~85`** — 살짝 누른 라벨·질문 텍스트.
- **`text-muted-foreground`** — 보조 설명·캡션·단위·힌트(작은 글씨 가독성용).

### 틴트 카드 공식 (시멘틱 표면의 표준 합성)
의미를 가진 카드/칩은 항상 **`border-{color}/25~30` + `bg-{color}/5~10` + `text-{color}`** 3종 세트로. (예: emerald 메트릭 타일 = `border-fd-emerald-400/25 bg-fd-emerald-400/[0.08] text-fd-emerald-400`, red 손실 카드 = `border-fd-red-400/25 bg-fd-red-400/5 text-fd-red-400`.) 채워진 솔리드 배경은 쓰지 않는다 — 라이트 캔버스 위 옅은 틴트가 기본.

### Don't
- ❌ 시멘틱 컬러를 장식으로 사용(예: Q 번호·아이콘 색에 fd-red). 의미 없는 자리엔 `--primary` 또는 뉴트럴.
- ❌ `--primary` 외 추가 브랜드 색 도입. 브랜드는 의도적으로 단색.
- ❌ 인라인 hex/oklch. 반드시 토큰·유틸 클래스 경유(그라데이션 헬퍼는 예외적으로 globals.css 안에서만 hex 보유).

---

## 2. 타이포 사용 규칙

- **패밀리:** `font-sans`(Pretendard Variable, 45–920) 단일. 별도 디스플레이 폰트 없음.
- **Eyebrow(섹션 라벨):** `text-xs font-bold uppercase tracking-widest text-primary` — 모든 섹션 도입부 공통. 영문 라벨(예: `Cost Simulator`, `Your Dedicated Care`).
- **Hero h1:** `text-3xl md:text-5xl font-black leading-tight tracking-tight`, 강조 라인은 `.text-gradient-brand`.
- **섹션 h2:** `text-2xl md:text-3xl font-bold tracking-tight text-foreground`.
- **카드 h3:** `text-lg md:text-xl font-bold leading-snug`, 강조 라인 `.text-gradient-brand`.
- **본문:** `text-sm md:text-base text-muted-foreground`. hero 리드는 `font-light leading-relaxed` 로 한 단계 가볍게.
- **메트릭 수치:** `font-black` 또는 `font-extrabold` + **`tabular-nums`** + 큰 스텝(`text-5xl md:text-6xl` 핵심 / `text-base md:text-lg` 보조). 숫자에는 항상 `tabular-nums`.
- **마이크로 라벨:** `text-[10px]~[11px] font-bold uppercase tracking-wider` (타임라인 라벨·결과 라벨·배지).

### Don't
- ❌ 본문에 `font-black`. 무게는 헤드라인·수치에만.
- ❌ 한글 텍스트 uppercase(영문 eyebrow/라벨 한정).

---

## 3. 간격 / 레이아웃 규칙

- **컨테이너:** 랜딩 전체 `container mx-auto max-w-6xl`. 섹션별로 좁힘 — 계산기 `max-w-5xl`, FAQ `max-w-3xl`, 본문 리드 `max-w-2xl`.
- **섹션 간:** `mb-20`(섹션 블록 사이) / hero 는 `my-10 md:my-16`.
- **도입부 리듬:** eyebrow → 헤드라인 `mb-3` → 본문 `mb-3~8`. 이 3단 리듬을 섹션마다 반복.
- **카드 내부 패딩:** 쇼케이스/큰 카드 `p-6 md:p-8`, 결과·중간 카드 `p-5~6`, 중첩 타일 `p-2.5~4`.
- **스택 간격:** 타임라인 `space-y-2.5`, FAQ `space-y-4`, 폼/슬라이더 그룹 `space-y-6`.
- **그리드:** 2단 비교/입력은 `md:grid-cols-2`, 메트릭 3-up 은 `grid-cols-3 gap-2`. 모바일은 단일 컬럼.

---

## 4. 표면 / 깊이 규칙

라이트 캔버스(`--background` 화이트) 위에서 **표면은 보더로 구분**하고 그림자는 최소화한다.

- **`.glass-panel`** — 반투명 흰색 + `backdrop-blur` + 옅은 보더. 가장 돋보여야 할 **쇼케이스 카드**(예: care-panel)에만.
- **`bg-surface-deep`** — 카드 안의 1차 중첩 영역(인물 카드, 결과 카드 배경).
- **`bg-surface-raised`** — 2차 중첩(결과 카드 안의 분해 박스, FAQ 본문 표면).
- **`bg-surface-overlay`** — 인터랙티브 트랙(슬라이더 트랙, 진행 막대 배경).
- **`border-border`** — 모든 표면 구분의 기본 1px 보더. 시멘틱 카드는 §1 틴트 공식의 컬러 보더로 대체.
- **그림자:** 평면 우선. `shadow-lg` 는 **핵심 CTA 버튼**처럼 떠 있어야 하는 요소에만.

---

## 5. 모양 규칙

`--radius` 기준 스케일(`globals.css`). 용도별 고정:
- **`rounded-2xl`** — 패널·쇼케이스 카드·섹션 카드(가장 큰 표면).
- **`rounded-xl`** — 카드 안 중첩 박스, 인물 카드, **CTA 버튼**.
- **`rounded-lg`** — 작은 타일·메트릭 칸·타임라인 하이라이트 행·슬라이더 트랙.
- **`rounded-md`** — 인라인 미니 배지("+30일 안착 보장" 류).
- **`rounded-full`** — eyebrow 배지·상태 칩·타임라인 점·아바타.

### Don't
- ❌ 같은 위계의 요소에 들쭉날쭉한 radius 혼용. 위 매핑을 일관 적용.

---

## 6. 컴포넌트 합성 패턴

> 모두 기준 화면(care-panel) 및 기존 랜딩 컴포넌트에서 검증된 합성. 새 화면은 이 패턴을 재사용한다.

- **섹션 eyebrow 배지** — `inline-block rounded-full border border-primary/20 bg-primary/10 px-3 py-1.5 text-xs font-bold uppercase tracking-widest text-primary`. 모든 섹션 도입부 첫 요소.
- **인라인 보장 배지** — `rounded-md border border-fd-emerald-400/30 bg-fd-emerald-400/10 text-[10px] font-bold text-fd-emerald-400`. 헤드라인 옆 약속·보장 태그.
- **쇼케이스 카드** — `.glass-panel rounded-2xl p-6 md:p-8`, 내부에 eyebrow→그라데이션 헤드라인→인물/요약 카드(`bg-surface-deep`)→타임라인→메트릭 타일→CTA 순.
- **인물(담당자) 카드** — `rounded-xl border border-border bg-surface-deep p-3` + `.bg-gradient-brand` 원형 아바타(`text-primary-foreground`) + 이름/역할/응답지표(긍정 수치는 `text-fd-emerald-400`).
- **타임라인 행** — 라벨(`w-12 text-[11px] font-bold`) + 점(`bg-primary`) + 제목/설명. **하이라이트 행**은 emerald 틴트(`border-fd-emerald-400/25 bg-fd-emerald-400/[0.06]`)로 강조 + 라벨/점/제목 emerald 전환 + `★`.
- **메트릭 타일 (3-up)** — `grid-cols-3 gap-2`, 각 칸 emerald 틴트 + `font-black` 수치 + 마이크로 라벨.
- **시멘틱 비교 카드(손실 vs 이득)** — red 틴트 카드 ↔ 화살표 ↔ emerald 틴트 카드. 큰 수치 `text-5xl md:text-6xl tabular-nums`.
- **CTA 버튼** — `.bg-gradient-brand rounded-xl py-2.5 text-center font-black text-primary-foreground shadow-lg transition hover:scale-[1.02]`. 보조 안내문은 아래 `text-[11px] text-muted-foreground`.
- **FAQ 아코디언** — `rounded-2xl border border-border bg-surface-raised`, 질문 Q번호 `text-primary`, 열림 본문 `border-t border-border/60 bg-surface-deep/40`.
- **슬라이더/막대** — 트랙 `bg-surface-overlay`, 핸들 `accent-primary`, 채움 막대 `bg-primary`(또는 손실은 `bg-fd-red-400`).
- **상태 게이지 칩** — 점수 구간별 §1 시멘틱(낮음 emerald → 보통 amber → 높음 orange → 매우 높음 red)을 틴트 공식으로.

### shadcn / 프레임워크 합성 주의
- 버튼+링크 조합은 `asChild` 가 아니라 **`render` prop**: `<Button render={<Link href="/" />}>…</Button>`.
- 신규 shadcn 컴포넌트는 `pnpm dlx shadcn@latest add <name>` → `src/components/ui/`.

---

## 7. Do / Don't

### Do
- ✅ 새 컴포넌트는 기존 토큰·유틸 클래스만 사용. 색은 §1 시멘틱 규약을 따른다.
- ✅ 섹션은 항상 eyebrow→헤드라인→본문 3단 리듬으로 연다.
- ✅ 수치는 `tabular-nums`, 의미 있는 수치는 시멘틱 컬러 + 틴트 카드.
- ✅ 새 토큰이 꼭 필요하면 **사용자 승인 후 `globals.css` 에 추가**하고, 이 문서엔 규칙만 적는다.

### Don't
- ❌ 다크 모드 전제 코드(현재 `:root`/`.dark` 동일 복제 = 라이트 강제). 다크 토큰 신설은 별도 결정 사항.
- ❌ 시멘틱 컬러 장식 사용 / `--primary` 외 브랜드 색 추가 / 솔리드 시멘틱 배경.
- ❌ 마케팅 카드에 무거운 그림자. 평면 + 보더가 기본.
- ❌ 이 문서에 hex/oklch/px 기재(토큰명만). 코드와 충돌 시 코드 우선.

---

## 8. 반응형 규칙

- 단일 분기 축은 Tailwind `md`(768px). 그 아래는 단일 컬럼 스택.
- 2단 비교/입력 그리드(`md:grid-cols-2`), 메트릭 3-up 은 모바일에서도 3-up 유지(작은 타일).
- 헤드라인·수치는 모바일 한 단계 축소(`text-3xl → md:text-5xl`, `text-5xl → md:text-6xl`).
- 카드 패딩 모바일 축소(`p-6 → md:p-8`, `p-4 → md:p-5`).

---

## 9. 코드 동기화 메모 (충돌 알림)

스킬 원칙상 **코드(`globals.css`)가 값의 단일 진실**이다. 현재 다음 문서가 옛 디자인 기준으로 **stale** 하므로 값 참고 금지:
- 루트 `CLAUDE.md` 스타일 표 — 다크 차콜네이비 배경 + amber/orange primary 로 기재(옛 다크 시안). **현재 코드는 라이트 + Forest Green.**
- `web/CLAUDE.md` "Design Tokens" 표 — `--primary: oklch(0.205 0 0)`(near-black) 등 shadcn 기본값 잔재. **현재 primary 는 forest green(`oklch(0.42 0.07 150)`).**

이 표들은 이 design.md 가 아니라 해당 CLAUDE.md 측에서 코드 값에 맞춰 갱신 대상.
