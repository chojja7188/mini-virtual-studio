# M1: C#·Unity 기본기

- 상태: 진행 중
- 시작: 2026-11-24

## 목표
ScriptableObject + uGUI로 만든 컨트롤 패널 프로토타입. 그 과정에서 C# 기초, Transform/Quaternion, 이벤트, 프레임 루프를 익힌다.

## 범위 밖
아바타(VRM), 네트워크, 외부 이벤트. 각각 M2~M4에서 한다.

## 단계
- [x] 1. Unity Hub로 Unity 6.3 LTS(`6000.3.29f1`) 설치, Windows Build Support (Mono) 모듈 추가
- [x] 2. `MiniVirtualStudio/`에 3D URP 템플릿으로 프로젝트 생성, 템플릿 안내 파일(TutorialInfo, Readme) 삭제
- [x] 3. git 초기화, Unity `.gitignore`, GitHub public 저장소 push
- [x] 4. 코드 편집기 Rider 설치, Unity External Script Editor = Rider, Assets → Open C# Project 확인
- [ ] 5. Rider 라이선스가 Non-commercial인지 확인 (처음엔 30일 평가판으로 시작됐음)
- [ ] 6. 에디터 화면 익히기: 큐브 만들고 Transform 단축키(W/E/R), Play 중 값 변경
- [ ] 7. 첫 C# 스크립트: 큐브를 회전시키는 MonoBehaviour (Update, Time.deltaTime, [SerializeField])
- [ ] 8. C# 차이점: struct(값 타입), property, ref/out
- [ ] 9. 3D 기본기: 피벗 공전 카메라, LookAt + Slerp
- [ ] 10. event/Action으로 컴포넌트 간 통신 (OnEnable 구독 / OnDisable 해제)
- [ ] 11. ScriptableObject 설정 에셋 + uGUI 컨트롤 패널 (M1 결과물)

## 진행 로그
- 2026-11-24: Unity 6.6으로 처음 만들었다가 LTS가 아니어서 6.3 LTS로 재생성(ADR 0001). 6.6 에디터 삭제. Rider 설치·연결까지 완료. 다음은 5번부터.

## 결정 사항
- Unity 6.3 LTS 고정 (ADR 0001)
- 코드 편집기: Rider (맥에서 Visual Studio 사용 불가, Unity 연동·정적 분석이 기본 내장)
- git 루트는 저장소 최상위, Unity 프로젝트는 `MiniVirtualStudio/` 하위. Unity용 `.gitignore`는 `MiniVirtualStudio/.gitignore`
- 개인 정보는 `*.local.*` 파일에만 두고 git에서 제외
