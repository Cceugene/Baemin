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

## 연관분석 마이닝
주문 내역에서 뽑은 장바구니 목록을 Apriori를 통해 연관규칙을 얻어냈다.\
[ARM 코드](https://github.com/Cceugene/review/blob/baemin/ARM_bascket_Tako.ipynb)

## 리뷰 분석
 Kaggle의 주문 데이터를 이용해 모델을 학습시키고, 앞서 크롤링 한 가게 리뷰 데이터를 사용해 고객들의 반응을 분석했다.\
[학습에 이용한 Kaggle 데이터](https://www.kaggle.com/datasets/ninetyninenewton/kr3-korean-restaurant-reviews-with-ratings)