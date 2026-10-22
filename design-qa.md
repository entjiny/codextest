# Design QA — J.ESTINA PEOPLE

final result: passed

## 비교 대상과 검증 환경

- Source visual truth: `docs/design/hr-dashboard-01.png` (1487 × 1058 px).
- Implementation: `docs/design/implementation/dashboard-desktop.png` (1487 × 1058 px).
- Desktop CSS viewport: 1487 × 1058, deviceScaleFactor 1, 초기 대시보드/전체 조직/승인 대기 8건.
- Mobile: `docs/design/implementation/dashboard-mobile.png`, CSS viewport 390 × 844, 전체 페이지 캡처. 초기 대시보드/전체 조직.
- 브라우저: 현재 클라우드 실행 머신의 headless Chromium, Playwright. 연결형 cloud browser 및 Sites는 제공되지 않아 해당 UI 미리보기·게시 검증은 하지 않음.
- 밀도 보정: 원본과 데스크톱 캡처 모두 1:1. 최종 비교는 같은 픽셀 크기로 수행함.
- Full-view evidence: `docs/design/implementation/comparison-final.png`, 원본과 구현을 한 이미지에 나란히 배치.
- Focused region evidence: `docs/design/implementation/comparison-detail.png`, 차트 제목·라벨·막대·텍스트를 동일 배율로 비교.

## Findings 및 변경 이력

1. [P2, 수정 완료] 초기 데스크톱의 상단 여백과 표 높이로 하단 정보가 원본보다 아래로 밀림.
   - Evidence: `docs/design/implementation/comparison-initial.png`.
   - Fix: 데스크톱 제목 위 보조 라벨 제거, KPI 높이·세로 여백·매장 표 행 높이 축소.
   - Post-fix: `comparison-final.png`에서 네 지표, 양쪽 현황, 매장 표 5행이 같은 뷰포트에 노출됨.
2. [P2, 수정 완료] 모바일 grid의 기본 최소 너비로 승인 표가 페이지 전체를 가로로 확장함.
   - Fix: section/grid 자식에 `min-width:0` 적용. 표 자체는 내부에서 가로 스크롤.
   - Post-fix: 모바일 캡처와 Playwright의 document.scrollWidth <= viewport 검증 통과. 메뉴 열기 및 다른 화면 이동도 통과.
3. [기능 수정 완료] 모달 입력 때 포커스가 재설정되는 문제를 방지하도록 포커스 관리 effect의 수명을 모달 단위로 고정함.
4. [검증 수정 완료] 초반 테스트의 접근성 이름·중복 버튼 선택자를 실제 DOM에 맞게 수정. 최종 사용자 동작 검증은 전체 통과.

## 필수 시각 검토

- Fonts/typography: 사용자 요청에 따라 원본의 세리프 표현을 포함한 모든 텍스트를 Pretendard 400/500/600/700으로 대체. 브라우저에서 폰트 로드 및 전체 텍스트 요소 computed font-family를 검사해 통과. 숫자·로고·폼·버튼까지 적용. 이미지 속 세리프 로고를 텍스트로 재현하는 대신 사용자 요청에 맞는 비공식 Pretendard 워드마크 사용.
- Spacing/layout: 로즈 사이드바, 4열 KPI, 조직 현황/승인 목록의 양분 배치, 하단 매장 표라는 원본 구조 유지. 모바일은 2열 KPI, 세로 섹션 및 접이식 메뉴. 수평 넘침 없음.
- Colors/tokens: 버건디 #71334b, 딥 버건디 #57243a, 로즈 #f1e3e4, 밝은 아이보리 바탕. 정보 구분은 옅은 행 구분선 중심. 포커스 표시 및 비활성 버튼 구별 가능.
- Image/asset fidelity: 원본에 사진·일러스트 없음. 표준 UI 아이콘은 Phosphor 라이브러리 사용. 왕관은 공식 로고가 아닌 표준 Crown 아이콘이며 브랜드 가이드 확정 전의 제안 표식. 손수 그린 SVG/CSS 이미지 대체 없음.
- Copy/content: 원본의 가상 수치를 유지하되 부족 매장 수와 표가 일치하도록 판교점의 인원을 조정. 오류 가능성이 있는 ‘근무율’을 ‘충족률’로 정리. 주말 반차 예시는 월요일로 수정. 집계 기준과 가상 데이터 안내를 명시. 원본의 장식 문구 일부는 Pretendard에 맞는 간결한 카피로 변경.

## 동작 검증

`npm test` 전체 통과:
- Pretendard 로딩과 전체 텍스트 요소 폰트 검사.
- 휴가 상세/확인 창/승인 후 대기 건수 갱신.
- 반려 사유 미입력 시 확정 비활성화, 입력 후 반려 탭에서 조회.
- 직원 검색, 1개 결과, 빈 결과.
- 필수 확인 메모 작성 후 근태 처리.
- 매장 CSV 다운로드 이벤트 및 파일명.
- 조직 필터 선택 시 KPI 갱신.
- 모바일 문서 가로 넘침 없음, 메뉴 열기/직원 화면 이동.
- 브라우저 pageerror 및 console error 없음.

`npm run build` 통과. 서버와 DB·실제 인증은 검증 범위에 포함하지 않음.

## Implementation Checklist

- [x] 선택된 1번 시안의 주요 구조와 색상 반영.
- [x] Pretendard 자체 제공 및 전체 UI 적용.
- [x] 핵심 입력·탐색·처리 흐름 구현.
- [x] 모바일 레이아웃 및 키보드 모달 포커스/Escape 처리.
- [x] 초기/최종 시각 비교 및 기능 검증.

## Follow-up Polish / Limitations

- [P3] 공식 브랜드 로고·가이드 확보 후 Crown 제안 표식을 교체할 수 있음.
- [P3] 좁은 모바일 표는 가로 스크롤로 나머지 열을 조회함. 추후 모바일 전용 요약 행 UI 검토 가능.
- 실제 인증·RLS·DB 저장·근무표·휴가 원장 연동 전인 프론트엔드 프로토타입. 새로고침하면 처리 상태가 초기화됨.
