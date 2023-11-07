# K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition
2022년 k-인공지능 제조데이터 분석 경진대회

# 용해탱크 AI 데이터 셋 분석진행

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

  <img width="242" alt="통계 사진" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/e8ca5536-3931-4857-9832-9337fcfa5bed">

* MELT_TEMP : MELT_TEMP의 MAX의 경우 이상치가 존재함
    
* MOTORSPEED
  >* MIN : 설비를 중지한 경우 0으로 표기
  >* MAX : 180이 넘는 이상치가 존재함

* MELT_WEIGHT : Std의 경우 1,217로 차이가 큰 편임  

* INSP : Std가 0으로 데이터들이 고르게 분포되어 있음

## HISTOGRAM

![image](https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/a4ff7998-bf7e-45c6-95ff-f4a41daaad3c)

<img width="484" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/6c63b26d-d4b3-4231-90b4-976cd5e1bdcd">

* MELT_TEMP : 비교적 균형적인 분포를 보이고 있음

* MOTORSPEED : 데이터가 극단적으로 분포되어 있음

* MELT_WEIGHT : 데이터가 한쪽으로 치우쳐 있음

* INSP : 비교적 균형적인 분포를 가지고 있음

* TAG : 클래스 불균형이 보임 → 이후 전처리 작업을 통해 보완해야함

## PATTERN ANALYSIS

<img width="244" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/226a3937-e0a5-4768-9c86-94ef4a7521e4">

* MELT_TEMP, MOTORSPEED : 비교적 일정한 패턴을 보이고 있음

* MELT_WEIGHT : 지속적으로 감소하는 패턴을 보이고 있음

* INSP : 비교적 불규칙적인 패턴을 보이고 있음

## CORRELATION ANALYSIS(PEARSON)

<img width="419" alt="image" src="https://github.com/shinho123/K-Artificial-Intelligence-Electronic-Manufacturing-Data-Analysis-Competition/assets/105840783/e6496099-7ef5-4a04-8268-6d30d6539c7a">

* 종속변인 'TAG'와 독립변수들과 상관분석을 수행하였을 때, 'MELT_WEIGHT'를 제외하고는 모두 양의 상관관계를 가지고 있음

* 'TAG'는 'MELT_WEIGHT'를 제외한 나머지 변수들과 선형 관계를 갖는다고 볼 수 있음

* 또한 상관관계는 낮게 나오지만 어떤 영향을 미치는지 알아보기 위해 'MELT_WEIGHT'를 포함하여 모델 분석을 수행함함
