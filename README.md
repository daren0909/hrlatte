# HR latte point

운동하면 티켓 1장, 라떼 한 잔에 티켓 1장을 쓰는 개인용 웹페이지입니다.

## 규칙

| 운동 종류 | 받는 티켓 |
|---|---|
| 걷기 | 1장 |
| 필라테스 | 1장 |
| 피티 | 1장 |

라떼 한 잔을 마시면 티켓 1장이 차감됩니다. 티켓이 없으면 라떼 버튼이 눌리지 않습니다.

## 기능

- 운동 기록: 운동 종류, 날짜, 메모를 입력합니다. 미래 날짜는 입력할 수 없습니다.
- 라떼 사용: 티켓 1장을 차감하고 카페 메모를 남깁니다.
- 이번 달 요약: 운동 종류별 횟수와 마신 라떼 잔 수를 보여줍니다.
- 기록 삭제: 버튼을 두 번 눌러야 삭제됩니다. 티켓이 마이너스가 되는 삭제는 막습니다.
- 백업: JSON 백업 파일을 내려받거나 불러올 수 있습니다. CSV로 내보내면 엑셀에서 열 수 있습니다.

## GitHub Pages로 배포하기

1. GitHub에서 새 저장소를 만듭니다. 예를 들어 이름을 `hr-latte-point`로 합니다.
2. 이 폴더의 `index.html`과 `README.md`를 저장소 최상위에 올립니다.
   - 웹에서 올릴 때: 저장소 화면의 **Add file → Upload files**에서 두 파일을 끌어다 놓고 **Commit changes**를 누릅니다.
   - 터미널에서 올릴 때:
     ```bash
     git init
     git add index.html README.md
     git commit -m "HR latte point 첫 배포"
     git branch -M main
     git remote add origin https://github.com/<사용자명>/hr-latte-point.git
     git push -u origin main
     ```
3. 저장소의 **Settings → Pages**로 이동합니다.
4. **Build and deployment**에서 Source를 **Deploy from a branch**로, Branch를 `main` / `/ (root)`로 선택하고 저장합니다.
5. 1~2분 뒤 `https://<사용자명>.github.io/hr-latte-point/`에서 열립니다.

## 데이터 저장 방식

기록은 브라우저의 localStorage에 저장됩니다. 서버나 GitHub로는 전송되지 않습니다.

- 같은 기기, 같은 브라우저에서만 기록이 이어집니다.
- 휴대폰과 PC의 기록은 서로 공유되지 않습니다.
- 브라우저 데이터를 지우거나 시크릿 모드를 쓰면 기록이 사라질 수 있습니다.
- 기기를 바꿀 때는 **백업 파일 내려받기**로 저장한 뒤, 새 기기에서 **백업 파일 불러오기**를 사용하세요.

저장소를 공개(public)로 만들어도 공개되는 것은 코드뿐입니다. 각자의 운동 기록은 각자의 브라우저에만 남습니다.

## 운동 종류 바꾸기

`index.html`에서 아래 부분만 고치면 됩니다.

```js
const ACTIVITIES = [
  { id: 'walk',    name: '걷기',     icon: '🚶' },
  { id: 'pilates', name: '필라테스', icon: '🧘' },
  { id: 'pt',      name: '피티',     icon: '🏋️' }
];
```

- 새 종류를 추가하려면 같은 형식으로 한 줄을 더하세요. `id`는 영문 소문자로, 다른 항목과 겹치지 않게 정합니다.
- 이미 기록이 있는 종류의 `id`를 바꾸거나 지우면, 그 종류의 기존 기록이 불러올 때 제외됩니다. 이름(`name`)과 아이콘(`icon`)은 자유롭게 바꿔도 됩니다.

## 파일 구성

```
index.html   화면, 스타일, 동작이 모두 들어 있는 단일 파일
README.md    이 설명서
```

별도의 빌드 과정이나 외부 라이브러리가 필요 없습니다. 글꼴만 Google Fonts에서 불러오며, 연결이 안 되면 시스템 글꼴로 표시됩니다.
