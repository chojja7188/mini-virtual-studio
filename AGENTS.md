# AGENTS.md

Unity 6 기반 **미니 버추얼 스튜디오** 프로젝트. VRM 아바타 연출, 실시간 멀티 유저 동기화, 외부 이벤트 연동, 방송 송출까지 다룬다.
Unity 프로젝트는 `MiniVirtualStudio/`에 있다.

> 개인 맥락(경력, 목표, 로드맵)은 `AGENTS.local.md`에 있다. 이 파일은 git에 올리지 않는다. 있으면 함께 읽는다.

## 개발 환경

- 장비: 맥북(Apple Silicon). Windows 빌드는 맥에서 Mono 백엔드로 만든다.

## 포트폴리오 프로젝트: 미니 버추얼 스튜디오

| 모듈 | 내용 | 결과물 |
|---|---|---|
| M1 | C#·Unity 기본기 | 컨트롤 패널 프로토타입 (ScriptableObject + uGUI) |
| M2 | 아바타·모션·카메라 | v1: VRM 로드, 표정 5종, 이모트 3종, Cinemachine 카메라 3대 |
| M3 | 실시간 네트워크 | v2: 2~4명 멀티 유저, 표정=상태 동기화, 이모트=RPC |
| M4 | 크리에이터 인터랙션 | v3: Node WebSocket 이벤트 → 리액션 큐 → OBS 투명 송출(Syphon) |
| M5 | 안정화·제품화 | v1.0: Profiler 개선, 풀링, 재접속, 설정 저장, GameCI, 테스트 |

## 기술 선택 (바꾸기 전에 먼저 물어볼 것)

- Unity 6 LTS, URP (Universal 3D 템플릿)
- 아바타: UniVRM (VRM 1.0), 테스트 아바타는 VRoid Studio로 직접 제작
- 애니메이션: Animator(Mecanim), Animation Rigging, 모션은 Mixamo
- 카메라: Cinemachine 3.x
- 입력: Input System
- 비동기: `Awaitable` + `destroyCancellationToken` (UniTask는 필요할 때만)
- 네트워크: **Photon Fusion 2 Shared Mode**(메인), Netcode for GameObjects(비교용 맛보기)
- 외부 이벤트: NativeWebSocket + Node `ws` 서버
- 송출: KlakSyphon(맥), KlakSpout/KlakNDI(윈도우·공용)
- CI: GitHub Actions + GameCI

## 코드 규칙

- 로직은 가능하면 MonoBehaviour 밖의 순수 C# 클래스로 분리한다 (Edit Mode 테스트 가능하게).
- 매 프레임 실행되는 코드(Update 등)에서 할당 금지: 문자열 결합, LINQ, `new`, 박싱, 반복 `GetComponent`.
- 이벤트는 `OnEnable`에서 구독하고 `OnDisable`에서 해제한다.
- async 루프에는 항상 `destroyCancellationToken`을 넘긴다.
- Animator 파라미터는 `Animator.StringToHash`로 캐싱한다.
- 네트워크 값은 "늦게 들어온 사람도 알아야 하나?" 기준으로 상태(Networked/NetworkVariable)와 RPC를 구분한다. 쓰기 권한은 값마다 한 곳만 둔다.
- 로그에는 태그를 붙인다: `[Net]`, `[Avatar]`, `[Event]`, `[Settings]`.
- 실제 크리에이터의 아바타나 저작물은 쓰지 않는다. 제페토 모델 추출물도 쓰지 않는다.

## 작업 방식

- 코드만 주지 말고 **왜 그렇게 하는지 짧게 설명**한다.
- Unity 에디터 작업(오브젝트 배치, Inspector 연결, 패키지 설치)이 필요하면 메뉴 경로까지 단계별로 알려 달라. 너는 에디터 화면을 볼 수 없으니, 필요하면 Console 에러나 스크린샷을 달라고 요청한다.
- 한 번에 큰 변경을 하지 말고 모듈 단위로 작게 나눠서 진행하고, 끝날 때마다 무엇을 확인하면 되는지 알려 달라.
- 커밋 메시지는 한국어로, 모듈 번호를 앞에 붙인다. 예: `M2: 표정 전환 컨트롤러 추가`
- 트러블슈팅 과정은 `docs/troubleshooting/`에 짧은 글로 남겨 달라. 포트폴리오 글감으로 쓴다.
- 여러 세션에 걸친 작업은 `docs/exec-plans/active/`에 계획을 쓰고 진행하면서 갱신한다. 세션을 시작하면 먼저 거기를 읽는다. 형식은 `docs/exec-plans/README.md`.
- 되돌리기 어려운 결정(기술 선택 변경, 동기화 설계 등)은 `docs/adr/`에 ADR로 남긴다. 형식은 `docs/adr/README.md`.
- 패키지 API가 버전마다 다를 수 있으니, 확실하지 않으면 공식 문서를 확인하거나 나에게 설치된 버전을 물어본다.
- 공개 저장소다. 개인 정보(이름, 경력, 지원처, 이메일)는 커밋하는 파일에 쓰지 않고 `AGENTS.local.md`에만 둔다.
