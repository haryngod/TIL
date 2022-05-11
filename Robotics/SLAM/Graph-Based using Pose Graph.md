# Graph Based SLAM
> 동영상 [[link](https://www.youtube.com/watch?v=BOqB4V18rPw&list=PLubUquiqNQdP_H6uUmU-9f0y_LheA3Hil&index=5&t=216s&ab_channel=SLAMKR)]

SLAM의 문제를 확률상의 그래프 모델로 표현 할 수 있다는 것이 기존의 필터 방식의 SLAM과 가장 큰 차이점이다. node와 edge를 표현 하기 때문에 graph에서 모든 pose의 값에 접근이 가능하기 때문에, grobal optimization이 가능하다. 이는 필터 방식의 SLAM이 시간이 지날수록 점점 error가 커진다는 것을 보완할 수 있다.

* Modeling
    * 그래프로 문제를 표현 할 수 있음
    * node : pose of robot
    * edge : 두 포즈 사이의 관계
* Front End & Back End
    * Front End : 매칭 되는 센서 데이터로 pose graph 생성(backend에서 풀 linear system을 만듬)
    * Back End : LSM을 사용하여 pose 최적화 
* 1D 예제
    * frontend(Graph construction)
        * Initial position: -3
        * 1st movement: 5
        * 2nd movement: 3
    * backend(Graph Optimization)
        * initial position을 linear system에 mapping 하면,
        $$\begin{bmatrix}1&& \\ && \\ && \end{bmatrix} \begin{bmatrix}x_0 \\ x_1 \\ x_2\end{bmatrix} = \begin{bmatrix}-3\\ \\\\\end{bmatrix}$$
        <center>&nbsp; &nbsp; &nbsp; &nbsp; [Information matrix][pose]=[Information vector]</center>

        즉, $\Omega \cdot \bold x = \bold b$ => $\bold x = (\Omega^T\Omega)^{-1}\Omega^T\bold b$
        * 1st movement를 풀어보면,
            $$x_1 = x_0 + 5$$
            라는 식이 나오고, 해당 식으로
            $$x_0-x_1 = -5$$
            $$-x_0+x_1 = 5$$
            라는 식이 나오게 되므로 initial position에서 나온 linear system mapping 정보에 업데이트 해주면, 아래와 같은 매트릭스 정보를 얻을 수 있게 된다.
            $$\begin{bmatrix}2&-1& \\ -1&1& \\ && \end{bmatrix} \begin{bmatrix}x_0 \\ x_1 \\ x_2\end{bmatrix} = \begin{bmatrix}-8\\5\\\\ \end{bmatrix}$$
        * 위와같은 정보로 Graph를 생성한다. 그래프를 생성한다는 것은 information matrix와 information vector를 생성 한다는 것이다.

* pose graph 생성 방법
    * odometry
        * 이전 스텝에서 잡은 pose를 기반으로 관측값을 통해 edge로 만들고 그 다음 pose값을 다시 node로 형성
    * ICP(Iterative closest point)

    * backend의 중요한 부분은 edge를 생성하는 것
        * edge는 센서의 관측값으로 형성 된다. edge는 constraint $e_{ij}^T\Omega_{ij}e_{ij} $가 존재하고 이는 노드와 노드 사이에서 나온 확률 분포의 마를라노미스 디스턴스이다. 

            pose $x_i$와 $x_{i+1}$이 있을 때, $z_i$가 관측 될 확률(posterier)
            $$p(z_i|x_i, x_{i+1})$$
            pose $x_i$와 $x_{i+1}$에서 관측 된 값
            $$h_i = (x_i^{-1}\cdot x_{i+1})$$
            이 둘의 차이가 minimize되도록 해야한다.
            $$n*exp(-{1\over2}(z_i-h_i)^T\Omega_i(z_i-h_i))$$
            posterier가 maximize 되어야한다.

* 정리해보자면,
    * node: pose of robot
    * edge: spatial constraint between two poses
    * constraint: measure of uncertainty $e_{ij}^T\Omega_{ij}e_{ij} $
    * $\bold X^* = argmin_x(\Sigma e_{ij}^T\Omega_{ij}e_{ij} )$
    즉, erorr를 minimize해주는 x pose 들의 집합을 구하는 것