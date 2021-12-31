# practice
각 과제 설명
+1. Titanic Survive<br />
  +(1) 사용된 data set : kaggle의 타이타닉 생존자 예측 데이터<br />
  +(2) Preprocessing<br />
  (가) 동행자의 수를 합쳐서 새로운 특징값 생성<br />
  (나) 혼자 탑승한 사람을 따로 분류하여 특징값 생성<br />
  (다) Age 피쳐의 결측값은 이름의 호칭(miss, mr, mrs, other)을 활용. 각 호칭 별 age값의 평균값으로 결측값을 채움<br />
  (라) 나머지 결측값은 각 피쳐의 최빈값, 중앙값으로 대체<br />
  (마) Age와 Fare 피쳐는 구간화한 후 라벨인코딩<br />
  (바) pclass, name, sex, embarked 피쳐는 target encoding<br />
(3) Model : RandomForest를 활용<br />

2. House Prices<br />
(1) 사용된 data set : kaggle의 집값 예측 데이터<br />
(2) preprocessing<br />
  (가) 결측값은 데이터 누락으로 발생하였다고 가정하고 data_description.txt 파일을 통해 누락된 값으로 처리.<br />
  몇몇 feature들은 중간값이나 최소값 등으로 대체<br />
  (나) outlier는 feature와 target간의 scatter plot을 직접 보고 제거<br />
  (다) feature들을 조합하여 새로운 feature 생성<br />
  (라) 하나의 값이 지나치게 많은 feature는 해당 열을 삭제<br />
  (마) 분포가 한쪽으로 치우쳐진 수치형 변수는 log 변환을 수행<br />
  (바) cardinality가 큰 categorical feature는 target encoding을 적용. 그 외에는 one hot encoding을 적용<br />
(3) Model : Elastic net과 XGBoost 두개의 모델을 만들고 각 예측값의 weight mean을 계산하여 최종 예측값으로 결정<br />

3. 심장 질환 예측 경진대회<br />
(1) 사용된 data set : DACON BASIC의 심장 질환 예측 데이터를 사용(주소 : https://dacon.io/competitions/official/235848/overview/description)<br />
(2) Preprocessing<br />
  (가) train data의 target값에 따른 분포의 차이를 시각화<br />
  (나) target값에 따른 분포의 차이를 보고 이를 통해 각 feature들을 구간으로 나눈 feature들을 생성<br />
  (다) 수치형 변수는 log변환 수행<br />
(3) Dimension reduction : PCA를 적용하여 차원축소<br />
(4) Model : KNN을 활용하였고 GridsearchCV를 활용하여 적절한 hyperparameter를 찾음<br />
(5) 결과 : Public(7등, score : 0.93939), Private(13등, score : 0.86)<br />
(6) 후기 : train data만 가지고 여러 모델에 적용했을 때에는 logistic regression이 knn보다 더 좋은 점수를 기록하였다<br />
하지만 public점수는 logistic regression보다 knn이 더 좋은 점수를 기록했기에 이를 선택하였다. <br />
그러나 public 점수는 test데이터의 일부만 가지고 평가를 하기에 public 점수만 가지고서 knn이 logistic regression보다 좋은 성능을 낸다고 생각한 것은 잘못된 판단이었다.<br />
만약 logistic regression을 사용했으면 더 높은 순위를 기록했을 수도 있었을 것이라 생각한다.<br />

4. HAM10000 데이터(XGB, CNN 사용)<br />
(1) 사용된 data set : Skin Cancer MNIST: HAM10000<br />
  데이터 설명 :  https://www.kaggle.com/kmader/skin-cancer-mnist-ham10000 참조<br />
  데이터 구성 : 피부병 image 데이터와 각 이미지에 대한 metadata로 구성<br />
  목표 : 피부병 진단(data set에서 dx값을 예측)<br />
(2) EDA 및 전처리<br />
  (가) target 값의 분포는 균등하지 않음(imbalanced data)<br />
  (나) 환자의 정보가 있는 metadata를 통해 EDA실시<br />
  (다) 각 feature값에 따라 target값의 분포가 조금씩 차이를 보이므로 metedata를 활용한 예측도 가능할 것으로 추측<br />
  (라) 결측값이 존재하므로 이를 처리(Age 변수). dx_type값의 consensus, histo값의 age 평균값으로 대체.<br />
  (마) metadata에 이미지 정보를 추가하기 위해 각 이미지의 RGB채널과 Gray채널의 평균,최대,최소,표준편차 값을 추가<br />
  (바) class가 불균형하므로 이미지 데이터는 data augmentaion을 적용. class가 더 적은 쪽에 더 많은 augmentation을 적용하여 더 많은 데이터를 확보<br />
  (사) 원본 이미지 파일은 크기가 너무 크므로(600x450) 학습에는 이를 축소하여(50x50) 사용.<br />
(3) Model : metadata에는 XGBoost를 적용. 이미지 데이터는 CNN을 적용<br />
(4) 결과 : XGBoost는 accuracy 0.79, F1-macro 0.585, CNN은 accuracy 0.76, F1-macro 0.57 기록<br />
(5) 총평 : CNN모델의 경우 data augmentation을 적용하기 전과 적용한 후를 비교하면 accuracy는 크게 향상되지는 않았으나 F1-macro는 적용한 후에 크게 증가하였다.<br />
질병을 판단하는데 있어서 환자의 metadata는 예측에 큰 도움을 줄 수 있다.<br />
이를 활용하여 metadata와 image 데이터, 혹은 그 이외의 데이터를 더 수집한 후 여러 모델을 생성하고 voting classifier를 적용하는 것도 고려할 수 있다.<br />

