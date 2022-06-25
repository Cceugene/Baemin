# 배달의 민족 사장님 사이트를 이용한 데이터 분석
## Why?
개인적인 상황에 맞는 데이터 분석이 뭐가 있을까 고민하다가 부모님이 운영하시는 가게의 데이터를 이용해서 무언가 해볼 수 있을 것 같다는 생각을 했다. 부모님께 여쭤본 결과 가게 주문 데이터, 리뷰 내역, 매출 데이터 등이 사장님 사이트라는 곳에서 제공되고 있었다. 그것들을 분석해서 가게 매출 또는 운영에 도움이 되면 좋을 것 같았다.

## HOW?
웹 스크래핑
 * selenium
 * webdriver

데이터 전처리 및 탐색
 * pandas

데이터 분석
 * Apriori
 * sklearn

# 데이터 분석
## 데이터 수집 및 탐색
raw 데이터가 배달의 민족 사이트에서 제공되지 않기때문에 웹스크래핑을 이용해서 데이터를 긁어왔다.\
[웹 스크래핑 코드](https://github.com/Cceugene/review/blob/baemin/crawling_baemin.ipynb)

## 시각화
주문내역의 날짜 데이터를 이용해 날짜/시간별 주문량을 분석했다.
* 각 메뉴의 요일별 주문량
<img width="800" alt="요일별 주문량" src="https://user-images.githubusercontent.com/90452911/175068632-1b2cce9f-987f-4158-b590-0177bd64dcc4.PNG">
치밥의 경우 주간/주말 주문량의 차이가 적음에도 다른 메뉴에 비해 주문량이 적게 나타났다.

* 시간대별
<img width="800" alt="시간대별_주문량" src="https://user-images.githubusercontent.com/90452911/175282168-de403f8b-2ffb-404b-a650-9c18f93d4161.PNG">

* 메뉴별 주문 시간대
<img width="800" alt="메뉴별 주문 시간대" src="https://user-images.githubusercontent.com/90452911/175574357-9f19a846-2f13-41b4-8746-92acf91a3d3e.PNG">


## 리뷰 분석
 치밥에 대한 소비자의 반응을 알아보기 위해 리뷰 데이터 분석을 시행했다.
 먼저 Kaggle의 주문 데이터를 이용해 모델을 학습시키고, 앞서 크롤링으로 수집 한 가게 리뷰 데이터를 사용해 고객들의 반응을 분석했다.\
[학습에 이용한 Kaggle 데이터](https://www.kaggle.com/datasets/ninetyninenewton/kr3-korean-restaurant-reviews-with-ratings)\
[감성분석 코드](https://github.com/Cceugene/review/blob/baemin/Sentimental_Analysis.ipynb)

 분석 결과 치밥 메뉴의 부정 리뷰의 키워드를 빈도수로 시각화한 결과 '닭고기'와 '누린내'라는 단어가 나타났다. 따라서 치밥의 판매가 저조한 원인이 닭고기의 누린내라는 것을 확인했다.

 치밥의 누린내 문제에 대응하는 방식을 두 가지로 나누어 **연관규칙 마이닝**과 **품질관리**를 사용하기로 했다. 품질관리는 누린내 문제를 해결해 단골손님의 유실을 막는 것, 연관분석 마이닝은 주간, 점심시간대에 많이 팔리는 치밥의 특성을 이용해 새로운 고객층을 늘리는 것을 목표로 하였다.

## 연관규칙 마이닝
 치밥의 주문내역을 분석하여 점심시간대 주문 특징을 찾아내고, 판매량을 늘릴 방안을 탐색하기 위해 연관규칙 마이닝을 시행하였다.
 그 결과 점심시간대 주문이 음식량에 영향을 끼친다는 것은 치밥의 주중 오전 주문 비중이 높은 것이 직장인들의 점심식사 때문이라는 추론을 뒷받침했다. 또한, '직화양념 치밥 도시락'과 음식량 추가 선택이 점심 주문에 영향을 준다는 사실을 발견했으며, __직화 양념의 양 조절이 용이하게 하는 등__ 의 변화를 통해 점심시간 주문량이 증가할 것으로 기대된다.
[치밥 메뉴 ARM 코드](https://github.com/Cceugene/Baemin/blob/master/05_ARM/ARM_chibab.ipynb)

주문 내역에서 뽑은 장바구니 목록을 Apriori를 통해 연관규칙을 얻어냈다.\
[ARM 코드](https://github.com/Cceugene/review/blob/baemin/ARM_bascket_Tako.ipynb)


