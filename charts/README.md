
## helm chart 를 만들고 github pages 로 배포하기
### docs 폴더 구성
```bash
# docs 폴더 준비하기
mkdir -p docs
# 우리가 만든 chart 를 압축해서 docs/폴더 안에 저장
helm package charts/helm01_member -d docs/

# index.yaml 파일을 docs/ 폴더에 생성하기
helm repo index docs/ --url https://<깃헙아이디>.github.io/<저장소 이름>
helm repo index docs/ --url https://lio464628.github.io/my-helm-chart/
```

### 만든 helm chart 저장소 사용해보기
```bash

helm repo add my-repo https://yunjuka.github.io/my-helm-chart/

helm repo ls
helm repo update
# 저장소에 어떤 chart 가 있는지 검색
helm search repo my-repo

# github pages 에서 내려받은 helm chart 를 배포해보기
# 이미 존재하는 namespace 가 있으면 삭제 후에
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

# 수정 후에 helm package, index 를 다시 실행하고 add commit 
```