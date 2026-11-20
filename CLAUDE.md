# CLAUDE.md — 노바리움 마법학교

이 파일은 Claude Code가 이 저장소에서 일할 때 따르는 지침이다. 세션을 시작하면 이 파일부터 읽는다.

## 프로젝트 한 줄

옛 마법학교 웹게임의 감성을 오마주한 **오리지널** 2D 픽셀 웹게임. 클라이언트는 Phaser + pixelfx, 서버는 Spring Boot.
주인(youngjun)은 웹 백엔드 개발자 취업을 준비 중인 4학년 학생이고, 이 프로젝트는 **백엔드 포트폴리오**이기도 하다.

## 세션 시작 시 읽는 순서

1. `docs/PROGRESS.md` — 어디까지 했는지, 막힌 것, 결정 사항
2. `docs/05-개발로드맵.md` — 체크 안 된 첫 작업 찾기
3. 그 작업에 필요한 문서만 골라 읽기
   - 게임 규칙·수치: `docs/03-시스템설계.md`
   - API·ERD·구조: `docs/04-기술설계.md`
   - 이펙트: `docs/06-이펙트매핑.md`
   - 설정·이름: `docs/02-세계관.md`
   - 전체 방향: `docs/01-기획서.md`
4. 원작 관련 요소가 나오면 `docs/07-IP가이드.md`

## 필수 플러그인

| 플러그인 | 용도 | 설치 (Claude Code 안에서) |
|---|---|---|
| pixelfx | 픽셀 이펙트 찾기·사용·제작 | `/plugin marketplace add patch-gg/pixelfx` → `/plugin install pixelfx@pixelfx` |
| ECC | 계획·TDD·리뷰·보안 워크플로 | `/plugin marketplace add https://github.com/affaan-m/ECC` → `/plugin install ecc@ecc` |

설치돼 있지 않으면 직접 설치하지 말고 사용자에게 위 명령을 안내한 뒤 기다린다. 플러그인 설치 후에는 Claude Code를 다시 시작해야 할 수 있다.

## 작업 방식 (한 작업 = 한 사이클)

```
/ecc:plan → (서버면) tdd-workflow → 구현 → 실행 확인 → /code-review → PROGRESS.md 갱신 → 커밋
```

1. **계획**: 로드맵의 다음 작업에 대해 `/ecc:plan "M?-? <작업명>"`. 계획을 사용자에게 짧게 보여 주고, 문서와 다른 결정이 필요하면 먼저 묻는다.
2. **서버 작업**은 테스트 먼저(ECC `tdd-workflow`, `springboot-tdd`). 도메인 규칙 → 서비스 → 컨트롤러 순.
3. **클라이언트 작업**은 실행해서 눈으로 확인. 이펙트는 pixelfx 스킬의 "그리기 → 렌더 → 보기 → 고치기" 순서를 따른다.
4. **검증**: 완료 조건을 하나씩 확인. 서버는 `./gradlew test`, 클라이언트는 `npm run build` + 실행 확인.
5. **리뷰**: `/code-review`. 마일스톤 끝에는 `/security-scan`도.
6. **기록**: 로드맵 체크박스 `[x]`, `docs/PROGRESS.md`에 날짜·한 일·결정·다음 할 일.
7. **커밋**: 작업 하나당 커밋 하나 이상. 메시지는 Conventional Commits + 작업 번호. 예: `feat(dorm): 가구 겹침 검증 추가 [M5-2]`

한 번에 한 작업만 한다. 작업이 너무 크면 하위 작업으로 쪼개 로드맵에 추가하고 사용자에게 알린다.

## 코드 규칙

### 서버 (server/)
- Java 21, 패키지 `com.novarium.<도메인>`, 도메인별 `controller / service / domain / repository / dto`
- 엔티티를 API 응답으로 직접 반환하지 않는다 → DTO(record)
- 보상·확률·수치 계산은 **서버에서만**. 클라이언트가 보낸 결과값을 믿지 않는다
- 시간은 `Clock`, 랜덤은 `RandomProvider`를 주입 (테스트에서 고정)
- 수치는 `game-balance.yml`로 분리, `docs/03-시스템설계.md`와 맞춘다
- 예외는 `BusinessException(ErrorCode)` + `@RestControllerAdvice`로 통일된 에러 응답
- 리소스 접근 시 소유자 확인 필수
- 스키마 변경은 Flyway 마이그레이션 파일로만

### 클라이언트 (client/)
- TypeScript strict
- 씬은 `src/scenes/`, pixelfx 관련 코드는 `src/fx/`에만 (다른 곳에서 pixelfx를 직접 import하지 않는다)
- 서버 호출은 `src/api/`를 통해서만
- 내부 해상도 320×180, 픽셀이 뭉개지지 않게 정수배 확대

## 사용자에게 설명할 때

- 한국어로, 짧게. 백엔드 개념(트랜잭션, 낙관적 락, JWT 등)을 처음 쓸 때는 **왜 그렇게 했는지** 한두 문장으로 설명해 준다 — 사용자가 면접에서 설명할 수 있게.
- 막히면 추측으로 밀어붙이지 말고 무엇이 막혔는지, 선택지가 뭔지 말하고 묻는다.

## 하지 말 것

- 원작 게임의 이름·캐릭터·그림·맵·대사 사용 (`docs/07-IP가이드.md`)
- `.env`, 시크릿, 토큰 커밋
- 로드맵에 없는 큰 기능을 묻지 않고 추가
- 테스트 없이 서버 로직 변경
- 문서와 코드가 어긋난 채로 작업 종료 (규칙을 바꿨으면 문서도 고친다)
