# RDBMS 코어 통계 및 옵티마이저 엔지니어링
### DBMS 통계 서브시스템 개발 및 비용 기반 쿼리 최적화 개선

본 프로젝트는 RDBMS 엔진 내부의 통계(Statistics) 서브시스템과 비용 기반 옵티마이저(Cost-Based Optimizer, CBO) 개선 작업을 중심으로 수행한 엔지니어링 경험을 정리한 문서입니다.

DB 통계 수집, 저장, 관리 로직을 구현하고, 해당 통계를 쿼리 플랜 생성 과정에서 활용하도록 최적화 로직을 개선하였습니다.

---

## 프로젝트 범위

다음과 같은 통계 기능을 설계 및 구현하였습니다:

- 테이블 통계 수집 및 저장
- 인덱스 통계 수집 및 정확도 개선
- 파티션 통계 수집 및 활용
- Skewed 데이터 대응 통계 로직 개발
- Histogram 기반 선택도(Selectivity) 개선
- 통계 기반 Cardinality 추정 로직 개선
- 옵티마이저 플랜 생성 시 통계 활용

---

## 주요 담당 업무

### 1️⃣ 통계 수집 및 관리 (PL/SQL)

- 통계 수집 프로시저 구현
- 통계 메타데이터 저장 구조 설계
- Sampling 기반 통계 수집 로직 개발
- 대용량 테이블에 대한 Sampling 정확도 개선
- Skewed 데이터 분포 대응 로직 구현
- Partition 단위 통계 수집 및 Global/Local 통계 조정
- Index 통계 수집 기능 구현

### 2️⃣ 옵티마이저 연동 (C++)

- 통계 데이터를 플랜 생성 로직에 연동
- Cardinality Estimation 개선
- Cost Model 입력값 정밀도 향상
- Clustering Factor 활용 로직 개선
- Index Scan / Full Table Scan 선택 로직 안정화

---

## 실제 구현 사례 (Tmax)

### 문제 상황

- Index 통계 수집 정확도가 낮음
- 특히 Clustering Factor 값이 실제 데이터 특성을 반영하지 못함
- 잘못된 Cardinality 추정으로 인해 Index Scan 관련 심각한 성능 저하 발생

### 원인 분석

- 기존 Index 통계 수집을 위해 수행되는 내부 쿼리의 실행 방식에 구조적 문제 존재
- 단순 쿼리 수정으로 해결 불가능
- 실행 경로 자체가 실제 데이터 정렬 특성을 반영하지 못함

### 해결 방법

- Index 통계 수집 전용 실행 노드(Node) 신규 개발
- 동일 쿼리라도 "Index 통계 수집 목적일 때만" 신규 로직을 타도록 실행 흐름 분기
- Clustering Factor 계산 로직 재설계

### 결과

- 90% 이상의 통계 정확도 확보
- Index Scan 관련 성능 저하 문제 해결
- Cost-Based Optimizer 의사결정 안정화

---

## ⚙️ 구현 기능 상세

### Sampling 정확도 개선
- Adaptive Sampling Ratio 적용
- 대용량 테이블에 대한 통계 편향 감소
- 추정 오차 최소화

### Skewed 데이터 대응
- Histogram 정밀도 개선
- Selectivity 보정 로직 구현
- 분포 기반 Cardinality 추정 보완

### Partition 통계
- Partition 단위 통계 수집
- Global / Local 통계 병합 로직 구현
- Partition Pruning 비용 추정 정확도 개선

### Index 통계 개선
- Clustering Factor 재계산 로직 구현
- 전용 통계 수집 노드 개발
- Index 기반 플랜 선택 정확도 향상

---

## 통계 → 옵티마이저 흐름

데이터  
→ Sampling  
→ 통계 계산  
→ 통계 저장  
→ Optimizer 입력  
→ Plan 생성  

핵심 개념:

- Cardinality Estimation
- Selectivity 계산
- Clustering Factor
- Histogram 기반 분포 모델링
- Cost-Based Optimization (CBO)

---

## 🛠 기술 스택

언어:
- PL/SQL (통계 수집 및 관리 로직 구현)
- C++ (옵티마이저 및 플랜 생성 로직 개선)

도메인:
- RDBMS 엔진 개발
- 비용 기반 쿼리 최적화
- 데이터베이스 내부 구조

---

## 기술적 성과

- Index 통계 정확도 90% 이상 향상
- Clustering Factor 계산 로직 개선
- 옵티마이저 오판(Plan Misestimation) 감소
- Index Scan 성능 저하 문제 해결
- 통계 기반 비용 모델 안정성 확보

---

## 핵심 역량

- DB 통계 구조에 대한 깊은 이해
- Query Plan 생성 과정에 대한 이해
- Cost-Based Optimizer 내부 로직 경험
- PL/SQL 기반 통계 수집 구현 능력
- 엔진 레벨 실행 노드 개발 경험

---

## 프로젝트 요약

본 프로젝트는 RDBMS 엔진 내부의 통계 서브시스템을 개선하고,  
해당 통계를 비용 기반 옵티마이저에 정확히 반영하도록 설계·구현한 코어 엔지니어링 작업입니다.

단순한 SQL 튜닝이 아닌,  
통계 수집 → 저장 → 옵티마이저 입력 → 플랜 생성까지  
엔진 레벨 전반을 다룬 DBMS 내부 개발 경험을 포함합니다.
