# 용해탱크 AI 데이터 셋 분석진행

## 프로젝트 수행기간 : 2022.10.10 ~ 2022.10.21
## 역할 : 코드 작성, 모델 개발

## 용해혼합 공정이란 : 
  * 분말 원재료를 액상 원재료에 녹이는 공정을 의미함

## 분석 목적
  * 용해혼합 공정 과정에서 가장 큰 영향을 주는 요인(변수)들을 식별함
  
  * 공정을 최적화 할 수 있는 분류 모델 생성

## 분석 목표
  * EDA(Exploratory Data Analysis), VI(Variable Importance)등을 통해 공정 과정에 영향을 주는 변수들을 분석함
  
  * Machine Learning, Deep Learning(Sequence) model등을 사용해보고 성능을 비교함

## EDA(Exploratory Data Analysis)
  ### 데이터 구조
<img width="524" alt="데이터 사진" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/57bbfca8-f275-46ec-a584-760b2a8c32a4">

* 총 6개의 Attribute로 구성됨
* 4개의 독립변수와 1개의 종속변수로 구분됨
  >* 독립 변수 : MELT_TEMP, MOTORSPEED, MELT_WEIGHT, INSP
  >* 종속 변수 : TAG
* STD_DT : 시계열 모델 생성을 위한 데이터 셋의 인덱스로 사용함

## SUMMARY STATISTICS
  
  ### 요약 통계 분석

```python
df[['MELT_TEMP', 'MOTORSPEED', 'MELT_WEIGHT', 'INSP']].describe()
```

  <img width="242" alt="통계 사진" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/e8ca5536-3931-4857-9832-9337fcfa5bed">

* MELT_TEMP : MELT_TEMP의 MAX의 경우 이상치가 존재함
    
* MOTORSPEED
  >* MIN : 설비를 중지한 경우 0으로 표기
  >* MAX : 180이 넘는 이상치가 존재함

* MELT_WEIGHT : Std의 경우 1,217로 차이가 큰 편임  

* INSP : Std가 0으로 데이터들이 고르게 분포되어 있음

## HISTOGRAM

```python
plt.figure(figsize = (12, 12))
for i in range(len(col_name)):
    num = 231 + i
    plt.subplot(num)
    plt.hist(df[col_name[i]])
    plt.xticks(rotation = 45)
    plt.title(col_name[i])
```

![image](https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/a4ff7998-bf7e-45c6-95ff-f4a41daaad3c)

<img width="484" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/6c63b26d-d4b3-4231-90b4-976cd5e1bdcd">

* MELT_TEMP : 비교적 균형적인 분포를 보이고 있음

* MOTORSPEED : 데이터가 극단적으로 분포되어 있음

* MELT_WEIGHT : 데이터가 한쪽으로 치우쳐 있음

* INSP : 비교적 균형적인 분포를 가지고 있음

* TAG : 클래스 불균형이 보임 → 이후 전처리 작업을 통해 보완해야함

## PATTERN ANALYSIS

```python
plt.figure(figsize = (12, 12))
for i in range(len(col_name)):
    num = 221 + i
    plt.subplot(num)
    plt.plot(df[col_name[i]][0:120])
    plt.xticks(rotation = 45)
    plt.title(col_name[i])
plt.subplots_adjust(left = 0.125, bottom = 0.1, right = 0.9, top = 0.9, wspace = 0.2, hspace = 0.35)
plt.show()
```

<img width="340" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/0f60d346-539a-4420-9973-50fdcd3594ed">

* x축은 초 단위이며, 1초당 10개의 timestep을 가지고 있음

* 본 그래프에서는 1수집주기(6초)기준으로 구간을 설정함

* MELT_TEMP, MOTORSPEED : 비교적 일정한 패턴을 보이고 있음

* MELT_WEIGHT : 지속적으로 감소하는 패턴을 보이고 있음

* INSP : 비교적 불규칙적인 패턴을 보이고 있음

## CORRELATION ANALYSIS(PEARSON)

```python
corr = df.corr(method = 'pearson')
corr
```

<img width="419" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/e6496099-7ef5-4a04-8268-6d30d6539c7a">

* 종속변인 'TAG'와 독립변수들과 상관분석을 수행하였을 때, 'MELT_WEIGHT'를 제외하고는 모두 양의 상관관계를 가지고 있음

* 'TAG'는 'MELT_WEIGHT'를 제외한 나머지 변수들과 선형 관계를 갖는다고 볼 수 있음

* 또한 상관관계는 낮게 나오지만 어떤 영향을 미치는지 알아보기 위해 'MELT_WEIGHT'를 포함하여 모델 분석을 수행함함

## MODELING

 ### DATA PREPROCESSING

<img width="513" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/cee98b5d-55c6-4f03-b151-b6b5538517cf">

## MODELING - DEEP LEARNING

 ### MODEL VERIFICATION - LSTM
 
<img width="452" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/471f111d-78a9-4bfa-b907-485987870b40">

* F1-SCORE : 0.89

* ACCURACY : 0.80

* PRECISION : 0.99

* RECALL : 0.80

## MODELING - MACHINE LEARNING

 ### MODEL VERIFICATION - CONFUSION MATRIX

 <img width="510" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/beb988d3-1971-4901-bde7-5686f4105a5c">

* 성능 순서 : Xgboost → Randomforest → Decision tree → Lightgbm → Catboost

## MODELING - FEATURE IMPORTANCE

 ### VI(Variable Importance) - Permutation Importance

```pyhon
perm = PermutationImportance(dsc).fit(x_test_values, y_test_values)
eli5.show_weights(perm, top = 4, feature_names = train_set_feature.columns.tolist())

perm = PermutationImportance(clf).fit(x_test_values, y_test_values)
eli5.show_weights(perm, top = 4, feature_names = train_set_feature.columns.tolist())

perm = PermutationImportance(xgb, scoring = "accuracy").fit(x_test_values, y_test_values)
eli5.show_weights(perm, top = 4, feature_names = train_set_feature.columns.tolist())

perm = PermutationImportance(cb, scoring = "accuracy").fit(x_test_values, y_test_values)
eli5.show_weights(perm, top = 4, feature_names = train_set_feature.columns.tolist())

perm = PermutationImportance(lgbm, scoring = "accuracy").fit(x_test_values, y_test_values)
eli5.show_weights(perm, top = 4, feature_names = train_set_feature.columns.tolist())
```

 <img width="372" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/19155bc5-a788-4f0c-9955-b47002d1179b">

* Feature Selection(→ 중요도 순서) :
 
 * MELT_TEMP → MELT_WEIGHT → INSP → MOTORSPEED

## CONCLUSION

* 본 프로젝트에서는 용해탱크 데이터 셋을 통해 총 2가지의 분석 목표를 가짐
 1. 설비운영값과 주요 품질검사항목의 결과값을 통해 생산품질을 예측할 수 있는 모델을 생성 후 검증을 진행함
 2. 생산품질에 영향을 주는 여러 요인들을 분석함

* EDA
 Correlation : **MELT_WEIGHT(용해탱크 내용중량)** 독립변수가 종속변수와 가장 관련이 없는 것으로 보였음

 Pattern : **INSP(수분함유량)**이 가장 불규칙한 패턴을 보였음

* MODELING : Deep Learning > Machine Learning
 Deep Learning : LSTM
 Machine Learning : Xgboost → Randomforest → Decision tree → Lightgbm → Catboost
 VI-Permutation Importance : MELT_TEMP, MELT_WEIGHT

* 설비운영값과 주요 품질검사항목의 결과값을 통해 분석을 진행한 결과 예측 모델에서는 Deep Learning(LSTM)의 성능이 가장 우수하였고, EDA에서는 **MELT_WEIGHT**가 종속변수와 관련 없는 변수로 도출되었으나 Machine Learning 모델의 VI를 통해 MELT_TEMP, MELT_WEIGHT의 독립변수가 성능 변화에 가장 큰 영향을 주는 요인으로 확인됨
