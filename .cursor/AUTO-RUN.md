# Agent 자동 실행 (Run / Always run)

Python이나 MD만으로는 Cursor의 **Run / Always run** 버튼을 대신 누를 수 없습니다.  
아래 설정으로 승인 창을 거의 안 뜨게 만듭니다.

## 1) Cursor UI에서 Run Mode 바꾸기 (필수)

1. `Ctrl + ,` → **Cursor Settings**
2. **Agents** → **Approvals & Execution** (또는 Run Mode)
3. 모드를 **`Run Everything`** 으로 선택

| 모드 | 효과 |
|------|------|
| Run Everything | 명령마다 승인 안 물음 (원하시는 동작) |
| Auto-review | 안전한 건 자동, 애매하면 물어볼 수 있음 |
| Allowlist | 허용 목록만 자동 |

이 프로젝트에는 `.cursor/permissions.json` 이 있어서,  
**Auto-review** 모드여도 git/파일/쉘 작업을 최대한 자동 허용하도록 안내합니다.

## 2) 적용 확인

- Cursor 창 다시 로드: `Ctrl + Shift + P` → `Developer: Reload Window`
- 그다음 에이전트에게 간단한 명령을 시키면 승인 없이 돌아가는지 확인

## 3) 주의

`Run Everything` 은 실수로 위험한 명령도 바로 실행될 수 있습니다.  
이 저장소(홈페이지) 작업용으로만 쓰는 걸 권장합니다.
