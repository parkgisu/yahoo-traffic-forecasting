# Yahoo Traffic Forecasting

## Overview

본 프로젝트는 Yahoo 웹 트래픽 시계열 데이터를 활용하여
시간에 따른 트래픽 변화를 예측하는 모델을 구축하는 것을 목표로 합니다.

시계열 데이터의 주기성과 패턴을 분석하고,
다양한 머신러닝 모델을 활용하여 예측 성능을 비교하였습니다.

---

## My Contribution

본 프로젝트는 3인 팀으로 진행되었으며,
저는 **모델링 및 실험 전반을 주도적으로 수행**하였습니다.

### 주요 작업

* 시계열 데이터 분석 (FFT, ACF 기반 주기 탐색)
* IQR 기반 이상치 탐지 및 데이터 분포 분석
* Sliding Window 기반 시계열 데이터 변환
* RandomForest, GradientBoosting, Prophet 모델 구현 및 비교
* Feature Engineering (시간, 요일, lag 기반 피처 생성)
* RandomizedSearchCV 기반 하이퍼파라미터 튜닝
* Feature 조합 탐색 (lag 24, 100, seasonal lag)
* Sample Weight를 활용한 데이터 분포 보정 실험

---

## Key Challenge

초기 구간에서 매우 높은 트래픽 값이 존재하였으며,
초기에는 이를 이상치로 판단하여 제거한 뒤 모델을 학습했습니다.

예시 코드:
slided_series = masked_series[264:]

그러나 해당 접근은 실제 데이터의 중요한 패턴을 제거하는 문제가 있었고,
모델이 특정 구간을 학습하지 못하는 한계를 발생시켰습니다.

---

## Improvement

이 문제를 해결하기 위해:

* 초반 데이터를 제거하는 대신 유지
* 해당 구간에 대해 낮은 weight를 부여하여 학습에 반영

예시 코드:
weights[y_train < 264] = 0.1

이를 통해 데이터의 왜곡 없이 모델이 전체 구간을 학습하도록 개선하였습니다.

---

## Modeling Approach

### Feature Engineering

* Sliding Window (24시간)
* Seasonal Lag (24, 48, ...)
* Long-term Lag (100, 200, ...)
* Time-based Features (Hour, Day of Week)

### Models

* RandomForest Regressor
* GradientBoosting Regressor
* Prophet

### Training Strategy

* Train/Test Split (80/20)
* RMSE 기반 성능 평가

---

## Results

### Best Performance

* RMSE ≈ 0.039 ~ 0.040

### Key Observation

* Feature Engineering이 성능에 큰 영향
* 단순 모델 변경보다 데이터 처리 방식이 더 중요
* 이상치 처리 방식이 모델 성능에 직접적인 영향을 미침

---

## Data

본 프로젝트는 Yahoo 웹 트래픽 시계열 데이터를 사용합니다.
데이터 파일은 포함되어 있지 않으며, 유사한 공개 시계열 데이터로 재현 가능합니다.

---

## Project Structure
.
├─ traffic_forecasting.ipynb 
└─ README.md

---

## Key Takeaways

* 이상치는 단순 제거 대상이 아니라 중요한 정보일 수 있음
* 데이터 전처리는 모델 성능보다 더 중요한 요소가 될 수 있음
* 모델이 어려운 구간까지 학습할 수 있도록 설계하는 것이 중요

---

## Future Work

* 이상치 처리에 대한 robust한 접근 (log transform, scaling 등)
* 시계열 전용 모델 (LSTM, Transformer) 적용
* 데이터 분포 기반 weighting 전략 개선
