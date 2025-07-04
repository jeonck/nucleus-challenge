Omniverse Nucleus의 설치에 관련된 환경 파일명과 저장 위치에 대한 정보는 다음과 같습니다.

**환경 파일명 및 저장 위치**

1. **환경 파일명**:
   - Omniverse Nucleus의 설정 파일은 주로 `nucleus-stack.env`와 `nucleus-cache.env` 파일을 포함합니다. 이 파일들은 Nucleus 서버의 환경 설정 및 캐시 동작을 제어하는 데 사용됩니다[2][13].

2. **저장 위치**:
   - 기본적으로 Nucleus의 설정 파일은 `/opt/ove` 디렉토리에 저장됩니다. 이곳에는 Nucleus의 구성 파일이 포함되어 있습니다.
   - Nucleus 데이터는 `/var/lib/omni` 디렉토리에 저장됩니다. 이 위치는 Nucleus 서버의 데이터 파일이 저장되는 곳으로, 정기적인 백업이 필요합니다[2][3].
  

Omniverse Nucleus의 데이터 디렉토리 구조와 각 파일 및 폴더의 설명은 다음과 같습니다.

**Nucleus 데이터 디렉토리 구조**

현재 Nucleus 데이터 디렉토리는 `/var/lib/omni/nucleus-data`에 위치하고 있으며, 다음과 같은 주요 폴더와 파일들이 포함되어 있습니다:

- **data**: Nucleus의 주요 데이터가 저장되는 디렉토리로, 사용자 데이터 및 프로젝트 파일이 포함됩니다.

- **empty**: 비어 있는 디렉토리로, 특정 작업이나 설정에 따라 사용될 수 있습니다.

- **local-accounts-db**: 로컬 사용자 계정 데이터베이스가 저장되는 곳입니다. 이곳에는 사용자 인증 정보와 관련된 데이터가 포함되어 있으며, Nucleus의 사용자 관리 기능과 관련이 있습니다[1].

- **log**: Nucleus 서버의 로그 파일이 저장되는 디렉토리입니다. 이 로그는 시스템의 동작 및 오류를 기록하여 문제 해결에 도움을 줍니다[2].

- **resolver-cache**: 파일 및 데이터의 빠른 접근을 위해 사용되는 캐시 데이터가 저장되는 디렉토리입니다. 이 캐시는 데이터 요청 시 성능을 향상시키는 데 기여합니다.

- **scratch**: 임시 파일이나 작업 중 생성된 데이터를 저장하는 디렉토리입니다. 일반적으로 작업이 완료되면 이 데이터는 삭제됩니다.

- **tags-db**: 태그 데이터베이스가 저장되는 곳으로, Nucleus에서 관리하는 자산이나 데이터에 대한 태그 정보를 포함합니다.

- **tmp**: 임시 파일이 저장되는 디렉토리로, 시스템의 일시적인 작업에 사용됩니다.

이러한 디렉토리와 파일들은 Nucleus의 기능을 지원하며, 데이터 관리 및 사용자 인증, 로그 기록 등 다양한 작업을 수행하는 데 필수적입니다.
[1] https://docs.omniverse.nvidia.com/nucleus/latest/config-and-info/auth_user_mgmt.html
[2] https://docs.omniverse.nvidia.com/nucleus/latest/enterprise/logging.html
[3] https://docs.omniverse.nvidia.com/utilities/latest/cache/enterprise.html

이 정보를 바탕으로 Omniverse Nucleus를 설치하고 구성할 때 필요한 파일과 그 위치를 확인할 수 있습니다.
[1] https://m.blog.naver.com/nswve/222839863401
[2] https://docs.omniverse.nvidia.com/nucleus/latest/enterprise/backing-up-and-restoring-nucleus.html
[3] https://like-grapejuice.tistory.com/634
[4] https://richardworld.tistory.com/entry/Omniverse-Introduction-and-Installation
[5] https://namhauk.tistory.com/
[6] https://challenge-sam.tistory.com/entry/NVIDIA-Omniverse-Isaac-Sim-%EC%84%A4%EC%B9%98
[7] https://docs.omniverse.nvidia.com/nucleus/latest/workstation/installation.html
[8] https://github.com/SungjoonCho/IsaacSim_Omniverse
[9] https://www.pollux.ai/ko/tech-blog
[10] https://docs.omniverse.nvidia.com/nucleus/latest/enterprise/cloud/cloud_nucleus_azure.html
[11] https://forums.autodesk.com/t5/flexsim-forum/how-do-i-set-up-a-nucleus-local-server-for-omniverse/td-p/13570927
[12] https://digitalstates.com/blog/installing-nvidia-guide/
[13] https://docs.omniverse.nvidia.com/utilities/latest/cache/enterprise.html
[14] https://docs.omniverse.nvidia.com/nucleus/latest/enterprise/installation/install-ove-nucleus.html
[15] https://isaac-christian.tistory.com/entry/NVIDIA-Omniverse-%EA%B0%9C%EB%B0%9C%EC%9E%90-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95-NVIDIA-Omniverse-Launcher
[16] https://velog.io/@raise_wise/2.-Omniverse-Isaac-Install
[17] https://blogs.nvidia.co.kr/blog/anyone-can-build-metaverse-applications-with-new-beta-release-of-nvidia-omniverse-1/
[18] https://docs.omniverse.nvidia.com/nucleus/latest/enterprise/installation/planning.html
[19] https://docs.omniverse.nvidia.com/workflows/latest/extensions/environment_configuration.html
[20] https://aws.amazon.com/blogs/spatial/deploying-nvidia-omniverse-nucleus-on-amazon-ec2/
[21] https://docs.omniverse.nvidia.com/kit/docs/connect-sdk/latest/docs/settings-config.html
[22] https://github.com/aws-samples/nvidia-omniverse-modular-solution-with-aws-cdk/blob/master/docs/omniverse-nucleus/README.md
[23] https://docs.omniverse.nvidia.com/connect/latest/alias/manual.html
[24] https://namhauk.tistory.com/64
