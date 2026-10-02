### 돈이 오가는 시스템의 서버를 만들고 싶은 백엔드 지망생입니다

인하대학교 전기전자공학부 3학년. Java와 Spring Boot로 서버를 만들고, 되돌리기 어려운 판단은 이유와 대가를 문서로 남깁니다.

---

### [stock-portfolio](https://github.com/younggeon03/stock-portfolio)

토스증권과 나무증권 계좌를 합쳐 실제 비중을 계산하는 포트폴리오 서버입니다. 같은 종목을 두 증권사에 나눠 들면 어느 앱에서도 진짜 비중이 보이지 않는 문제에서 시작했습니다. 조회·분석 전용이며 주문 기능은 없습니다.

`Java 21` `Spring Boot 3.3` `JPA` `MySQL` `Flyway` `Docker` `GitHub Actions`

**틀린 숫자보다 에러를 택했습니다.** 환율 조회가 실패했을 때 `1`로 때우던 코드가 해외 주식을 1,380배 작게 보여준 뒤로, 모르는 값은 기본값으로 채우지 않고 예외를 던집니다. ([결정기록 005](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md#005-환율을-못-구하면-예외를-던진다))

**실수로도 주문이 나갈 수 없습니다.** 증권사 연동 인터페이스에 주문 메서드 자체가 없습니다. ([결정기록 001](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md#001-주문-api를-만들지-않는다))

- 금액은 `BigDecimal`로 계산하고, `double`은 변동성 같은 통계에만 씁니다
- 증권사는 `BrokerageClient` 인터페이스 구현 하나로 추가되고, 비중·분석 코드는 바뀌지 않습니다
- 증권사 액세스 토큰은 AES-256-GCM으로 암호화해 DB에 저장합니다
- 스키마는 Flyway 마이그레이션으로만 바꾸고, Hibernate는 검증만 합니다
- 테스트 142개(JUnit 5, AssertJ)는 외부 API와 DB 없이 돕니다
- `main`은 PR로만 들어가고, PR마다 테스트·커버리지·이미지 빌드가 돌며 병합 시 ghcr.io에 이미지를 게시합니다
- 결정기록 11건에 각 판단의 맥락, 대가, 되돌리는 법을 적었습니다

[아키텍처](https://github.com/younggeon03/stock-portfolio/blob/main/docs/아키텍처.md) · [결정기록](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md) · [운영](https://github.com/younggeon03/stock-portfolio/blob/main/docs/운영.md)

---

### 그 밖의 작업

- [mildang](https://github.com/younggeon03/mildang) — 해커톤 팀 프로젝트 "밀당"의 모바일 웹(PWA) 프론트엔드. React, Vite

### 쓰는 기술

- **서버** Java 21, Spring Boot, Spring Data JPA, MySQL, Flyway
- **테스트·배포** JUnit 5, AssertJ, Docker, GitHub Actions, GHCR
- **프론트** HTML·CSS·JavaScript, React
