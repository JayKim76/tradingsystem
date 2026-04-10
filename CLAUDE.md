# 세인투 개인 투자 비서 — Claude Code 프로젝트

## 역할
매일 시장 데이터를 분석하여 일일 투자 브리핑과 주간 포트폴리오 리포트를 생성하고,
Google Sheets에 수치 데이터를 기록하며 Telegram으로 핵심 요약을 전송한다.

## 디렉터리 구조
```
data/
  investor_profile.md      # 투자자 프로필 (스타일, 철학, 리스크 규칙)
  portfolio.csv            # 보유 종목 현황
  watchlist.csv            # 관찰 종목 목록
prompts/
  daily_briefing_runbook.md     # 일일 브리핑 실행 가이드
  weekly_portfolio_runbook.md   # 주간 리포트 실행 가이드
  telegram_summary_rules.md     # 텔레그램 요약 규칙
config/
  secrets.env.example      # API 키 플레이스홀더
reports/
  daily/YYYY-MM-DD.md      # 일일 브리핑 상세 리포트
  weekly/YYYY-Wxx.md       # 주간 포트폴리오 리포트
logs/                      # 실행 로그
docs/
  google_sheets_schema.md        # Sheets 스키마 정의
  google_sheets_manual_setup.md  # Sheets 수동 설정 가이드
  telegram_manual_setup.md       # Telegram 수동 설정 가이드
```

## 핵심 원칙
1. **브리핑 프롬프트는 짧게**: 종목 수량·평단을 반복하지 말 것. `data/portfolio.csv`와 `data/watchlist.csv`를 참조하는 방식으로 운영.
2. **보고서는 markdown**: 상세 분석은 `reports/` 하위 파일에 저장.
3. **Google Sheets는 숫자·짧은 메모만**: 긴 본문은 markdown 파일 경로로 대체.
4. **Telegram은 Sheets 기록 성공 후 전송**: 실패 시 로그에 기록하고 스킵.

## 일일 실행 (매일 09:00 KST)
```
prompts/daily_briefing_runbook.md 참조
```

## 주간 실행 (매주 월요일 09:10 KST)
```
prompts/weekly_portfolio_runbook.md 참조
```

## 환경 변수
`config/secrets.env.example` 복사 후 `.env`로 저장하고 실제 값 입력.
자세한 설정 절차: `docs/google_sheets_manual_setup.md`, `docs/telegram_manual_setup.md`

## 투자자 프로필 요약
- 이름: 세인투 | 경력: 7년 | 통화: KRW + USD
- 스타일: Value / Quality / Cash Flow / Shareholder Return / Margin of Safety
- 위험 성향: 공격적 중립
- 상세: `data/investor_profile.md`
