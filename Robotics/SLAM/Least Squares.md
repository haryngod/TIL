# Least Square

* 값을 정확하게 알 수 없을 때 예측치 $\hat{x}$의 오차를 최소화 해주는 것
* Optimization을 사용 하는 모든 곳에서 사용 됌

카메라는 3차원 공간상의 하나의 점을 pixel 좌표로 표현 가능한가?   
어떤 포즈 x를 observation model을 통해서 예측 포즈 z가 나오지만, real world에서의 noise와 같은 이유로 정확한 좌표는 알 수 없다. 그렇기 때문에 error term을 더해주고 나서야 실제 포즈 z를 추출 할 수 있게 된다.

$$\hat z = f(x)$$ 
$$\hat z + e = z$$
$$f(x) + e = z$$
$$e(x) = z - f(x)$$

z로부터 x를 추출{: .text-center}
* 노이즈가 포함 된 z
* 예측 측정치, f(x)
* error function 연산
* Approximate an optimal state vector $x^*$ >minimize error 를 얻음

$$e(x) = actual Measurement- predictedMeasurement$$

정확한 센서 에러 코스트 function을 위해
$$e=e^T\cdot\Omega\cdot e$$

$\Omega$는 Heteroskedasticity이다. SLAM에서는 여러개의 센서를 쓰고 있는데, 각 센서들의 분산의 역수로 이루어진 information matrix이다. Digonal을 제외한 다른 원소는 서로 다른 센서가 각각에 영향을 주지 않는다는 가정을 하기 때문에 0이다. 만약 센서 1이 가장 노이즈, 에러가 적은 분포를 가졌다면 첫번째 항이 가장 큰 영향을 준다고 볼 수 있다.

$$\begin{bmatrix}e_1&e_2&e_3 \end{bmatrix} \cdot \begin{bmatrix} 1 \over \sigma_1^2 & 0 & 0 \\ 0 & 1 \over \sigma_2^2 & 0 \\ 0 & 0 & 1 \over \sigma_3^2 \end{bmatrix} \cdot \begin{bmatrix} e_1\\ e_2\\e_3 \end{bmatrix} = {e_1^2\over\sigma_1^2} + {e_2^2\over\sigma_2^2} + {e_2^2\over\sigma_2^2} $$

$$x^* = \underset{x}{\operatorname{argmin}} \sum_{x=0}^{t}e(x)$$

---
> 미래의 나야, 애 좀 써주라.

