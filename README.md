# 📈 Pipe Production Process Schedule Optimization Using EDA & Data Prediction

## 🏭 파이프 생산 공정 순서 배열 최적화

### 📑 Table of Contents
- [🔍 Problem Definition (문제 정의)](#problem-definition-문제-정의)
- [🧪 Experimental Design (실험 설계)](#experimental-design-실험-설계)
  - [🛠️ Data Preprocessing System (데이터 전처리 시스템)](#data-preprocessing-system-데이터-전처리-시스템)
  - [🤖 Prediction Model Application (예측 모델 적용)](#prediction-model-application-예측-모델-적용)
  - [⚙️ Optimization Algorithm Application (최적화 알고리즘 적용)](#optimization-algorithm-application-최적화-알고리즘-적용)
- [📊 Hypothesis Testing (가설 검정)](#hypothesis-testing-가설-검정)
- [✅ Conclusion (결론)](#conclusion-결론)
- [🔮 Future Work (향후 연구)](#future-work-향후-연구)

---

### 🔍 Problem Definition (문제 정의)

현대RB의 파이프 제조 공정에서 다양한 파이프 종류와 그에 따른 소요 시간을 고려하여 공정 배열을 최적화하는 것이 본 연구의 주요 목표이다. 특히, 파이프 철판의 소요 시간이 정확히 파악되지 않아 최적화 과정에 어려움이 있으며, 모든 파이프의 소요 시간이 기록되지 않아 새로운 파이프가 공정 순서에 포함될 경우 최적화가 더욱 복잡해지는 문제가 존재한다.

---
![image](https://github.com/user-attachments/assets/ec9bb665-f860-4855-9f2a-d536eac4c327)
---
### 🧪 Experimental Design (실험 설계)

#### 🛠️ Data Preprocessing System (데이터 전처리 시스템)

1. **📦 사분위수의 Box형 데이터 분포에서 이상치값 필터링**
2. **🔧 휴리스틱 방식으로 직접 데이터 전처리**
3. **💡 고안한 데이터 전처리 시스템**
   - Step5를 제외한 다른 공정은 완료 시간이 없어 시작 시간만 존재하므로 정확한 작업 시간을 알기 어렵다. 이를 딥러닝을 통해 계산할 계획이다.
   - Step5의 데이터 품질은 Random Forest Regressor로 검증했으며, Mean Absolute Error가 약 1.25로 양호한 결과를 보였다.
   - Step5 데이터를 작업 시작 시간을 기준으로 정렬하고, 각 공정 간의 시간 차이를 출력값으로 하여 모델을 학습시킨다.
   - 모델 학습이 잘 되면 공정 시작 시간만으로 작업 시간을 예측할 수 있게 된다.
   - 입력 데이터는 timestamp 타입을 사용했으며, 6월 한 달의 데이터만 있어 기준 값으로 `DATE(2023, 6, 1)`을 사용했다.
   - 파이프의 외경, 두께, 중량 등을 기준으로 작업 시간을 회귀 모델에 적용했을 때 Step6을 제외한 모든 결과가 양호했으며, Step6은 데이터 자체가 좋지 않아 예측 결과가 좋지 않았다.

#### 🤖 Prediction Model Application (예측 모델 적용)

기록되지 않은 파이프의 각 단계별 공정 소요 시간을 예측할 수 있는 모델을 개발 및 적용하여, 새로운 파이프가 공정 순서에 포함될 때 최적화가 가능하도록 하였다.

- **📥 입력 변수**: '외경/폭', '두께', '등록 길이' (x, y, z)
- **📤 출력 변수**: Step1~Step6까지의 소요 시간

1. **🛠️ MultiOutputRegressor와 RandomForestRegressor의 조합을 활용한 다중 출력 회귀 모델 구현**
2. **🧠 MLPRegressor**: 다층 퍼셉트론을 기반으로 한 강력한 회귀 모델로, 복잡한 비선형 관계를 학습하여 예측 모델을 구현
3. **🧩 MLP 신경망을 학습하여 예측 모델 구현**

#### ⚙️ Optimization Algorithm Application (최적화 알고리즘 적용)

공정 순서 배열을 최적화하기 위한 다양한 방법을 적용하여 전체 공정 소요 시간을 단축하는 방안을 모색하였다.

1. **🔧 CP-SAT Solver (Constraint Programming - CP)**
   - **📏 Constraint Programming (CP)**: 변수들의 도메인과 제약 조건을 정의하여 해를 찾는 방법으로, 스케줄링, 자원 배분 등의 문제를 해결하는 데 효과적이다.
   - **🧩 SAT Solver**: 논리식의 만족 가능성을 판단하는 알고리즘으로, 복잡한 조합 최적화 문제를 해결하는 데 사용된다.
2. **🏃 Greedy Algorithm**
   - 그리디 알고리즘은 매 단계에서 최적이라고 생각되는 선택을 하는 방식으로, `heapq`를 활용하여 자료구조 알고리즘을 적용함으로써 최적화를 수행

---

### 📊 Hypothesis Testing (가설 검정)

데이터 전처리와 예측 모델을 통해 도출된 소요 시간을 기반으로 공정 배열을 최적화하면, 기존의 공정 소요 시간보다 총 소요 시간을 단축할 수 있다.

---

### ✅ Conclusion (결론)

구축된 데이터 전처리 시스템과 예측 모델을 활용하여 공정 순서 배열을 최적화함으로써 원래 파이프의 총 공정 소요 시간보다 더 빠르게 공정을 완료할 수 있었다. 이는 현대RB의 파이프 제조 공정의 효율성을 크게 향상시킬 수 있음을 시사하며, 향후 공정 최적화에 있어 유용한 방법론으로 활용될 수 있다.

---

### 🔮 Future Work (향후 연구)

- **🔍 KAN 알고리즘**을 적용하여 가장 최적의 예측 모델을 찾는다.
- **🎮 강화학습**을 통해 공정 순서 배열을 최적화한다.

---

## Testing

Google Colab 환경에서 해당 jupyter notebook을 실행하면 원활하게 작업 가능합니다.

```
pip install ortools
```
