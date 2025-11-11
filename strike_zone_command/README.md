# KBO 투수 스트라이크 존 제구력 연구 (2021-2025)

## 📁 프로젝트 개요

**연구 기간**: 2021-2025 시즌
**분석 투수**: 236명 (100구 이상)
**총 투구 수**: 221,914구 (2025 시즌)
**데이터베이스**: kbo_migration_v2 (PostgreSQL)
**최종 업데이트**: 2025-11-11

---

## 📊 최종 보고서

### ⭐ 주요 보고서
- **[COMPREHENSIVE_REPORT_V2.md](COMPREHENSIVE_REPORT_V2.md)** ← **최신 종합 보고서**
  - 전체 연구 결과 종합 (2021-2025)
  - 모든 데이터 출처 명시 (CSV 파일별)
  - 데이터 기반 사실과 인사이트 명확 분리
  - 757줄, 23KB

### 이전 보고서 (참고용)
- [COMPREHENSIVE_REPORT.md](COMPREHENSIVE_REPORT.md) - V1 버전
- [ABS_ANALYSIS_REPORT.md](ABS_ANALYSIS_REPORT.md) - ABS 분석 단독
- [FINAL_REPORT.md](FINAL_REPORT.md) - Baseball Savant 기준

---

## 🎯 핵심 연구 결과 요약

### 1. ABS 기준 제구율 (2025)
- **평균 제구율**: 47.06% (투구의 절반도 존에 안 들어감)
- **규정 존 (공 중심)**: 38.35%
- **Shadow 존 (엣지)**: 8.70%
- **생존편향**: +2.20%p (100-200구 투수 vs 1000구+ 투수)

### 2. 시즌별 추이 (2021-2025)
- **2024년 미스터리 급락**: -3.00%p (48.65% → 45.65%)
- **5년 연속 출전자 평균**: -1.12%p 하락
- **3년 연속 개선 투수**: 4명 only
- **3년 연속 하락 투수**: 49명 (개선 vs 하락 = 1:12)

### 3. Heart/Shadow/Waste 분포 (2025)
- **Heart (정중앙)**: 16.66%
- **Shadow (엣지)**: 41.56%
- **Waste (볼)**: 41.77%
- **→ KBO 투수들은 중앙을 피하고 엣지 위주로 승부**

### 4. CSW% (Called Strike + Whiff %)
- **평균 CSW%**: 26.92% (10구 중 2.7개만 순수 스트라이크)
- **Shadow 존 판정률**: 30.84%
- **초구 CSW%**: 31.05%

### 5. 상황별 제구력 변화
- **투스트라이크**: Heart -5.06%p, Shadow +5.06%p (의도적 회피)
- **득점권 (RISP)**: Heart -2.00%p, Shadow +2.67%p
- **구종별**: Fastball 52.77% vs OffSpeed 38.52% (차이 14.25%p)

---

## 📂 데이터 파일 (총 20개 CSV + 5개 JSON)

### ABS 존 분석 (Part 1)
1. **abs_zone_summary.csv** - 전체 존별 평균/표준편차
2. **abs_pitcher_command_100plus.csv** - 100구+ 투수 236명 개별 데이터
3. **abs_pitcher_command_stats.csv** - 500구+ 투수 147명 개별 데이터
4. **abs_survival_bias_analysis.csv** - 투구수 그룹별 생존편향 분석

### 시즌별 추이 분석 (Part 2)
5. **yoy_league_trend.csv** - 2021-2025 리그 평균 추이
6. **yoy_all_seasons.csv** - 1,328 투수-시즌 조합 전체 데이터
7. **yoy_pitcher_trends.csv** - 개별 투수 연도별 추이
8. **yoy_change_summary.csv** - 연도별 변화량 요약

### Heart/Shadow/Waste 분석 (Part 3)
9. **heart_shadow_pitchers.csv** - 투수별 Heart/Shadow/Waste 비율
10. **heart_shadow_by_pitch_type.csv** - 구종별 (Fastball/Breaking/OffSpeed)
11. **heart_shadow_by_count.csv** - 카운트별 (First Pitch, Ahead, Behind, etc.)
12. **heart_shadow_by_runners.csv** - 주자 상황별 (Empty, On Base, RISP)

### CSW% 및 판정 분석 (Part 4)
13. **called_strike_pitchers.csv** - 투수별 CSW% 데이터
14. **shadow_called_strike_rate.csv** - Shadow 존 판정률
15. **fps_zone_vs_called.csv** - 초구 Zone% vs Called Strike% 비교
16. **called_strike_by_count.csv** - 카운트별 CSW%
17. **called_strike_by_runners.csv** - 주자 상황별 CSW%

### 구종/상황별 상세 분석
18. **abs_pitch_type_analysis.csv** - 구종별 ABS 존 제구율
19. **abs_count_analysis.csv** - 카운트별 ABS 존 제구율
20. **abs_runners_analysis.csv** - 주자 상황별 ABS 존 제구율

### JSON 중간 결과 (5개)
- **abs_analysis_result.json** - ABS 분석 종합
- **yoy_analysis_result.json** - Y2Y 분석 종합
- **abs_detailed_analysis.json** - 상세 상황별 분석
- **heart_shadow_analysis.json** - Heart/Shadow 분석 종합
- **called_strike_analysis.json** - CSW% 분석 종합

---

## 🔬 연구 방법론

### ABS (Automated Ball-Strike) 존 정의
```
규정 스트라이크 존 + 공 반지름 (1.45" = 0.121 ft)

가로: |plate_x| ≤ 0.829 ft (규정 0.708 + 0.121)
세로: (sz_bot - 0.121) ≤ plate_z ≤ (sz_top + 0.121)
```

### Heart/Shadow/Waste 존 (Baseball Savant 기준)
```
Heart (정중앙):
  - 가로: |plate_x| ≤ 0.4333 ft (규정 - 3.3")
  - 세로: (sz_bot + 0.333) ≤ plate_z ≤ (sz_top - 0.333)

Shadow (엣지 링):
  - (규정 존 ± pad 이내) AND NOT Heart
  - Vertical pad: ±4.0"
  - Horizontal pad: ±3.3"

Waste (명백한 볼):
  - Shadow 존 밖
```

### 필터링 기준
- **100구 이상**: 생존편향 완화 (236명)
- **500구 이상**: 안정적 샘플 (147명)
- **시즌별 50구 이상**: Y2Y 분석 (1,328 투수-시즌)

---

## 💡 주요 인사이트

### 1. "네모 안으로 던지기"의 실제 난이도
- 평균 투수는 47.06%만 성공 (10구 중 5구도 안 됨)
- 정중앙(Heart)은 16.66%만 → **의도적 중앙 회피 전략**
- 순수 스트라이크(CSW)는 26.92% → **10구 중 2.7개만**

### 2. 제구력 개선은 극히 드물다
- 3년 연속 개선 vs 하락 = **4명 vs 49명** (비율 1:12)
- 5년 연속 출전자도 평균 -1.12%p 하락
- 최대 개선 +6.67%p vs 최대 하락 -25.20%p

### 3. 생존편향이 실재한다
- 100-200구 투수: 45.16% (조기 강등)
- 1000구+ 투수: 47.36% (생존자)
- **차이 +2.20%p** → 못 던지는 투수는 일찍 퇴출됨

### 4. 2024년 미스터리
- 2023: 48.65% → 2024: 45.65% (**-3.00%p 급락**)
- 원인 불명 (트래킹 시스템 변경? 볼 규격 변경?)

### 5. 상황에 따른 전략 변화
- **투스트라이크**: Heart -5.06%p, Shadow +5.06%p (중앙 회피)
- **득점권**: Heart -2.00%p, Waste +2.67%p (볼 유도)
- **Fastball은 제어 가능, OffSpeed는 어려움** (차이 14.25%p)

---

## 🚀 향후 연구 과제

### 데이터 보강 필요
1. **leverage_index** 컬럼 추가 → 압박 상황별 제구력
2. **구속/무브먼트** 데이터 → 구위 vs 제구 분리
3. **포수/심판 ID** → 프레이밍 효과 (2023 이전)
4. **릴리스 포인트** → 산포/일관성 분석

### 고급 분석 (ENHANCEMENT_PLAN.md)
1. **Edge%** - 의도적 엣지 히트 능력
2. **p(Strike) 확률 모델** - 동적 스트라이크 존
3. **산포 분석** - 타원적분, 95% 신뢰타원
4. **Intent proxy** - 카운트별 최적 타깃 일치도
5. **혼합효과 모형** - 투수/타자/포수/심판 랜덤효과 통제

---

## 📂 파일 구조

```
/Users/mksong/Documents/baseball_research_results/strike_zone_command/
├── README.md (이 파일)
├── COMPREHENSIVE_REPORT_V2.md ⭐ 최종 보고서
├── COMPREHENSIVE_REPORT.md (V1)
├── RESEARCH_DESIGN.md (연구 설계)
│
├── CSV 데이터 (20개)
│   ├── abs_*.csv (ABS 분석 4개)
│   ├── yoy_*.csv (Y2Y 분석 4개)
│   ├── heart_shadow_*.csv (Heart/Shadow 5개)
│   ├── called_strike_*.csv (CSW% 5개)
│   └── fps_zone_vs_called.csv
│
└── JSON 중간 결과 (5개)
```

---

## 📝 최종 결론

### KBO 투수 제구력의 3대 사실

#### 1. 평균 제구율 47.06% - 절반도 안 됨
- KBO 평균 투수는 10구 중 5구도 존에 못 던진다
- 정중앙(Heart)은 16.66%만 → 의도적 회피
- 순수 스트라이크(CSW)는 26.92% → 10구 중 2.7개만

#### 2. 개선은 거의 불가능 - 개선 vs 하락 = 1:12
- 3년 연속 개선 4명 vs 하락 49명
- 5년 연속 출전자도 평균 -1.12%p 하락
- 제구력은 타고난 능력이며 개선 극히 어려움

#### 3. 생존편향 존재 - 실제는 더 어려움
- 100-200구 조기 강등자: 45.16%
- 1000구+ 생존자: 47.36%
- 실제 KBO 투수 평균은 47%보다 낮을 것

### 데이터가 말하는 진실
**"네모 안으로 던지기"는 야구에서 가장 어려운 기술이며, 이를 잘하는 투수는 극소수다.**

---

## 🔗 관련 정보

- **연구 저장소**: `/Users/mksong/Documents/baseball_research/strike_zone_command_2025/`
- **공개 결과**: `/Users/mksong/Documents/baseball_research_results/strike_zone_command/`
- **데이터베이스**: kbo_migration_v2 (PostgreSQL)

---

**연구 수행**: 밍밍 (User)
**분석 지원**: Claude (Anthropic)
**최종 업데이트**: 2025-11-11
