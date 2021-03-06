# 데이터분석 캡스톤 디자인 연구

# 주제: 코로나로 인해 변화된 유동인구가 상권에 미친 영향력 분석
#### 소프트웨어융합학과 19학번 류지혜
*** 목차
1) 연구 개요
2) 연구 과제 수행 방법
3) 연구 과제 수행 과정
4) 연구 결과
5) 결론 및 제언

## 1. 연구 개요
코로나로 인해 사회적 거리두기가 시행됨에 따라 거리에 다니는 사람들이 줄어들었다. 2020년 2월의 경우 2019년에 비해 <b>유동인구가 약 85% 줄었다</b>는 것이 확인되었다. 이에 따라 가장 크게 영향을 받은 것은 소상공인들이었다. 매출이 나오지 않아 문을 닫는 가게도 많아졌다. 하지만 타격을 많이 받지 않은 가게들도 분명히 있었다. 그 이유는 매출에 있어서 인구의 영향을 받되 유동인구와 배후인구 모두에 영향을 받아 차이가 생겼기 때문이다. 즉 유동인구가 줄어도 배후인구가 확보된다면 매출의 감소 폭을 줄일 수 있다는 뜻이 된다. 그러나 이는 업종별로 차이가 있다. 예를 들어, 여행업종의 경우 배후인구의 확보가 어렵기 때문에 유동인구가 줄면 타격이 클 수밖에 없다. 이러한 상황을 목격하면서 <b>상권분석에 있어서 매출확보에 유동인구의 영향력을 분석하는 것이 소상공인들의 생존에 있어서 필요하다</b>는 것을 확인해 이에 대한 연구를 진행하게 되었다.


## 2. 연구 과제 수행 방법
1) 데이터 셋 확보
2) 데이터 전처리
3) 업종별 상관분석
4) 업종 기준 군집분석 
5) 군집별 상관분석
6) 군집별 매출 추정


## 3. 연구 과제 수행 과정
### 0) 연구 목표
유동인구의 변화만으로 매출을 예상할 수 있는가?

### 1) 데이터셋
제주도 데이터 허브에서 구한 3가지 데이터를 활용했다.
- 읍면동 단위 내국인 유동인구: API를 이용해서 2017~2019까지의 유동인구 데이터 수집
- 업종별 카드 매출과 매장수 현황: 2017~2019까지의 매출에 대한 데이터 활용
- 유동인구 비율 대비 업종별 매출액 현황: 2020년 유동인구 및 매출에 대한 데이터 활용

### 2) 데이터 전처리
1차 시도에서는 나이대별로 구분된 데이터를 사용하려했으나 데이터수가 너무 적어서 이를 다시 3가지의 나이대별로 진행하였다. 하지만 이도 필요치 않다고 생각하여 나이대에 대한 구분을 제외하였다. 해당 내용은 Capstone_Data_Preprocessing.ipynb에서 확인 가능하다. 데이터의 경우 용량제한으로 인해 전처리 후 데이터만 포함하였다. 전처리 전 데이터의 경우 제주 데이터허브에서 구할 수 있다.

- 유동인구 데이터 전처리</br>
이 데이어틔 경우 일별, 읍면동별, 성별, 나이별로 readPop(거주인구), workPop(근무인구), visitPop(방문인구)로 나뉘어있다. 이를 그대로 사용하기에는 성별과 나이대별은 필요없다고 판단하여 월별, 읍면동별로 인구의 평균으로 전환하였다. (월별로 전환한 것은 매출액 데이터가 월별로 수집되어 있기 때문이다.) <br>
아래 사진은 Final_Pop_M.csv이다.<br>
![image](https://user-images.githubusercontent.com/51522587/123582748-56faef80-d819-11eb-9508-08f59a966bf4.png)


- 매출액 데이터 전처리</br>
사용한 데이터는 '업종별 카드 매출과 매장수 현황' 데이터로 월별(일자는 1일밖에 없어서 의미없음), 읍면동별, 업종별(업종코드 및 업종명), 이용자 구분별, 관광구분별, 연령대별, 성별, 이용금액(매출액), 매장수, 업종명대분류로 데이터가 이뤄져있다. 이 데이터의 경우 법정동명으로 되어있기 때문에 이를 행정구역기준 읍면동 기준으로 전환하는 과정이 필요하다. 또한, 이들 중에서 필요한 이용금액에 대해서 총액을 구하고 매장수 데이터도 같이 처리한다. (만약 위와 같은 구분이 필요하다면 이 데이터를 사용하는 것이 좋지만 매출액에 대한 정보만 필요하다면 '유동인구 비율 대비 업종별 매출액 현황'을 활용하는 것을 추천한다. 이 데이터의 경우 행정구역 기준 읍면동별로 정리되어 있다.) <br>
아래사진은 Final_S.csv이다.</br>
![image](https://user-images.githubusercontent.com/51522587/123582965-bfe26780-d819-11eb-9e13-9753f3412285.png)<br>
아래사진은 Final_M+S_2.csv이다.<br>
![image](https://user-images.githubusercontent.com/51522587/123295520-bffc1200-d550-11eb-8d2d-cb20b6b9ad91.png)

--> 최종: 유동인구 데이터 + 매출액 데이터(38583개)

- 2020년 데이터 전처리</br>
위는 '유동인구 비율 대비 업종별 매출액 현황' 데이터로 읍면동별, 업종별, 남녀별, 매출액, 거주인구, 근무인구, 방문인구로 데이터가 구성되어 있다. 이 유동인구의 경우 매일 24시간대별 데이터의 총합이기때문에 24*30(달별로 차이가 있지만 30으로 통일함)으로 나눠줘야 한다. 또한 남녀 데이터는 필요없으니 구분되어 있는 데이터를 합쳐준다.<br>
위의 사진은 Final_2020.csv이다.<br>
![image](https://user-images.githubusercontent.com/51522587/123295416-a8248e00-d550-11eb-8ab9-1394abd1a68c.png)

--> 최종: 2767개

### 3) 업종별 상관분석
3,4,5,6 과정에 대해서는 Capstone_Analysis.ipynb에서 확인 가능하다. </br>
유동인구라고 하면 거주인구, 근무인구, 방문인구 모두를 일컫는다. </br>

각 업종에 대해서 유동인구와 매출 사이의 상관분석을 진행한다. 업종의 경우 총 40개이다.</br>
![image](https://user-images.githubusercontent.com/51522587/123268702-3e4cba00-d539-11eb-9cdc-b03c0df4a620.png)

업종별 데이터가 300개 미만의 경우 데이터수가 충분하지 않다고 판단하여 제외하였다. 해당 업종은 총 7개로 기타 대형 종합 소매업, 면세점, 내항 여객 운송업, 정기 항공 운송업, 기타 수상오락 서비스업, 기타 갬블링 및 베팅업, 그외 기타 분류안된 오락관련서비스업이다. 이 업종들은 특정 지역에 소수로 포진하고 있어 데이터가 적게 수집되었다고 해석할 수 있다.


### 4) 업종 기준 군집분석
상관계수의 최소값이 0.7이상일 경우 군집분석으로 진행했을 때 매출 추정 정확도가 떨어지는 것을 확인하여 이에 대해서는 군집 1개로 진행하였다. 나머지의 경우 Dendrogram의 결과에 따라 3개 또는 4개로 진행하였다. </br>
군집을 나누는 기준의 경우 상관계수가 가장 높은 feature를 기준으로 하였다. </br>
![image](https://user-images.githubusercontent.com/51522587/123629691-888ead80-d84f-11eb-97cb-446f471b71db.png)


### 5) 군집별 상관분석
군집별 상관분석을 통해 군집을 이루기 전과 비교해 상관계수가 1 또는 -1에 가까워졌음을 확인할 수 있다. </br>
![image](https://user-images.githubusercontent.com/51522587/123294580-f2593f80-d54f-11eb-96d8-fab671d474cd.png)


### 6) 군집별 매출 추정(다중회귀분석)
2017~2019년 까지의 데이터를 train data로 2020년 데이터를 test data로 추정한다. 이에 대해서 군집으로 구성하기 전과 후를 비교한다. 정확도의 경우 R제곱 값을 사용하였다. </br>
![image](https://user-images.githubusercontent.com/51522587/123294725-13ba2b80-d550-11eb-9789-8759931f8043.png)


## 4. 연구 결과
매출액과 유동인구 사이의 상관계수 최소값을 기준으로 3개로 나눠 분석을 진행하였다. 이는 비슷한 경향성을 보이기 때문에 이렇게 나눴다.</br>
결과에 대해서는 Result.xlsx를 참고하길 바란다.
1) 0.7 이상인 경우 </br>
- 해당 업종: 체인화 편의점, 빵 및 과자류 소매업, 한식 음식점업, 비알콜 음료점업</br>
총 4개의 업종이 해당하며 이의 경우 다중회귀분석의 정확도가 평균 약 69%로 이들 업종의 경우 유동인구가 매출에 있어서 69%의 영향력을 갖고 있음을 확인할 수 있다. </br>
또한 이는 읍면동에 구애받지 않고 어느 지역에서든 유동인구와의 강한 연계성을 갖고 있다는 것을 해석할 수 있다.


2) 0.4 이상 0.7 미만의 경우</br>
- 해당업종: 슈퍼마켓, 차량용 주유소 운영업, 중식음식점업, 일식 음식점업, 서양식 음식점업, 기타 외국식 음식점업, 피자/햄버거/샌드위치 및 유사 음식점업 </br>
총 7개의 업종이 해당하며 이의 경우 다중회귀분석의 정확도가 군집분석을 하기 전에 대부분 약 50%였다. 하지만 군집분석을 진행하였을 경우 7개 업종 중 6개에 대해서 4개 그룹 중 1개 또는 2개의 그룹에 대해서 정확도가 향상된 것을 확인할 수 있다. 이는 일부 그룹의 경우 해당 업종에 대해서 매출액과 유동인구 사이의 상관계수가 적은데 전체로 분석되어 매출 추정에 있어서 정확도를 떨어뜨린 것으로 해석할 수 있다. 


3) 0.4 미만인 경우</br>
- 해당업종: 기타 음식료품 위주 종합 소매업, 그외 기타 종합 소매업, 육류소매업, 수산물 소매업, 과실 및 채소 소매업, 건강보조식품 소매업, 차량용 가스 충전업, 화장품 및 방향제 소매업, 호텔업, 여관업, 휴양콘도 운영업, 일반유층 주점업, 기타 주점업, 자동차 임대업, 스포츠 및 레크레이션 용품 임대업, 여행사업, 전시 및 행사 대행업, 골프장 운영업, 그외 기타 스포츠시설 운영업, 욕탕업, 마사지업</br>
총 22개의 업종이 해당하며 이의 경우 다중회귀분석의 정확도가 군집분석을 하기 전에 대부분 30%이하였다. 이중 15개의 업종의 경우 군집분석을 진행하였을 때 4개 그룹 중 3개 이상의 그룹에 대해서(3개 그룹일 경우 2개 이상의 그룹에 대해서) 정확도가 향상된 것을 확인할 수 있다. 이는 지역에 따라 유동인구와 매출액이 갖는 연관 관계가 달라 전체적으로 했을 때는 매출 추정에 대해 적은 정확도를 보이지만 군집으로 진행했을 때는 정확도가 개선된 것으로 해석할 수 있다. 이외 6개의 업종의 경우 4개 그룹 중 1개 또는 2개 그룹에 대해서 정확도가 향상된 것을 확인할 수 있다. 이는 앞서와 마찬가지로 일부 그룹의 경우 해당 업종에 대해서 매출액과 유동인구 사이의 상관계수가 적은데 전체로 분석되어 매출 추정에 있어서 정확도를 떨어뜨린 것으로 해석할 수 있다. <br>


4) 추가</br>
서양식 음식점업과 마사지업 업종의 경우 집단분석을 했을 경우에 오히려 매출 추정 정확도가 떨어졌다. 이는 앞서 상관계수가 0.7이상에서 보였던 것과 마찬가지의 모습이다. 이들은 유동인구가 매출에 있어 각각 53%, 38%의 설명력을 보여주고 있어 1/3이상의 영향력이 있는 것으로 확인된다. 이들의 경우 지역적인 연관성이 아닌 다른 요인에 의해 영향을 받는 것으로 해석할 수 있다.


## 5. 결론 및 제언
1) 결론
- 매출액과 유동인구 사이의 관계는 유의미하나 유동인구만으로는 설명이 불가능하다.
- 군집분석을 해도 정확률이 정확률이 떨어진 경우 유동인구가 아닌 다른 요인에 의한 영향력이 더 크다고 해석할 수 있다. 하지만 이에 대해서는 추가적인 분석이 필요하다.
- 군집분석의 경우 같은 그룹에 속하는 읍면동을 확인하였으나 이에 대해서 구체적인 요인의 경우 분석이 필요하다.

2) 제언
- 연구를 진행하기에 앞서 많은 논문들을 참고하여 설계하였다. 하지만 처음이었던만큼 많은 시행착오가 있었고 데이터분석 방법에 있어서도 최대한의 타당성을 보장하기 위해서 노력하였다. 하지만 부족한 부분이 있다는 것을 충분히 인지하고 있으며 이에 대해서 분석방법에 대해서 더 조사하여 진행할 필요가 있다고 생각된다.</br>
- 군집별로 분류하여 매출을 추정하였는데 추가적으로 각 군집에 대한 추가조사를 진행할 수 있으리라 생각된다. 각 읍면동이 어떤 면에서 밀접한 관계를 맺게 되었는 지에 대해서 분석을 한다면 부족한 정확도에 대해서 개선할 수 있을 것이라 생각하기 때문이다. <br>
- 분석결과에 대해서 예외적으로 나타난 결과에 대해서 추가적인 분석이 필요할 것으로 생각된다. 이는 유동인구만으로는 설명하기 힘들다고 판단된다. <br>

