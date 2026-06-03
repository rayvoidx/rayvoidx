<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,100:3b82f6&height=160&section=header&text=Jaehyun%20Park&fontSize=42&fontColor=ffffff&fontAlignY=38&desc=%20AI%20Engineer&descSize=18&descAlignY=58&descColor=94a3b8" width="100%"/>

</div>

<div align="center">

<br/>

[![Email](https://img.shields.io/badge/jhpark95@hanyang.ac.kr-EA4335?style=flat-square&logo=Gmail&logoColor=white)](mailto:jhpark95@hanyang.ac.kr
)
[![GitHub](https://img.shields.io/badge/@rayvoidx-181717?style=flat-square&logo=GitHub&logoColor=white)](https://github.com/rayvoidx)

</div>

---

## 🚀 Flagship Project — Social Trend Agent

### 자율 멀티 에이전트 트렌드 인텔리전스 시스템

에이전트가 자율적으로 계획, 수집, 분석, 보고하는 트렌드 분석 시스템: LangGraph 오케스트레이션, MCP 도구 프로토콜, 에이전틱 도구 호출 루프 기반

<img width="1278" height="726" alt="image" src="https://github.com/user-attachments/assets/8a8d79f7-adb1-41eb-91bc-a1add2321e4d" />

<br/>

[![v1.3.0](https://img.shields.io/badge/Release-v1.3.0-blue?style=for-the-badge)](https://github.com/rayvoidx/social-trend-agent/releases)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![LangGraph](https://img.shields.io/badge/LangGraph-0.2+-FF6F00?style=for-the-badge)](https://langchain-ai.github.io/langgraph/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![React 19](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/)

<br/>

[![License](https://img.shields.io/badge/License-BSD--3-green?style=flat-square)](./LICENSE)

</div>

---

## 시스템 설명

대부분의 "AI 에이전트"는 단순한 LLM 래퍼입니다. **이 시스템은 다릅니다** — 스스로 계획하고, 평가하고, 학습합니다.

```
기존 방식:   사용자 → 프롬프트 → LLM → 출력 (원샷)

이 시스템:   사용자 입력 → LLM이 적합한 에이전트 라우팅 → MCP 도구로 멀티 플랫폼 수집
             → LLM이 분석 → LLM이 품질 자체 평가(critic) → 미달 시 보강(refine, 최대 3회)
             → 과거 분석 메모리에서 학습 → 리포트 + 후속 질문 제안
```

**Plan → Execute → Evaluate → Refine** 자율 루프가 핵심입니다 — LLM이 결과 품질을 4차원으로 자체 평가하고, 기준 미달 시 스스로 보강하며, 과거 분석에서 **학습**합니다. 크리에이터 URL을 입력하면 채널 DNA 분석으로 자동 분기합니다.

---

## 아키텍처

```
사용자 입력 (키워드 or @채널핸들 / youtube.com/... )
    │
    ├── [트렌드 쿼리] ─────────────────────────────────┐
    │                                                  │
    │   [크리에이터 URL 감지(CREATOR_URL_RE) → 분기]    │
    ▼                                                  ▼
FastAPI(:8000) ── SSE 스트리밍 ──► React(:5173) ── Creator DNA Card
    │                                                  ▲
    ▼                                                  │
3-Gear 오케스트레이터 (LangGraph)                       │
  Gear 1 route_request → Gear 2 plan_workflow → Gear 3 execute
    ├── news_trend_agent   → Brave Search → 뉴스·커뮤니티
    ├── viral_video_agent  → Brave Search → YouTube·TikTok
    └── social_trend_agent → Brave Search → X·Instagram
          │
          ▼  각 에이전트 StateGraph:
   router → collect → normalize → analyze → summarize
                                    → critic ⇄ refine (최대 3회, 4차원 품질 게이트)
                                    → report → notify
          │
          ├── Creator Intelligence (/api/creator/analyze)
          │     YouTube Data API v3 → Brave fallback → LLM DNA 분석 ──┘
          │     → top_topics / content_pillars / ContentDraft ×3
          │
          ├── Competitor Radar (/api/radar)  — 키워드 모니터(최대 5) + 변화 감지 알림
          ├── Signal Archive (/api/signal-archive) — 토픽 진화·크로스 신시사이즈·주간 다이제스트
          └── 일일 스케줄러 (src/core/scheduler.py) — 인기 검색어 자동 분석 → Slack
          │
    ┌─────┴──────────────┐
    ▼                    ▼
Supabase PostgreSQL    Redis(:6380)        Prometheus(:9091) ← /metrics
(insights, agent_memory,  (SSE 히스토리,    AgentTracer: 노드별 실행 추적
 query_logs, creators)     rate limit)
```

### 자기개선 루프 (Self-Critique)

핵심 혁신: LLM이 자신의 분석 품질을 평가하고, 기준 미달 시 스스로 보강합니다.

```
summarize → critic (4차원 LLM 품질 평가) → score < 0.6 ? → refine → critic (재평가)
                                          score ≥ 0.6 ? → report (보고서 생성)
                                          최대 3회 반복
```

4차원 품질 평가: **커버리지** · **사실 정확성** · **실행 가능성** · **균형/편향**

### 릴리스 타임라인

| 버전       | 날짜       | 코드네임             | 핵심 기술 과제                                            |
| ---------- | ---------- | -------------------- | --------------------------------------------------------- |
| **v1.0.0** | 2026-03-31 | Initial Release      | LangGraph 3-에이전트, Brave Search, SSE 스트리밍          |
| **v1.1.0** | 2026-04-05 | Light & Modes        | quick/standard/deep 모드 분기, Agent Collaboration 시각화 |
| **v1.2.0** | 2026-04-10 | Harness & Agentic    | AgenticToolLoop, SelfEvaluator, 성능 50–60%↑              |
| **v1.3.0** | 2026-05-27 | Creator Intelligence | Creator DNA 분석, HMAC stateless OAuth, SOTA 2026 갱신    |

---

## 💻 Tech Stack

### Agentic AI

![LangGraph](https://img.shields.io/badge/-LangGraph-111827?style=for-the-badge&logoColor=ffffff)
![LangChain](https://img.shields.io/badge/-LangChain-0A192F?style=for-the-badge&logo=LangChain&logoColor=ffffff)
![MCP](<https://img.shields.io/badge/-MCP%20(Anthropic)-334155?style=for-the-badge&logoColor=ffffff>)
![Claude Code](https://img.shields.io/badge/-Claude%20Code-111827?style=for-the-badge&logoColor=ffffff)

### LLM APIs

![Anthropic](https://img.shields.io/badge/-Anthropic%20Claude-111827?style=for-the-badge&logoColor=ffffff)
![OpenAI](https://img.shields.io/badge/-OpenAI%20GPT--5-412991?style=for-the-badge&logo=OpenAI&logoColor=ffffff)
![Google Gemini](https://img.shields.io/badge/-Google%20Gemini-4285F4?style=for-the-badge&logo=Google&logoColor=ffffff)
![Meta Llama](https://img.shields.io/badge/-Meta%20Llama-FF5E00?style=for-the-badge&logo=Meta&logoColor=ffffff)

### Backend

![Python](https://img.shields.io/badge/-Python%203.11+-3776AB?style=for-the-badge&logo=Python&logoColor=ffffff)
![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=for-the-badge&logo=FastAPI&logoColor=ffffff)
![Pydantic](https://img.shields.io/badge/-Pydantic%20v2-E92063?style=for-the-badge&logoColor=ffffff)
![HTTPX](https://img.shields.io/badge/-HTTPX-111827?style=for-the-badge&logoColor=ffffff)
![JWT](https://img.shields.io/badge/-JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=ffffff)

### Frontend

![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?style=for-the-badge&logo=TypeScript&logoColor=ffffff)
![React](https://img.shields.io/badge/-React%2019-61DAFB?style=for-the-badge&logo=React&logoColor=000000)
![Vite](https://img.shields.io/badge/-Vite-646CFF?style=for-the-badge&logo=Vite&logoColor=ffffff)
![Tailwind CSS](https://img.shields.io/badge/-Tailwind%20CSS-06B6D4?style=for-the-badge&logo=TailwindCSS&logoColor=ffffff)
![TanStack Query](https://img.shields.io/badge/-TanStack%20Query-FF4154?style=for-the-badge&logo=ReactQuery&logoColor=ffffff)

### Data & Storage

![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=for-the-badge&logo=PostgreSQL&logoColor=ffffff)
![SQLAlchemy](https://img.shields.io/badge/-SQLAlchemy-D71F00?style=for-the-badge&logoColor=ffffff)
![Redis](https://img.shields.io/badge/-Redis-DC382D?style=for-the-badge&logo=Redis&logoColor=ffffff)
![Pinecone](https://img.shields.io/badge/-Pinecone-0EA5E9?style=for-the-badge&logoColor=ffffff)

### Observability & DevOps

![Prometheus](https://img.shields.io/badge/-Prometheus-E6522C?style=for-the-badge&logo=Prometheus&logoColor=ffffff)
![Langfuse](https://img.shields.io/badge/-Langfuse-111827?style=for-the-badge&logoColor=ffffff)
![GitHub Actions](https://img.shields.io/badge/-GitHub%20Actions-2088FF?style=for-the-badge&logo=GitHubActions&logoColor=ffffff)
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=for-the-badge&logo=Docker&logoColor=ffffff)
![Vercel](https://img.shields.io/badge/-Vercel-000000?style=for-the-badge&logo=Vercel&logoColor=ffffff)

### Cloud & Testing

![Google Cloud](https://img.shields.io/badge/-Google%20Cloud-4285F4?style=for-the-badge&logo=google-cloud&logoColor=ffffff)
![Microsoft Azure](https://img.shields.io/badge/-Microsoft%20Azure-0078D4?style=for-the-badge&logo=Microsoft-Azure&logoColor=ffffff)
![pytest](https://img.shields.io/badge/-pytest-0A9EDC?style=for-the-badge&logoColor=ffffff)
![Ruff](https://img.shields.io/badge/-Ruff-111827?style=for-the-badge&logoColor=ffffff)
![Pyright](https://img.shields.io/badge/-Pyright-2563EB?style=for-the-badge&logoColor=ffffff)

---

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:3b82f6,100:0f172a&height=100&section=footer" width="100%"/>
