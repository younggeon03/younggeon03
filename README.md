### 백엔드 개발자를 준비하는 인하대학교 전기전자공학부 3학년입니다

Java와 Spring Boot로 서버를 만들고, 되돌리기 어려운 판단은 이유와 대가를 문서로 남깁니다.

### Projects

**[stock-portfolio](https://github.com/younggeon03/stock-portfolio)** · 개인 프로젝트

토스증권과 나무증권 계좌를 합쳐 실제 비중을 계산하는 포트폴리오 서버입니다. 조회 전용이며 주문 기능은 없습니다.

`Java 21` `Spring Boot 3.3` `JPA` `MySQL` `Flyway` `Docker` `GitHub Actions`

- 환율 조회가 실패하면 기본값 1 대신 예외를 던집니다. 해외 주식이 1,380배 작게 보이던 문제를 이렇게 막았습니다 ([결정기록 005](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md#005-환율을-못-구하면-예외를-던진다))
- 증권사 연동 인터페이스에 주문 메서드를 두지 않아 주문이 구조적으로 나갈 수 없습니다 ([결정기록 001](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md#001-주문-api를-만들지-않는다))
- 금액은 `BigDecimal`로 계산하고, 증권사 토큰은 AES-256-GCM으로 암호화해 저장합니다. 스키마는 Flyway로만 바꿉니다
- 테스트 142개가 있고, PR마다 테스트·커버리지·이미지 빌드를 돌리며 `main`에 병합하면 GHCR에 이미지를 게시합니다
- 되돌리기 어려운 판단 11건을 결정기록으로 남겼습니다

[아키텍처](https://github.com/younggeon03/stock-portfolio/blob/main/docs/아키텍처.md) · [결정기록](https://github.com/younggeon03/stock-portfolio/blob/main/docs/결정기록.md) · [운영](https://github.com/younggeon03/stock-portfolio/blob/main/docs/운영.md)

**[mildang](https://github.com/younggeon03/mildang)** · 공모전 팀 프로젝트

모바일 웹(PWA) 프론트엔드입니다. 팀원 저장소를 fork했고, [프론트 버그 수정 PR](https://github.com/killerwhale-15/mildang/pull/1)을 올렸습니다.

`React` `Vite`

### Tech

Java 21 · Spring Boot · Spring Data JPA · MySQL · Flyway · JUnit 5 · AssertJ · Docker · GitHub Actions
