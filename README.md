# 출근시간 코인거래는 신중하게...

### 시작하기 앞서

가상화폐와 관련된 기사를 살펴보다가 흥미로운 기사를 발견하였습니다.  

#### "출근시간 코인거래는 신중하게..." - 랩투아이, 데이터 분석결과서 밝혀

기사를 요약하자면 최근 3년간 비트코인 가격은 8 ~ 14시 사이에 수익률이 낮고 새벽 4 ~ 7시에 상승세를 보인다는 것입니다.  
따라서 비트코인 가격은 시간에 따라 통계적으로 유의미한 차이를 보인다고 밝혔습니다.  
기사의 자세한 내용은 아래의 링크를 참고해주시길 바랍니다.

https://it.chosun.com/site/data/html_dir/2022/08/08/2022080801896.html

저는 최근 1년간 시간대별 비트코인 가격이 기사와 동일한 패턴, 통계적으로 유의미한 차이를 보이는지 살펴보도록 하겠습니다.  

### 미리보는 결론

**수익률이 시간에 따라 다르다는 것은 통계적으로 유의미한 차이를 보였습니다.  
하지만 모든 시간이 관련있는 것은 아니었고 특정 시간에서만 차이가 있음을 관찰하였습니다.**

### 데이터 수집

업비트에서 2021년 9월 1일부터 2022년 8월 31일까지 1년간 비트코인 데이터를 수집하였습니다.  
해당 데이터는 시가, 고가, 저가, 종가, 거래량, 거래대금으로 구성되어 있습니다.  

### 데이터 전처리

![image](https://user-images.githubusercontent.com/50400392/191185751-2635608e-fe25-4a77-8d06-1b35de205b84.png)

데이터 통계치를 관찰하다보니 결측치가 발견되었습니다.  
노란색으로 강조된 부분은 데이터 개수를 의미하는데, 1년의 모든 시간대를 합치면 8760(365일 * 24시간)개의 데이터가 수집되어야 하지만 10개의 데이터가 결측된 것을 확인할 수 있습니다.  

![image](https://user-images.githubusercontent.com/50400392/191186023-705b939b-3aa7-4667-987b-179280f8cac5.png)

결측된 시간은 위의 10개의 시간에서 발생했음을 확인할 수 있었고 결측 직전의 값을 가져와 결측치를 처리하였습니다.  

### EDA (탐색적 데이터 분석)

![image](https://user-images.githubusercontent.com/50400392/191186367-518c8c82-e4aa-4565-bf57-8ecf28368056.png)

해당 그래프는 시간대별 수익률(Returns)을 나타냅니다. 수익률은 시간별로 모은 데이터들의 중앙값으로 표현하였습니다.  
기사에서 보았던 그래프와 유사한 패턴을 가지는 것을 관찰할 수 있었습니다.  

미국 증시가 시작되는 23시부터 수익률은 감소하다가 새벽 4 ~ 7시까지 증가하는 추세이며, 한국 증시가 시작되는 8시부터 다시 감소하는 형태를 보이고 있습니다.  
이러한 수익률 패턴을 검정하기 위해 위의 기사에서 언급한 통계적 분석방법을 사용하여 검정해보도록 하겠습니다.  

### 분산(ANOVA)분석

먼저, ANOVA 분석방법 중에서 크루스칼 왈리스 검정을 사용해야 하는 데이터인지 확인하는 작업이 필요합니다.  
정규성, 등분산성, 독립성을 데이터가 가지고 있는지 없는지에 따라 다른 검정 방법을 사용해야하기 때문입니다.  

정규성 검정에는 여러 방법이 있지만 Kolmogorow-Smirnov test를 이용하여 확인하였습니다.  
'귀무가설은 정규분포를 따른다(대립가설은 정규분포를 따르지 않는다).'이며 데이터가 정규성을 갖고 있다면 유의수준보다 p값이 높아 귀무가설을 채택하게되고 그렇지 않은 경우, 귀무가설을 기각하여 데이터 분포가 정규성을 띠고 있지 않다는 것을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/50400392/191187682-1070e421-071d-44d7-8627-9e59324f5e19.png)

22시일 때 수익률은 정규성을 가지지 않습니다.  
모든 시간대를 활용하여 분산분석을 시행하기 위해 비모수 분산분석방법인 크루스칼-왈리스 검정 방법을 사용하는 것이 바람직함을 확인하였습니다.  

![image](https://user-images.githubusercontent.com/50400392/191187919-c1906529-0f3a-4acd-8ea0-3cd746bc7f93.png)

크루스칼-왈리스 검정을 실행한 결과, p값이 유의수준보다 낮았으므로 시간대별 수익률 차이가 통계적으로 유의미하다는 것을 알 수 있습니다.  
그렇다면 모든 시간대가 수익률과 관계를 보이고 차이가 있는지 관찰하기 위해 본페로니 교정법을 사용하여 사후검정을 실시하였습니다.  

![image](https://user-images.githubusercontent.com/50400392/191189362-096091e7-79c8-4b4a-843e-6a657cf1f808.png)

본페로니 교정법을 시행한 결과입니다.  
x축과 y축은 시간을 의미하며 값이 "1"과 가깝다면 "시간대별 수익률 차이가 없다."라고 할 수 있고 "0.05"이하라면 "유의미한 차이가 있다."라고 말할 수 있습니다.  
수익률이 가장 낮았던 8시가 5, 6, 17시와 차이가 있음을 알 수 있습니다.  
하지만 대부분의 시간대에서 차이가 없음을 관찰할 수 있었습니다.  

따라서 다른 조건들이 동일하다고 가정한다면 8시에 비트코인 거래를 하는 것보다 5, 6, 17시에 거래하는 것이 확률적으로 더 나은 수익률을 가져갈 수 있습니다.  

### 마치며...

특정 시간에 대해 유의미한 차이를 보이지만 시간대별로 분산이 크기 때문에 특정 시간대에 무조건 더 높다/낮다라고 말할 수는 없습니다.  
시간말고도 다양한 변수들에 의해 가격변동이 이루어지는 만큼, 반드시 8시를 피하고 새벽에만 비트코인 거래하는 것은 바람직하지 않습니다.  
8시에서도 평균적으로 낮다는 것이지, 다른 시간대보다 수익률이 높은 경우도 빈번하게 관찰되기 때문입니다.  
하지만 통계적으로 유의미한 차이를 보였으므로 비트코인 거래를 할 때, 참고사항정도로 생각하고 거래하는 것이 바람직하다고 생각됩니다.  

