
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