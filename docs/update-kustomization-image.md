# update-kustomization-image

Kustomization 파일이 포함된 레포지토리(Config Repository)를 클론하여 `kustomization.yaml` 파일에 이미지 digest를 업데이트하는 Task입니다.

### Steps

1. Config 레포지토리 clone
2. `kustomization.yaml` 파일 값 업데이트
3. Config 레포지토리에 변경사항 push

### Parameters

|      **Key**       |           **Description**            | **Default** |
| :----------------: | :----------------------------------: | :---------: |
|   GIT_REPOSITORY   |       클론할 Git 저장소의 URL        |     ""      |
|      GIT_REF       |        체크아웃할 Git 브랜치         |   "main"    |
|     NEW_IMAGE      |    빌드하고 푸시할 이미지의 이름     |     ""      |
|     NEW_DIGEST     |        새 이미지의 digest 값         |     ""      |
| KUSTOMIZATION_PATH | 업데이트할 kustomization 파일의 경로 |     ""      |
|   GIT_USER_NAME    |           Git 사용자 이름            |     ""      |
|   GIT_USER_EMAIL   |          Git 사용자 이메일           |     ""      |

### Usage

```yaml
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  name: update-kustomization-image-run
spec:
  taskRef:
    name: update-kustomization-image
  params:
    - name: GIT_REPOSITORY
      value: "https://github.com/your-org/config-repo.git"
    - name: GIT_REF
      value: "main"
    - name: NEW_IMAGE
      value: "your-registry.com/your-app"
    - name: NEW_DIGEST
      value: "sha256:abc123def456..."
    - name: KUSTOMIZATION_PATH
      value: "environments/prod"
    - name: GIT_USER_NAME
      value: "user"
    - name: GIT_USER_EMAIL
      value: "user@example.com"
  workspaces:
    - name: shared
      volumeClaimTemplate:
        spec:
          accessModes:
            - ReadWriteOnce
          resources:
            requests:
              storage: 1Gi
```

Config 레포지토리에 아래와 같이 값이 업데이트됩니다.

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

# ...

images:
  - digest: sha256:<digest>
    name: <your-image-name>
```
