# Demon Slayer Recreation

DirectX11 기반 6인 팀 프로젝트로, 원작 전투 연출을 재현하는 과정에서 **전투 카메라**, **각성 컷신**, **미니맵 UI**를 구현했습니다.  
포트폴리오 문서 기준으로 제 핵심 기여는 Combat Camera System, Awakening Cutscene System, Minimap UI System입니다.

## Project Info
- **Engine**: DirectX11
- **Development Period**: 60 Days
- **Team Size**: 6
- **Role**: Gameplay / Camera / UI System Programming

## My Core Contributions
- Combat Camera System
- Awakening Cutscene System
- Minimap UI System

---

## 1. Combat Camera System

전투 상황에 따라 카메라 시점을 동적으로 전환하는 시스템입니다.

### Feature
- 플레이어와 적의 전투 구도를 더 명확하게 보여주기 위한 전투 카메라
- 일반 전투 시 기본 Battle View 유지
- 콤보 및 근거리 전투 상황에서는 Side View로 전환

### How it Works
- 플레이어-적 전투축을 기준으로 카메라 방향을 계산
- 플레이어와 적의 중심점인 **BattleCenter**를 바라보도록 시선을 처리
- Combo 및 근거리 전투 상황에서만 Side Camera로 전환
- 카메라 위치와 시선 방향을 **Lerp 기반 보간**으로 갱신해 급격한 화면 변화 완화

### Result
- Player-Enemy 중심 프레이밍으로 전투 대상의 가시성을 확보
- Combo 구간에서 공격 흐름이 더 잘 보이도록 연출 강화
- 카메라 이동과 회전이 안정적으로 유지되도록 개선

### Troubleshooting
- 특정 돌진 스킬 사용 시 플레이어가 적을 관통하면서 전투축 방향이 반전되어, Side Camera가 반대 방향으로 급회전하는 문제가 있었습니다.
- 이를 해결하기 위해 외적 계산에 사용되는 전투축 부호를 별도 방향값으로 보정하여, 스킬 사용 이후에도 의도한 방향의 Side Camera를 유지하도록 수정했습니다.

---

## 2. Awakening Cutscene System

각성 연출을 다중 카메라 컷 시퀀스로 구성한 시스템입니다.

### Feature
- 캐릭터별 각성 연출을 여러 컷으로 나누어 재생
- 카메라 연산과 컷 시퀀스 제어를 분리한 구조
- 데이터 기반으로 컷 연출 확장 가능

### How it Works
- 컷 데이터 구조체에 **각도 / 거리 / 오프셋 / 지속시간** 등을 정의
- `CutInFinish()`에서 단일 컷 카메라 연산 수행
- `CutInCamera()`에서 컷 시퀀스 재생과 전환을 담당
- 마지막 컷 종료 후 전투 카메라 상태로 복귀

### Design Point
- 연출 차이를 if/else 분기보다 **데이터 조합**으로 제어할 수 있도록 설계
- 카메라 계산 로직과 컷 진행 로직을 분리해 유지보수성과 확장성을 높임

### Result
- 캐릭터별 컷 연출을 코드 수정 없이 데이터만으로 확장 가능
- 컷신 종료 후 전투 흐름으로 자연스럽게 복귀

---

## 3. Minimap UI System

플레이어의 현재 위치와 진행 방향을 확인할 수 있는 동적 미니맵 UI 시스템입니다.

### Feature
- 플레이어 중심의 주변 영역만 보여주는 미니맵
- 위치와 방향을 즉시 파악할 수 있는 UI 제공

### How it Works
- 플레이어의 월드 좌표를 맵 기준 **UV 좌표**로 정규화
- 정규화된 UV 중심값을 기준으로 미니맵 표시 범위를 계산
- 중심 좌표가 가장자리에 가까울 경우 clamp 처리
- 셰이더에서 범위 밖 픽셀을 alpha cut 하여 표시 영역만 렌더링
- 플레이어 Look 벡터를 이용해 아이콘 회전 적용

### Design Point
- CPU에서 UV 중심 좌표를 계산하고
- GPU 셰이더에서 표시 범위를 잘라내는 방식으로 동적 미니맵 구현

### Result
- 전체 맵 대신 주변 영역만 표시해 가독성을 높임
- 플레이어 위치와 진행 방향을 빠르게 인식할 수 있어 탐색 편의성 향상

---

## Tech Stack
- C++
- DirectX11
- Camera System
- UI System
- Gameplay Programming
- HLSL

## What I Focused On
이 프로젝트에서는 단순한 기능 구현보다,  
**전투 가시성**, **연출 전환 구조**, **좌표계 기반 UI 처리**를 시스템 단위로 설계하는 데 집중했습니다.

## Demo / Portfolio
- Portfolio PDF 기반 정리
- 시연 영상 및 세부 코드 설명은 별도 첨부 예정
