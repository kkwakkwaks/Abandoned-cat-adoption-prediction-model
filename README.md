# 유기묘 입양 예측 모델

![image](https://user-images.githubusercontent.com/99347825/202905638-9204f362-2a7b-4572-9401-cc1ff48b1dfe.png)

## **DATA** https://data.austintexas.gov/Health-and-Community-Services/Austin-Animal-Center-Outcomes/9t4d-g238
![image](https://user-images.githubusercontent.com/99347825/202905659-2038c6d9-6ad6-469c-9fee-9ae1d02aac23.png)
![image](https://user-images.githubusercontent.com/99347825/202905691-15b836a9-0f88-4e91-9245-04c5fcd37e47.png)
* 타겟특성으로 유기묘들의 현 상황을 나타내는 'outcome type'을 사용했다.
* 총 **9개**의 특성값으로 이루어져 있는데 사망이나 실종 등의 특성값이 너무 작아 이를 모두 예측할 수 없을 것이라 판단하여 이를 **이진분류**로 나눴다.
* 국내 유기동물 관련 어플리케이션 '포인핸드'를 바탕으로 입양 및 기과는 완료, 그 외의 것들은 진행중으로 분류하였을 때, 55:44로 **균형있는 데이터**라고 볼 수 있다.

![image](https://user-images.githubusercontent.com/99347825/202905732-3832250e-bc1c-499b-8996-c6986c83edf3.png)
* 타겟특성을 제외하고 총 **11개**의 feature가 있는데 이를 **9개**로 줄였다.
* 그 중 'outcome subtype'이라는 특성은 안락사의 이유 등의 내용을 담고있는 특성으로 대부분 입양가지 못 한 고양이들에게만 있는 데이터이기에 **과적합**을 일으킬 수 있다 생각하여 삭제했다.
![image](https://user-images.githubusercontent.com/99347825/202905755-6bbe6ee8-145d-4188-bacf-57ace970955e.png)
![image](https://user-images.githubusercontent.com/99347825/202905779-3c12d9aa-4a57-4678-9e89-fa9cde04949f.png)
* 기준모델은 모델의 최빈값인 0.55로 잡고 모델의 성능을 확인했다.
![image](https://user-images.githubusercontent.com/99347825/202905827-c207a680-c686-41f5-a164-1905a205ea9f.png)
* **Randomforest**와 **XGboost** 두 가지 모델 중 더 성능이 좋은 모델을 선택하는 방식으로 진행했다.
![image](https://user-images.githubusercontent.com/99347825/202905976-431edb8c-535d-4461-b2d2-52d8cfcb2cd1.png)
* 현 상황에서는 입양 확률이 낮은 고양이를 입양 가능하다고 예측하여 고양이를 돌보지 않는 것이 더 좋지 않은 상황을 초래하기에 **정밀도**를 모델 평가 지표로 사용했다.
![image](https://user-images.githubusercontent.com/99347825/202905985-066bf023-1457-4acc-822c-59e271278588.png)
* **TargetEncoder**진행 후, RandomForest와 XGBoost 두 가지 모델을 비교했을 때 RandomForest의 성능이 더 좋아 이를 최종 모델로 선택했다.
![image](https://user-images.githubusercontent.com/99347825/202906000-e67277fb-6f8b-425e-b702-c917c0c42de4.png)
* 최종 모델의 정확도는 **0.87**로 기준 모델인 **0.55**보다 높기에 의미있는 모델이라고 볼 수 있다.
* Target 값이 없는 test data로 모델의 성능을 확인한 결과 성능이 0.85로 나왔다.
![image](https://user-images.githubusercontent.com/99347825/202906006-e8029e82-f7cc-44c3-8243-98fb885cb721.png)
* 예측에 가장 영향을 많이 미친 특성들을 살펴보면 차례대로 **중성화여부, 중성화여부를 포함한 성별, 생후 일 수**등이 있다.
* 각 특성들이 어떻게 영향을 미쳤는지 확인해보면 **중성화가 되어있을 때, 이름이 있을 때, 생후 6주 이상 1년 미만**인 고양이들의 입양 확률이 높다고 볼 수 있다.

### 파이차트와 pdp plot을 통한 가설 검정
![image](https://user-images.githubusercontent.com/99347825/202906050-ed3d9076-fe3e-447c-859a-85d0b477d98c.png)
* m : male, f : female
* 중성화 여부는 영향을 미치지만, 성별은 크게 영향을 미치지 않는 것을 볼 수 있다.
* 파이차트를 통해 입양률을 확인해봐도 성별에는 큰 차이가 없다.
![image](https://user-images.githubusercontent.com/99347825/202906036-a0d24c10-aff1-4b84-bc90-d2d978ee9911.png)
* pdp plot을 봐도 중성화 여부는 입양 여부에 영향을 끼치지만 성별은 큰 영향을 끼치지 않는 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/99347825/202906063-b47b3f46-b5ea-4be0-9a77-fc309ec3f32a.png)
* 가설과는 다르게 이름이 있는 경우에 입양 확률을 0.2정도 더 높인다는 것을 알 수 있다.
![image](https://user-images.githubusercontent.com/99347825/202906075-184dec37-1783-4297-a2f1-9bfd3722c69a.png)
* 아주 약간이지만 품종묘의 경우에 입양 확률을 조금 더 높인다는 것을 알 수 있다.
![image](https://user-images.githubusercontent.com/99347825/202906091-e24b86a0-3556-43e3-8944-b00162abdbbd.png)
* 나이가 어릴수록 입양을 잘 갈 것이라 생각했지만 나이와는 관계없는 분포가 나타났고, 오히려 신생묘일 때 입양률이 낮은 것을 볼 수 있다.
![image](https://user-images.githubusercontent.com/99347825/202906101-c6ca4618-c54a-47c4-bd14-eec6cbdc0722.png)
* raw data를 통해 그 이유를 살펴보니 생후 6주 이내의 고양이들은 대부분 협력단체로 이동된 것을 확인할 수 있다.

### 결론
![image](https://user-images.githubusercontent.com/99347825/202906109-5293308e-e68e-4ffc-8d44-62d3c7fbdff3.png)
* 위 가설들을 바탕으로 보았을 때, **중성화가 되어있지 않고, 이름이 없고, 품종묘가 아닐수록** 입양 갈 확률이 낮다고 볼 수 있으므로 이 특징을 가진 고양이들의 입양을 더 돕는 방향으로 해야한다는 것이 결론이다.

![image](https://user-images.githubusercontent.com/99347825/202906122-53f10006-5329-4395-931b-a3a3186eba10.png)
* 전체 데이터에서 transfer 비중이 높은데 이를 모두 0으로 분류한 점이 아쉬웠다.
* 이진분류가 아닌 입양/협력단체이동/진행중 등으로 타겟을 더 세세하게 나눈다면 모델 성능이 더 좋아질 것이라 생각된다.
* 유기묘의 입양 시점을 알 수 있는 데이터를 추가하여 시간에 따른 입양률을 알 수 있기에 이 또한 성능에 영향을 미칠 것이라 생각된다.
