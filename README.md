# build-pipeline-with-slack

해당 pipeline은 GitOps의 CI 절차에 맞게 이미지를 빌드하고, 배포 코드가 포함된 Config 레포지토리에 빌드한 이미지의 해시값을 저장합니다.

1. 컨테이너 이미지 빌드
2. 이미지 레지스트리에 배포
3. Config 레포지토리에 있는 Kustomize.yaml에 이미지 Digest 변경
4. 성공 여부를 Slack으로 알림

### Dependency

| **Task**  | **Version** |              **Tekton Hub URL**              |
| :-------: | :---------: | :------------------------------------------: |
| git-clone |     0.9     | https://hub.tekton.dev/tekton/task/git-clone |
|  buildah  |     0.9     |  https://hub.tekton.dev/tekton/task/buildah  |

### Tasks

|          **Task**          | **Version** |              **Parameter Info**              |
| :------------------------: | :---------: | :------------------------------------------: |
|         git-clone          |     0.9     | https://hub.tekton.dev/tekton/task/git-clone |
|          buildah           |     0.9     |  https://hub.tekton.dev/tekton/task/buildah  |
| update-kustomization-image |     0.1     | [docs](./docs/update-kustomization-image.md) |
|   send-to-channel-slack    |     0.1     |   [docs](./docs/send-to-channel-slack.md)    |

### Parametes

#### 애플리케이션 저장소 관련

|           **Key**           |        **Description**         | **Default** |
| :-------------------------: | :----------------------------: | :---------: |
|   APPLICATION_REPOSITORY    |  애플리케이션 Git 저장소 URL   |     ""      |
|    APPLICATION_REVISION     |        Git 브랜치/태그         |    main     |
| APPLICATION_CONTAINER_IMAGE |     빌드할 컨테이너 이미지     |     ""      |
|   APPLICATION_CONTEXT_DIR   | 애플리케이션 컨텍스트 디렉토리 |     "."     |

#### 설정 저장소 관련

|          **Key**          |     **Description**     | **Default** |
| :-----------------------: | :---------------------: | :---------: |
|     CONFIG_REPOSITORY     |   설정 Git 저장소 URL   |     ""      |
|      CONFIG_REVISION      |     Git 브랜치/태그     |    main     |
| CONFIG_KUSTOMIZATION_PATH | Kustomization 파일 경로 |     ""      |

#### Git 사용자 정보

|    **Key**     |  **Description**  | **Default** |
| :------------: | :---------------: | :---------: |
| GIT_USER_NAME  |  Git 사용자 이름  |     ""      |
| GIT_USER_EMAIL | Git 사용자 이메일 |     ""      |

#### Slack 관련

|      **Key**       |    **Description**     |    **Default**     |
| :----------------: | :--------------------: | :----------------: |
| SLACK_TOKEN_SECRET | Slack 토큰 시크릿 이름 | slack-token-secret |
|  SLACK_CHANNEL_ID  |     Slack 채널 ID      |         ""         |
|   SLACK_BOT_NAME   |     Slack 봇 이름      |     Tekton Bot     |

### Usage
