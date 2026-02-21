---
type: research
note_status: literature
confidence_level: medium
source_agent: claude-code
generated_via: manual
verified_by: none
sources:
  - https://github.com/browser-use/browser-use
  - https://github.com/Skyvern-AI/skyvern
  - https://github.com/EmergenceAI/Agent-E
  - https://github.com/lavague-ai/LaVague
  - https://docs.anthropic.com/en/docs/agents-and-tools/computer-use
  - https://openai.com/index/introducing-operator/
  - https://deepmind.google/models/project-mariner/
  - https://github.com/browser-use/n8n-nodes-browser-use
  - https://browser-use.com/pricing
  - https://www.skyvern.com/blog/best-free-open-source-browser-automation-tools-in-2025/
  - https://www.firecrawl.dev/blog/best-browser-agents
  - https://o-mega.ai/articles/top-10-browser-use-agents-full-review-2026
created: 2026-02-21
reviewed_at: 2026-02-21
---

# AI 브라우저 에이전트 조사 (2025-2026)

> AI가 화면을 보고 판단해서 클릭, 입력, 탐색하는 자율 에이전트 도구 비교.
> 용도: 쿠팡/네이버 등 이커머스 사이트에서 상품 리서치, 경쟁사 분석, 데이터 수집.

---

## 1. 도구별 상세 비교

### 1-1. Browser Use (추천 1순위)

| 항목 | 내용 |
|------|------|
| **GitHub** | [browser-use/browser-use](https://github.com/browser-use/browser-use) — 78,000+ stars |
| **오픈소스** | MIT 라이선스 (상업 사용 자유) |
| **셀프호스팅** | 가능. Python 패키지로 로컬 실행 |
| **언어** | Python (3.11+) |
| **동작 방식** | DOM 파싱 + 스크린샷 분석 (하이브리드) → Playwright로 액션 실행 |
| **지원 LLM** | Claude (Anthropic), GPT-4o (OpenAI), Gemini (Google), **Ollama 로컬 모델**, DeepSeek, 기타 LiteLLM 호환 |
| **Headless 모드** | 지원. `headless=True` 옵션 |
| **성능** | WebVoyager 벤치마크 89.1% (최고 수준) |
| **설치 난이도** | 쉬움 (pip/uv 한 줄) |
| **안정성** | 커뮤니티 최대, 활발한 업데이트 |

**설치:**
```bash
pip install browser-use
playwright install
```

**기본 사용 예시:**
```python
from browser_use import Agent, Browser
from langchain_anthropic import ChatAnthropic

import asyncio

async def main():
    agent = Agent(
        task="쿠팡에서 '무선 이어폰' 검색 후 상위 10개 상품명과 가격 수집",
        llm=ChatAnthropic(model="claude-sonnet-4-5-20250514"),
        browser=Browser()
    )
    result = await agent.run()
    print(result)

asyncio.run(main())
```

**가격 (셀프호스팅):**
- 도구 자체: 무료
- LLM API 비용만 발생 (Claude Sonnet: $3/$15 per 1M tokens)
- 태스크당 약 10~30스텝 → 대략 $0.05~$0.30/태스크

**가격 (Browser Use Cloud):**
- 가입 시 $10 무료 크레딧
- Pay As You Go: 초기화 $0.01/태스크 + 스텝당 $0.002~$0.05 (모델별)
- Starter: $100/월 (50 동시 세션)
- Business: $500/월 (250 동시 세션, 50% 할인)
- 브라우저 세션: $0.06/시간 (Business $0.03/시간)

---

### 1-2. Skyvern

| 항목 | 내용 |
|------|------|
| **GitHub** | [Skyvern-AI/skyvern](https://github.com/Skyvern-AI/skyvern) — 20,400+ stars |
| **오픈소스** | AGPL-3.0 (상업 사용 시 주의 — 수정 코드 공개 의무) |
| **셀프호스팅** | 가능. Docker 또는 Python |
| **동작 방식** | LLM + Computer Vision, XPath/CSS 셀렉터 불필요 |
| **지원 LLM** | GPT-4o, Claude, 기타 (설정 가능) |
| **Headless 모드** | 지원 |
| **특이 기능** | Route Memorization (AI가 경로 학습 후 Playwright 스크립트로 컴파일 → 사이트 변경 시 자동 치유) |
| **설치 난이도** | 중간 (Docker 권장) |
| **안정성** | 프로덕션 사용 사례 있음 (보험 견적, 정부 서류) |

**가격 (셀프호스팅):** 무료 + LLM API 비용
**가격 (클라우드):** $0.05/스텝, 엔터프라이즈 커스텀

**장점:** Route Memorization이 반복 태스크에 매우 유용 — 첫 실행은 AI가 학습, 이후는 컴파일된 스크립트로 빠르고 저렴하게 실행.
**단점:** AGPL 라이선스, Browser Use 대비 커뮤니티 규모 작음.

---

### 1-3. Anthropic Computer Use

| 항목 | 내용 |
|------|------|
| **제공** | Anthropic API (beta) |
| **오픈소스** | 데모 컨테이너 오픈소스, API 자체는 상용 |
| **셀프호스팅** | Docker 데모 제공 (로컬에서 실행 가능) |
| **동작 방식** | 스크린샷 → Claude가 해석 → 마우스/키보드 시뮬레이션 (전체 데스크톱 제어) |
| **지원 LLM** | Claude 전용 (Sonnet 4.5, Opus 4.5 등) |
| **Headless 모드** | Xvfb + VNC로 headless 가능 |
| **설치 난이도** | 쉬움 (Docker 한 줄) |

**Docker 실행:**
```bash
docker run -d --name claude-computer-use \
  -p 5900:5900 -p 6080:6080 \
  -e ANTHROPIC_API_KEY=your_key \
  anthropic/computer-use-demo
```
→ `http://localhost:6080`에서 noVNC로 화면 확인 가능

**가격:**
- Claude Sonnet 4.5: $3/$15 per 1M tokens
- Computer Use는 시스템 프롬프트에 ~500 토큰 추가
- 실사용 기준: 시간당 약 $10~$20 (스크린샷 전송 때문에 토큰 소모 큼)

**장점:** 브라우저뿐 아니라 전체 데스크톱 제어 가능 (파일 관리, 터미널 등).
**단점:** 스크린샷 기반이라 토큰 비용 높음, 브라우저 전용 도구 대비 속도 느림.

---

### 1-4. Claude for Chrome (Anthropic)

| 항목 | 내용 |
|------|------|
| **형태** | Chrome 확장 프로그램 |
| **출시** | 2025년 8월 |
| **접근** | Anthropic Max 플랜 ($100~$200/월), 2025년 12월부터 Pro/Team/Enterprise 확대 |
| **특징** | 사이드바에서 Claude와 대화하며 브라우저 제어 위임 |
| **셀프호스팅** | 불가 (Anthropic SaaS 전용) |
| **서버 사용** | 불가 (데스크톱 Chrome 전용) |

**용도:** 개인 리서치/브라우징 보조. 서버 자동화에는 부적합.

---

### 1-5. OpenAI Operator

| 항목 | 내용 |
|------|------|
| **제공** | OpenAI (operator.chatgpt.com) |
| **오픈소스** | 비공개 |
| **모델** | Computer-Using Agent (CUA) — GPT-4o 기반 |
| **접근** | ChatGPT Pro ($200/월), Plus ($20/월, 제한적) |
| **셀프호스팅** | 불가 |
| **성능** | WebVoyager 87%, OSWorld 38.1% |
| **지역 제한** | 미국 우선 (2025), 점진 확대 중 |

**장점:** 별도 설치 없이 웹에서 바로 사용.
**단점:** API 없음, 셀프호스팅 불가, 미국 외 접근 제한, 자동화 파이프라인 통합 어려움.

---

### 1-6. Google Project Mariner

| 항목 | 내용 |
|------|------|
| **제공** | Google DeepMind |
| **모델** | Gemini 2.0 |
| **성능** | WebVoyager 83.5% |
| **접근** | Google AI Ultra 플랜 (미국 한정) |
| **오픈소스** | 비공개 |
| **셀프호스팅** | 불가 |
| **특이 기능** | Teach & Repeat (워크플로우 학습), 10개 태스크 동시 처리 |

**장점:** Google 생태계 통합, 동시 태스크 처리.
**단점:** 미국 한정, 비공개, API 제한적.

---

### 1-7. LaVague

| 항목 | 내용 |
|------|------|
| **GitHub** | [lavague-ai/LaVague](https://github.com/lavague-ai/LaVague) |
| **오픈소스** | Apache 2.0 |
| **셀프호스팅** | 가능 (Python) |
| **동작 방식** | 자연어 명령 → 특정 액션 실행 (자율 에이전트가 아닌 명령 기반) |
| **지원 LLM** | 기본 GPT-4o, 커스터마이즈 가능 |
| **설치 난이도** | 쉬움 |

**특징:** Browser Use/Skyvern과 달리 "자율 에이전트"가 아닌 "명령 실행기"에 가까움. "검색 버튼 클릭" 같은 구체적 지시를 실행. 반복적이고 명확한 태스크에 적합.
**단점:** 복잡한 탐색/리서치에는 Browser Use가 우위.

---

### 1-8. Agent-E

| 항목 | 내용 |
|------|------|
| **GitHub** | [EmergenceAI/Agent-E](https://github.com/EmergenceAI/Agent-E) — 1,200+ stars |
| **오픈소스** | MIT 라이선스 |
| **셀프호스팅** | 가능 |
| **동작 방식** | DOM 파싱 전용 (스크린샷 미사용) |
| **지원 LLM** | GPT-4 Turbo 최적화, Ollama/LiteLLM 호환 |
| **성능** | WebVoyager 73% |
| **설치 난이도** | 중간 (uv + 설정) |

**장점:** DOM 전용이라 토큰 비용 낮음.
**단점:** 성능이 Browser Use 대비 낮음, 커뮤니티 작음.

---

## 2. 종합 비교표

| 도구 | 오픈소스 | 셀프호스팅 | LLM 선택 | Headless | 벤치마크 | 설치 난이도 | 서버 사용 | 비용 (셀프) |
|------|----------|------------|----------|----------|----------|-------------|-----------|-------------|
| **Browser Use** | MIT | O | 모든 LLM | O | 89.1% | 쉬움 | O | LLM API만 |
| **Skyvern** | AGPL-3.0 | O | GPT/Claude | O | - | 중간 | O | LLM API만 |
| **Computer Use** | 데모만 | O (Docker) | Claude 전용 | O (Xvfb) | - | 쉬움 | O | 높음 (스크린샷) |
| **Claude Chrome** | X | X | Claude 전용 | X | - | 쉬움 | X | $100~200/월 |
| **Operator** | X | X | CUA 전용 | X | 87% | 없음 | X | $20~200/월 |
| **Mariner** | X | X | Gemini 전용 | X | 83.5% | 없음 | X | Google Ultra |
| **LaVague** | Apache 2.0 | O | 커스터마이즈 | O | - | 쉬움 | O | LLM API만 |
| **Agent-E** | MIT | O | GPT/Ollama | O | 73% | 중간 | O | LLM API만 |

---

## 3. 서버(Headless) 환경에서의 실행

### 3-1. 서버에서 사용 가능한 도구

셀프호스팅 가능한 모든 도구(Browser Use, Skyvern, Computer Use, LaVague, Agent-E)는 headless 서버에서 실행 가능.

**Browser Use headless 설정:**
```python
from browser_use import Browser, BrowserConfig

browser = Browser(config=BrowserConfig(headless=True))
```

### 3-2. VNC/noVNC로 원격 화면 보기

서버에서 AI 에이전트가 브라우저를 조작하는 화면을 원격에서 실시간 모니터링하는 방법.

**방법 1: Anthropic Computer Use 데모 (가장 간단)**
```bash
docker run -d --name claude-computer-use \
  -p 6080:6080 -p 8501:8501 \
  -e ANTHROPIC_API_KEY=$KEY \
  -e WIDTH=1920 -e HEIGHT=1080 \
  anthropic/computer-use-demo
```
→ `http://서버IP:6080/vnc.html`에서 noVNC로 접속

**방법 2: Docker + Xvfb + noVNC 직접 구성**
```dockerfile
FROM python:3.11-slim

# 가상 디스플레이 + VNC
RUN apt-get update && apt-get install -y \
    xvfb x11vnc novnc websockify \
    chromium chromium-driver

# Browser Use 설치
RUN pip install browser-use playwright
RUN playwright install chromium

# 가상 디스플레이 시작
ENV DISPLAY=:99
CMD Xvfb :99 -screen 0 1920x1080x24 & \
    x11vnc -display :99 -forever -nopw & \
    websockify --web /usr/share/novnc 6080 localhost:5900 & \
    python your_agent_script.py
```

**방법 3: 기존 Docker 이미지 활용**
- [ConSol/docker-headless-vnc-container](https://github.com/ConSol/docker-headless-vnc-container)
- [SeleniumHQ/docker-selenium](https://github.com/SeleniumHQ/docker-selenium) (VNC 내장)

접속: `http://서버IP:6080/vnc.html` (noVNC) 또는 VNC 클라이언트로 `서버IP:5900`

---

## 4. 이커머스/셀러 용도 활용 사례

### 4-1. 상품 리서치 자동화

**Browser Use 예시:**
```python
agent = Agent(
    task="""
    쿠팡에서 '에어프라이어' 검색 후:
    1. 상위 20개 상품의 이름, 가격, 리뷰 수, 별점 수집
    2. 결과를 JSON으로 정리
    """,
    llm=ChatAnthropic(model="claude-sonnet-4-5-20250514"),
    browser=Browser(config=BrowserConfig(headless=True))
)
```

### 4-2. 경쟁사 가격 모니터링

- 특정 상품 URL 목록을 주고 매일 가격 변동 체크
- 크론잡으로 n8n 또는 직접 스크립트 스케줄링
- 가격 변동 시 Slack/텔레그램 알림

### 4-3. 키워드 검색 결과 수집

- 네이버 쇼핑에서 키워드별 상위 노출 상품 수집
- 쿠팡 검색 결과 순위 트래킹
- 카테고리별 베스트셀러 모니터링

### 4-4. 한국 이커머스 사이트 주의사항

| 사이트 | 주의점 |
|--------|--------|
| **쿠팡** | WAF 차단 강함. 웹 스크래핑 시 차단 가능성 높음. Open API 병행 권장 |
| **네이버** | 데이터센터 IP 차단. 주거용/모바일 프록시 필요. JS 렌더링 필수 (2025년 업데이트) |
| **공통** | 과도한 요청 시 IP 차단. 적절한 딜레이 + 프록시 로테이션 필요 |

> **중요**: AI 브라우저 에이전트는 사람처럼 행동하므로 기존 스크래핑 대비 차단율이 낮지만, 대량 실행 시 여전히 프록시가 필요할 수 있음.

---

## 5. n8n과의 통합

### 5-1. Browser Use + n8n (공식 커뮤니티 노드)

**설치:**
n8n Settings → Community Nodes → `n8n-nodes-browser-use` 검색 → Install

또는:
```bash
npm install n8n-nodes-browser-use
```

**필요 사항:**
- Browser Use Cloud 계정 (cloud.browser-use.com)
- API Key 발급
- n8n에서 Credential 설정

**주요 기능:**
- 자연어로 웹 태스크 기술
- 구조화된 데이터 추출 (상품, 연락처, 기사 등 템플릿)
- 커스텀 JSON 스키마로 추출 형식 정의
- 최대 200스텝, 3600초 타임아웃
- 지원 모델: Gemini 2.5 Flash, GPT-4.1, Claude Sonnet 4 등

**워크플로우 예시:**
```
Schedule Trigger (매일 9AM)
  → Browser Use 노드 ("쿠팡에서 무선이어폰 TOP 10 가격 수집")
  → Google Sheets 노드 (결과 저장)
  → Slack 노드 (가격 변동 알림)
```

### 5-2. Browserless + n8n (대안)

Browserless는 클라우드 헤드리스 브라우저 서비스. n8n에서 HTTP Request 노드로 연동.

```
HTTP Request → Browserless API (페이지 렌더링/스크래핑)
  → AI Agent 노드 (Claude/GPT로 데이터 분석)
  → 후속 처리 (DB 저장, 알림 등)
```

### 5-3. n8n AI Agent 노드 현황 (2026년 2월)

n8n에 내장된 AI Agent 노드:
- **Tools Agent**: 도구(검색, HTTP, 코드 실행 등)를 사용하는 범용 에이전트
- **Chat Model 연동**: OpenAI, Anthropic, Google, DeepSeek, Groq, Azure 등
- **Memory**: 대화 컨텍스트 유지
- **400+ 통합**: Slack, Google Sheets, Notion, Telegram 등과 즉시 연동

---

## 6. 추천 조합 (OpenClaw 환경 기준)

### 최적 스택

```
Browser Use (셀프호스팅, Python)
  + Claude Sonnet 4.5 (LLM)
  + Docker + noVNC (서버 모니터링)
  + n8n (워크플로우 오케스트레이션)
```

**이유:**
1. **Browser Use** — MIT 라이선스, 최고 성능(89.1%), 가장 큰 커뮤니티, Python 기반으로 기존 스크립트와 통합 쉬움
2. **Claude Sonnet** — 이미 사용 중인 Anthropic API 활용, 비용 효율적
3. **셀프호스팅** — 프록시 설정, 실행 빈도, 데이터 보안 완전 제어
4. **n8n 통합** — 공식 커뮤니티 노드 존재, 기존 워크플로우와 연결 용이

### 비용 예상 (월간)

| 항목 | 예상 비용 |
|------|-----------|
| Browser Use | 무료 (셀프호스팅) |
| Claude Sonnet API (하루 50태스크 x 20스텝) | ~$50~100/월 |
| 서버 (기존 인프라 활용) | $0 추가 |
| 프록시 (필요 시) | $20~50/월 |
| **합계** | **~$70~150/월** |

### 차선책: Skyvern

반복적인 동일 사이트 태스크가 많다면 Skyvern의 Route Memorization이 장기적으로 비용 절감. 첫 실행만 AI가 학습하고 이후는 컴파일된 스크립트로 실행 → LLM 비용 대폭 절감.

---

## 7. 빠른 시작 가이드 (Browser Use)

```bash
# 1. 설치
pip install browser-use
playwright install chromium

# 2. 환경변수
export ANTHROPIC_API_KEY="your-key"

# 3. 테스트 스크립트
python3 << 'EOF'
import asyncio
from browser_use import Agent, Browser, BrowserConfig
from langchain_anthropic import ChatAnthropic

async def main():
    browser = Browser(config=BrowserConfig(headless=True))
    agent = Agent(
        task="Go to google.com and search for 'AI browser agent' and return the first 3 results",
        llm=ChatAnthropic(model="claude-sonnet-4-5-20250514"),
        browser=browser
    )
    result = await agent.run()
    print(result)
    await browser.close()

asyncio.run(main())
EOF
```

---

*조사일: 2026-02-21 | 다음 리뷰: 시장이 빠르게 변하므로 3개월 후 재조사 권장*
