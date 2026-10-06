# HR latte point

운동하면 티켓 1장, 라떼 한 잔에 티켓 1장을 쓰는 웹페이지입니다. Google 계정으로 로그인하면 컴퓨터, 휴대폰, 다른 브라우저 어디서 열어도 같은 기록이 보입니다.

## 규칙

| 운동 종류 | 받는 티켓 |
|---|---|
| 걷기 | 1장 |
| 필라테스 | 1장 |
| 피티 | 1장 |

라떼 한 잔을 마시면 티켓 1장이 차감됩니다. 티켓이 없으면 라떼 버튼이 눌리지 않습니다.

## 구성

```
index.html           화면과 동작
firebase-config.js   Firebase 연결 설정 (직접 채워야 함)
firestore.rules      데이터 보안 규칙 (Firebase 콘솔에 붙여 넣음)
README.md            이 설명서
```

GitHub Pages는 파일만 보여 주는 서비스라 기록을 저장할 곳이 없습니다. 그래서 기록은 Google의 Firebase(Firestore)에 저장하고, GitHub Pages는 화면만 보여 줍니다. 개인 사용량이면 Firebase 무료 요금제(Spark) 안에서 충분합니다.

## 설정 순서

처음 한 번만 하면 됩니다. Firebase 콘솔 화면의 메뉴 이름은 바뀔 수 있으니 비슷한 항목을 찾아 주세요.

### 1. GitHub 저장소 만들기

1. GitHub에서 새 저장소를 만듭니다. 예: `hr-latte-point`
2. 저장소의 **Settings → Pages**에서 Source를 **Deploy from a branch**, Branch를 `main` / `/ (root)`로 저장합니다.
3. 사이트 주소를 확인합니다. 예: `https://<사용자명>.github.io/hr-latte-point/`

### 2. Firebase 프로젝트 만들기

1. https://console.firebase.google.com 에서 **프로젝트 추가**를 누릅니다. Google 애널리틱스는 꺼도 됩니다.
2. **Authentication → 시작하기 → 로그인 방법**에서 **Google**을 사용 설정합니다.
3. **Authentication → 설정 → 승인된 도메인**에 `<사용자명>.github.io`를 추가합니다.
   - 이 단계를 빼면 로그인할 때 "승인된 도메인" 오류가 납니다.
4. **Firestore Database → 데이터베이스 만들기**를 누릅니다. 위치는 서울(`asia-northeast3`)을 고르고, 프로덕션 모드로 시작합니다.
5. Firestore의 **규칙** 탭에서 기존 내용을 지우고 `firestore.rules` 파일 내용을 붙여 넣은 뒤 **게시**합니다.
   - 이 단계를 빼면 화면에 "기록을 읽을 권한이 없어요"가 뜹니다.
6. **프로젝트 설정(톱니바퀴) → 내 앱 → 웹(`</>`)**으로 앱을 추가합니다. Firebase Hosting은 체크하지 않습니다.
7. 화면에 나오는 `firebaseConfig` 값을 `firebase-config.js`에 그대로 옮겨 적습니다.

### 3. GitHub에 올리기

네 파일을 저장소 최상위에 올립니다.

- 웹에서: 저장소 화면의 **Add file → Upload files**에 파일을 끌어다 놓고 **Commit changes**
- 터미널에서:
  ```bash
  git init
  git add index.html firebase-config.js firestore.rules README.md
  git commit -m "HR latte point 배포"
  git branch -M main
  git remote add origin https://github.com/<사용자명>/hr-latte-point.git
  git push -u origin main
  ```

1~2분 뒤 사이트 주소로 접속해 **Google로 로그인**을 누르면 됩니다. 다른 기기에서도 같은 Google 계정으로 로그인하면 같은 티켓이 보입니다.

## 보안에 대해

- `firebase-config.js`의 값은 공개 저장소에 올라가도 괜찮습니다. Firebase 웹 설정값은 비밀번호가 아니라 프로젝트를 가리키는 식별자입니다.
- 실제 보호는 `firestore.rules`가 합니다. 로그인한 사람은 자기 기록(`users/<본인 uid>/entries`)만 읽고 쓸 수 있습니다. 다른 사람의 기록은 볼 수 없습니다.
- 주소를 아는 다른 사람도 자기 Google 계정으로 로그인해 자기 티켓을 모을 수 있습니다. 본인만 쓰게 하려면 규칙의 `request.auth.uid == uid` 조건에 `&& request.auth.token.email == '본인 이메일'`을 더하세요.
- 추가로 막고 싶다면 Google Cloud 콘솔에서 API 키 사용 범위를 `<사용자명>.github.io`로 제한할 수 있습니다.

## 동작 방식

- 기록은 즉시 화면에 반영되고, 인터넷이 끊겨 있으면 연결된 뒤 자동으로 서버에 저장됩니다. 오른쪽 위에 현재 상태가 표시됩니다.
- 한 기기에서 기록하면 같은 계정으로 열려 있는 다른 기기 화면에도 바로 반영됩니다.
- 기록은 한 건마다 별도 문서로 저장되므로, 두 기기에서 동시에 기록해도 서로 덮어쓰지 않습니다.
- 이전 버전(브라우저에만 저장)으로 쌓은 기록이 있으면, 같은 브라우저에서 처음 로그인할 때 계정으로 옮길지 묻습니다. 같은 기록을 두 번 옮겨도 중복되지 않습니다.
- 백업 파일 내려받기, 백업 파일에서 추가, CSV 내보내기를 지원합니다.

## 운동 종류 바꾸기

두 곳을 함께 고쳐야 합니다.

1. `index.html`의 `ACTIVITIES`
   ```js
   const ACTIVITIES = [
     { id: 'walk',    name: '걷기',     icon: '🚶' },
     { id: 'pilates', name: '필라테스', icon: '🧘' },
     { id: 'pt',      name: '피티',     icon: '🏋️' }
   ];
   ```
2. `firestore.rules`의 `d.activity in ['walk', 'pilates', 'pt']` 목록 (고친 뒤 Firebase 콘솔에서 다시 게시)

`id`는 영문 소문자로, 서로 겹치지 않게 정합니다. 이미 기록이 있는 종류의 `id`를 바꾸거나 지우면 그 기록은 화면에서 빠집니다. `name`과 `icon`은 자유롭게 바꿔도 됩니다.

## 내 컴퓨터에서 미리 보기

파일을 더블클릭해 여는 방식(`file://`)으로는 로그인이 되지 않습니다. 폴더에서 아래 명령을 실행한 뒤 `http://localhost:8000`으로 여세요. `localhost`는 Firebase에 기본으로 승인되어 있습니다.

```bash
python3 -m http.server 8000
```
