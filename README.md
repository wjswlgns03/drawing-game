## 개발 상세 계획서

### 1. 개요

그림을 그리고 맞추는 소셜 게임을 개발하기 위한 상세한 기술 및 설계 방안입니다. 
해당 게임은 실시간 협력과 경쟁을 중점으로 하며, 

Atomic Design, 마이크로 프론트엔드, AWS Lambda 또는 Supabase 등 기술을 활용하여 
비용 효율적이고 확장 가능한 구조를 목표로 합니다.

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
