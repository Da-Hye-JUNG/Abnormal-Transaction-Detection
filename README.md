# 신용카드 이상 거래 탐지 프로젝트
비어플 학회 팀 프로젝트

# 들어가며

안녕하세요. 비어플에서 `신용카드 이상거래 탐지 분석`을 진행한 2조 신보람 최다희 정다혜 박지원입니다.

# 프로젝트 개요

## 프로젝트 목표

<aside>
💡 2013년 9월 유럽의 카드 소유자들의 **신용카드 거래 데이터를 분석**하고 **이상 거래 여부를 판단**한다

</aside>

수립한 이상여부를 예측하는 모델을 더 고도화하여 **신용카드 부정 사용 및 손실 방지에 적용**함으로써 고객의 신뢰를 얻고 보호하는데 기여할 수 있다.

## 사용 데이터 소개

![image](https://github.com/Da-Hye-JUNG/Abnormal-transaction-detection/assets/96599427/1eb8537c-32c8-4001-b28d-50390d47adeb)


2013년 9월 유럽에서 2일동안 카드 소유자들의 신용카드 거래 정보를 나타낸 데이터이며 첫 거래 시간을 기준으로 얼마나 차이가 나는지를 초단위로 나타낸 time변수, 금융거래 정보로 pca처리가 된 28개의 V변수 그리고 거래금액을 나타낸 amount변수 마지막으로 이상거래 여부를 나타낸 Target변수로 구성되어 있다.

![image](https://github.com/Da-Hye-JUNG/Abnormal-transaction-detection/assets/96599427/8ce3f6f9-985f-4b77-bdba-234008ac8c3f)


31개의 변수로 구성되어 있으며 정상거래가 284,315건(99.8%), 이상거래가 492건(0.172%)로 매우 불균형을 보인다.

# 프로젝트 수행 과정

## 전처리 방안

### ✅ Time 변수를 통해 Hour 변수 생성

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97ae9738-716c-41e9-8d6a-2e66dca980b9/a82e6975-c4ab-42e0-a07e-5e97b8ad0c6d/Untitled.png)

1일차와 2일차의 히스토그램이 비슷한 형태를 띠고 있어 시간대 변수인 hour변수 생성을 하였다. 여기서 낮은 거래량을 보이는 특정 시간은 거래량이 낮은 새벽으로 추측할 수 있다. time변수가 초단위로 나타나 있음을 이용하여 time변수로 0부터 23까지의 1일차 hour변수를 생성하고 같은 방법으로 0부터 23까지의 2일차 hour변수를 생성했다.

![생성된 Hour 변수 그래프](https://prod-files-secure.s3.us-west-2.amazonaws.com/97ae9738-716c-41e9-8d6a-2e66dca980b9/adbbb789-5e56-4673-8d04-7b957b66e37b/Untitled.png)

생성된 Hour 변수 그래프

생성된 Hour변수에 따른 이상거래와 정상거래의 비율을 나타낸 결과, Hour가 2일 때 이상거래의 비율이 가장 높음을 확인하였다.

### ✅ V변수 제거

①   각 주성분들의 표준편차 제곱 값이 0.7 미만

> <주성분 개수 결정을 다음과 같이 한 이유>

주성분 개수 결정 방법(누적비율 값, **개별 고윳값**, 스크리 플롯 참조) 중에서 주성분의 수가 많고 종속 데이터가 심한 불균형을 보이기 때문에 **정보 손실을 최소화**하기 위해 결정
> 

| 변수명 | 표준편차 제곱 값 | 변수명 | 표준편차 제곱 값 |
| --- | --- | --- | --- |
| V1 | 3.836476 | V15 | 0.837800 |
| V2 | 2.726810 | V16 | 0.767816 |
| V3 | 2.299021 | V17 | 0.721371 |
| V4 | 2.004677 | V18 | 0.702537 |
| V5 | 1.905074 | V19 | 0.662660 |
| V6 | 1.774940 | V20 | 0.594323 |
| V7 | 1.530395 | V21 | 0.539524 |
| V8 | 1.426474 | V22 | 0.526641 |
| V9 | 1.206988 | V23 | 0.389950 |
| V10 | 1.185590 | V24 | 0.366807 |
| V11 | 1.041851 | V25 | 0.271730 |
| V12 | 0.998400 | V26 | 0.232542 |
| V13 | 0.990567 | V27 | 0.162919 |
| V14 | 0.837800 | V28 | 0.108955 |

→ V19 ~ V28 변수

②   Class에 따른 histogram 유사한 변수

![image](https://github.com/Da-Hye-JUNG/Abnormal-transaction-detection/assets/96599427/60231134-1559-4b12-b27e-57624822daea)


![image](https://github.com/Da-Hye-JUNG/Abnormal-transaction-detection/assets/96599427/c39aa967-b248-470d-9e41-21b3d6ff3bdc)


→ V13, V15, V20 ~ V28 변수

③   Class에 따른 box plot 유사한 변수 

![image](https://github.com/Da-Hye-JUNG/Abnormal-transaction-detection/assets/96599427/36e897bc-32b9-4c3a-9362-0717efd20842)


![image](https://github.com/Da-Hye-JUNG/Abnormal-transaction-detection/assets/96599427/9949c861-1d6b-4bab-b895-ab540b61592f)


→ V8, V13, V15, V20 ~ V28변수

<aside>
🗣️ 세가지 조건을 모두 만족하는 V20 ~ V28변수 제거

</aside>

### ✅ Amount 변수 Robust scaling

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97ae9738-716c-41e9-8d6a-2e66dca980b9/4ac88662-aa25-47c8-be5b-11bc6d11afe7/Untitled.png)

거래 금액을 나타내는 amount변수는 범위가 0부터 2만5691로 매우 큰 범위를 가지고 있음을 확인하였다. 이 변수는 다른 변수들에 비해 영향력이 커 해석할 때 어려움이 있을 수 있다. 따라서 amount변수의 범위를 줄이기 위해 로버스트 스케일링을 진행하였다.
로버스트 스케일링은 중앙값과 IQR을 사용한 scaling으로 outlier의 영향을 최소화할 수 있다는 장점이 있다. 우리 조는 outlier가 class를 분류하는 지표가 될 수 있다고 판단하여 outlier를 제거하지 않았고 대신 outlier의 영향을 최소화하기 위해 스케일링 방법 중 로버스트 스케일링을 선택하게 되었다.

## 모델링

### ✅ SMOTE TOMEK + 랜덤포레스트

> SMOTE TOMEK

SMOTE방법과 TOMEK방법이 결합된 샘플링 방법
SMOTE방법 : 수가 적은 클래스의 점을 하나 선택해 k개의 가까운 데이터 샘플을 찾아 그 사이에 새로운 점을 생성하는 방법
TOMEK방법 : 가까이 붙어 있는 서로 다른 클래스의 데이터 중 다수 클래스에 속하는 데이터를 삭제하는 방법
> 

모델의 성능은 다음과 같다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97ae9738-716c-41e9-8d6a-2e66dca980b9/18992091-5332-49f2-b625-4d8211e2ff4d/Untitled.png)

정확도는 0.99 f1score는 0.88로 과적합의 가능성이 있다.

### ✅ One Class SVM

> One Class SVM

이진 분류에서 하나의 클래스만 학습시켜 불균형 데이터를 예측하는 비지도 학습 알고리즘
> 

모델의 성능은 다음과 같다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97ae9738-716c-41e9-8d6a-2e66dca980b9/fae96099-999f-416c-9c49-0a5ba578dd56/Untitled.png)

정확도는 0.92 재현율은 0.92 정밀도는 0.99 f1score는 0.96으로 모두 0.92이상의 값을 가지는 것을 확인하였다. **모든 평가지표에서 0.92 이상**의 점수를 달성하며 불균형 문제를 해결한 **One Class SVM** 모델을 최종모델로 선정하였다.   

# 대회 팀 구성 및 역할

신보람 - 데이터 엔지니어링 및 모델링

최다희 - 데이터 엔지니어링 및 모델링

정다혜 - 데이터 엔지니어링 및 모델링

박지원 - 데이터 엔지니어링 및 모델링

## 기술 스택

Data&ML - R, RStudio

## 한계점

### 1. 종속변수의 범주 불균형

불균형을 해결하기 위해서 sampling을 하고 모델링을 진행하였지만 과적합을 피할 수 없었다.

### 2. 변수에 대한 정보 부족

이미 pca처리된 데이터가 대부분으로 더 효과적인 분석이 어려웠다.

# 감사합니다.

---

지금까지 비어플 이상거래 2조였습니다. 감사합니다.
