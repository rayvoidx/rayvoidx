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

LLM이 스스로 **무엇을 조사할지 결정**하고 → MCP 도구를 **자율 선택·호출**하고 → 결과를 **자체 평가**하며 → 과거 분석에서 **학습**하는 자율 트렌드 인텔리전스 시스템

<div align="center">

https://github.com/user-attachments/assets/806e3668-c61e-4d21-bed3-ba150b0b0871

</div>

### 아키텍처

```
사용자 입력 (키워드 or @채널핸들/youtube.com/...)
    │
    ├──[트렌드 쿼리]──────────────────────────────────────────┐
    │                                                      │
    │  [크리에이터 URL 감지 → Creator Intelligence 분기]        │
    │          │                                           │
    ▼          ▼                                           ▼
FastAPI (:8000) ──SSE 스트리밍──► React (:5173)
    │                   │
    │                   └── Creator DNA Card
    │                        (구독자/평균뷰/콘텐츠 기둥/
    │                          콘텐츠 초안 A/B/C)
    ▼
3-Gear Orchestrator (LangGraph)
    ├── news_trend_agent   → Brave Search → 뉴스·커뮤니티
    ├── viral_video_agent  → Brave Search → YouTube·TikTok
    └── social_trend_agent → Brave Search → X·Instagram
              │
              ▼
    AgenticToolLoop (최대 8회 반복)
    LLM ──→ [brave_search | fetch_url | youtube_trending | ...]
    LLM ──→ SelfEvaluator (품질 게이트)
    LLM ──→ AgentMemory (토픽별 학습 누적)
              │
Creator Intelligence Pipeline (/api/creator/analyze)
    ├── YouTube Data API v3 → 채널 통계 (구독자·평균뷰·참여율)
    ├── Brave Search fallback → 채널 공개 데이터
    └── LLM DNA 분석 → top_topics / content_pillars / audience_profile
                      → ContentDraft × 3 (title A/B/C + script + hashtags)
              │
    ┌─────────┴──────────┐
    ▼                    ▼
Supabase PostgreSQL    Redis (:6380)
(insights, memory,     (SSE 히스토리,
 query_logs, creators)  rate limit)
```

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
