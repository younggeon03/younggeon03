Java와 Spring Boot로 서버를 만들고, 증권사 API를 붙이는 쪽은 Python으로 씁니다. 되돌리기 어려운 판단은 이유와 대가를 문서로 남깁니다.

younggeon03@gmail.com

### Projects

**[stock-portfolio](https://github.com/younggeon03/stock-portfolio)** · 개인 프로젝트

토스증권과 나무증권 계좌를 합쳐 실제 비중을 계산하는 포트폴리오 서버입니다. 조회 전용이며 주문 기능은 없습니다.

`Java 21` `Spring Boot 3.3` `JPA` `MySQL` `Flyway` `Docker` `GitHub Actions`

- 환율 조회가 실패하면 기본값 1 대신 예외를 던집니다. 해외 주식이 1,380배 작게 보이던 문제를 이렇게 막았습니다 ([결정기록 005](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md#005-환율을-못-구하면-예외를-던진다))
- 증권사 연동 인터페이스에 주문 메서드를 두지 않아 주문이 구조적으로 나갈 수 없습니다 ([결정기록 001](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md#001-주문-api를-만들지-않는다))
- 공개 조회가 요청마다 기관 보유 수만 줄을 다시 비교하고 있었습니다. 조회 결과를 캐시하고 배치가 데이터를 바꿀 때만 비우도록 고쳐, 동시 30명에서 p95 지연을 8.8초에서 0.099초로 줄였습니다 (k6)
- 금액은 `BigDecimal`로 계산하고, 증권사 토큰은 AES-256-GCM으로 암호화해 저장합니다. 스키마는 Flyway로만 바꿉니다
- 테스트 239개가 있고, PR마다 테스트·커버리지·이미지 빌드를 돌리며 `main`에 병합하면 GHCR에 이미지를 게시합니다. 되돌리기 어려운 판단 22건은 결정기록으로 남겼습니다

[아키텍처](https://github.com/younggeon03/stock-portfolio/blob/main/docs/아키텍처.md) · [결정기록](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md) · [운영](https://github.com/younggeon03/stock-portfolio/blob/main/docs/운영.md)

**kis-ai-quant-trader** · 개인 프로젝트 · 비공개 (대회 종료 후 공개 예정)

한국투자증권 Open API로 실제 대회 계좌(제16회 뱅키스 대학생 모의투자대회 AI부문)를 운용하는 자동매매 엔진입니다. 2026-10-13부터 네이버 클라우드 서버에서 systemd 서비스로 돌고 있습니다.

`Python 3.13` `asyncio` `httpx` `WebSocket` `FastAPI` `Prometheus` `systemd` `NCP`

- 실서버 첫 기동에서 유량 초과가 6분간 74회 났습니다. 문서값(초당 2건)을 믿지 않고 거절률을 직접 재서 실효 한도(초당 1건)로 토큰 버킷을 다시 잡아 0~3회로 줄였습니다
- 요청 하나의 시간을 분해해 네트워크 왕복 144ms, 유량 대기 904ms로 대기열이 86%임을 확인했습니다. 전송 계층이 아니라 호출 수를 줄여 284종목 판정을 305.0초에서 19.8초로 단축했습니다
- 유량 제어는 Mock까지 포함해 모든 REST 호출이 통과합니다. 주문 > 계좌 > 시세 우선순위라 시세 폴링이 몰려도 손절 주문이 밀리지 않습니다
- `.env` 한 줄로 내부 가상 체결 엔진과 실제 KIS 서버를 바꿉니다. 계좌 발급 전에 전체 파이프라인을 검증하고, 발급 후에는 코드 수정 없이 실서버에 붙였습니다
- 성과 수치에는 생존 편향·파라미터 선택편향·표본 부족을 문서에 같이 적었습니다. 측정한 설정과 지금 돌리는 설정이 다른 구간은 "측정 지문이 없다"고 밝혀 둡니다

**미래에셋증권 AI 페스티벌** · 공모전 팀 프로젝트 · 백엔드 담당 · 비공개

금융 문서·상품 검색 기반 연금 QA 에이전트입니다. 5인 팀에서 한 달 동안 커밋 1,454개 중 448개를 작성했습니다.

`Python` `FastAPI` `Pydantic` `PostgreSQL` `pgvector` `SQLAlchemy 2.0` `Docker` `NCP`

- 문서청크 벡터검색과 상품 조건필터를 pgvector로 한 DB 안에서 처리했습니다. 성능이 모자라면 별도 벡터 엔진으로 분리할 계획이었는데, 끝까지 분리하지 않고 구성요소를 늘리지 않는 쪽을 택했습니다
- 연금 세제 계산은 LLM에 맡기지 않고 코드로 구현했습니다. 틀리면 안 되는 숫자라고 판단했습니다
- `GET /answer` 하나로 질문을 받고 5개 필드 JSON으로 응답합니다. AI 파트는 HTTP로만 부르고 계약을 고정해 양쪽이 따로 움직일 수 있게 했습니다
- 5명이 브랜치 292개를 동시에 다뤄, 작업 순서서·상태판·일지를 같이 맡아 진행 상황을 글로 고정했습니다

**[mildang-server](https://github.com/inha-likelion-team2/mildang-server)** · 공모전 팀 프로젝트 · 백엔드 담당

밀가루 흥정 챌린지 앱 "밀당"의 백엔드 API 서버입니다. 커밋 93개 중 90개를 작성했습니다.

`Java 21` `Spring Boot` `JPA` `PostgreSQL` `JWT` `Docker`

- TossPayments 결제 승인 시 같은 주문번호 재요청(뒤로가기·더블탭)은 기존 결제를 돌려주고, 승인 API를 두 번 부르지 않습니다
- 잔액은 `balance = total − spent − prepaid` 항등식이 항상 성립하도록 변경 지점을 두 곳으로 제한했습니다
- 상태 전이 API는 같은 종착 상태 재요청이면 200, 다른 상태로의 교차 전이면 409로 응답합니다
- 트랜잭션 안에서 호출하는 외부 AI 서버에 타임아웃을 걸어, AI가 멈춰도 DB 커넥션 풀이 마르지 않게 했습니다
- 테스트 223개

프론트엔드는 팀원 저장소를 [fork](https://github.com/younggeon03/mildang)해 두었고, [프론트 버그 수정 PR](https://github.com/killerwhale-15/mildang/pull/1)도 올렸습니다.

### Tech

Java 21 · Spring Boot · Spring Data JPA · MySQL · PostgreSQL · Flyway · JWT · JUnit 5 · AssertJ · Python · FastAPI · asyncio · Docker · GitHub Actions
