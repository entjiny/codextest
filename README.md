# J.ESTINA PEOPLE

제이에스티나 HR 대시보드의 인터랙티브 프론트엔드 프로토타입입니다. [선택된 1번 시안](docs/design/hr-dashboard-01.png)을 바탕으로 구성했으며 **로고, 제목, 숫자, 입력 요소까지 모든 텍스트에 Pretendard**를 적용했습니다. 폰트는 npm 패키지에 포함해 자체 제공하며, 외부 폰트 CDN은 사용하지 않습니다.

## 화면 미리보기

![구현된 대시보드](docs/design/implementation/dashboard-desktop.png)

[모바일 화면](docs/design/implementation/dashboard-mobile.png) · [원본 시안](docs/design/README.md) · [PRD](docs/JESTINA_HR_Dashboard_PRD.md) · [디자인 검증](design-qa.md)

## 실행

Node.js 22 이상을 권장합니다. 현재 환경에서는 Node.js 24로 검증했습니다.

```bash
npm ci
npm run dev -- --host 0.0.0.0 --port 4173
```

배포용 빌드는 `npm run build`이며 결과는 `dist/`입니다. React + Vite로 구성했습니다. 별도 환경 변수나 API 키는 필요하지 않습니다.

## 체험할 수 있는 동작

- 사이드바 탐색 및 모바일 메뉴
- 조직 필터와 지표·직원 목록 연결
- 휴가 상세, 승인 확인, 사유 필수 반려, 상태별 탭과 대기 건수 갱신
- 직원 이름·사번 검색, 상세 조회, 검색 결과 없음
- 근태 메모 입력 후 확인 처리 및 미처리 건수 갱신
- 매장 인력 상세와 CSV 다운로드
- 알림 메뉴, 사용자 정보, 키보드 모달 탐색과 Escape 닫기

## 검증

개발 서버가 실행 중인 상태에서:

```bash
npm test
```

Playwright와 `/usr/bin/chromium`을 사용합니다. 다른 경로라면 `TEST_BROWSER`로 지정할 수 있으며 서버 주소는 `TEST_URL`로 지정합니다. 테스트는 폰트, 승인·반려, 검색, 근태 확인, CSV 다운로드, 조직 필터, 모바일 탐색·화면 넘침, 브라우저 오류를 검사합니다.

## 범위

가상 데이터 기반이며 실제 제이에스티나 서비스나 공식 브랜드 가이드가 아닙니다. 재직 인원 248명은 집계 예시이고 직원 디렉터리는 9명만 제공합니다. 데이터 갱신 시각도 데모 기준 시각입니다. 변경 사항은 현재 브라우저 세션에서만 유지되며 새로고침하면 초기화됩니다. 실제 인증, 권한 제어, DB 저장, 휴가 차감, 근무표 연동은 연결하지 않았습니다. HR 운영에 사용하기 전에 백엔드와 권한 검증이 필요합니다.
