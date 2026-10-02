# Mister World Employee Console — EC-B01 기본 실행 환경 작업서

작성일: 2026-10-03. 담당: AI-Console Owner / 직원 Frontend. 형식: Markdown(.md). 상태: EC-B01 기본 실행 환경 구현 및 필수 검증 완료. 공통 계약을 새로 승인하는 문서가 아니다.

## 1. 역할과 이번 작업

직원 Console을 기능별로 개발하기 위한 첫 실행 기반을 만든다. 목표는 브라우저 실행, React 화면 구조, 라우팅(주소에 따라 화면을 선택하는 기능), 공통 UI, 개발 환경 구분, 자동 검사와 테스트다. 로그인·상품·재고 기능 전체를 한 번에 구현하지 않는다.

관련 승인 요구사항: [FR-01, FR-10, FR-11](https://github.com/WonhoOne/docs/blob/main/requirements/requirements.md). 이번 작업은 이 기능들을 위한 실행 기반이며 해당 요구사항의 전체 완료를 뜻하지 않는다. 업무 API 호출은 이번 범위에 없다.

## 2. 승인 기준과 읽기 순서

공통 SSOT는 [WonhoOne/docs/main](https://github.com/WonhoOne/docs)이며 확인 기준은 Baseline v0.2 / commit `cad8daed210cfb60078f24cabe14c2f383f3ef65`다. 2026-10-02에 승인 문서와 필수 요구·도메인·규칙·NFR·구조·API·ERD·기여 절차를 읽었고, 2026-10-03 main SHA가 동일함을 재확인했다. 이후 작업 시작 전 최신 main을 다시 확인한다. 미병합 PR은 제안이다.

먼저 승인 Baseline과 중앙 계약을 읽고, 직원 Console 내부 기획과 이 작업서를 읽는다. 공통 DTO·API·규칙은 중앙 문서를 링크해 참조하며 별도 사본으로 관리하지 않는다.

## 3. 문서 위치와 GitHub 반영

직원 내부 작업서는 `WonhoOne/ai-console/docs/employee-console-foundation-work-order.md`에 둔다. Markdown은 GitHub가 바로 읽기 좋게 표시하고 변경 전후를 비교할 수 있는 텍스트 형식이다. Word/PDF 제출본은 이번 작업에 필요하지 않다.

GitHub 업로드가 앱 실행의 기술적 필수조건은 아니다. 팀 공유와 개발 이력 추적을 위해 작업서를 저장소에 두는 것이 적절하며 사용자가 이번 작업서 업로드를 요청했다. 문서만 `codex/employee-console-foundation-work-order` 브랜치와 검토용 PR에 올린다. main 직접 변경·자동 병합·검토자 지정·고객 Frontend/Backend 변경은 하지 않는다. 구현 코드는 이번 업로드 범위에 포함하지 않는다. 후속 코드 업로드는 별도 사용자 지시와 팀 Issue/Branch/PR/Test 절차를 따른다.

고객 Frontend도 [구현 작업서 CP4](https://github.com/WonhoOne/frontend/blob/main/docs/implementation/checkpoints/CP4-FOUNDATION-IMPLEMENTATION-PLAN.md), [구현 Master Plan](https://github.com/WonhoOne/frontend/blob/main/docs/implementation/IMPLEMENTATION-MASTER-PLAN.md), [문서 안내](https://github.com/WonhoOne/frontend/blob/main/docs/README.md)를 main에 보관한다. 문서 공개 여부와 형식을 참고했으며 고객 코드를 복사하거나 수정하지 않는다.

## 4. 포함·제외 범위

포함: 로컬 `employee-console/`, React/TypeScript/Vite/npm, 버전 고정 lockfile, Router와 Query Provider, 로그인 준비 화면과 메뉴, 보호 주소의 로그인 이동, 잘못된 주소 안내, 공통 버튼/상태 안내, 반응형 기본 배치, 개발 전용 미리보기, 설정 오류 화면, Vitest/RTL/Playwright, ESLint/Prettier, 실행 README.

제외: 실제 로그인 및 토큰 저장, 상품 조회/입력/저장, 재고 조회/추가, 업무 복구, Voice, Backend 호출, DB, 계정 생성, 공통 계약 수정, 배포. 기능이 아직 없으면 준비 중으로 표시하고 성공한 것처럼 보이지 않는다.

## 5. 기술·구조와 데이터 경계

`src/app`은 시작 설정·메뉴·라우팅, `src/shared/ui`는 업무와 무관한 공통 UI, `src/mocks`는 개발 전용 기반 표시, `src/test`와 `tests/e2e`는 자동 검증을 담당한다. 기능이 생길 때 features/pages/integrations 경계를 추가하고 빈 폴더를 미리 늘리지 않는다. 핵심 코드에는 친절한 한국어 역할·이유·실패 처리 주석을 단다.

Node 24.19.0으로 실행 호환성을 확인한다. 고객의 최소 Node 버전이나 의존성 목록을 그대로 복제하지 않는다. 이 PC에는 전역 npm이 없어 이미 준비된 npm 캐시를 scripts/run.ps1로 현재 터미널에서만 연결한다. npm 도구의 번들 의존성은 앱 의존성에서 제외한다. 전역 설정은 바꾸지 않는다.

개발 기본 모드는 mock, 운영은 backend만 허용한다. 개발 전용 모듈은 DEV 조건에서 동적 import한다. 서버 origin은 형식을 검사하고 비밀값/경로/query가 섞인 주소를 거절한다. 설정 오류가 있으면 Mock으로 대체하지 않고 안내한다. URL/CORS 미정인 실제 서버로 요청하지 않는다. 5174는 임시 Mock 로컬 포트다.

확정된 후속 인증 정책: 같은 탭에서 새로고침 후 유효 로그인 유지, 미저장 상품 입력 폐기. sessionStorage에 인증값/최초 만료 시각만 보관하고 draft는 메모리에 둔다. 최초 만료 시각을 새로고침으로 늘리지 않는다. 이번 EC-B01은 해당 인증 기능을 구현하지 않으며 EC-B02에서 검증한다.

## 6. 실행 절차와 검증 기준

1. Node 확인 → 의존성 설치 및 lockfile 확인.
2. 로컬 개발 서버 실행 → 로그인 준비 화면 확인.
3. 보호 주소 `/products`, `/products/new`, `/products/:productId/edit`, `/inventory` 직접 진입이 로그인으로 이동하는지 확인.
4. 개발용 화면 미리보기와 실제 기능이 구별되는지 확인. 새로고침 시 미리보기 선택은 초기화한다.
5. 타입 검사·ESLint·Prettier·Vitest·운영 빌드 수행.
6. Playwright로 직접 주소·새로고침·메뉴 이동·320px 가로 넘침·잘못된 주소 검증.
7. 운영 번들에 개발 Mock 모듈과 개발 안내 문자열이 포함되지 않는지 검사. 운영 Mock/서버 주소 누락/잘못된 origin 거절 테스트.

README의 npm 또는 scripts/run.ps1 명령으로 재현한다. 통과한 검사와 실행하지 않은 검사를 구분해 결과를 기록한다. Backend 연결 성공·인증 안전성 전체·상품/재고 요구사항 완료로 확대해 보고하지 않는다.

## 7. 완료 조건과 남은 사항

완료: 소스·실행 설명·고정 의존성 준비, 필수 자동 검사 및 실행 기반 브라우저 흐름 통과, 업무 기능 미구현 표시, 공통 계약/API 추가 없음, 다른 파트 변경 없음.

실제 연동 전 필요: API 가용 시점, 서버 URL/CORS, 직원 테스트 계정. 폼 구현/연동에서 계속 추적할 항목: 문자열 제한·금액/추가량 상한·동시 수정 정책. TBD를 임의로 공식 규칙으로 만들지 않는다. API 일정 질문은 기존 [ai-console#4](https://github.com/WonhoOne/ai-console/issues/4)를 참고하며 이 문서 게시가 질문 해결을 의미하지 않는다.

## 8. 작업 결과와 다음 작업

로컬 파일 작성과 패키지 설치 완료. 타입 검사·ESLint·Prettier·Vitest 6개·운영 빌드 통과. 운영 dist에서 개발 Mock module/안내 marker가 발견되지 않음을 확인했다. Chromium 브라우저 시나리오 1개는 보호 주소·메뉴·새로고침·320px·잘못된 주소 검증을 통과했다. 브라우저 테스트는 Vite 직접 실행으로 구성했다. 격리 환경에서 프로세스 종료가 지연되어 일반 실행 환경에서 재검증했고 정상 종료 코드 0 / 1 passed를 확인했다. 최종 npm audit 취약점 0건을 확인했다. 보조 npm 패키지에서 발견된 3건은 해당 패키지를 앱 의존성에서 제거해 해소했다. 1280px/320px 스크린샷을 열어 기본 배치도 확인했다. 실제 로그인/업무 테스트는 미실행이며 Backend 호출은 없다.

다음 작업은 EC-B02 직원 인증이다. 로그인 응답 검증·권한·sessionStorage 복구·만료/로그아웃·이전 세션 응답 차단을 먼저 구현한 뒤 상품 조회, 상품 폼, 재고 순으로 진행한다.