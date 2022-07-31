# 우리가게 치밥 판매량 늘리기
* 개인 프로젝트
* 기간: 2022년 4월 1일 ~ 2022년 00월 00일
## 개요
현재 가게에서 판매중인 메뉴는 국밥, 냉면, 타코야끼, 치밥 총 네 가지입니다. 이 중 타코야끼와 치밥의 판매가 상대적으로 적습니다. 특히 최근 치밥의 닭고기에서 누린내가 난다는 리뷰가 달리고 있다고 합니다.\
이에 따라 본 프로젝트의 목표는 배달의 민족 사장님 사이트에서 제공되는 가게 주문 및 리뷰 내역을 통해 현재 가게의 문제점을 파악하고 개선 방안까지 제시하는 것입니다.

## 기술스택
**웹 크롤링**
 * selenium
 * BeautifulSoup

**데이터 전처리 및 시각화**
 * pandas
 * matplotlib
 * seaborn

**데이터 분석**
 * Apriori
 * sklearn

# 데이터 분석
## EDA를 통한 문제 정의
### 데이터 수집 및 전처리
배달의 민족 사이트에서는 가게에 대한 데이터가 파일로 제공되지 않기때문에 크롤링을 통해 데이터를 긁어 와야 합니다. 대학교에서 운영하는 메이저러너 프로그램과 웹 프로그래밍 과목에서 배운 웹 크롤링 기법과 HTML5 문법, 그리고 따로 공부 한 selenium을 통해 사이트에서 가게 주문 내역과 리뷰를 데이터로 가져올 수 있었습니다.\
[웹 크롤링 코드](https://github.com/Cceugene/review/blob/baemin/crawling_baemin.ipynb)\
[전처리 코드]()

### 데이터 탐색
수집 한 데이터를 각 메뉴별로 분석해 보았습니다.
* 각 메뉴의 요일별 주문량
<img width="800" alt="요일별 주문량" src="https://user-images.githubusercontent.com/90452911/175068632-1b2cce9f-987f-4158-b590-0177bd64dcc4.PNG">
치밥의 경우 주간/주말 주문량의 차이가 적음에도 다른 메뉴에 비해 주문량이 적게 나타났습니다.

* 시간대별
<img width="800" alt="시간대별_주문량" src="https://user-images.githubusercontent.com/90452911/175282168-de403f8b-2ffb-404b-a650-9c18f93d4161.PNG">

* 메뉴별 주문 시간대
<img width="800" alt="메뉴별 주문 시간대" src="https://user-images.githubusercontent.com/90452911/175574357-9f19a846-2f13-41b4-8746-92acf91a3d3e.PNG">
다른 메뉴는 식사 시간대에 판매량이 높은 반면, 타코야끼는 늦은 저녁 9시, 치밥은 11시경에 판매가 가장 높았습니다.

치밥과 타코야끼의 경우 판매가 저조하며 특히 치밥은 일반 식사메뉴와 다른 특성을 가지고 있다는 것을 알았습니다.\
이에 따라 치밥의 판매를 개선하기 위한 문제를 다음과 같이 정의했습니다.
1. 유입 고객에 대한 구매 활성화\
    → 구매 데이터로부터 고객층의 특징을 분석
2. 기존 고객의 재구매로 이어지지 않는 원인 발견 및 개선\
    → 구매 데이터 및 리뷰 분석

[문제정의 EDA 코드]()

## 문제1. 치밥과 타코야끼의 특성
### 1-1. EDA
치밥의 주 구매 고객이 누구이며, 배달 주문에서 어떤 특성을 갖는지 밝히기 위해 탐색적 데이터분석을 실행했습니다. 그 결과는 다음과 같습니다.

<img width="410" alt="치밥 요일별 판매량" src="https://user-images.githubusercontent.com/90452911/182031192-569bfdac-d53d-4ff8-9bb3-a0ba6243764f.PNG">

치밥은 평일과 주말의 구분이 없으며, 월요일 주문이 가장 많다.

<img width="410" alt="치밥 시간대별 금액 분포" src="https://user-images.githubusercontent.com/90452911/182031209-d6be971a-acd5-4eab-8e58-026b8e08955f.PNG">

치밥의 시간대별 금액 분포를 보면 11시경에 높은 금액의 주문량이 많다.

<img width="800" alt="메뉴별_진짜전체1" src="https://user-images.githubusercontent.com/90452911/182031280-cca0ae06-9be4-419b-ba80-c8a86accdfa9.PNG">
<img width="800" alt="메뉴별_진짜전체2" src="https://user-images.githubusercontent.com/90452911/182031302-75379ffc-e0e6-4c39-bf15-94fffa7b4b17.PNG">

시간대별 주문량을 메뉴별로 나누어 보았을 때, '매콤양념', '매콤간장', '고기만 2배' 메뉴는 주문이 적고, 다른 메뉴와 피크타임이 다르다.

<img width="800" alt="메뉴별_전체1" src="https://user-images.githubusercontent.com/90452911/182031312-588d1fe9-c1d4-4451-945b-a5d7e37fe4aa.PNG">
<img width="800" alt="메뉴별_전체2" src="https://user-images.githubusercontent.com/90452911/182031315-4c02c0e4-12d1-4ff8-b26b-41ad667576bd.PNG">

평일 주문의 경우 역시 위 세 메뉴에 대한 패턴이 다른 메뉴와 다르게 나타났다.

- 치밥의 주 고객층은 평일 11시경 주문자임
- 치밥의 주 고객층은 매운맛이나 양이 많은 메뉴가 아닌 가벼운 메뉴를 선호함
- 따라서 직장인 또는 학생일 가능성\

**문제 해결 방안**
> 치밥의 주문량을 늘리려면 메뉴를 점심식사에 맞는 가벼운 메뉴로 구성하고, 리뷰이벤트 등의 서비스 메뉴를 기름진 튀김류 보다는 음료 등으로 가벼운 메뉴를 선택하는 것이 좋음
예) 직장인을 타겟으로 리뷰이벤트 메뉴로 아이스 아메리카노를 제공한다.

[치밥에 대한 주문 특성 EDA 코드](https://github.com/Cceugene/Baemin/blob/master/03_EDA/%5BEDA%5D%20Problem1_%EC%B9%98%EB%B0%A5%EC%9D%98%20%ED%8A%B9%EC%A7%95.ipynb)

## 문제2. 치밥 최초 구매가 재구매로 이어지지 않는 이유
### 2-1. EDA
[코드]()

### 2-2. 리뷰 분석
치밥에 대한 소비자의 반응을 알아보기 위해 감성분석 기법을 이용해 리뷰를 분석했습니다.

먼저 Kaggle의 배달 주문 데이터를 이용해 모델을 학습시키고, 앞서 크롤링으로 수집한 가게 리뷰 데이터를 사용해 고객들의 반응을 분석했습니다.\
[학습에 이용한 Kaggle 데이터](https://www.kaggle.com/datasets/ninetyninenewton/kr3-korean-restaurant-reviews-with-ratings)\
[감성분석 코드](https://github.com/Cceugene/review/blob/baemin/Sentimental_Analysis.ipynb)

분석 결과 치밥 메뉴의 부정 리뷰의 키워드를 빈도수로 시각화한 결과 '닭고기'와 '누린내'라는 단어가 나타났습니다. 따라서 치밥의 판매가 저조한 원인이 닭고기의 누린내라는 것을 확인했습니다.

### 2-3. 연관규칙 마이닝
치밥의 주문내역을 분석하여 점심시간대 주문 특징을 찾아내고, 판매량을 늘릴 방안을 탐색하기 위해 연관규칙 마이닝을 시행하였다.
그 결과 점심시간대 주문이 음식량에 영향을 끼친다는 것은 치밥의 주중 오전 주문 비중이 높은 것이 직장인들의 점심식사 때문이라는 추론을 뒷받침했다. 또한, '직화양념 치밥 도시락'과 음식량 추가 선택이 점심 주문에 영향을 준다는 사실을 발견했으며, _직화 양념의 양 조절이 용이하게 하는 등_ 의 변화를 통해 점심시간 주문량이 증가할 것으로 기대된다.\
[치밥 메뉴 ARM 코드](https://github.com/Cceugene/Baemin/blob/master/05_ARM/ARM_chibab.ipynb)
