# Graph Based SLAM

* Modeling
    * 그래프로 문제를 표현 할 수 있음
    * node : pose of robot
    * edge : 두 포즈 사이의 관계
* Front End & Back End
    * Front End : 매칭 되는 센서 데이터로 pose graph 생성
    * Back End : LSM을 사용하여 pose 최적화
