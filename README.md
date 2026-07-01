
## 나만의 helm chart 를 만들고 github pages 로 배포 해보자 

<img src="./assets/image01.png">

### templates yaml 파일 작성법

<img src="./assets/image02.png">

### 작성한 chart 가 잘 실행되는지 확인해 보자

<img src="./assets/image03.png">


### 작성한 helm chart 소스 코드를 일단 github 에 push 한다

```bash

git init
git add .
git commit -m "helm01_member 파일 추가함"
git remote add origin  git@github.com:oli999/my-helm-chart.git
git push -u origin master

```

### docs 폴더를 구성한다.

```bash

# docs 폴더 준비하기 
mkdir -p docs
# 우리가 만든 chart 를 압축해서 docs/ 폴더 안에 저장
helm package charts/helm01_member -d docs/
# index.yaml 파일을 docs/ 폴더에 생성하기
helm repo index docs/ --url  https://<나의github아이디>.github.io/저장소의이름/
helm repo index docs/ --url  https://oli999.github.io/my-helm-chart/

git add ./docs
git commit -m "release: member-app chart v1.0.0 package"
git push origin master

```

### push 후에 github 에  pages 에 관련된 설정을 1번만 해주면 다음부터는 자동으로 pages 에 반영된다.

<img src="./assets/image04.png">
<img src="./assets/image05.png">

### 우리가 만든 helm chart 저장소를 사용해 보기

```bash
# helm 저장소 등록 
helm repo add my-repo https://oli999.github.io/my-helm-chart/
# helm 저장소 목록 확인
helm repo ls
# 업데이트 
helm repo update
# 저장소에 어떤 chart 들이 있는지 검색 
helm search repo my-repo
NAME                    CHART VERSION   APP VERSION     DESCRIPTION               
my-repo/member-app      0.1.0           1.0.0           fastapi member application

# github pages 에서 내려 받은 helm chart 를 배포해보기
# 이미 존재하는 namespace 가 있으면 삭제후에 
kubectl delete ns helm01
# 배포하기
helm install member-release my-repo/member-app -n helm01 --create-namespace
# 확인하기
kubectl get pod,svc -n helm01

# helm01 네임스페이스에 배포된 목록확인
helm ls -n helm01

# uninstall 하기 
helm uninstall member-release -n helm01
kubectl delete ns helm01

# helm01_member 의 Chart.yaml 파일을 수정 (버전 올리기)
version: 0.1.1      
appVersion: "1.0.1" 

# 수정후에 helm package , index 를 다시 실행하고 add, commit, push 하면 자동으로 github pages 가 동작한다

# 우리가 만든 chart 를 압축해서 docs/ 폴더 안에 저장
helm package charts/helm01_member -d docs/
# index.yaml 파일을 docs/ 폴더에 생성하기
helm repo index docs/ --url  https://oli999.github.io/my-helm-chart/
git add ./docs
git commit -m "release: member-app chart v1.0.1 package"
git push origin master

# push 후 1~2 분 뒤에 업데이트 
helm repo update
# 저장소에 어떤 chart 들이 있는지 검색 
helm search repo my-repo
# version 이 올라간걸 확인할수 있다.
NAME                    CHART VERSION   APP VERSION     DESCRIPTION               
my-repo/member-app      0.1.1           1.0.1           fastapi member application
```


### 새로운 chart 만들고 배포화기
```bash

# 새로 만든 chart 를 압축해서 docs/ 폴더 안에 저장
helm package charts/helm02_micro -d docs/
# index.yaml 파일을 업데이트
helm repo index docs/ --url https://yunjuka.github.io/my-helm-chart/

# docs 에 tgz 파일을 넣고, index.yaml 파일에 새로운 정보를 넣은 다음 push 하면 배포가 자동으로 된다
git add .
git commit -m "helm02_micro chart 추가함"
git push


```