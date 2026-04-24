# Lovable 빌드 가이드 — Gemini 풀코스 강의 사이트 재현

> 기준 앱: https://lg-gemini-lecture-260421-fwqy.vercel.app/  
> 목표: 동일 구조 · 동일 콘텐츠를 Lovable + GitHub로 재현

---

## 1. 기술 스택 결정

현재 앱은 **Next.js App Router** 기반입니다.  
Lovable은 **React + Vite + React Router** 환경이므로, 아래와 같이 대응합니다.

| 원본 (Next.js) | Lovable 대응 |
|---------------|-------------|
| `app/orientation/setup/page.tsx` | `src/pages/orientation/Setup.tsx` |
| `app/period/[id]/page.tsx` | `src/pages/period/PeriodPage.tsx` |
| `app/period/[id]/clip/[clipId]/page.tsx` | `src/pages/period/ClipPage.tsx` |
| `app/appendix/[slug]/page.tsx` | `src/pages/appendix/AppendixPage.tsx` |
| Layout (글로벌 네비) | `src/components/layout/SiteLayout.tsx` |

**Lovable 초기 프롬프트에 반드시 명시할 것:**
```
Use React Router v6 with nested routes.
Use Tailwind CSS for all styling.
Use shadcn/ui for UI components.
Create a multi-page lecture site with the following route structure: [아래 라우트 구조 붙여넣기]
```

---

## 2. 라우트 구조 (React Router)

```
/                          → HomePage
/orientation/setup         → OrientationPage
/period/1                  → Period1Page (Gems)
/period/1/clip/01-concept  → ClipPage (동적)
/period/1/clip/02-schedule-gem
/period/1/clip/03-voe-gem
/period/1/clip/04-persona-gem
/period/1/clip/05-message-gem
/period/1/checkpoint       → CheckpointPage
/period/2                  → Period2Page (Deep Research)
/period/2/clip/01-basics
/period/2/clip/02-benchmark
/period/2/clip/03-interview-research
/period/2/clip/04-fast-vs-deep
/period/2/checkpoint
/period/3                  → Period3Page (AI Studio)
/period/3/clip/01-overview
/period/3/clip/02-phase-1
/period/3/checkpoint
/period/4                  → Period4Page (NotebookLM)
/period/4/clip/01-rag
/period/4/clip/02-mckinsey
/period/4/clip/03-tips-search
/period/4/clip/04-audio
/period/4/clip/05-archive
/period/4/checkpoint
/appendix/cheatsheet-tools
/appendix/cheatsheet-workspace
/appendix/cheatsheet-prompts
/appendix/cheatsheet-sheets
/appendix/cheatsheet-pipeline
/appendix/cheatsheet-share-page
```

---

## 3. GitHub 파일 구조

```
project-root/
├── src/
│   ├── main.tsx
│   ├── App.tsx                  ← 라우터 정의
│   ├── components/
│   │   ├── layout/
│   │   │   ├── SiteLayout.tsx   ← 글로벌 네비게이션 + 푸터
│   │   │   ├── SiteNav.tsx      ← 상단 교시 네비바
│   │   │   └── PeriodSidebar.tsx ← 교시 내부 사이드바
│   │   ├── ui/                  ← shadcn/ui 기본 컴포넌트
│   │   ├── content/
│   │   │   ├── TipBlock.tsx     ← TIP 콜아웃 박스
│   │   │   ├── WarningBlock.tsx ← 주의 콜아웃 박스
│   │   │   ├── CoreBlock.tsx    ← 핵심 콜아웃 박스
│   │   │   ├── CodeBlock.tsx    ← 코드 블록 (복사 버튼 포함)
│   │   │   ├── CompareTab.tsx   ← "AI 없이 vs AI 사용" 탭 비교
│   │   │   ├── StepList.tsx     ← 번호 Step 리스트
│   │   │   └── CheckpointList.tsx ← 체크포인트 체크박스 목록
│   ├── pages/
│   │   ├── HomePage.tsx
│   │   ├── orientation/
│   │   │   └── OrientationPage.tsx
│   │   ├── period/
│   │   │   ├── Period1Page.tsx   ← 1교시 인트로
│   │   │   ├── Period2Page.tsx
│   │   │   ├── Period3Page.tsx
│   │   │   ├── Period4Page.tsx
│   │   │   └── clips/
│   │   │       ├── p1/
│   │   │       │   ├── Clip01Concept.tsx
│   │   │       │   ├── Clip02ScheduleGem.tsx
│   │   │       │   ├── Clip03VoeGem.tsx
│   │   │       │   ├── Clip04PersonaGem.tsx
│   │   │       │   └── Clip05MessageGem.tsx
│   │   │       ├── p2/
│   │   │       │   ├── Clip01Basics.tsx
│   │   │       │   ├── Clip02Benchmark.tsx
│   │   │       │   ├── Clip03InterviewResearch.tsx
│   │   │       │   └── Clip04FastVsDeep.tsx
│   │   │       ├── p3/
│   │   │       │   ├── Clip01Overview.tsx
│   │   │       │   └── Clip02Phase1.tsx
│   │   │       └── p4/
│   │   │           ├── Clip01Rag.tsx
│   │   │           ├── Clip02Mckinsey.tsx
│   │   │           ├── Clip03TipsSearch.tsx
│   │   │           ├── Clip04Audio.tsx
│   │   │           └── Clip05Archive.tsx
│   │   ├── checkpoint/
│   │   │   ├── Checkpoint1.tsx
│   │   │   ├── Checkpoint2.tsx
│   │   │   ├── Checkpoint3.tsx
│   │   │   └── Checkpoint4.tsx
│   │   └── appendix/
│   │       ├── CheatsheetTools.tsx
│   │       ├── CheatsheetWorkspace.tsx
│   │       ├── CheatsheetPrompts.tsx
│   │       ├── CheatsheetSheets.tsx
│   │       ├── CheatsheetPipeline.tsx
│   │       └── CheatsheetSharePage.tsx
│   ├── data/
│   │   ├── navigation.ts        ← 네비 링크 배열
│   │   ├── period1.ts           ← 1교시 콘텐츠 데이터
│   │   ├── period2.ts
│   │   ├── period3.ts
│   │   ├── period4.ts
│   │   └── appendix.ts
│   └── styles/
│       └── globals.css
├── public/
│   └── screenshots/             ← 강의 스크린샷 이미지
│       └── 00-gemini-entry.png
├── index.html
├── vite.config.ts
├── tailwind.config.ts
├── package.json
└── README.md
```

---

## 4. Lovable 초기 프롬프트 (복붙용)

Lovable에서 새 프로젝트 생성 후 아래 프롬프트를 그대로 입력하세요.

```
Build a multi-page Korean corporate AI training lecture site called "Gemini 풀코스".

Tech stack:
- React + Vite
- React Router v6 (nested routes)
- Tailwind CSS
- shadcn/ui components

Design style:
- Clean, professional, document-like (similar to technical documentation sites)
- White background with subtle gray borders
- Navy/dark blue accent color (#1e3a5f or similar)
- Korean language throughout
- Code blocks with copy button
- Callout boxes: TIP (blue), WARNING (orange/yellow), CORE (green)
- Side navigation within each period (chapter)
- Top navigation bar showing all periods

Route structure:
/ → Home
/orientation/setup → Orientation
/period/1 → Period 1 (Gems)
/period/1/clip/01-concept → Gems concept
/period/1/clip/02-schedule-gem → Schedule Gem exercise
/period/1/clip/03-voe-gem → VoE Gem exercise
/period/1/clip/04-persona-gem → Persona Gem exercise
/period/1/clip/05-message-gem → Message Coach Gem (optional)
/period/1/checkpoint → Period 1 checkpoint
(repeat pattern for periods 2, 3, 4)
/appendix/cheatsheet-tools → Appendix cheatsheet

Key UI components to build:
1. SiteNav - top navbar with period tabs (00~04, 부록)
2. PeriodSidebar - left sidebar with clip list for current period
3. TipBlock - collapsible tip callout
4. CodeBlock - syntax-highlighted code with one-click copy
5. CompareTab - two-tab comparison (AI없이 / AI사용)
6. StepList - numbered step list with icons
7. CheckpointList - interactive checkbox list (saved to localStorage)

Start by creating the App.tsx router structure and SiteLayout with SiteNav.
```

---

## 5. 핵심 컴포넌트 사양

### 5-1. SiteNav (상단 네비바)

```tsx
// 탭 구조
const navItems = [
  { label: "00", sublabel: "오리엔테이션", href: "/orientation/setup" },
  { label: "01", sublabel: "Gems", desc: "실행하기", href: "/period/1" },
  { label: "02", sublabel: "Deep Research", desc: "질문하기", href: "/period/2" },
  { label: "03", sublabel: "AI Studio", desc: "실행하기", href: "/period/3" },
  { label: "04", sublabel: "NotebookLM", desc: "듣기", href: "/period/4" },
  { label: "부록", sublabel: "", href: "/appendix/cheatsheet-tools" },
]
// 현재 경로에 따라 active 탭 하이라이트
// 모바일: 드롭다운 또는 스크롤 가능한 탭
```

### 5-2. PeriodSidebar (교시 내 사이드바)

```tsx
// 교시마다 다른 클립 목록 렌더링
// 예시: Period 1
const period1Clips = [
  { index: "00", label: "인트로", href: "/period/1" },
  { index: "01", label: "Gems 개념·인터페이스", href: "/period/1/clip/01-concept" },
  { index: "02", label: "오늘 일정 도우미 Gem", href: "/period/1/clip/02-schedule-gem" },
  { index: "03", label: "VoE 분석가 Gem", href: "/period/1/clip/03-voe-gem" },
  { index: "04", label: "페르소나 3종 Gem + 질문지", href: "/period/1/clip/04-persona-gem" },
  { index: "05", label: "메시지 코치 Gem", tag: "선택", href: "/period/1/clip/05-message-gem" },
  { index: "", label: "체크포인트", href: "/period/1/checkpoint" },
]
// tag="선택"이면 배지 표시
```

### 5-3. CodeBlock (핵심 컴포넌트)

```tsx
// 반드시 구현해야 할 기능:
// 1. 복사 버튼 (우상단 고정)
// 2. 복사 완료 시 "✓ 복사됨" 피드백 (1.5초)
// 3. 긴 코드는 max-height + 스크롤
// 4. 코드블록 제목 (▶ Gem 인스트럭션 — 오늘 일정 도우미) 표시

interface CodeBlockProps {
  title?: string       // "▶ Gem 인스트럭션 — 오늘 일정 도우미" 형태
  code: string
  collapsible?: boolean  // 접기/펼치기 토글
  defaultOpen?: boolean
}
```

### 5-4. CompareTab (AI 없이 vs Gem 비교)

```tsx
// 원본 앱의 탭 전환 UI 재현
interface CompareTabProps {
  leftLabel: string   // "AI 없이 · 수기 메모"
  rightLabel: string  // "일정 도우미 Gem"
  leftContent: React.ReactNode
  rightContent: React.ReactNode
}
// 탭 전환 시 슬라이드 애니메이션
```

### 5-5. CheckpointList (체크포인트)

```tsx
// localStorage로 체크 상태 유지 (브라우저 세션 간 저장)
// "지우기" 버튼으로 초기화
// 체크된 개수 표시: "0/3 체크됨"
interface CheckpointItem {
  id: string
  label: string
  required?: boolean
}
```

### 5-6. 콜아웃 박스 3종

```tsx
// TIP: 파란 계열 배경, 💡 아이콘
// WARNING (주의): 노란/주황 계열, ⚠️ 아이콘
// CORE (핵심): 초록 계열, ✅ 아이콘
// 제목(선택)과 본문

interface CalloutProps {
  type: "tip" | "warning" | "core"
  title?: string
  children: React.ReactNode
}
```

---

## 6. 콘텐츠 데이터 분리 방식

콘텐츠는 **TypeScript 데이터 파일**로 분리해서 관리하면 수정이 쉽습니다.

### `src/data/period1.ts` 예시 구조

```typescript
export const period1Meta = {
  number: 1,
  tool: "Gems",
  title: "나만의 전문가 만들기",
  primarySkill: "실행하기",
  secondarySkill: "축적하기",
  timeOfDay: "오전, 반복 업무 자동화 준비",
  estimatedMinutes: 70,
  clipCount: 5,
}

export const period1Clips = [
  {
    id: "01-concept",
    title: "Gems 개념·인터페이스",
    estimatedMinutes: 5,
    tag: null,
  },
  {
    id: "02-schedule-gem",
    title: "실습 ① 오늘 일정 도우미 Gem",
    estimatedMinutes: 15,
    tag: null,
  },
  {
    id: "03-voe-gem",
    title: "실습 ② VoE 분석가 Gem",
    estimatedMinutes: 15,
    tag: null,
  },
  {
    id: "04-persona-gem",
    title: "실습 ③ 페르소나 3종 Gem + 질문지",
    estimatedMinutes: 20,
    tag: null,
  },
  {
    id: "05-message-gem",
    title: "실습 ④ 메시지 코치 Gem",
    estimatedMinutes: 10,
    tag: "선택",
  },
]

export const gemsConceptContent = {
  comparisonTable: [
    { item: "역할 설정", without: "매번 프롬프트에 붙여넣기", with: "저장된 인스트럭션 자동 적용" },
    { item: "결과 일관성", without: "대화마다 형식이 달라짐", with: "항상 같은 구조로 출력" },
    { item: "팀 공유", without: "프롬프트 문서로 전달", with: "Gem 링크 한 줄로 공유" },
    { item: "적합한 상황", without: "일회성 작업", with: "반복 패턴이 있는 업무" },
  ],
  gemList: [
    { num: 1, name: "오늘 일정 도우미", role: "하루 체크리스트 자동 생성", reuse: "매일 아침 반복" },
    { num: 2, name: "VoE 분석가", role: "발언 분류·패턴 탐지", reuse: "설문·면담·녹음 정리" },
    { num: 3, name: "페르소나 3종", role: "세대·직군별 인터뷰 대상자 시뮬레이션", reuse: "인터뷰·조사 설계" },
    { num: 4, name: "메시지 코치", role: "민감한 사실을 3가지 버전으로", reuse: "메일·공지·피드백" },
  ],
}
```

---

## 7. Lovable 추가 프롬프트 순서 (단계별)

Lovable은 한 번에 전체를 완성하기 어려우므로 **단계별로 프롬프트**를 입력하세요.

### Step 1 — 레이아웃 & 라우터

```
Create the base layout with:
- SiteLayout component with SiteNav at top
- React Router v6 routes for all pages listed above
- Korean font (Noto Sans KR from Google Fonts)
- Navy primary color (#1e3a5f), white background
- Responsive sidebar that collapses on mobile
```

### Step 2 — 공통 컴포넌트

```
Create these shared content components:
1. CodeBlock with copy button (clipboard API), collapsible option, title prop
2. CalloutBox with type prop (tip=blue, warning=orange, core=green)
3. CompareTab with two-tab toggle, left/right content slots
4. StepList with numbered steps, icon support
5. DataTable for comparison tables (responsive, with header row styling)
6. CheckpointList with localStorage persistence and clear button
```

### Step 3 — 홈페이지

```
Create the HomePage with:
- Hero section: "네 개의 툴, 여섯 개의 역량." heading
- Subtitle: "Gems · Deep Research · AI Studio · NotebookLM을 교시별로 익히며..."
- CTA button: "0교시부터 시작하기" → /orientation/setup
- Period cards grid (4 cards): each showing period number, tool name, title, primary/secondary skill badges, clip count
- Appendix card at bottom
```

### Step 4 — 0교시 오리엔테이션

```
Create OrientationPage at /orientation/setup with:
- Estimated time badge: "예상 10분"
- Scene narrative paragraph (오전 8:30 김지연 장면)
- Table of contents with anchor links
- Section: 강의 구조 (4-row table: 교시/메인툴/무엇을 만드는가)
- Section: JB 6역량 (table + relationship description)
- Section: Gemini 접속하기 (3-step numbered list)
- Section: 오늘 실습 준비물 (bullet list)
- Section: 실행 환경 체크리스트 (4 numbered items with detail)
- Section: JB의 하루 07:00~17:30 (narrative paragraph)
- Bottom nav: "다음으로 — 1교시 시작" button
```

### Step 5 — 1교시 클립들 (Gems)

```
Create all Period 1 clip pages:

/period/1 - intro page with period meta, skill badges, flow timeline, CTA to first clip

/period/1/clip/01-concept:
- Comparison table (일반대화 vs Gem)
- 3-step Gem manager walkthrough
- 4-row Gem list table
- TIP callout: "Gem은 완성품이 아니다"
- Prev/Next navigation

/period/1/clip/02-schedule-gem:
- Scene paragraph (오전 8:35)
- CompareTab (AI 없이 / 일정 도우미 Gem)
- Step 1: 3-substep Gem creation flow
- CodeBlock with title "▶ Gem 인스트럭션 — 오늘 일정 도우미" (full instruction)
- Step 2: test input CodeBlock + example output table
- Step 3: tuning tips
- Checkbox deliverables list
- Prev/Next nav

/period/1/clip/03-voe-gem:
- Scene paragraph
- Comparison table (일반 Gemini vs VoE 분석가 Gem)
- 4-step creation flow
- CodeBlock: VoE 분석가 인스트럭션
- Step 2: test input CodeBlock
- TIP callout
- Step 3: 30분 완주 3-step flow
- Checkpoint deliverables
- Prev/Next nav

/period/1/clip/04-persona-gem:
- Scene paragraph
- CompareTab
- Persona 3종 table (프로필/대화특성)
- 5-step Gem creation flow
- 3 CodeBlocks: 베테랑 / MZ / 팀장 인스트럭션
- TIP callout: "말투와 대화 규칙이 핵심"
- CodeBlock: 페르소나에게 질문 받기 프롬프트
- CORE callout: "페르소나에게 직접 물어보기"
- Comparison table: 페르소나별 질문 차이
- Tone adjustment CodeBlock
- Deliverables
- Prev/Next nav

/period/1/clip/05-message-gem:
- Optional badge
- Scene paragraph + CompareTab
- 4-step creation flow
- CodeBlock: 메시지 코치 인스트럭션
- Test input CodeBlock
- Result comparison table (버전/표현/적합상황)
- WARNING callout: "3가지 중 정답은 없습니다"
- 3 expandable additional scenarios (accordion)
- Deliverables
- Prev/Next nav
```

### Step 6 — 1교시 체크포인트

```
Create /period/1/checkpoint:
- Summary table: 4 Gems created
- "기억할 세 가지 감각" numbered list
- CheckpointList component with 3 items (localStorage saved)
- "다음으로" section with link to Period 2
- Supplementary cheatsheet links
```

### Step 7 — 2교시 (Deep Research)

```
Create all Period 2 pages following same pattern:

/period/2 - intro (질문하기/설득하기 badges, 55min, 4 clips)

/period/2/clip/01-basics:
- Comparison table (일반 Gemini vs Deep Research)
- "언제 꺼내야 하나" 3-question decision guide
- 4-step execution flow
- CodeBlock: 업계 현황 리서치 프롬프트 예시
- 3 result usage methods
- CodeBlock: 후속 토론 질문 변환 프롬프트
- TIP callouts

/period/2/clip/02-benchmark:
- Scene paragraph
- CORE callout: "Deep Research 모드 활성화"
- CodeBlock: 5대 기업 분석 프롬프트
- CodeBlock: 소스 신뢰도 평가 표 프롬프트
- WARNING callout: "이 프롬프트를 쓰는 이유"
- 4-column selection criteria table
- CodeBlock: 시사점 정리 프롬프트
- Deliverables

/period/2/clip/03-interview-research (선택):
- CodeBlock: 직군·세대별 고충 리서치 프롬프트
- Step 2: 페르소나 Gem 업데이트 CodeBlock
- CORE callout: "시뮬레이션 + 실제 데이터"
- 4-step practical flow

/period/2/clip/04-fast-vs-deep:
- Full comparison table (7 rows)
- 3-step decision tree
- 3 scenario examples (A/B/C cards)
- "설계 프롬프트 품질이 가른다" + 4요소 list
- CodeBlock: 범용 Deep Research 템플릿
- Deliverables
```

### Step 8 — 3교시 (AI Studio)

```
Create all Period 3 pages:

/period/3 - intro (실행하기/말하기, MAIN badge, 25min main + 110min advanced)

/period/3/clip/01-overview:
- Gemini앱 vs AI Studio comparison table
- 3-step Build access flow
- "Build의 작동 방식" quote box
- TIP callout

/period/3/clip/02-phase-1:
- Goal statement
- CodeBlock: Phase 1 리서치 대시보드 프롬프트 (full)
- 5-step flow timeline (15min breakdown)
- TIP box: "70% 기준 — 5개 체크" (checklist style)
- 3 correction request CodeBlocks
- CORE callout: "핵심 원칙"
- Next step options (메인 마무리 / 심화 계속)
```

### Step 9 — 4교시 (NotebookLM)

```
Create all Period 4 pages:

/period/4 - intro (듣기/축적하기, 85min, 5 clips)

/period/4/clip/01-rag:
- DEEP DIVE badge
- 일반Gemini vs NotebookLM table
- RAG 4-stage comparison table
- 3-step notebook setup flow
- "출처 추적" 3-step walkthrough
- 3 base prompt CodeBlocks

/period/4/clip/02-mckinsey:
- Target document with external link
- Step 1: 3-step NotebookLM setup
- TIP: URL fallback 2가지
- Step 2: 10 question CodeBlocks (grouped: 필수★/선택)
- TIP: question priority guide
- Step 3: Audio Overview + share flow
- Checkpoint summary

/period/4/clip/03-tips-search:
- Rationale paragraph
- 5 recommended keywords (badge list)
- Search channel tips
- 5-column source quality table
- CodeBlock: 소스 신뢰도 평가 표 프롬프트
- Step 3: 4-step curation flow
- 3 curation question CodeBlocks
- TIP: 자산화 안내

/period/4/clip/04-audio (선택):
- Section A: 음성 파일 업로드 3-step
- WARNING: 음성 업로드 팁
- Section B: Audio Overview 3-step
- Usage scenario table (3 rows)

/period/4/clip/05-archive (선택):
- Drive폴더 vs NotebookLM table
- 4-step archive build flow
- 3 test query CodeBlocks
- Audio Overview sharing steps
- TIP: "만드는 것보다 유지하는 것"
```

### Step 10 — 부록 치트시트

```
Create /appendix/cheatsheet-tools:
- Quick-access link list (6 URLs)
- 4-tool comparison table
- Per-tool cheatsheet sections with:
  - How-to table
  - Common mistakes TIP callout
- "어느 도구로 갈까?" decision guide (6 bullet rules)
- Common habits list (5 items)
- Links to other 5 cheatsheets

(cheatsheet-workspace, cheatsheet-prompts, cheatsheet-sheets,
 cheatsheet-pipeline, cheatsheet-share-page 는 동일 패턴으로 추가)
```

---

## 8. 스타일 가이드 (Tailwind 기준)

```
색상:
- Primary (네이비): #1e3a5f → text-[#1e3a5f], bg-[#1e3a5f]
- Accent: #2563eb (blue-600)
- Background: white (#ffffff)
- Surface: gray-50 (#f9fafb)
- Border: gray-200 (#e5e7eb)
- Text: gray-900 (본문), gray-600 (보조)

TIP 박스: bg-blue-50, border-l-4 border-blue-400, text-blue-900
WARNING 박스: bg-amber-50, border-l-4 border-amber-400, text-amber-900
CORE 박스: bg-green-50, border-l-4 border-green-500, text-green-900

코드블록: bg-gray-900, text-gray-100, font-mono, rounded-lg
선택(optional) 배지: bg-gray-100 text-gray-600 text-xs px-2 py-0.5 rounded

교시 번호 배지:
- 큰 숫자 (01, 02...): text-6xl font-bold text-gray-100 (배경처럼)
- 역량 배지: bg-blue-100 text-blue-800 (주), bg-gray-100 text-gray-600 (부)

하단 prev/next 네비: border-t pt-8 flex justify-between
```

---

## 9. GitHub README.md (루트에 추가)

```markdown
# Gemini 풀코스 — JB AX 교육 강의 사이트

LG전자 Junior Board(JB) 대상 Gemini AI 툴 중심 4교시 강의 웹사이트입니다.

## 구조

| 교시 | 툴 | 주 역량 |
|------|-----|---------|
| 0교시 | 오리엔테이션 | 환경 세팅 |
| 1교시 | Gems | 실행하기 |
| 2교시 | Deep Research | 질문하기 |
| 3교시 | AI Studio | 실행하기 |
| 4교시 | NotebookLM | 듣기 |

## 로컬 실행

\`\`\`bash
npm install
npm run dev
\`\`\`

## 콘텐츠 수정

각 교시 콘텐츠는 `src/data/period{N}.ts` 파일에서 관리합니다.
코드블록, 프롬프트, 테이블 데이터를 변경하면 해당 페이지에 즉시 반영됩니다.

## 배포

Vercel 연동 후 main 브랜치 push 시 자동 배포됩니다.
```

---

## 10. 주의사항 & 팁

### Lovable에서 자주 막히는 포인트

| 상황 | 해결 방법 |
|------|----------|
| 라우트가 중첩되어 네비가 사라짐 | `<Outlet />`을 SiteLayout에 명시적으로 배치 |
| 코드블록 복사 버튼이 안 됨 | `navigator.clipboard.writeText()` + HTTPS 확인 |
| 한국어 폰트가 깨짐 | `index.html` `<head>`에 Google Fonts Noto Sans KR 링크 추가 |
| 체크포인트 상태가 새로고침 시 사라짐 | localStorage 키를 페이지별로 분리 (`checkpoint-p1`, `checkpoint-p2`...) |
| 사이드바가 모바일에서 메인 콘텐츠를 가림 | `lg:block hidden` + 햄버거 토글 버튼 추가 |

### 콘텐츠 추가·수정 워크플로우

1. `src/data/period{N}.ts` 에서 데이터 수정
2. 해당 클립 페이지 컴포넌트에서 데이터 import하여 렌더링
3. 새 클립 추가 시: data 파일 → 클립 컴포넌트 → App.tsx 라우트 → sidebar 배열 순으로 추가
