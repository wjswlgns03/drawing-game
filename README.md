## 개발 상세 계획서

### 1. 개요

그림을 그리고 맞추는 소셜 게임. 

해당 게임은 실시간 협력과 경쟁을 중점으로 하며, 
Atomic Design, 마이크로 프론트엔드, AWS Lambda, Supabase 등 기술을 활용한 프로젝트.

---

### 2. 기술 스택 및 설계

#### 2.1 프론트엔드

1. **Atomic Design 기반의 UI 컴포넌트 설계**
    
    - 컴포넌트를 Atoms, Molecules, Organisms, Templates, Pages로 나누어 재사용성과 유지보수성을 극대화.
    - Storybook으로 각 컴포넌트의 독립적인 테스트 및 문서화 진행.
2. **마이크로 프론트엔드 아키텍처**
    
    - Webpack Module Federation을 활용하여 각 게임 모드 및 관리 페이지를 독립적인 모듈로 분리.
    - 독립 배포 가능하도록 설계하여 기능 업데이트 시 영향 범위를 최소화.
3. **기술 스택**
    
    - React: 컴포넌트 기반 UI 개발.
    - TypeScript: 안정적인 코드 작성 및 유지보수.
    - TailwindCSS: 빠르고 일관된 스타일링.

#### 2.2 백엔드

1. **AWS Lambda 또는 Supabase 기반 서버리스 아키텍처**
    
    - AWS Lambda: 사용량에 따라 자동으로 확장 및 축소되어 비용 최소화.
    - Supabase: PostgreSQL 기반의 완전 관리형 데이터베이스와 인증 서비스 제공.
2. **WebSocket API**
    
    - 실시간 사용자 간 소통 및 게임 상태 동기화를 위해 WebSocket 활용.
    - Amazon API Gateway WebSocket 또는 Supabase Realtime을 사용하여 서버리스로 구현.
3. **트래픽 대응**
    
    - AWS Auto Scaling과 CloudFront 또는 Supabase’s 글로벌 분산 네트워크를 통해 전 세계 트래픽 부하 분산.
    - Supabase의 데이터 프로비저닝을 자동으로 확장.
4. **OAuth 로그인**
    
    - Google, Facebook, Kakao 등 주요 소셜 플랫폼과의 통합.
    - Supabase Auth를 통해 인증 및 사용자 관리를 처리.

#### 2.3 비용 최적화

1. AWS Lambda와 Supabase를 주요 서비스로 사용하여 인프라 비용 절감.
2. S3와 CloudFront 또는 Supabase Storage를 사용한 정적 자산 제공으로 빠른 로드 시간과 낮은 비용 유지.
3. CloudWatch 또는 Supabase Metrics를 활용한 리소스 모니터링 및 최적화.

---

### 3. 게임 진행 방식

#### 3.1 실시간 게임 로직

- Lambda와 WebSocket 또는 Supabase Realtime을 기반으로 한 실시간 데이터 동기화.
- DynamoDB Streams 또는 Supabase’s Event Triggers를 통해 게임 이벤트 처리.
- 클라이언트는 그림 데이터 및 정답 제출을 JSON 형태로 서버에 전달.

#### 3.2 게임 상태 관리

- 각 사용자와 라운드 정보를 유지하기 위한 상태 관리:
    - Redis 기반 캐싱으로 빠른 조회 제공.
    - Supabase를 통해 영구 저장.

#### 3.3 게임 모드 및 정책

- 기본 모드, 팀 기반 협업 모드, 테마 기반 게임, 타임 어택 모드 등 다양한 모드를 지원.
- 사용자 투표 시스템과 부정행위 방지 규칙 구현.

---


project-root/
├── frontend/                          # 프론트엔드 코드
│   ├── components/                    # Atomic Design에 따른 컴포넌트
│   │   ├── atoms/                     # 최소 단위 UI 요소
│   │   ├── molecules/                 # 여러 atoms로 구성된 UI 요소
│   │   ├── organisms/                 # 기능적 UI 블록
│   │   ├── templates/                 # 페이지 템플릿
│   │   └── pages/                     # 완성된 페이지
│   ├── hooks/                         # 재사용 가능한 React hooks
│   ├── contexts/                      # 전역 상태 관리 (React Context API)
│   ├── services/                      # API 요청 관련 코드
│   ├── styles/                        # TailwindCSS 설정 및 전역 스타일
│   ├── utils/                         # 공통 유틸리티 함수
│   ├── tests/                         # 컴포넌트 테스트
│   ├── storybook/                     # Storybook 설정 및 문서화
│   ├── assets/                        # 프론트엔드 정적 자산
│   │   ├── images/                    # 이미지 파일
│   │   ├── icons/                     # 아이콘 파일
│   │   └── fonts/                     # 웹폰트
│   └── microfrontends/                # 마이크로 프론트엔드 관련 코드
│       ├── game-mode1/                # 게임 모드 1
│       ├── game-mode2/                # 게임 모드 2
│       ├── admin-panel/               # 관리 페이지
│       └── shared/                    # 공통 모듈
│           ├── components/            # 공통 컴포넌트
│           ├── utils/                 # 공통 유틸리티
│           └── styles/                # 공통 스타일
├── backend/                           # 백엔드 코드
│   ├── functions/                     # AWS Lambda 또는 Supabase Edge Functions 코드
│   │   ├── auth/                      # 인증 관련 Lambda 함수
│   │   ├── game-logic/                # 게임 로직 처리
│   │   ├── websocket/                 # 실시간 WebSocket 통신
│   │   └── event-handlers/            # DynamoDB 또는 Supabase 트리거 처리
│   ├── database/                      # 데이터베이스 관련 파일
│   │   ├── schemas/                   # DB 스키마 정의
│   │   ├── migrations/                # DB 마이그레이션 파일
│   │   └── seeds/                     # 초기 데이터 생성 스크립트
│   ├── services/                      # 서비스 레이어
│   │   ├── supabase/                  # Supabase 관련 서비스
│   │   ├── aws/                       # AWS 관련 서비스
│   │   └── third-party/               # 외부 API 연동
│   ├── config/                        # 백엔드 설정 파일
│   │   ├── env/                       # 환경변수 파일
│   │   └── app-config.ts              # 애플리케이션 설정
│   ├── utils/                         # 공통 유틸리티 함수
│   ├── middlewares/                   # API 미들웨어
│   └── tests/                         # 백엔드 테스트 코드
│       ├── unit/                      # 단위 테스트
│       ├── integration/               # 통합 테스트
│       └── e2e/                       # E2E 테스트
├── public/                            # 정적 자산 (S3 또는 CloudFront 배포)
│   ├── images/                        # 이미지 파일
│   ├── icons/                         # 아이콘 파일
│   └── assets/                        # 기타 정적 파일
├── docs/                              # 문서화 파일
│   ├── architecture/                  # 시스템 설계 문서
│   ├── requirements/                  # 요구사항 정의서
│   ├── api/                           # API 스펙 문서
│   └── readme-assets/                 # README에서 사용하는 이미지 및 리소스
├── scripts/                           # 빌드, 배포, CI/CD 스크립트
│   ├── deploy/                        # 배포 관련 스크립트
│   ├── build/                         # 빌드 스크립트
│   └── lint/                          # 린트 및 코드 정리 스크립트
├── config/                            # 글로벌 설정 파일
│   ├── eslint/                        # ESLint 설정
│   ├── prettier/                      # Prettier 설정
│   └── tailwind.config.js             # TailwindCSS 설정
├── tests/                             # 프로젝트 전역 테스트
│   ├── e2e/                           # E2E 테스트
│   ├── integration/                   # 통합 테스트
│   └── unit/                          # 단위 테스트
├── .env                               # 환경변수 파일
├── package.json                       # 프로젝트 메타데이터 및 종속성
├── README.md                          # 프로젝트 개요
├── tsconfig.json                      # TypeScript 설정
└── yarn.lock                          # Yarn 종속성 잠금 파일
