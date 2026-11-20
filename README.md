# 노바리움 마법학교 (Novarium Academy)

옛 마법학교 웹게임에서 영감을 받은 **오리지널** 2D 픽셀 웹게임.
원소 마을 출신 신입생이 되어 마법을 배우고, 기숙사 방을 꾸미고, 펫을 키운다.

> 🚧 개발 중 — 진행 상황은 [docs/PROGRESS.md](docs/PROGRESS.md)

## 기술 스택

| 영역 | 기술 |
|---|---|
| Client | Vite · TypeScript · Phaser · [pixelfx](https://github.com/patch-gg/pixelfx) |
| Server | Java 21 · Spring Boot · Spring Data JPA · Spring Security(JWT) · Flyway |
| DB | H2(로컬) · MySQL |
| 개발 도구 | Claude Code + [ECC](https://github.com/affaan-m/ECC) + pixelfx skill |

## 문서

| 문서 | 내용 |
|---|---|
| [기획서](docs/01-기획서.md) | 컨셉, 핵심 재미, MVP 범위 |
| [세계관](docs/02-세계관.md) | 학교·마을·NPC·펫 설정 |
| [시스템 설계](docs/03-시스템설계.md) | 마법·펫·기숙사·상점·퀘스트 규칙과 수치 |
| [기술 설계](docs/04-기술설계.md) | 구조, API, ERD, 테스트·보안 |
| [개발 로드맵](docs/05-개발로드맵.md) | 마일스톤 M0~M7 |
| [이펙트 매핑](docs/06-이펙트매핑.md) | 게임 상황 ↔ pixelfx 이펙트 |
| [IP 가이드](docs/07-IP가이드.md) | 오마주 범위와 금지 사항 |

## Claude Code로 개발 시작하기

**1. 저장소 받기**
```bash
git clone https://github.com/<계정>/novarium.git
cd novarium
claude
```

**2. 플러그인 설치 (Claude Code 안에서, 처음 한 번)**
```
/plugin marketplace add patch-gg/pixelfx
/plugin install pixelfx@pixelfx
/plugin marketplace add https://github.com/affaan-m/ECC
/plugin install ecc@ecc
```
설치 후 Claude Code를 다시 시작한다.

**3. 진행**
```
/next      # 로드맵의 다음 작업을 한 사이클 진행
/status    # 진행 상황 요약
```

Claude Code는 `CLAUDE.md` → `docs/PROGRESS.md` → `docs/05-개발로드맵.md` 순으로 읽고 작업한다.

## 라이선스

코드: 추후 결정 · pixelfx 이펙트: MIT (patch-gg)
