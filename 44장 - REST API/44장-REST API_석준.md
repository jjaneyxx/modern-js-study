# 44장 - REST API

## REST API란?

**REST(Representational State Transfer)**: HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처 원리.

**REST API**: REST를 기반으로 서비스 API를 구현한 것.

**RESTful API**: REST의 원칙을 잘 지킨 API. 아래 링크에서 언급된 REST의 제한 조건, 원칙, 목표를 잘 따른 REST API.

[위키피디아 - REST](https://ko.wikipedia.org/wiki/REST)

cf. 다른 아키텍쳐도 있을까?

→ RPC(클라이언트가 서버의 함수를 직접 실행), GraphQL(원하는 데이터를 클라이언트가 정의하고 요청), WebSocket(양방향 통신, 실시간 데이터 교환) ...

## REST API의 구성

- 자원: URI
- 행위: HTTP 요청 메서드
- 표현: 페이로드(핵심 데이터. 전송의 근본적인 목적이 되는 데이터의 일부분)

## REST API 설계 원칙

1. URI는 리소스를 표현해야 한다.
2. 행위는 HTTP 요청 메서드로 표현한다.

## 예시

❌ 행위와 자원에 대한 정보가 URI에 포함됨.

- `GET /getUser?id=1` → 메서드명이 URI에 포함됨
- `POST /createUser` → 동사가 포함됨
- `GET /users/update/1` → HTTP 메서드가 아닌 URI에서 동작을 구분

✅ 행위를 메서드로, 자원을 URI로.

- `GET /users/1` → 사용자 정보 조회
- `POST /users` → 사용자 생성
- `PUT /users/1` → 사용자 정보 전체 수정
- `PATCH /users/1` → 사용자 정보 부분 수정
- `DELETE /users/1` → 사용자 삭제
