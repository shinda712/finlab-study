# FINLAB — 개인 금융 학습 실험실

**FINLAB = FINance + LABoratory**

GitHub Pages 기반의 개인 경제·투자 학습 웹앱. 금융 개념을 읽는 데 그치지 않고 슬라이더/계산기로 가정을 바꿔 실험하고, Gemini AI와의 대화에서 얻은 내용을 학습 DB에 축적한다.

## 운영 환경
- 웹: https://shinda712.github.io/finlab-study/
- 코드: GitHub `shinda712/finlab-study` (`main` 브랜치, Pages)
- 백엔드: Supabase 프로젝트 `cqxbkaxsvycgjmpniwwz` (서울)
- 대상 기기: iPhone 홈 화면 앱 + Mac Safari
- 현재 문서화된 버전: **FINLAB 3.2.1** (실제 코드/배포 버전 우선 확인)

## 현재 기능
- 경제 개념 라이브러리, 검색, 사용자 메모, 즐겨찾기 및 학습 완료 표시
- 인터랙티브 계산기: 현재가치/할인율, PER·이익수익률, 고든 성장모형, EPS×PER
- Supabase Auth: 이메일 링크 및 이메일·비밀번호 로그인
- Supabase 기반 학습 기록 및 실험 시나리오 동기화
- Gemini AI 채팅(Supabase Edge Function), 대화 기록 저장·기기간 불러오기
- AI 응답 검토 후 학습노트로 등록
- 홈 화면 설치용 manifest, 버전 확인/업데이트 알림

## 주요 파일
- `index.html`: 현재 단일 페이지 앱, UI/프론트엔드 로직
- `config.js`: **공개 가능한** Supabase 프로젝트 URL / publishable key만 포함
- `manifest.webmanifest`, `icon.svg`: 홈 화면 앱
- `version.json`: 최신 버전 확인용; 릴리스 시 버전도 함께 올릴 것
- `README.md`: 개요·사용법
- `ROADMAP.md`: 다음 기능 및 완료 기준
- `AGENTS.md`: AI 개발 에이전트용 수정·보안·검증 규칙

## 데이터 흐름
1. FINLAB 웹앱 → Supabase Auth(사용자 인증).
2. 학습 자료/메모/진도 → `public.finlab_workspaces` (user_id, payload JSONB, revision).
3. AI 대화 → `public.finlab_ai_messages` (사용자별 RLS).
4. AI 요청 → `finlab-gemini-chat` Edge Function → Gemini API.
5. AI 일일 사용량 → `public.finlab_ai_usage` 및 `finlab_ai_take_quota`.

**주의:** 브라우저에는 Gemini/OpenAI 등의 비밀키를 넣지 않는다. 민감한 설정은 Supabase Edge Function Secrets로 관리한다.

## 활용 방법
- Mac/iPhone에서 위 주소로 접속 후 **같은 FINLAB 계정**으로 로그인한다.
- AI에 질문 → 답변 검토 → **학습 노트로 저장** → 개념 라이브러리에서 복습.
- 새 버전이 나오면 버전 알림/업데이트를 사용한다. 홈 화면에서 오래된 페이지가 보이면 배포와 브라우저 캐시를 점검한다.
- 중요한 데이터는 가끔 JSON으로 별도 백업한다.

## 새 ChatGPT 대화에서 계속 개발하기
“**FINLAB 발전시키자.** GitHub `shinda712/finlab-study`의 `README.md`, `ROADMAP.md`, `AGENTS.md`를 먼저 확인하고, 실제 코드와 Supabase 상태를 점검한 뒤 우선순위가 가장 높은 미완료 작업부터 개발해 줘. 기존 데이터/로그인/동기화/AI 기능은 유지하고 변경 후 테스트해 줘.”

## 개발 주의 사항
프로젝트 문서는 방향을 제시하는 자료이며, **실제 코드/배포/DB가 최종 확인 대상**이다. 상세 체크리스트는 [AGENTS.md](AGENTS.md), 기능 계획은 [ROADMAP.md](ROADMAP.md)를 참고한다.
