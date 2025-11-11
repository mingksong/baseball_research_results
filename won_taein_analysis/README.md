# 원태인 투수 상세 분석 보고서

## ⚠️ 중요 업데이트 (2025-11-11)
**버그 수정**: `04_opponent_analysis.py`의 home/away 로직 오류 수정
- **문제**: SS(삼성)가 상대팀으로 잘못 분류됨 (원태인은 SS 소속)
- **수정**: 상대팀 판단 로직 수정 완료
- **결과**: 올바른 9개 타팀 데이터로 업데이트

## 📋 연구 목적
원태인 투수의 투구 패턴, 피로도, 상대별 성적을 종합 분석하여 투구 전략 및 기용 방안 제시

## 🔍 분석 항목
1. **로케이션 분석**: 투구 위치 분포, 구종별 위치 패턴
2. **피로도 분석**: 등판 간격, 투구 수, 구속 변화 추이
3. **상대별 분석**: 팀별 성적, 타자별 대결 성적

## 📊 데이터
- **투수**: 원태인 (pcode: 69446)
- **기간**: 2021-2025 (5시즌)
- **데이터 소스**: KBO 야구 데이터베이스

## 🚀 실행 방법

### 1. 데이터베이스 설정 (필수!)
```bash
# 프로젝트 루트에서 config.py 설정 (최초 1회)
cd /path/to/baseball_research
cp config.example.py config.py
# config.py 파일을 열어서 실제 DB 정보 입력
```

📚 **자세한 설정**: [프로젝트 루트 README.md](../README.md) 참조

### 2. 분석 스크립트 실행
```bash
# 1. 데이터 추출
python scripts/01_extract_data.py

# 2. 로케이션 분석
python scripts/02_location_analysis.py

# 3. 피로도 분석
python scripts/03_fatigue_analysis.py

# 4. 상대별 분석
python scripts/04_opponent_analysis.py
```

## 📈 결과
- `report/` 폴더에 모든 CSV 파일 및 시각화 저장
- 최종 보고서: `report/final_report.md`
