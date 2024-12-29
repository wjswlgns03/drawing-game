# API 문서

## 인증 API

### 로그인
- **엔드포인트**: `/auth/login`
- **메서드**: `POST`
- **설명**: 타사 토큰을 사용하여 사용자를 인증하고, 이후 요청에 사용할 세션 토큰을 반환합니다.

#### 요청:
```json
{
  "provider": "google",
  "token": "abc123"
}
```
- **필드**:
  - `provider`: (문자열) 인증 제공자, 예: `google`, `facebook`.
  - `token`: (문자열) 타사 OAuth 토큰.

#### 응답:
```json
{
  "user_id": "12345",
  "token": "jwt_token"
}
```
- **필드**:
  - `user_id`: (문자열) 인증된 사용자의 고유 식별자.
  - `token`: (문자열) 이후 API 호출에 사용할 JWT 토큰.

#### 에러:
- **401 Unauthorized**: 유효하지 않거나 만료된 토큰.
  ```json
  {
    "error": "Invalid token."
  }
  ```

---

## 게임 API

### 게임 시작
- **엔드포인트**: `/game/start`
- **메서드**: `POST`
- **설명**: 지정된 방에서 새로운 게임 세션을 시작합니다.

#### 요청:
```json
{
  "room_id": "room123",
  "mode": "basic"
}
```
- **필드**:
  - `room_id`: (문자열) 게임 방의 고유 식별자.
  - `mode`: (문자열) 게임 모드 (예: `basic`, `team`, `time_attack`).

#### 응답:
```json
{
  "game_id": "game123",
  "status": "started"
}
```
- **필드**:
  - `game_id`: (문자열) 새로 생성된 게임 세션의 고유 식별자.
  - `status`: (문자열) 게임 상태 (예: `started`, `paused`).

#### 에러:
- **400 Bad Request**: 누락되거나 잘못된 매개변수.
  ```json
  {
    "error": "Invalid room ID."
  }
  ```
- **404 Not Found**: 방이 존재하지 않음.
  ```json
  {
    "error": "Room not found."
  }
  ```

---

## WebSocket API

### 일반 정보
- **엔드포인트**: `/ws`
- **프로토콜**: WebSocket
- **설명**: 그림 액션 및 상태 동기화와 같은 게임 이벤트를 실시간으로 처리합니다.

### 메시지 형식

#### 클라이언트에서 서버로 보내는 메시지:
- **유형**: `draw`
  ```json
  {
    "type": "draw",
    "data": {
      "x": 10,
      "y": 20
    }
  }
  ```
  - `type`: (문자열) 메시지 유형, 예: `draw`, `guess`.
  - `data`: (객체) 관련 정보를 포함한 페이로드.

#### 서버에서 클라이언트로 보내는 메시지:
- **유형**: `sync`
  ```json
  {
    "type": "sync",
    "data": {
      "x": 10,
      "y": 20
    }
  }
  ```
  - `type`: (문자열) 메시지 유형, 예: `sync`, `game_update`.
  - `data`: (객체) 동기화된 게임 상태 정보를 포함한 페이로드.

### 에러 처리
- **400 Bad Request**: 잘못된 메시지 형식.
  ```json
  {
    "error": "Invalid message type."
  }
  ```
- **1011 Internal Server Error**: WebSocket 통신 중 서버 오류.

---

## 추가 정보

### 속도 제한
- 모든 API 엔드포인트에는 속도 제한이 적용됩니다:
  - **인증 API**: 사용자당 분당 10회 요청.
  - **게임 API**: 사용자당 분당 20회 요청.
  - **WebSocket 메시지**: 연결당 초당 50개 메시지.

### 보안
- 모든 엔드포인트는 안전한 통신을 위해 HTTPS를 요구합니다.
- JWT 토큰은 모든 인증된 요청의 `Authorization` 헤더에 포함되어야 합니다:
  ```http
  Authorization: Bearer jwt_token
  ```
- WebSocket 연결은 핸드셰이크 동안 유효한 JWT 토큰을 요구합니다.

### 버전 관리
- API는 시맨틱 버전을 따릅니다. 현재 버전: `v1.0.0`.
  - 예시 기본 URL: `https://api.example.com/v1`

---


