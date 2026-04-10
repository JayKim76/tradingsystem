# Telegram 수동 설정 가이드

비개발자 기준으로 작성된 단계별 설정 절차입니다.

---

## 1단계: Telegram 봇 생성

1. Telegram 앱 실행 (모바일 또는 데스크톱)
2. 검색창에 **`@BotFather`** 검색 → 채팅 시작
3. `/newbot` 입력 → Enter
4. 봇 이름 입력 (예: `세인투 투자비서`)
5. 봇 사용자명 입력 (영문+숫자, 반드시 `bot`으로 끝나야 함, 예: `saeintu_invest_bot`)
6. 성공 시 메시지에서 **Bot Token** 복사:
   ```
   예시: 7123456789:AAExxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```
7. `config/.env`에 입력:
   ```
   TELEGRAM_BOT_TOKEN=복사한_토큰
   ```

---

## 2단계: Chat ID 확인

### 방법 A — 개인 채팅 (가장 간단)
1. 방금 만든 봇 이름 검색 → 채팅 시작 → `/start` 입력
2. 브라우저에서 아래 URL 접속 (토큰 교체):
   ```
   https://api.telegram.org/bot{BOT_TOKEN}/getUpdates
   ```
3. 응답 JSON에서 `"chat":{"id":숫자}` 부분의 숫자 복사
4. `config/.env`에 입력:
   ```
   TELEGRAM_CHAT_ID=복사한_숫자
   ```

### 방법 B — 그룹 채팅 (알림을 그룹으로 받을 경우)
1. Telegram 그룹 생성 → 봇을 멤버로 추가
2. 그룹에 아무 메시지 전송
3. 위 getUpdates URL 접속 → `"chat":{"id":-숫자}` 복사 (그룹은 음수)
4. `config/.env`에 입력

---

## 3단계: 연결 테스트

터미널에서 아래 명령어 실행 (토큰과 chat_id 교체):
```bash
curl -s -X POST \
  "https://api.telegram.org/bot{BOT_TOKEN}/sendMessage" \
  -d "chat_id={CHAT_ID}&text=✅ 투자 비서 연결 테스트 성공"
```
Telegram에 메시지가 도착하면 설정 완료입니다.

---

## 확인
`config/.env`에 아래 2개 값이 모두 채워져 있어야 합니다:
```
TELEGRAM_BOT_TOKEN=✅
TELEGRAM_CHAT_ID=✅
```

---

## 참고: 봇 추가 설정 (선택)
- `/setdescription` → 봇 설명 추가
- `/setuserpic` → 봇 프로필 사진 설정
- 개인 채팅은 봇이 먼저 메시지를 보낼 수 없으므로 반드시 **사용자가 먼저 `/start`를 보내야** 합니다.
