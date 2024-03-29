###### markdown에 익숙해지자
https://gist.github.com/ihoneymon/652be052a0727ad59601
http://www.stat.columbia.edu/~liam/teaching/compstat-spr14/lauren-notes.pdf
### Dynamic Programming

한 번 푼 문제는 다시 풀지 않는다.
불필요한 연산을 줄이기 위해 메모리를 대신 사용한다, 시간복잡도를 낮추고 공간복잡도는 지양.
즉, 이미 계산한 값 같은 경우 메모리에 저장.

하위의 작은 문제들을 풀고 이를 이용해 더 큰 문제를 풀어가는 방법. 
격자를 사용한다. 격자의 각 칸을 최적화하면 전체의 최적값을 찾을 수 있다.
격자의 각 칸이 '하위 문제', 실전으로 적용했을 때 문제를 어떻게 하위 문제로 나눌 지 생각해야 하는 것이 중요하다.

The key difference between the two is that in dynamic programming we're making one decision at a time, whereas in linear programming we're making all the decisions up front.

### Stochastic Programming / Stochastic Optimization (추계적 최적화) 확률론적 프로그래밍

>예측성 문제들을 함수로 구현해 내려면 예측값을 추론하기 위한 수학적 과정들이 필요하다.  이런 수학적 과정으로는 통계적 가설수립, 확률 분포 모형 함수생성, 데이터 시뮬레이션을 통한 통계적 추론 , 데이터 샘플링, 확률분포간의 비교과정등이 있다. 또한 이러한 확률적 프로그래밍 방법에는 컴퓨팅 자원도 많이 필요해 OS와 프레임워크수준에서 CPU병렬처리나 GPU등의 사용도 효과적으로 지원도 되어야 한다.

[출처] Probabilistic Programming Frameworks 비교|작성자 IDEO


Linear 일 수도 있고, Dynamic 일 수 도 있다.
이 분야의 연구들은 잘 정형화된 문제에서 좋은 성능을 보이고 있지만, 
조금만 더 복잡한 문제(현실성 있는 문제)에 적용하기 위해서는 많은 노력을 필요로 한다. 
문제 자체를 표현하기가 어렵기 때문이다. 
대표적인 방법들로는 Genetic Algorithm, Tabu Search, Simulated Annealing 등

일단, 메인 components는
시나리오/ 각 시나리오의 확률/ 기댓값

Ex. '수요량'이 불확실한 상황에서, 수요 시나리오와 시나리오별 예상 확률이 들어간다.

책상: 목재 8보드, 목공일 2시간, 다듬질 4시간
식탁: 목재 6보드, 목공일 1.5시간, 다듬질 2시간
의자: 목재 1보드, 목공일 0.5시간, 다듬질 1.5시간
자원의 단위가격은 목재 $2/보드, 목공일 $5.2/시간, 다듬질 $4/시간 이다. 

Dakota 가구회사는 각 상품의 수요만큼 혹은 그 이하만큼 생산해서, 순이익(상품을 팔아서 번 돈 - 자원을 확보하는 데 쓴 돈)을 최대화하고자 한다 (수요를 초과하는 상품은 판매하지 못한다고 가정). 
그런데 문제는, 수요량이 불확실하다는 것이다. 예상되는 수요 시나리오와 시나리오별 예상 확률은 다음과 같다.
적은 수요: 책상 50개, 식탁 20개, 의자 200개 (확률 30%)

보통 수요: 책상 150개, 식탁 110개, 의자 225개 (확률 40%)

많은 수요:  책상 250개, 식탁 250개, 의자 500개 (확률 30%)

#### make it two-stages
왜 핸들러 문제가 이렇게 어려울까?
정작 가격을 정해 '입찰'하는데 그게 비즈니스에게는 비용으로 작용하기 때문이다.

시나리오는 a~z라 할 때 a~z 모두가 mutually exclusive하게 확률로 1을 구성해야 할듯한데
그 시나리오별로 자원을 달리 확보할 필요가 있고, 자원을 달리 확보했을 때의 그 시나리오 아래 상품 판매액은 또 다르다.
확률적으로 여러가지 가능한 상황 - 상황별 자원 - 판매 - 전체 수입
확률적으로 여러가지 가능한 가격플랜 - 플랜별 핸들건들 가격- 입찰 - 전체 비용

1. 시나리오와 무관하게 비용 가격들을 정하고, 가상의 비용을 정한다.
2. 각 시나리오에 대해 정해진 핸들건 가격으로 전체 비용을 최소화하는 가격을 정한다. 
3. 시나리오별 비용과 시나리오의 확률을 곱해 모두 더하면 전체 비용의 기댓값을 얻는다.
4. 가상의 비용과 전체 비용 기댓값을 비교..아 모르겠다

이 때, 순이익의 '기댓값'을 최대화하기 위해선 목재, 목공일, 다듬질을 각각 얼마나 확보해야 할까?

너무 적게 확보해두면 상품을 많이 만들지 못해 순이익이 적을 수 있고, 너무 많이 확보해두면 수요를 초과해 팔지 못하는 상품이 많을 수 있다.
얼핏 보면 이 문제는 평범한 선형계획 최적화 문제로 만들 수 있어 보인다. 3개 시나리오 각각에 대응하는 변수들을 만들면 되니까. 
하지만 만약 시나리오가 수십개, 수백개라면 어떻게 할 것인가? 
그렇게 큰 행렬을 만드는 것은 매우 복잡한 작업임에 틀림없다. 

[출처] Stochastic Linear Programming 문제해결용 MATLAB 코드 + 간단 설명|작성자 song4energy


multi-stage stochastic programming to deal with uncertain demand, dynamic pricing
======
https://www.sciencedirect.com/science/article/pii/S1026309811000836#br000240
요것 괜찮은 듯 하다.
https://web.stanford.edu/~boyd/papers/pdf/pric_learn_unc_dem.pdf
http://blog.daum.net/w-ko/78

> 이것 유용 [https://www2.isye.gatech.edu/~anton/stochoptiebook.pdf]

two-stage stochastic integer programs
===
Two-stage stochastic programs refine modeling of uncertain futures by including both the impacts of Stage 1 choices before the future is known, and separate choices explicitly addressing recourse for each possible future scenario.

→ Such integer (or discrete) stochastic programs are especially difficult to solve and only very moderate progress has been reported so far. A discussion of two-stage stochastic integer programs with recourse can be found in Birge and Louveaux (1997).

현실 문제 ( scenario가 너무 많고 + 이진변수라는 점 ) ⇒ 관련 연구가 별로 없고 특별히 어려운 문제라고 함.

→ deterministic으로 두고 푸는 수 밖에 없겠다

Most optimization models are deterministic—not because OR analysts really believe that all problem parameters are known with certainty, but because useful prescriptive results can often be obtained only if stochastic variation is ignored.

For convenience and tractability, deterministic models assume all model parameters are known with certainty even though they are truly only estimates of the values that will arise in real application.
