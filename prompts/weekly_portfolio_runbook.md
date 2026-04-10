# 주간 포트폴리오 리포트 Runbook

**실행 시각**: 매주 월요일 09:10 KST  
**출력 파일**: `reports/weekly/YYYY-Wxx.md`  
**대상 주간**: 직전 월~금 (5거래일)  
**투자자 프로필**: `data/investor_profile.md` 참조  
**보유 종목**: `data/portfolio.csv` 참조 (섹터 그룹 + thesis 중심)  
**관찰 종목**: `data/watchlist.csv` 참조

---

## Step 1 — 데이터 수집
주간 수치를 수집한다:

```
지수 주간 수익률: KOSPI, KOSDAQ, S&P500, NASDAQ
섹터 ETF 주간 수익률: 반도체, 금융, 에너지, 헬스케어
주요 보유 종목 주간 등락률
환율 주간 변화: USD/KRW
미국 10년물 금리 주간 변화
VIX 주간 변화
주간 주요 뉴스 (Fed 발언, 실적, 지정학)
```

---

## Step 2 — 리포트 프롬프트

```
당신은 세인투의 개인 투자 비서입니다.
투자 스타일: Value / Quality / Cash Flow / Shareholder Return / Margin of Safety
주요 섹터: [한국] 반도체·금융·자동차·방산 / [미국] 금융·헬스케어·에너지·플랫폼
참조 파일: data/portfolio.csv (섹터 그룹 + thesis 참조), data/watchlist.csv

대상 주간: {YYYY-MM-DD} ~ {YYYY-MM-DD} (W{xx})
수집 데이터: {Step 1 수치 삽입}

아래 구조로 reports/weekly/{YYYY-Wxx}.md 를 작성하라.
수량·평단은 파일 참조로 대체하고 본문에 반복 기재하지 않는다.
```

---

## Step 3 — 리포트 구조 (출력 템플릿)

```markdown
# 주간 포트폴리오 리포트 — {YYYY} W{xx}
**기간**: {YYYY-MM-DD} ~ {YYYY-MM-DD}

## 1. 주간 수익률 vs 벤치마크
| 항목 | 주간 수익률 |
|------|------------|
| 내 포트폴리오 (추정) | |
| KOSPI | |
| S&P500 | |
| 블렌드 벤치마크 (50:50) | |
| 초과수익률 | |

## 2. 수익/손실 기여 종목
**Top 기여 (상위 3)**
- [ticker]: +X.X%, 사유

**Bottom 기여 (하위 3)**
- [ticker]: -X.X%, 사유

## 3. 비중 변화 요약
- 목표 비중 대비 현재 괴리 큰 종목 (±3%p 이상)
- 조정 필요 여부

## 4. Thesis 점검
| 종목 | Thesis 상태 | 변화 사유 |
|------|-------------|-----------|
| 005930 | 유지/변경/훼손 | |
| 000660 | | |
| ... (전 종목 1줄 요약) | | |

## 5. 밸류에이션 변화
- PER/PBR이 크게 변한 종목 위주로 기술 (2~3개)
- 내재가치 재산정 필요 여부

## 6. 리스크 맵
현재 포트폴리오의 주요 리스크 3개:
1. 
2. 
3. 

## 7. 리밸런싱 후보
- 매도 검토: [ticker] — 사유
- 매수 확대 검토: [ticker] — 사유
- 현금 비중: X% (목표 대비)

## 8. 다음 주 주요 촉매
- 경제지표 / 실적 발표 / 중앙은행 이벤트 목록

## 9. Watchlist 승격 후보
진입 조건 충족 임박 종목:
- [ticker]: 현재 X, 진입 목표 Y, 거리 Z%

## 10. 다음 주 액션 플랜
- [ ] 
```

---

## Step 4 — Google Sheets 기록
`docs/google_sheets_schema.md`의 `주간포트폴리오리포트` 탭 스키마에 맞춰 아래 값만 기록한다:

| 컬럼 | 값 |
|------|----|
| 주간시작일 | YYYY-MM-DD (월요일) |
| 주간종료일 | YYYY-MM-DD (금요일) |
| 포트폴리오1주수익률 | % |
| 벤치마크1주수익률 | % (블렌드) |
| 초과수익률 | % |
| 최고기여종목 | ticker (수익률) |
| 최저기여종목 | ticker (수익률) |
| 투자포인트변경여부 | 있음/없음 |
| 리밸런싱후보 | ticker 쉼표 구분 |
| 다음주주요촉매 | 세미콜론 구분 (60자 이내) |
| 주간액션요약 | 40자 이내 |
| 리포트파일 | reports/weekly/YYYY-Wxx.md |
| 생성시각 | ISO 8601 |

기록 실패 시 `logs/YYYY-Wxx_error.log`에 기록하고 Telegram 전송을 스킵한다.

---

## Step 5 — Telegram 전송
`prompts/telegram_summary_rules.md`의 Weekly 규칙에 따라 요약 생성 후 전송.
