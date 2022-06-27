# Git Branch 전략
## Git-flow
* 항상 유지되는 2개의 메인 브랜치와 역할을 완료하면 사라지는 3개의 보조 브랜치로 구성
    * 메인 브랜치
        * master: release 브랜치
        * develop: 다음 출시 버전 개발 브랜치
    * 보조 브랜치
        * feature: 기능 개발
        * release: 출시 준비
        * hotfix: 출시 버전에서 발생한 버그 수정
## Git-flow 프로세스
    * develop 브랜치에서 개발 기능을 위한 feature 브랜치를 만듬
    * 기능이 완성되면 develop 브랜치에 merge
    * 배포 버전의 기능들이 develop 브랜치에 모두 머지 되면 QA를 위해 release 브랜치 생성
    * release 브랜치에서 오류 발생하면 release 브랜치 내에서 수정. 배포를 위해 master 브랜치로 머지. butfix있으면 devel 브랜치에도 머지
    * 마스터에서 버그가 발생하면 hotfix 브랜치를 만들어서 master/devel 머지

* --no-ff 옵션으로 feature 브랜치 머지하면 브랜치 존재여부가 기록에 남음