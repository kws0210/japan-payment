# 일본 결제 도우미

일본에서 결제할 때 보유 카드·쿠폰과 남은 혜택을 비교하는 모바일 웹 계산기다.

[계산기 열기](https://kws0210.github.io/japan-payment/) · [미리 할 작업](https://kws0210.github.io/japan-payment/prepare/) · [계산 근거](https://kws0210.github.io/japan-payment/basis/)

## 기능

- 돈키호테·간사이공항·그 외 매장의 결제수단을 비교한다.
- 보유 카드·쿠폰·행사 등록·예상 이용료와 잔여 횟수를 자동 반영한다.
- 오사카 돈키호테 46개 지점을 검색하고 지점별 행사 적용 여부를 판단한다.
- 전화번호별 설정과 사용 횟수를 Firebase에 저장하고, 로그아웃 뒤 같은 번호로 복원한다.
- 환율을 자동 조회하며 첫 제공처가 실패하면 보조 제공처로 재시도한다.

## 구성

| 경로 | 내용 |
| --- | --- |
| [japan-payment/index.html](japan-payment/index.html) | 계산기와 Firebase 웹 클라이언트 |
| [japan-payment/prepare/](japan-payment/prepare/) | 준비 가이드와 화면 이미지 |
| [japan-payment/basis/](japan-payment/basis/) | 할인·수수료·점포 판정 근거 |
| [_japan-payment-firebase/](<_japan-payment-firebase/>) | Firestore 규칙, 테스트, 운영 문서 |
| [docs/CURRENT.md](docs/CURRENT.md) | 현재 결정 사항, 구현 규칙, 확인이 필요한 조건 |
| [docs/HISTORY.md](docs/HISTORY.md) | 개발·조사·변경·배포 검증의 상세 이력 |

## 프로젝트 기록

작업을 이어갈 때는 [현재 현황과 결정 사항](docs/CURRENT.md)을 먼저 읽는다.
[상세 기록](docs/HISTORY.md)에는 과거 가정과 정정·폐기 이유도 보존한다.
쿠폰·수수료·점포 조사 문서는 현재 현황에서 바로 찾을 수 있다.
기록의 개인 경로·대화 식별자는 공개본에서 제외했다.

## 로컬 실행

저장소 루트에서 실행한다.

```sh
python3 -m http.server 8000
```

브라우저에서 <http://localhost:8000/japan-payment/>를 연다.
현재 HTML은 운영 Firebase 프로젝트 `kws0210-japan-payment`에 연결된다.
결제 계산과 횟수 조정은 입력한 번호의 운영 기록에 반영된다.

## 테스트

Node.js 22에서 추천·환율 테스트를 실행한다.
이 두 테스트는 외부 패키지 설치 없이 실행할 수 있다.

```sh
cd _japan-payment-firebase
npm run test:recommendation
npm run test:rates
```

GitHub Actions도 같은 테스트를 실행한다.
Firestore 규칙의 에뮬레이터 검증과 배포 절차는 [Firebase 운영 문서](_japan-payment-firebase/README.md)를 따른다.

## 운영 기준

문자 인증 없이 전화번호만 입력하는 방식이며, 번호를 아는 사람은 같은 기록을 조회·변경할 수 있다.
브라우저는 로그인 정보를 유지하고 설정·사용 기록은 서버에 저장한다.
KB 노리 체크(UnionPay)는 예시 상품 기준이며, 트래블월렛은 충전한 엔화 잔액 결제 기준이다.
카카오페이 앱 환율에 포함된 미확인 비용과 실제 결제수단별 환율 차이는 확정 원화 지출로 간주하지 않는다.

이 저장소에서 프로젝트 코드를 관리하고, 공개 서비스는 [kws0210.github.io](https://github.com/kws0210/kws0210.github.io)의 `japan-payment/`에서 제공한다.
최초 코드는 기존 저장소의 [bfa0255](https://github.com/kws0210/kws0210.github.io/commit/bfa025515c2c28ae7bbdc9539c3ff3ff8873b68f) 버전에서 가져왔다.
