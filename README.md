# Eddie's Forge

### Eddie의 도구 공방

**내 기기와 앱을 하나의 작업 흐름으로.**

Eddie's Forge는 기기와 앱 사이의 단절을 줄이는 오픈소스 도구 모음입니다.
첫 번째 목표는 Tailscale 기반 클립보드 동기화와 로그인·비밀번호 동기화입니다.

> 현재 상태: 기능 명세와 기술 검증 계획만 있는 초기 프로젝트입니다.
> 실행 가능한 동기화 도구는 아직 없습니다.

## 도구

| 도구 | 목표 | 상태 |
| --- | --- | --- |
| Clipboard Bridge | Tailscale로 여러 기기의 Raycast 클립보드 기록 동기화 | 설계 · 연동 검증 전 |
| Credential Bridge | Aside · Chrome · iCloud 키체인의 마지막 사용자 변경을 감지해 로그인·비밀번호 동기화 | 설계 · 연동 검증 전 |

### Clipboard Bridge

- 같은 tailnet에 연결된 승인된 기기 사이에서 동기화합니다.
- 새로 복사한 내용과 기존 Raycast 기록의 이전을 별도 기능으로 다룹니다.
- 오프라인 변경을 재연결 후 전달하고 중복·동기화 루프를 방지합니다.
- 텍스트부터 검증하고 이미지·파일·서식·고정 항목은 지원 가능성을 따로 확인합니다.
- 민감한 클립보드 표시, 제외 앱, 보관 기간, 삭제 전파를 설계에 포함합니다.

Raycast의 공식 Cloud Sync는 Clipboard History를 동기화하지 않습니다.
공식 Clipboard API의 기록 접근 범위도 offset 0–5로 제한됩니다.
전체 기록을 읽고 다른 기기의 Raycast 기록에 보존하는 경로는 아직 검증하지 않았습니다.
현재 클립보드 전달만으로 전체 기록 동기화를 달성했다고 간주하지 않습니다.

### Credential Bridge

- Aside Vault, Chrome의 저장된 로그인, iCloud 키체인을 대상으로 합니다.
- 파일 수정 시각이 아닌 실제 항목 변경을 감지하는 것을 목표로 합니다.
- 동기화로 생긴 변경과 사용자가 직접 수정한 변경을 구분합니다.
- 순서가 확인되는 사용자 변경은 자동 반영하고, 동시 수정은 두 버전을 보존해 선택받습니다.
- 비밀번호를 클립보드나 AI 모델에 보내지 않고 처리하는 것을 설계 원칙으로 삼습니다.

Apple의 iCloud Passwords 확장은 Chrome에서 iCloud 키체인 비밀번호 자동 완성과
새 비밀번호 저장을 지원합니다. 이것이 Chrome 자체 저장소와의 전체 병합을 뜻하지는 않습니다.
Aside는 가져오기·내보내기를 제공하지만 세 저장소의 무인 양방향 동기화는 아직 검증하지 않았습니다.

## 개발 순서

1. 각 앱의 지원 인터페이스와 읽기·쓰기·변경 감지 가능 여부 검증
2. Tailscale 기반 두 기기 간 텍스트 클립보드 전달 실험
3. Raycast 전체 기록 연동 가능성 판정 및 지원 범위 공개
4. 비밀번호 연동 실험과 충돌·삭제·복구 검증
5. 실제 다중 기기 검증 후 설치 가능한 버전 제공

초기 검증 대상은 macOS입니다. 다른 운영체제 지원 여부는 아직 정하지 않았습니다.
북마크·탭·방문 기록·쿠키·로그인 세션 복제는 현재 범위에 포함하지 않습니다.
패스키·TOTP·인증서·일반 키체인 항목도 초기 비밀번호 동기화 범위에서 제외합니다.

## 문서

- [기능 명세와 검증 기준](docs/design.md)
- [기여 안내](CONTRIBUTING.md)
- [보안 정책](SECURITY.md)
- [MIT 라이선스](LICENSE)

## 참고 자료

2026-10-21에 공식 문서를 확인했습니다. 앱 버전별 실제 동작은 별도 검증 대상입니다.

- [Raycast Cloud Sync](https://manual.raycast.com/cloud-sync)
- [Raycast Clipboard API](https://developers.raycast.com/api-reference/clipboard)
- [Tailscale encryption](https://tailscale.com/kb/1504/encryption)
- [Aside Password Manager](https://docs.aside.com/help/password-manager)
- [Aside 가져오기·내보내기](https://docs.aside.com/help/passwords)
- [Apple iCloud Passwords](https://chromewebstore.google.com/detail/icloud-passwords/pejdijmoenmkgeppbflobdenhhabjlaj)

Raycast, Tailscale, Aside, Google, Apple의 공식 프로젝트가 아닙니다.
