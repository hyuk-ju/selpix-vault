---
type: research
note_status: literature
confidence_level: medium
source_agent: agent-cron
generated_via: cron
verified_by: agent
sources:
  - https://github.com/ouranos-labs/pseolint
  - https://github.com/FlorianOtel/claude-orchestra
  - https://github.com/eworthing/agent-skills
  - https://github.com/Sasitilak/strait
  - https://github.com/leto1210/mafreebox-mcpserver
  - https://claude.ai/
  - https://claude.com/ko-kr/download
  - https://m.blog.naver.com/cjs0308cjs/223666458927
  - https://claudeai.kr/
  - https://vibe.naver.com/album/36800831
created: 2026-04-29
reviewed_at: 2026-04-29
---

# AI 코딩 빠른 레이더 — 2026-04-29 02:34 KST

## 핵심 3줄

1. 이번 수동 점검은 GitHub/API 기반 신호가 가장 안정적이고, Threads/한국어 검색은 공개 검색 결과에서 GitHub 링크가 붙은 항목만 낮은 강도로 참고하는 게 맞습니다.
2. `MCP server`, `Claude Code`, `Codex CLI`, `agent harness` 쿼리는 최근 pushed repo를 계속 감시할 가치가 있습니다.
3. 한국/Threads 자료는 아직 강한 단독 근거로 쓰기보다, 글 안의 GitHub 주소를 추출해 유지보수/최근 push 여부로 검증하는 방식이 안전합니다.

## 주목할 것

1. [z3z1ma/agent-loom](https://github.com/z3z1ma/agent-loom)
   - 출처/신뢰도: HN + GitHub HTML 확인, medium
   - 왜 중요: Markdown knowledge graph로 coding-agent 실행 품질을 높이는 방향이라 `mv-brain` 작업기억/문맥정리에 참고 가능.
   - 바로 할 일: README/구조 확인 후 Hermes skill 또는 `mv-brain` docs 흐름에 흡수할지 보류 검토.
2. [trycua/cua](https://github.com/trycua/cua)
   - 출처/신뢰도: HN + GitHub HTML 확인, medium
   - 왜 중요: macOS 앱 백그라운드 제어는 로컬-first 편집 도구/데모 자동화에 연결 가능.
   - 바로 할 일: 실제 안전성/권한 모델 확인 전까지 watchlist.
3. [yakkomajuri/agentport](https://github.com/yakkomajuri/agentport)
   - 출처/신뢰도: HN + GitHub HTML 확인, medium
   - 왜 중요: destructive ops에 2FA를 붙이는 agent gateway 패턴은 Hermes 자동화 안전장치 아이디어로 좋음.
   - 바로 할 일: 권한/승인 플로우만 참고.
4. [PatrickSqx/MindCheck](https://github.com/PatrickSqx/MindCheck)
   - 출처/신뢰도: HN + GitHub HTML 확인, medium
   - 왜 중요: AI coding log를 분석해 과위임/품질 저하를 보는 도구. Claude/Codex 작업 회고 콘텐츠에 적합.
   - 바로 할 일: 우리 작업 로그에 적용 가능한 지표만 추출.

## GitHub Fresh Repo Searches

- [ouranos-labs/pseolint](https://github.com/ouranos-labs/pseolint): stars=2, pushed=2026-04-28T17:34:32Z, SpamBrain-proof your pSEO before you publish, audit page relationships, not just pages. AI-triaged findings, 8 LLM providers, local telemetry, cost caps, MCP server.
- [FlorianOtel/claude-orchestra](https://github.com/FlorianOtel/claude-orchestra): stars=0, pushed=2026-04-28T17:34:30Z, Setup for using claude code in orchestra mode, with multiple subagents for resarch, plan and implementation
- [eworthing/agent-skills](https://github.com/eworthing/agent-skills): stars=0, pushed=2026-04-28T17:34:30Z, Reusable skills for AI coding agents (Claude Code, Codex, Gemini CLI, Copilot CLI)
- [Sasitilak/strait](https://github.com/Sasitilak/strait): stars=0, pushed=2026-04-28T17:34:30Z,  Move your AI agent sessions between Claude Code, Codex, and OpenCode
- [leto1210/mafreebox-mcpserver](https://github.com/leto1210/mafreebox-mcpserver): stars=0, pushed=2026-04-28T17:34:17Z, MCP Server running with Freebox OS Ultra Dashboard 
- [mikecolbert/today-list-app-Replit](https://github.com/mikecolbert/today-list-app-Replit): stars=0, pushed=2026-04-28T17:34:15Z, Today list application vibe coded in Replit
- [NaetheraS/claude-skills-pack](https://github.com/NaetheraS/claude-skills-pack): stars=0, pushed=2026-04-28T17:34:13Z, ✨ Enhance your AI-assisted development workflow with 25 skills, 14 plugins, and 9 MCP servers in this curated Claude Skills Pack.
- [sharkitect-solutions/sharkitect-claude-toolkit](https://github.com/sharkitect-solutions/sharkitect-claude-toolkit): stars=0, pushed=2026-04-28T17:34:09Z, Personal Claude Code skill library and complete setup guide. 111 custom skills + plugin/MCP/marketplace install guide.

## Reddit Fresh Signals

- r/AskVibecoders: [A hands-on tutorial for using Claude as a Product Manager (open-source, free, 11 modules)](https://reddit.com/r/AskVibecoders/comments/1sy83qk/a_handson_tutorial_for_using_claude_as_a_product/) score=1 comments=0
- r/ClaudeCode: [Claude code web + MCP](https://reddit.com/r/ClaudeCode/comments/1sy7xmm/claude_code_web_mcp/) score=1 comments=0
- r/quantfinance: [Are these numbers good enough to pass a challenge, considering that I’ve already tested them over two years and I consistently get similar monthly results, as well as a five-figure PnL](https://reddit.com/r/quantfinance/comments/1sy7xls/are_these_numbers_good_enough_to_pass_a_challenge/) score=0 comments=0
- r/AiBuilders: [Needlecast - a modern agentic IDE compatible with every CLI](https://reddit.com/r/AiBuilders/comments/1sy7iko/needlecast_a_modern_agentic_ide_compatible_with/) score=2 comments=0
- r/ClaudeAI: [Built an Opensource Persistent memory layer for Coding agent (64% token reduction on SWE benchmarks)](https://reddit.com/r/ClaudeAI/comments/1sy4z7p/built_an_opensource_persistent_memory_layer_for/) score=2 comments=3
- r/ObsidianMD: [I built Codexian, an Obsidian plugin for using OpenAI Codex CLI inside your vault](https://reddit.com/r/ObsidianMD/comments/1sy2zx4/i_built_codexian_an_obsidian_plugin_for_using/) score=0 comments=1
- r/Odoo: [We built an MCP server that gives AI agents full user-equivalent access to Odoo](https://reddit.com/r/Odoo/comments/1sy4l9v/we_built_an_mcp_server_that_gives_ai_agents_full/) score=2 comments=2
- r/opencodeCLI: [Semble: code search MCP for OpenCode that matches transformer accuracy on CPU](https://reddit.com/r/opencodeCLI/comments/1sy01b6/semble_code_search_mcp_for_opencode_that_matches/) score=10 comments=10
- r/mcp: [Semble: a local code search MCP server with near-transformer accuracy on CPU](https://reddit.com/r/mcp/comments/1sxzxay/semble_a_local_code_search_mcp_server_with/) score=1 comments=0
- r/GithubCopilot: [To leave or not to leave](https://reddit.com/r/GithubCopilot/comments/1sy7x8l/to_leave_or_not_to_leave/) score=0 comments=2
- r/alphatesters: [Building a Micro-SaaS that "Vibe-Checks" marketplace listings for distress signals. Launching in 10 days. 🛠️](https://reddit.com/r/alphatesters/comments/1sy7wnv/building_a_microsaas_that_vibechecks_marketplace/) score=1 comments=0
- r/PromptEngineering: [CogniSeeds: First Principles for Adaptive Minds](https://reddit.com/r/PromptEngineering/comments/1sy7vhq/cogniseeds_first_principles_for_adaptive_minds/) score=1 comments=0
- r/ClaudeCode: [Claude code web + MCP](https://reddit.com/r/ClaudeCode/comments/1sy7xmm/claude_code_web_mcp/) score=1 comments=0
- r/cybersecurity: [PBN Protocol Boundary Network](https://reddit.com/r/cybersecurity/comments/1sy7lcq/pbn_protocol_boundary_network/) score=0 comments=2
- r/linux_gaming: [GGST throws out of matches quite often](https://reddit.com/r/linux_gaming/comments/1sy7gea/ggst_throws_out_of_matches_quite_often/) score=1 comments=0

## HN Fresh Signals

- [Ask HN: Is it just me or is Claude Code getting worst?](https://news.ycombinator.com/item?id=47936579) points=8 comments=7 created=2026-04-28T16:21:56Z
- [Show HN: Loom – A Markdown knowledge graph for better coding-agent execution](https://github.com/z3z1ma/agent-loom) points=1 comments=0 created=2026-04-28T16:14:28Z
- [Show HN: Drive any macOS app in the background without stealing the cursor](https://github.com/trycua/cua) points=4 comments=1 created=2026-04-28T16:03:34Z
- [OpenAI Models, Codex, and Managed Agents Come to AWS](https://openai.com/index/openai-on-aws/) points=3 comments=0 created=2026-04-28T17:13:09Z
- [Apple integrates Claude and Codex into Xcode 26.3 for 'agentic coding'](https://venturebeat.com/technology/apple-integrates-anthropics-claude-and-openais-codex-into-xcode-26-3-in-push) points=2 comments=0 created=2026-04-28T02:27:27Z
- [An open-source spec for Codex orchestration: Symphony](https://openai.com/index/open-source-codex-orchestration-symphony/) points=21 comments=2 created=2026-04-27T17:52:40Z
- [Show HN: Loom – A Markdown knowledge graph for better coding-agent execution](https://github.com/z3z1ma/agent-loom) points=1 comments=0 created=2026-04-28T16:14:28Z
- [Show HN: MindCheck – Analyze your AI coding logs for over-delegation](https://github.com/PatrickSqx/MindCheck) points=3 comments=0 created=2026-04-28T12:30:09Z
- [Show HN: I replaced a memory app with two Markdown files and a Git repo](https://news.ycombinator.com/item?id=47912675) points=2 comments=1 created=2026-04-26T18:37:59Z
- [Show HN: Loom – A Markdown knowledge graph for better coding-agent execution](https://github.com/z3z1ma/agent-loom) points=1 comments=0 created=2026-04-28T16:14:28Z
- [Show HN: I built a search engine for llms.txt sites](https://statespace.com/) points=2 comments=0 created=2026-04-28T16:02:05Z
- [Show HN: Hahooh – Give AI agents the power to build their own MCP tools](https://hahooh.xyz/download) points=2 comments=0 created=2026-04-28T13:45:02Z
- [Show HN: Drive any macOS app in the background without stealing the cursor](https://github.com/trycua/cua) points=4 comments=1 created=2026-04-28T16:03:34Z
- [Show HN: Integrations gateway for agents with 2FA for destructive ops (OSS)](https://github.com/yakkomajuri/agentport) points=3 comments=2 created=2026-04-28T14:24:48Z
- [Show HN: Simple SDK for agent-to-agent communication](https://github.com/AgentWorkforce/relay) points=2 comments=0 created=2026-04-28T14:15:32Z

## 수동 보강 — GitHub HTML 검증

- [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code): GitHub 페이지 접근 OK, stars=확인 필요, desc=The agent harness performance optimization system. Skills, instincts, memory, security, and research-first development for Claude Code, Codex, Opencode, Cursor and beyond. - affaan
- [claude-code-chinese/claude-code-guide](https://github.com/claude-code-chinese/claude-code-guide): GitHub 페이지 접근 OK, stars=확인 필요, desc=Claude Code国内如何使用 ？最容易懂的 Claude Code 介绍与教学指南（2026年最新） - claude-code-chinese/claude-code-guide
- [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem): GitHub 페이지 접근 OK, stars=확인 필요, desc=A Claude Code plugin that automatically captures everything Claude does during your coding sessions, compresses it with AI (using Claude's agent-sdk), and injects relevant context 
- [z3z1ma/agent-loom](https://github.com/z3z1ma/agent-loom): GitHub 페이지 접근 OK, stars=확인 필요, desc=Loom is a Markdown-native project state protocol for AI agents. - z3z1ma/agent-loom
- [trycua/cua](https://github.com/trycua/cua): GitHub 페이지 접근 OK, stars=확인 필요, desc=Open-source infrastructure for Computer-Use Agents. Sandboxes, SDKs, and benchmarks to train and evaluate AI agents that can control full desktops (macOS, Linux, Windows). - trycua
- [yakkomajuri/agentport](https://github.com/yakkomajuri/agentport): GitHub 페이지 접근 OK, stars=확인 필요, desc=Secure gateway to connect your agents to any service with granular permissions.  - GitHub - yakkomajuri/agentport: Secure gateway to connect your agents to any service with granula
- [PatrickSqx/MindCheck](https://github.com/PatrickSqx/MindCheck): GitHub 페이지 접근 OK, stars=확인 필요, desc=Analyse your AI conversation logs to measure cognitive engagement over time - PatrickSqx/MindCheck

## 한국/Threads 공개 검색 신호

- `"클로드 코드" "github.com"` [Sign in - Claude](https://claude.ai/)
- `"클로드 코드" "github.com"` [Claude 다운로드 | Claude 다운로드](https://claude.com/ko-kr/download)
- `"클로드 코드" "github.com"` [클로드 AI 사용법, Claude 무료 유료 차이, 버전별 가격 비교](https://m.blog.naver.com/cjs0308cjs/223666458927)
- `"클로드 코드" "github.com"` [Claude(클로드) 한국어 무료 버전](https://claudeai.kr/)
- `"바이브코딩" "github.com"` [VIBE (바이브)](https://vibe.naver.com/album/36800831)
- `"바이브코딩" "github.com"` [VIBE](https://vibe.naver.com/album/2233212)
- `"바이브코딩" "github.com"` [VIBE](https://vibe.naver.com/magazines/62477)
- `"바이브코딩" "github.com"` [여인의 눈물 VIBE (바이브)](https://vibe.naver.com/track/44036855)
- `"AI 코딩 에이전트" "github.com"` [如何彻底禁用搜狗输入法的旺仔AI？ - 知乎](https://www.zhihu.com/question/1903860201389548284)
- `"AI 코딩 에이전트" "github.com"` [从2025年来看，AI 泡沫是否会在一两年内破灭？ - 知乎](https://www.zhihu.com/question/1943070667252670985)
- `"AI 코딩 에이전트" "github.com"` [有没有大佬帮我解释一下AI infra到底是干啥的？ - 知乎](https://www.zhihu.com/question/4023337465)
- `"AI 코딩 에이전트" "github.com"` [媒体称 AI 大厂月薪 3 万元「疯抢」文科生，为何会出现这个现象 ...](https://www.zhihu.com/question/2016095792616744489)
- `"MCP 서버" "github.com"` [如何使用MCP-Reborn反编译Minecraft源码-百度经验](https://jingyan.baidu.com/article/624e745992d6cc75e9ba5a31.html)
- `"MCP 서버" "github.com"` [水帖：一个用于医学类查询的MCP](https://www.dxy.cn/bbs/newweb/pc/post/52509160)
- `"MCP 서버" "github.com"` [How to find MCP certification online? - Training, Certification, and Program Support](https://trainingsupport.microsoft.com/en-us/mcp/forum/all/how-to-find-mcp-certification-online/8eb88b0d-d065-485a-8182-9da2ba5ffae7)
- `"MCP 서버" "github.com"` [I have the paper copy of my MCP from 1998 and I am trying to get my - Training ...](https://trainingsupport.microsoft.com/en-us/mcp/forum/all/i-have-the-paper-copy-of-my-mcp-from-1998-and-i-am/e993bcde-b83c-436e-a22b-476428aebc90)
- `site:threads.net "클로드 코드" "github.com"` [Sign in - Claude](https://claude.ai/)
- `site:threads.net "클로드 코드" "github.com"` [Claude 다운로드 | Claude 다운로드](https://claude.com/ko-kr/download)
- `site:threads.net "클로드 코드" "github.com"` [클로드 AI 사용법, Claude 무료 유료 차이, 버전별 가격 비교](https://m.blog.naver.com/cjs0308cjs/223666458927)
- `site:threads.net "클로드 코드" "github.com"` [Claude(클로드) 한국어 무료 버전](https://claudeai.kr/)
- `site:threads.net "바이브코딩" "github.com"` [VIBE (바이브)](https://vibe.naver.com/album/36800831)
- `site:threads.net "바이브코딩" "github.com"` [VIBE](https://vibe.naver.com/album/2233212)
- `site:threads.net "바이브코딩" "github.com"` [VIBE](https://vibe.naver.com/magazines/62477)
- `site:threads.net "바이브코딩" "github.com"` [여인의 눈물 VIBE (바이브)](https://vibe.naver.com/track/44036855)

### 한국/Threads 내 GitHub URL 검증

- GitHub API는 unauth rate limit에 걸렸지만, GitHub HTML 페이지 접근으로 보강 확인함.
- [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code): 페이지 접근 OK. Claude Code 운영 패턴 모음으로 이미 Hermes skill 선별 흡수 진행 중.
- [claude-code-chinese/claude-code-guide](https://github.com/claude-code-chinese/claude-code-guide): 페이지 접근 OK. 중국어권 Claude Code 가이드라 한국 자료 신호와는 별도 참고 수준.
- [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem): 페이지 접근 OK. coding agent memory 계열 후보.

## 우리한테 적용

- mv-brain: MCP/agent harness/CLI workflow 중 README·demo·FCPXML export 자동화에 붙일 수 있는 repo만 후보로 남깁니다.
- Hermes/자동화: 좋은 workflow는 skill/cron pre-run context로 흡수하되, 로그인·쿠키 기반 수집은 제외합니다.
- 콘텐츠/홍보: 한국어 글감은 `어떤 repo를 써봤고 무엇이 실패/개선됐는지` 형식의 Threads/X/블로그 소재로 변환합니다.

## 버릴 것/보류

- X/Threads/커뮤니티에서만 언급되고 GitHub/docs/release로 확인 안 되는 주장.
- 최근 push가 오래됐거나 archived 된 repo.
- 모델 성능 자랑뿐이고 실제 개발 workflow 개선이 없는 글.

## 관련 문서

- [[Projects/AI-MV-Agent]]
- [[Research/Ideas]]
- [[Memory/변경로그]]
