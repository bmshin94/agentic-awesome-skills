# Agentic Awesome Skills (AAS) — 한국어 분석 & 활용 가이드

> AAS 저장소가 **무엇인지 / 언제 쓰는지 / 어떻게 설치하는지**, 그리고
> **라이선스 경계**와 **수익화 방향**까지 한 번에 정리한 문서입니다.

## 저장소 주소

| 구분 | 주소 |
| :--- | :--- |
| **원본 (upstream)** | https://github.com/sickn33/agentic-awesome-skills |
| **이 저장소 (fork)** | https://github.com/bmshin94/agentic-awesome-skills |
| npm 패키지 | https://www.npmjs.com/package/agentic-awesome-skills |
| Skill Workbench | https://sickn33.github.io/agentic-awesome-skills/workbench |

- 분석 시점: **v17.0.0**
- 규모: 파일 **23,066개**, 스킬 **2,115개**, 플러그인 번들 **59개**

---

## 1. 이게 뭐하는 저장소인가?

한 줄로 말하면 **"AI 코딩 에이전트에게 먹이는 전문가 매뉴얼 모음집"** 입니다.

Claude Code, Cursor, Codex CLI, Gemini CLI 같은 AI 에이전트는 실력은 좋지만
내 프로젝트의 규칙이나 특정 도메인의 관례를 모릅니다.
그래서 매번 길게 설명해야 하는데, 그 설명을 **재사용 가능한 마크다운 문서**로
미리 만들어 둔 것이 바로 **스킬(Skill)** 입니다.

### 스킬 파일의 구조

스킬은 특별한 프로그램이 아니라 **마크다운 파일 한 장**입니다.

```markdown
---
name: code-reviewer
description: "Elite code review expert specializing in modern AI-powered code"
risk: critical
source: community
date_added: "2026-02-27"
---

## Use this skill when      <- 언제 쓰는지
## Do not use this skill when  <- 언제 쓰면 안 되는지
## Instructions            <- 실제 작업 절차
```

### 동작 원리: Progressive Disclosure (점진적 공개)

스킬 2,000개를 전부 메모리에 올리면 컨텍스트가 터집니다. 그래서:

1. 평소에는 각 스킬의 `description` **한 줄만** 로드해 둡니다.
2. 사용자가 "이 코드 리뷰해줘"라고 하면
3. 에이전트가 매칭되는 스킬을 찾아 **그때서야 본문 전체를 읽어옵니다.**

덕분에 방대한 카탈로그를 유지하면서도 컨텍스트를 아낄 수 있습니다.

---

## 2. 폴더 구조

| 경로 | 정체 | 설명 |
| :--- | :--- | :--- |
| `skills/` | 스킬 본체 **2,024개 폴더** | 핵심. 각 폴더에 `SKILL.md` + 선택적 `scripts/`, `references/` |
| `plugins/` | 번들 **59개** | 역할별 세트 (웹개발팩, 보안팩 등) |
| `tools/bin/` | CLI 3종 | `install.js`(설치), `aas.js`(검증/계획), `aas-mcp.js`(MCP 서버) |
| `tools/scripts/` | 유지보수 스크립트 | 검증, 감사, 인덱싱, 보안 스캐너 |
| `apps/web-app/` | React 웹앱 | 스킬 검색/탐색 UI |
| `skills_index.json` | 검색 색인 (1.5MB) | 전체 스킬 메타데이터 |
| `docs/users/` | 사용자 문서 | `getting-started.md` 부터 시작 |
| `supabase/` | DB 마이그레이션 | 웹앱의 즐겨찾기 기능용 |
| `START_APP.bat` | 윈도우 실행기 | 더블클릭으로 웹앱 실행 |
| `CLAUDE.md` | 페르소나 설정 | 이 fork에만 존재 (원본에는 없음) |

---

## 3. 카탈로그 통계

### 카테고리 분포 (상위)

| 카테고리 | 개수 | 카테고리 | 개수 |
| :--- | ---: | :--- | ---: |
| uncategorized | 332 | security | 83 |
| development | 187 | content | 67 |
| cloud | 146 | business | 67 |
| ai-ml | 129 | web-development | 65 |

### 위험도(risk) 분포

| 위험도 | 개수 | 의미 |
| :--- | ---: | :--- |
| `critical` | **1,098** | 주의해서 사용 (전체의 절반 이상) |
| `safe` | 853 | 안전 |
| `none` | 103 | 위험 신호 없음 |
| `offensive` | **61** | 실제 공격 도구 (인가된 모의해킹 전용) |

### 출처(source) 분포

| 출처 | 개수 |
| :--- | ---: |
| community | **1,395** |
| self | 132 |
| vibeship-spawner-skills (Apache 2.0) | 54 |
| zhaoxuya520/reverse-skill (MIT) | 43 |
| official (Anthropic/Google/MS 등) | 14 |

> `community` 1,395개는 출처가 명확히 특정되지 않은 항목입니다.
> 품질 편차가 있고, 재배포·상업적 활용 시 주의가 필요합니다.

---

## 4. 설치 방법

### 사전 준비

```bash
node -v          # v22 이상 필요 (package.json engines)
git --version    # 2.25 이상 (sparse-checkout 사용)
```

### 방법 A. 플러그인 설치 (Claude Code 사용자 권장)

```
/plugin marketplace add bmshin94/agentic-awesome-skills
/plugin install agentic-bundle-essentials
/plugin                                     # 설치 목록 확인
```

주요 번들과 내용물:

| 번들 | 개수 | 포함 스킬 |
| :--- | ---: | :--- |
| `agentic-bundle-essentials` | 5 | concise-planning, git-pushing, lint-and-validate, systematic-debugging, test-driven-development |
| `agentic-bundle-web-wizard` | 8 | react-patterns, nextjs-best-practices, tailwind-patterns, seo-audit, frontend-design 등 |
| `agentic-bundle-full-stack-developer` | 8 | api-patterns, auth-implementation-patterns, database-design, stripe-integration 등 |
| `agentic-bundle-python-pro` | 7 | fastapi-pro, django-pro, async-python-patterns, python-testing-patterns 등 |
| `agentic-bundle-qa-testing` | 7 | browser-automation, e2e-testing-patterns, test-fixing, ci-cd-and-automation 등 |
| `agentic-bundle-devops-cloud` | 8 | docker-expert, kubernetes-architect, terraform-specialist, aws-serverless 등 |
| `agentic-bundle-data-analytics` | 7 | sql-pro, postgres-best-practices, data-storytelling, ab-test-setup 등 |
| `agentic-bundle-security-engineer` | 8 | 침투 테스트 계열 — **인가된 환경에서만 사용** |

전체 목록 확인:

```bash
grep '"name"' .claude-plugin/marketplace.json
```

### 방법 B. npx 직접 설치 (스킬 단위 선택)

설치 대상 지정:

```
--claude        ~/.claude/skills
--cursor        ~/.cursor/skills
--codex         ~/.codex/skills
--gemini        ~/.gemini/skills
--kiro          ~/.kiro/skills
--antigravity   ~/.agents/skills
--agy           ~/.gemini/antigravity-cli/skills
--path <dir>    임의 경로
```

설치 대상 필터:

```
--skills <csv>     정확한 스킬 ID 지정
--category <csv>   카테고리별
--risk <csv>       위험도별 (safe, none, critical, offensive)
--tags <csv>       태그별
--all              전체 (권장하지 않음)
```

안전 옵션:

```
--dry-run          미리보기만, 파일 쓰지 않음
--release <ver>    특정 npm 릴리스로 고정
```

실전 예시:

```bash
# 1) 반드시 미리보기 먼저
npx agentic-awesome-skills --claude \
  --skills systematic-debugging,test-driven-development --dry-run

# 2) 정적 보안 감사
npx agentic-awesome-skills audit --skills systematic-debugging

# 3) 검토 후 실제 설치 (--dry-run 제거)
npx agentic-awesome-skills --claude \
  --skills systematic-debugging,test-driven-development

# 4) 안전 등급만 설치
npx agentic-awesome-skills --claude --risk safe,none --dry-run

# 5) 프로젝트 로컬 설치 (팀 공유용)
npx agentic-awesome-skills --path .claude/skills --skills react-patterns

# 6) 여러 툴 동시 설치
npx agentic-awesome-skills --claude --cursor --skills brainstorming
```

> 필터 없이 실행하면 전체 카탈로그가 설치됩니다.
> 항상 `--skills` 또는 `--risk` 필터를 함께 사용하세요.

### 방법 C. 웹앱으로 탐색

```bash
npm install
npm run app:dev      # 또는 Windows: START_APP.bat 더블클릭
```

### 방법 D. AAS Core MCP (v17 신규, 고급)

에이전트가 프로젝트를 분석해 스킬을 직접 고르게 하는 방식입니다.

```bash
# MCP 서버 등록
node tools/bin/aas.js mcp configure --host claude --scope project \
  --config ~/.claude.json --cache-root <절대경로>

# 에이전트가 만든 매니페스트 검증
node tools/bin/aas.js stack validate --manifest aas-stack.json

# 설치 계획 미리보기 (실제 설치 아님)
node tools/bin/aas.js stack plan --manifest aas-stack.json \
  --target-root . --out plan.json
```

> `stack apply`(실제 적용)와 `stack recover`는 CLI 도움말에
> `EXPERIMENTAL; NOT CERTIFIED`로 표기되어 있습니다.
> 계획 확인까지만 사용하고, 실제 설치는 방법 A/B를 권장합니다.

---

## 5. 사용 방법

설치 후에는 평소처럼 대화하면 에이전트가 알아서 스킬을 불러옵니다.

```
# 자동 발동
"로그인 기능 만들어줘"
  -> test-driven-development 스킬이 매칭되어 테스트부터 작성

# 명시적 호출
"@systematic-debugging 써서 이 에러 원인 찾아줘"
"@react-patterns 기준으로 이 컴포넌트 리팩토링해줘"
"@security-auditor 로 이 API 취약점 검사해줘"
```

관리 명령:

```bash
ls ~/.claude/skills/                    # 설치 목록 확인
rm -rf ~/.claude/skills/react-patterns  # 삭제
cat skills/systematic-debugging/SKILL.md  # 설치 전 내용 확인
```

플러그인 제거:

```
/plugin uninstall agentic-bundle-essentials
```

---

## 6. 안전 수칙

1. **`--all` 사용 금지** — 전체 설치 시 에이전트 컨텍스트가 고갈되어
   truncation 오류나 크래시 루프가 발생할 수 있습니다.
   (`docs/users/agent-overload-recovery.md`, `windows-truncation-recovery.md` 참고)
2. **항상 `--dry-run` 먼저** — 무엇이 설치되는지 확인 후 실행.
3. **`audit` 서브커맨드 활용** — 명령 실행, 네트워크, 자격증명, 파일시스템,
   권한 상승, 파괴적 동작, 심볼릭 링크, 바이너리 신호를 정적 분석합니다.
   단, 안전을 **보증하지는 않습니다.**
4. **`offensive` 61개 주의** — 실제 공격 기법 문서입니다.
   인가된 모의해킹/CTF 등 합법적 맥락에서만 사용하세요.
5. **설치 전 `SKILL.md` 확인** — 설치는 파일 복사일 뿐 실행되지 않지만,
   로드된 후 에이전트의 행동에 영향을 줍니다.

---

## 7. 라이선스 경계 (상업적 활용 시 필수 확인)

| 대상 | 라이선스 | 상업적 이용 |
| :--- | :--- | :--- |
| 코드 / 도구 (`LICENSE`) | **MIT** | 가능 (저작권 고지 유지) |
| 원본 문서 / 콘텐츠 (`LICENSE-CONTENT`) | **CC BY 4.0** | 가능 (출처 표기 필수) |
| HackTricks, OWASP 계열 | **CC-BY-SA** | 카피레프트 — 2차 저작물도 동일 라이선스 |
| 공식 벤더 스킬 (Anthropic/Google/OpenAI/Microsoft 등) | **Proprietary** | **재배포·재판매 불가** |
| `community` 출처 1,395개 | 불명확 | 개별 확인 필요 |

상세 출처는 `docs/sources/sources.md`(179줄)에 정리되어 있습니다.

### 결론

- **스킬 카탈로그를 그대로 묶어 판매하는 것은 권장하지 않습니다.**
  Proprietary 항목이 섞여 있고, 다수 항목의 출처가 불명확하며,
  무엇보다 원본이 GitHub에 무료로 공개되어 있습니다.
- **직접 작성한 스킬**, **도구/서비스**, **교육 콘텐츠**는
  라이선스 제약 없이 자유롭게 상업화할 수 있습니다.

---

## 8. 수익화 아이디어

저장소를 분석하며 확인된 **실제 공백**:

- 한국어 문서 **0개** (중국어 `docs_zh-CN`, 베트남어 `docs/vietnamese`는 존재)
- 미분류 스킬 **332개**
- `critical` 등급 **1,098개** — 기업 도입 시 거버넌스 부담
- 출처 불명확 **1,395개** — 품질 검증 미흡

### 아이디어 요약

| # | 아이디어 | 진입 난이도 | 수익 속도 | 라이선스 |
| :-- | :--- | :--- | :--- | :--- |
| 1 | **한국 특화 스킬 직접 제작** | 중 | 3~6개월 | 안전 (자체 저작물) |
| 2 | **사내 스킬 거버넌스 SaaS (B2B)** | 높음 | 6개월+ | 안전 (도구 판매) |
| 3 | **교육 콘텐츠 / 강의** | 낮음 | 1~2개월 | 안전 (출처 표기) |
| 4 | **검증된 큐레이션 SaaS** | 중 | 6개월+ | 주의 필요 |
| 5 | **컨설팅 / 구축 대행** | 낮음 | 즉시 | 안전 |

#### 1. 한국 특화 스킬 제작

전 세계 2,115개 중 한국 비즈니스 도메인 스킬은 사실상 없습니다.

```
korean-fintech-compliance   전자금융감독규정, 망분리
pipa-privacy-audit          개인정보보호법 (GDPR과 다름)
toss-payments-integration   토스페이먼츠 (stripe-integration은 이미 존재)
naver-kakao-oauth           국내 소셜 로그인
nara-jangteo-rfp            나라장터 제안서
kr-ecommerce-law            전자상거래법, 통신판매업 신고
```

무료 공개로 신뢰를 쌓고 기업 커스텀 제작으로 연결하는 경로가 현실적입니다.

#### 2. 거버넌스 SaaS

오픈소스 콘텐츠를 판매하지 않고 **관리 도구**를 판매하므로 라이선스 이슈가 없습니다.

- 사내 설치 스킬 스캔 및 인벤토리
- 위험도 / 라이선스 자동 감사
- 승인된 스킬만 배포하는 사내 마켓플레이스
- ISMS-P 대응용 감사 로그

기반 스크립트(`tools/scripts/security_scanner.py`, `audit_skills.py`)는
MIT라 그대로 활용 가능합니다.

#### 3. 교육 콘텐츠

CC BY 4.0이므로 출처를 표기하면 강의 자료로 자유롭게 활용할 수 있습니다.
한국어 번역본을 무료 배포해 유입을 만들고 강의·컨설팅으로 전환하는 방식.

---

## 9. 기술 스택 검토 (React / PHP)

### 이미 존재하는 React 앱

`apps/web-app/`에 완성도 높은 React 앱이 포함되어 있습니다. **MIT 라이선스**이므로
fork 후 상업적 활용이 가능합니다.

```
React 19.2 + TypeScript 6 + Vite 8
Tailwind CSS 4 + Supabase + React Router 8
Vitest + ESLint + @phosphor-icons/react + react-virtuoso
```

재사용 가능한 구성 요소:

| 파일 | 역할 |
| :--- | :--- |
| `src/components/SkillCard.tsx` | 스킬 카드 UI |
| `src/components/OutcomeExplorer.tsx` | 목적별 탐색 |
| `src/components/ShortlistReview.tsx` | 후보 검토 |
| `src/components/InstallationHandoff.tsx` | 설치 명령 생성 |
| `src/components/SkillStarButton.tsx` | 즐겨찾기 (Supabase 연동) |
| `src/pages/Workbench.tsx` | 스택 검토 화면 |
| `src/utils/catalogSearch.ts` | 카탈로그 검색 |
| `supabase/migrations/` | DB 마이그레이션 |

즉, 검색·필터·카드 UI·SEO·테스트가 이미 갖춰져 있어
**로그인 / 결제 / 다국어 / 대시보드**만 추가하면 됩니다.

### PHP(Laravel) 적합성

**유리한 부분**

| 기능 | 이유 |
| :--- | :--- |
| 인증 | Breeze / Jetstream |
| 구독 결제 | Cashier |
| 관리자 페이지 | Filament |
| 팀·권한 관리 | 기본 제공 |
| 국내 배포 | 저렴한 호스팅 선택지 |

**불리한 부분**

설치 CLI(`install.js`), MCP 서버(`aas-mcp.js`), 검증 스크립트가
모두 Node.js / Python 기반이라 PHP로 재구현하면 비용이 큽니다.
다만 이 도구들은 **사용자 로컬에서 실행**되므로 웹 서버가 담당할 필요는 없습니다.

### 권장 구성 (하이브리드)

```
[ 프론트엔드 ]  React 19 — 기존 apps/web-app fork
       |  REST API
[ 백엔드 ]      Laravel 11 (PHP)  또는  Supabase
       |
[ 로컬 스캐너 ] Node.js CLI — 기존 tools/ 로직 재활용
                결과를 API로 전송
```

용도별 추천:

| 목표 | 추천 스택 |
| :--- | :--- |
| 거버넌스 SaaS (B2B, 관리자 중심) | React + **Laravel + Filament** |
| 큐레이션 SaaS (B2C, 검색 중심) | **React + Supabase** (기존 앱 확장) |
| 빠른 프로토타입 | 기존 `apps/web-app` 그대로 fork |

시작 명령:

```bash
npm install && npm run app:dev
cp -r apps/web-app ../my-skill-saas    # 별도 프로젝트로 분리
```

---

## 10. 참고 문서

| 문서 | 내용 |
| :--- | :--- |
| `docs/users/getting-started.md` | 입문 가이드 |
| `docs/users/plugins.md` | 플러그인 배포 구조 |
| `docs/users/aas-core.md` | AAS Core / MCP 상세 |
| `docs/users/bundles.md` | 번들 목록 |
| `docs/users/workflows.md` | 실행 플레이북 |
| `docs/users/security-and-antivirus.md` | 보안 및 백신 경고 대응 |
| `docs/users/agent-overload-recovery.md` | 컨텍스트 과부하 복구 |
| `docs/sources/sources.md` | 출처 및 라이선스 |
| `LICENSE` / `LICENSE-CONTENT` / `TERMS.md` | 라이선스 및 이용약관 |

---

## 부록: 빠른 시작 체크리스트

```
[ ] node -v 로 v22 이상 확인
[ ] /plugin marketplace add bmshin94/agentic-awesome-skills
[ ] /plugin install agentic-bundle-essentials
[ ] "@systematic-debugging 써서 이 버그 잡아줘" 로 테스트
[ ] 만족하면 본인 스택에 맞는 번들 1개 추가
[ ] --all 은 절대 사용하지 않기
```
