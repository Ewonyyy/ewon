# Design Project — Default Workflow

이 디렉터리는 **디자인 → 코드 변환** 워크플로 전용입니다. 세션이 시작되면 아래 규칙이 자동 적용됩니다.

## 라우팅 규칙

### Figma MCP → executor (sonnet)
`mcp__figma__*` 툴 호출은 **반드시 `executor` 서브에이전트(model=`sonnet`)**를 통해 수행합니다.

```
Agent(subagent_type="executor", model="sonnet",
      prompt="Figma 노드 <fileKey>:<nodeId> 가져와서 <목표> 수행")
```

- 메인 스레드에서 직접 `mcp__figma__*`를 호출하지 마세요. 컨텍스트가 디자인 JSON으로 오염됩니다.
- 이미 `executor` 컨텍스트 안이라면 그대로 호출 OK (재위임 금지).

## 8-Stage Design Pipeline

| 단계 | 목적 | 1순위 (skill) | Agent | Model |
|------|------|----------------|--------|--------|
| ① 요구 정리 | 모호함 제거 | `/oh-my-claudecode:deep-interview` | analyst | Opus |
| ② 리서치 | 레퍼런스·라이브러리 | `/oh-my-claudecode:external-context` | document-specialist | Sonnet |
| ③ 설계/IA | 구조·토큰 설계 | `/oh-my-claudecode:plan` | planner | Opus |
| ④ 디자인 구현 | HTML/CSS 작성 | `/oh-my-claudecode:autopilot` | executor | Sonnet |
| ⑤ 리뷰 | 일관성·접근성 | — | code-reviewer / critic | Sonnet / Opus |
| ⑥ 반복 개선 | 피드백 자동 반영 | `/oh-my-claudecode:ralph` · `ultrawork` | executor | Sonnet |
| ⑦ 검증 | 스크린샷 비교 | `/oh-my-claudecode:visual-verdict` | verifier / qa-tester | Sonnet |
| ⑧ 문서화 | 가이드·README | — | writer | Haiku |
| ⑧ 패턴 보관 | 시스템 누적 | `/oh-my-claudecode:wiki` · `remember` | — | — |

## 적용 가이드

1. **새 디자인 작업 시작** → ①부터 순서대로. 모호함이 없으면 ②③ 스킵 가능.
2. **Figma URL이 들어오면** → ④ 단계로 직진하되 **executor(sonnet)** 위임 필수.
3. **수정 요청이 반복되면** → ⑥ ralph/ultrawork로 자동화.
4. **결과물 인수인계 전** → ⑦ visual-verdict로 스크린샷 비교 후 ⑧ 문서/위키 갱신.

## 작업 디렉터리

- HTML/CSS 결과물: `./` (예: `skeleton-overview.html`)
- 패턴 라이브러리: `/oh-my-claudecode:wiki` 사용
- 프로젝트 메모: `.omc/` (자동 관리)
