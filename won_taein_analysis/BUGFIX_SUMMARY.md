# 원태인 분석 버그 수정 요약

**수정일**: 2025-11-11
**발견자**: User (밍밍)
**수정자**: Claude

---

## 🐛 발견된 문제

### 증상
- 원태인 선수 분석 보고서에서 **SS(삼성)이 상대팀으로 나타남**
- SS 상대 타석 수: 1,615개 (전체의 48.5%)
- 원태인은 SS 소속이므로 자기 팀과 대결 불가능

### 원인
**파일**: `scripts/04_opponent_analysis.py`
**위치**: Line 32-39, `get_opponent_team()` 함수

**잘못된 로직**:
```python
def get_opponent_team(row):
    """상대팀 추출"""
    if row['inning_half'] == 'top':
        # 초 = 원정팀 공격 = 홈팀이 수비 (원태인이 홈팀)
        return row['away_team_code']  # ❌ 잘못됨
    else:
        # 말 = 홈팀 공격 = 원정팀이 수비 (원태인이 원정팀)
        return row['home_team_code']  # ❌ 잘못됨
```

**문제점**:
- `inning_half`만으로는 원태인이 홈/어웨이인지 판단 불가
- 원태인이 홈팀인 경우와 원정팀인 경우를 구분하지 않음
- 결과적으로 SS(홈)일 때 초는 SS를 상대로, SS(어웨이)일 때 말은 SS를 상대로 잘못 집계

---

## ✅ 수정 내용

### 올바른 로직
```python
def get_opponent_team(row):
    """
    상대팀 추출
    원태인은 SS 소속이므로:
    - SS가 home_team이고 inning_half='top' → 홈 경기, 상대는 away_team
    - SS가 away_team이고 inning_half='bottom' → 원정 경기, 상대는 home_team
    """
    if row['home_team_code'] == 'SS':
        # 원태인이 홈팀 (SS가 홈)
        return row['away_team_code']
    else:
        # 원태인이 원정팀 (SS가 어웨이)
        return row['home_team_code']
```

### 검증 로직 추가
```python
# 검증: SS가 상대팀으로 나오면 안 됨
ss_count = (df_pa['opponent_team'] == 'SS').sum()
print(f"\n⚠️ 검증: SS가 상대팀으로 분류된 타석 수 = {ss_count} (0이어야 정상)")
if ss_count > 0:
    print("❌ 오류: SS가 상대팀으로 분류되었습니다!")
else:
    print("✅ 정상: SS는 상대팀으로 분류되지 않음")
```

---

## 📊 수정 전후 비교

### 수정 전 (잘못된 데이터)
```
opponent_team  pa_count  avg    obp
SS (삼성)      1,615     .233   .292  ← 원태인 소속팀 (오류!)
LT (롯데)        263     .232   .270
KT (KT)          190     .258   .337
NC (NC)          153     .288   .353
WO (키움)        224     .179   .281
...
```

### 수정 후 (올바른 데이터)
```
opponent_team  pa_count  avg    obp
LT (롯데)        457     .247   .284
HT (KIA)         455     .257   .332
HH (한화)        454     .192   .278
KT (KT)          431     .225   .285
WO (키움)        382     .215   .304
SK (SSG)         341     .264   .340
NC (NC)          311     .251   .325
LG (LG)          268     .257   .299
OB (두산)        231     .229   .273
```

**검증 결과**: ✅ SS가 상대팀 목록에서 완전히 제거됨

---

## 🔄 수정된 파일 목록

### 1. 스크립트
- ✅ `scripts/04_opponent_analysis.py` - 로직 수정
- ✅ `scripts/04_opponent_analysis_old.py` - 백업 (참고용)

### 2. 데이터 파일
- ✅ `report/13_team_performance.csv` - 재생성
- ✅ `report/14_batter_performance_all.csv` - 재생성
- ✅ `report/15_batter_performance_10pa.csv` - 재생성
- ✅ `report/16_base_situation_performance.csv` - 재생성
- ✅ `report/17_pitch_type_by_top_batters.csv` - 재생성

### 3. 시각화
- ✅ `report/viz_08_team_avg.png` - 재생성
- ✅ `report/viz_09_top_batters_avg.png` - 재생성
- ✅ `report/viz_10_base_situation.png` - 재생성

### 4. 보고서
- ✅ `report/FINAL_REPORT.md` - 섹션 4.1 수정 + 수정 공지 추가
- ✅ `README.md` - 버그 수정 공지 추가

### 5. 결과물 폴더
- ✅ `/Users/mksong/Documents/baseball_research_results/won_taein_analysis/`
  - FINAL_REPORT.md 업데이트
  - 13_team_performance.csv 업데이트
  - viz_08_team_avg.png 업데이트
  - README.md 업데이트

---

## 📈 영향 범위

### 영향받은 분석
- ✅ **4.1 상대 팀별 성적** - 완전히 재작성
- ✅ **관련 시각화** - 재생성

### 영향받지 않은 분석
- ✅ **1. 기본 정보** (투구 수, 등판 수 등)
- ✅ **2. 로케이션 분석** (구종별 위치, 존별 분포)
- ✅ **3. 피로도 분석** (등판 간격, 구속 변화)
- ✅ **4.2 주요 타자 대결 성적** (타자 pcode 기반)
- ✅ **4.3 베이스 상황별 성적**

**이유**: 상대팀 분류만 잘못되었고, 타자별 분석은 `batter_pcode`로 집계되므로 영향 없음

---

## 🔍 재발 방지

### 1. 검증 로직 추가
- 자기 팀(SS)이 상대팀으로 분류되는지 자동 확인
- 타석 수 합계 검증 (전체 타석 수와 일치해야 함)

### 2. 코드 리뷰 체크리스트
- [ ] home/away 로직 사용 시 선수/팀 소속 확인
- [ ] inning_half만으로 상대팀 판단하지 않기
- [ ] 데이터 검증 로직 추가

### 3. 테스트 케이스
```python
# 필수 검증
assert (df_pa['opponent_team'] == 'SS').sum() == 0, "SS should not be opponent"
assert df_pa['opponent_team'].nunique() == 9, "Should have 9 opponents (10 teams - SS)"
assert df_pa.groupby('opponent_team')['pa_id'].count().sum() == len(df_pa), "All PAs accounted"
```

---

## 📝 교훈

### 발견 과정
1. User가 보고서 검토 중 이상 발견: "SS 상대 데이터가 있다"
2. 원태인이 SS 소속이므로 논리적 모순 확인
3. 04_opponent_analysis.py 코드 리뷰
4. inning_half 기반 로직의 오류 발견

### 핵심 교훈
1. **도메인 지식 필수**: 야구 규칙(투수는 자기 팀과 대결 안 함)
2. **검증 로직 중요**: 자명한 조건(자기 팀 제외) 자동 확인
3. **데이터 Sense**: 1,615/3,330 = 48.5%는 비정상적으로 높은 비율
4. **코드 리뷰**: 비즈니스 로직은 반드시 검증 필요

---

## ✅ 완료 체크리스트

- [x] 버그 원인 파악
- [x] 올바른 로직으로 수정
- [x] 검증 로직 추가
- [x] 데이터 재생성
- [x] 시각화 재생성
- [x] 보고서 업데이트
- [x] 수정 공지 추가
- [x] 결과물 폴더 동기화
- [x] 백업 파일 보관
- [x] 버그 수정 요약 문서 작성

---

**수정 완료일**: 2025-11-11
**검증 상태**: ✅ PASS (SS가 상대팀에서 완전히 제거됨)
