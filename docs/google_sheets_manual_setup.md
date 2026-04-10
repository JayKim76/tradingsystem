# Google Sheets 수동 설정 가이드

비개발자 기준으로 작성된 단계별 설정 절차입니다.

---

## 1단계: Google 스프레드시트 생성

1. [Google Sheets](https://sheets.google.com) 접속 후 로그인
2. 왼쪽 상단 **"+ 새 스프레드시트"** 클릭
3. 파일명을 **"세인투 투자 비서 데이터"** 로 변경
4. 하단 탭을 두 개 만든다:
   - 탭 1: 시트 이름 → `일일브리핑`
   - 탭 2: + 버튼 클릭 → 시트 이름 → `주간포트폴리오리포트`
5. 각 탭의 1행에 `docs/google_sheets_schema.md`의 컬럼명을 순서대로 입력
6. 주소창 URL에서 스프레드시트 ID 복사:
   ```
   https://docs.google.com/spreadsheets/d/{여기가_SPREADSHEET_ID}/edit
   ```
7. `config/secrets.env.example`을 복사해 `config/.env`로 저장 후 `GOOGLE_SHEETS_SPREADSHEET_ID` 값에 붙여넣기

---

## 2단계: Google Cloud 프로젝트 및 OAuth 설정

1. [Google Cloud Console](https://console.cloud.google.com) 접속
2. 상단 프로젝트 선택 드롭다운 → **"새 프로젝트"** → 이름: `investment-assistant`
3. 왼쪽 메뉴 → **"API 및 서비스"** → **"라이브러리"**
4. 검색창에 `Google Sheets API` 입력 → 클릭 → **"사용 설정"**
5. 왼쪽 메뉴 → **"사용자 인증 정보"** → **"+ 사용자 인증 정보 만들기"** → **"OAuth 클라이언트 ID"**
6. **애플리케이션 유형**: `데스크톱 앱` 선택 → 이름: `investment-assistant` → **"만들기"**
7. 팝업에서 **클라이언트 ID**와 **클라이언트 보안 비밀번호** 복사
8. `config/.env`에 입력:
   ```
   GOOGLE_CLIENT_ID=복사한_클라이언트_ID
   GOOGLE_CLIENT_SECRET=복사한_클라이언트_보안_비밀번호
   ```

---

## 3단계: OAuth 동의 화면 및 Refresh Token 발급

1. 왼쪽 메뉴 → **"OAuth 동의 화면"**
2. 사용자 유형: **"외부"** → 저장 후 계속
3. 앱 이름: `투자 비서`, 지원 이메일: 내 Google 이메일 입력 → 저장
4. **"테스트 사용자"** 탭 → **"+ ADD USERS"** → 내 Google 이메일 추가

### Refresh Token 발급 (터미널 사용)
```bash
# Python 환경이 있는 경우
pip install google-auth-oauthlib

python3 - <<'EOF'
from google_auth_oauthlib.flow import InstalledAppFlow

SCOPES = ['https://www.googleapis.com/auth/spreadsheets']
flow = InstalledAppFlow.from_client_config(
    {
        "installed": {
            "client_id": "여기에_CLIENT_ID",
            "client_secret": "여기에_CLIENT_SECRET",
            "redirect_uris": ["urn:ietf:wg:oauth:2.0:oob"],
            "auth_uri": "https://accounts.google.com/o/oauth2/auth",
            "token_uri": "https://oauth2.googleapis.com/token"
        }
    },
    SCOPES
)
creds = flow.run_local_server(port=0)
print("Refresh Token:", creds.refresh_token)
EOF
```
5. 브라우저가 열리면 구글 계정 로그인 → 권한 허용
6. 출력된 Refresh Token을 `config/.env`의 `GOOGLE_REFRESH_TOKEN`에 입력

---

## 확인
`config/.env`에 아래 4개 값이 모두 채워져 있어야 합니다:
```
GOOGLE_CLIENT_ID=✅
GOOGLE_CLIENT_SECRET=✅
GOOGLE_REFRESH_TOKEN=✅
GOOGLE_SHEETS_SPREADSHEET_ID=✅
```
