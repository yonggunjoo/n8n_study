# n8n Study Project

n8n 워크플로우 자동화 도구를 학습하고 실습하는 프로젝트입니다.

## 목차
- [n8n이란?](#n8n이란)
- [핵심 장점](#핵심-장점)
- [설치 및 설정](#설치-및-설정)
- [Docker 실행 과정](#docker-실행-과정)
- [n8n 워크플로우 구성](#n8n-워크플로우-구성)
- [AI 모델 연동](#ai-모델-연동)
- [실제 업무 활용 사례](#실제-업무-활용-사례)
- [코드리뷰봇 구축하기](#코드리뷰봇-구축하기)

## n8n이란?

n8n은 워크플로우 자동화 플랫폼입니다.

### 주요 특징
- **AI를 활용한 업무 자동화** (AI는 선택사항)
- **코딩 없이 값 입력만으로 연동 가능**
- **다양한 플랫폼과 서비스 연결 지원**

### 핵심 장점
- **유연성**: 다양한 기업 환경에 적응
- **호환성**: 여러 AI 제공업체 지원  
- **편의성**: 코딩 없는 워크플로우 구성
- **경제성**: 셀프 호스팅 시 비용 효율적

## 설치 및 설정

### 설치 준비사항
- Docker Desktop 설치
- 운영체제별 설치 가이드 참조

### Docker 볼륨 생성
```bash
$ docker volume create n8n_data
```

### Docker 컨테이너 실행
```bash
$ docker run -it --rm \
 --name n8n \
 -p 8888:5678 \
 -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
 -e N8N_RUNNERS_ENABLED=true \
 -e N8N_TUNNEL_MODE=false \
 -e EDITOR_BASE_URL=https://abc123.ngrok.io \
 -e WEBHOOK_URL=https://abc123.ngrok.io \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n
```

### ngrok 설정 (별도 터미널에서 실행)
```bash
ngrok http 8888
```

#### 중요한 설정 사항

**1. N8N_TUNNEL_MODE 설정**
- ngrok를 사용할 때는 `N8N_TUNNEL_MODE=false`로 설정해야 합니다
- `true`로 설정하면 n8n 자체 터널과 충돌할 수 있습니다

**2. URL 형식**
- `EDITOR_BASE_URL`과 `WEBHOOK_URL`에 실제 ngrok URL을 입력해야 합니다
- 예: `https://abc123.ngrok.io`

**3. 포트 설정**
- 포트 충돌시 `-p 5678:5678` → `-p 8888:5678`로 변경

### ngrok URL 확인
ngrok가 제공하는 URL을 복사합니다:
```
Forwarding: https://abc123.ngrok.io -> http://localhost:8888
```

## Docker 실행 과정

### 컨테이너 실행
- 공식 문서 명령어 복사 실행
- 포트 8888에서 서비스 시작
- 볼륨 마운트로 데이터 영속성 확보

### 백그라운드 실행
- `-d` 옵션으로 데몬 모드 실행
- 터미널 종료 후에도 서비스 지속

### 초기 계정 설정
- 관리자 이메일 설정
- 계정 생성 및 로그인

### 접속 방법
- **로컬 접속**: http://localhost:8888
- **외부 접속**: ngrok URL 사용

### 회원가입
![image](doc/img/회원가입.png)

## n8n 워크플로우 구성

### 트리거 설정
- **수동 실행**: 직접 워크플로우 시작
- **웹훅**: 외부 이벤트 기반 실행
- **스케줄**: 시간 기반 자동 실행

### 주요 기능 카테고리
- **AI**: 인공지능 기능
- **액션**: 실행 작업
- **플로우**: 워크플로우 제어
- **Human-in-the-loop**: 사람 개입 지점
- **코드**: 사용자 정의 코드 실행

## AI 모델 연동

### 지원하는 AI 제공업체
- **OpenAI**
- **Anthropic**
- **Google Gemini**
- **AWS Bedrock**

## 실제 업무 활용 사례

### 다중 플랫폼 연동
- Slack, Jira, Notion, GitHub 연결

### 스크럼 관리 자동화
- Notion 티켓과 Slack 메시지 비교

### AI 기반 보고서 생성
- 일일/주간/월간 자동 리포트

## 데이터 저장 및 관리

- **영구 데이터 저장**: 볼륨을 생성하여 컨테이너 재시작 시에도 데이터 유지
- **n8n_data 볼륨**: `/home/node/.n8n` 디렉토리에 마운트
- **필요한 n8n 이미지**: 자동으로 다운로드 및 설정

---

## 프로젝트 구조
```
n8n_study/
├── doc/
│   ├── docker_settings.md
│   └── n8n.md
└── README.md
```

이 프로젝트를 통해 n8n의 강력한 워크플로우 자동화 기능을 학습하고 실무에 적용할 수 있습니다.

## 코드리뷰봇 구축하기

GitHub Pull Request에 대해 AI가 자동으로 코드리뷰를 수행하는 워크플로우를 구축하는 방법입니다.

### 1. GitHub 토큰 생성

GitHub에서 Personal Access Token을 생성해야 합니다.
- **URL**: https://github.com/settings/apps
- **경로**: Settings > Personal access token > tokens(classic)

### 2. N8N Flow 설정

#### 트리거 추가
1. n8n 워크플로우에서 새로운 트리거를 추가합니다.
   ![image](doc/img/트리거설정메뉴.png)

2. 트리거 타입을 선택합니다.
   ![image](doc/img/트리거설정-1.png)
   ![image](doc/img/트리거설정-2.png)

#### GitHub 연결 설정
1. **Credential to connect with** 설정
   - 사전작업: GitHub Token 발급 필요
   - Settings > Personal access token > tokens(classic)에서 토큰 생성
   
   ![image](doc/img/github_creadential_to_connect_with.png)

2. **GitHub 트리거 설정**
   - 아래 이미지와 같이 설정 완료 후 Test URL 복사
   - Execute step 버튼 클릭
   
   ![image](doc/img/github_trigger.png)

3. **PR Webhook 확인**
   - PR Webhook 단계를 성공하고 요청 결과를 확인
   
   ![image](doc/img/PR_webhook.png)

### 3. AI Agent 추가

#### AI Agent 노드 추가
1. '+' 버튼을 클릭하여 AI Agent 추가
   ![image](doc/img/ai_agent.png)

2. **AI Agent 설정**
   ![image](doc/img/Ai_Agent_설정.png)
   ![image](doc/img/ai_agent.png)

#### Chat Model 및 Tool 설정
1. **Chat Model 추가**
   - 사용할 AI 모델을 선택합니다.

2. **Tool: GitHub MCP 추가**
   - **GitHub MCP Server**: https://github.com/github/github-mcp-server
   - **End Point**: https://api.githubcopilot.com/mcp/
   - **인증**: GitHub Bearer 토큰 입력

### 4. 워크플로우 완성 및 테스트

#### 완성된 Flow
![image](doc/img/완성flow.png)

#### 코드리뷰 결과 확인
실제 Pull Request에서 AI가 작성한 리뷰 코멘트를 확인할 수 있습니다.
![image](doc/img/pr_review.png)

### 코드리뷰봇 동작 흐름
1. **GitHub PR 생성/업데이트** → Webhook 트리거 발동
2. **n8n에서 PR 정보 수신** → 변경된 코드 분석
3. **AI Agent 실행** → 코드 품질 검토 및 개선사항 도출
4. **GitHub에 리뷰 코멘트 작성** → PR에 자동으로 피드백 제공

이를 통해 개발팀의 코드 품질을 향상시키고 리뷰 프로세스를 자동화할 수 있습니다.