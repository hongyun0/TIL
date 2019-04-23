## Git Branch Strategy
개발자들이 Git을 사용할 때 좀 더 명확하고 깔끔하게 버전관리를 하고 싶어서 Git 브랜치 전략을 만들었다.   
대표적으로 Git-flow, Github-flow, Gitlab-flow 등이 있고, 그 중 Git-flow만 간단히 정리해봄!

![image](https://datasift.github.io/gitflow/GitFlowMasterBranch.png)

브랜치 종류는 크게 5가지로 나뉜다.   
화살표는 Merge를 의미한다.

#### Master
실제로 배포가 된 버전만을 기록하여 관리합니다.   
해당 버전이 어떤 버전인지를 나타내는 태그를 달아두는 브랜치 입니다.   

#### Develop
개발 중인 상태를 기록하여 관리합니다.   
현업에 있어서 주된 브랜치라고 할 수 있습니다.   

#### Release
배포를 위해 Develop 브랜치를 Base 로 하여 생성된 브랜치 입니다.   
배포를 위한 준비가 끝나면 Master 브랜치로 Merge하고 해당 브랜치는 삭제합니다.   
또한 Develop 브랜치와도 상태를 맞춰야하기 때문에 Develop 브랜치에도 Merge합니다.     

#### Hotfix
실제로 배포가 나갔을 때 급하게 수정할 일이 생기면 Master 브랜치를 Base로 하여 생성됩니다.   
수정이 완료되면 Master 브랜치에 Merge 하고 새로운 버전의 태그를 달아둡니다.   
또한 Develop 브랜치와도 상태를 맞춰야하기 때문에 Develop 브랜치에도 Merge합니다.   

#### Feature
새로운 기능 혹은 고쳐야할 기능에 대해 생성하는 브랜치 입니다.   
실제로 가장 많이 생성되고 삭제되는 브랜치라고 생각하면 됩니다.   
기능개발이 완료되면 develop 브랜치에 Merge 합니다.
