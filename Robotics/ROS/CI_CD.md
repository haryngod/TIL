# CI/CD
## Testing
* Tools: Gtest, unitest, ROStest, ROSlaunch Check

## CI(Continuous Integration)
* Automated build and test for every software change
* Tools: Travis, Jenkins, Bitbucket

## Deployment
* Methods installing robot software in 'production' systems
* Tools: Docker, Debian, Cmake

**You DON'T have PRODUCTION READY code if you don't do these THREEE Things!**

# CI(Continuous Integration)
## Why CI
* 개발 중
    * 시스템 안정성
    * 기술적 의심 제거
* 시스템이 운용할 수 있는지 확실히 하기 위해
    * 빌드
    * 유닛 & 시스템 테스트
    * 배포
* 수동으로 하기엔 복잡하고 번거로움
    * 클린 환경 필요
    * 모든 플랫폼을 커버 해야함
    * 시간이 오래 걸림
* 오픈소스를 의존하는경우 필수적임
* If you're running a company that doesn't have CI, you're not running a company - Mike Ferguson, CTO at Fetch Robotics(ROSCon 2015)

## CI Software
* Server software for in-house
    * Jenkins
    * Buildbot
* Cloud-based service
    * travis
    * circleci
    * drone.io
    * magnumCI

## industrial_ci package
* https://github.com/ros-industrial/industrial_ci
* ROS 개발자들 사이에 일반적으로 쓰임
* Travis CI 가 주로 쓰임
* ROS Indigo, Jade, Kinetic(2017년버전)
* Pre-release test 지원
* Local/cloud 모두 사용 가능
* Private 프로젝트 이용 가능

```yaml
sudo: required
dist: trusty
language: generic
env:
    - ROS_DISTRO='indigo'
install:
    - git clone --depth 1 https://github.com/ros-industrial/industrial_ci.git .ci_config
script:
    - .ci_config/travis.sh
```

### Workflow

* Depeloper pull request or commit
* Repository request CI using CI configuration file
* fetch industrial_ci repository

## Other Option
* ROS Buildfarm
    * cloud-bases
    * Prioritize accuracy over speed
* buildbot_ros
    * Runs on your server machine with buildbot

> https://youtu.be/rdv3nywmUxM?t=2595