# 폰으로 Claude Code 부리기 — 설정 3가지

PC에서 돌리던 Claude Code 작업을 스마트폰(Claude 앱)에서 이어받아 지시·확인하기 위한 설정입니다.
1·2번은 **PC의 Claude 데스크톱 앱**에서 켜는 토글이고, 3번은 권한 확인을 줄이는 설정입니다.

## 1. 리모트 컨트롤 켜기

PC의 Claude 데스크톱 앱 → **Settings → Claude Code → Remote Control** 을 켭니다.

- 이 PC에서 돌던 세션을 휴대폰의 Claude 앱(Code 탭)에서 그대로 보고 제어할 수 있습니다.
- 데스크톱 앱 대신 터미널에서 쓰려면, 작업할 폴더(예: 이 저장소)에서 `claude remote-control` 을 실행합니다.
- PC의 앱(또는 위 명령)이 켜져 있어야 휴대폰에서 연결됩니다.

## 2. 휴대폰·claude.ai에서 세션 시작 허용

같은 화면(Remote Control 바로 아래)의 **세션 시작 허용** 설정을 켭니다.

- 휴대폰이나 claude.ai에서 *새* 세션을 이 PC의 폴더로 시작할 수 있게 됩니다.
- 이 저장소처럼 실제 작업할 폴더만 목록에 추가하세요. 홈 디렉터리 전체는 공유하지 않는 편이 안전합니다.

## 3. 권한 확인 줄이기 (영상의 "권한 묻지 않는 모드")

세션의 모드 선택(입력창 옆 드롭다운)에서 **Auto** 또는 **Accept edits** 를 고릅니다.

- Accept edits: 파일 수정은 묻지 않고 진행, 그 외 명령은 확인.
- Auto: 대부분의 도구 호출을 묻지 않고 진행 (조직 설정·모델에 따라 보이지 않을 수 있음).

특정 명령만 항상 허용하고 싶다면 저장소에 `.claude/settings.json` 을 만들어 커밋합니다.
이 프로젝트 기준 권장 예시 (테스트·읽기 전용 git·상태 조회만 허용, 비밀 파일 읽기 금지):

```json
{
  "permissions": {
    "allow": [
      "Bash(python -m pytest:*)",
      "Bash(pytest:*)",
      "Bash(python director.py status)",
      "Bash(python scripts/cli_diagnostics.py:*)",
      "Bash(git status:*)",
      "Bash(git diff:*)",
      "Bash(git log:*)",
      "Bash(git show:*)",
      "Bash(git worktree list:*)"
    ],
    "deny": [
      "Read(./.env)",
      "Read(./**/*.token)",
      "Read(./companion.local.json)"
    ]
  }
}
```

### 주의

- 권한 확인을 완전히 끄는 모드는 AI가 파일 삭제·push 같은 동작도 묻지 않고 할 수 있다는 뜻입니다.
  이 프로젝트의 원칙(원본 무수정, 돈·자격증명은 사람이)과 맞추려면 위처럼 **필요한 명령만 허용**하는 쪽을 권장합니다.
- `python director.py run/mission/do` 는 실제 구독 CLI를 호출하므로 허용 목록에 넣지 않았습니다.
