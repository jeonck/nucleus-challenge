**Nucleus Tools**는 NVIDIA Enterprise Nucleus Server 환경에서 데이터 관리와 유지보수를 위한 다양한 고급 기능을 제공하는 유틸리티 모음입니다. 이 도구들은 Docker 컨테이너로 제공되며, 서버의 데이터 백업, 복원, 파일 관리, 개인정보 비식별화 등 엔터프라이즈 환경에서 필수적인 작업을 자동화하고 간소화하는 데 사용됩니다[1].
### 실행 
sudo docker run -d --restart always -e "ACCEPT_EULA=Y" -p 8081:80 $NUC_TOOLS docs
### 주요 기능

| 기능 | 설명 |
|---|---|
| **dump** | Nucleus 인스턴스의 전체 또는 일부(서브트리) 백업 생성 |
| **download** | Nucleus 인스턴스의 특정 서브트리(폴더, 파일 등) 다운로드 |
| **upload** | 백업 파일이나 개별 파일을 Nucleus 인스턴스에 업로드 |
| **rm/ls** | Nucleus 인스턴스 내 파일 및 디렉터리 삭제(rm), 목록 확인(ls) |
| **scrub_users** | 개인정보 비식별화(예: GDPR 준수), 특정 사용자 정보 무작위화 및 익명화 |

### 활용 사례

- **정기 백업 및 복원**: 대규모 프로젝트 데이터의 정기적 백업 및 필요 시 신속한 복원으로 데이터 유실 위험 최소화
- **데이터 마이그레이션**: 온프레미스와 클라우드 환경 간, 또는 서버 간 파일/폴더 이동 및 동기화[2]
- **파일 관리 자동화**: 불필요한 파일/폴더 정리, 대량 데이터의 일괄 업로드·다운로드
- **개인정보 보호**: GDPR 등 데이터 프라이버시 규정 준수를 위한 사용자 데이터 비식별화(sanitize)
- **운영 자동화**: 스크립트·자동화 파이프라인에 도구를 연동해 반복 작업 최소화

이 도구들은 NVIDIA Licensing Portal을 통해 엔터프라이즈 고객에게만 제공되며, Nucleus Workstation 버전과는 호환되지 않습니다[1].

[1] https://docs.omniverse.nvidia.com/nucleus/latest/enterprise/nucleus_tools.html
[2] https://blogs.nvidia.co.kr/blog/anyone-can-build-metaverse-applications-with-new-beta-release-of-nvidia-omniverse-1/
[3] https://nucleuscoffeetools.kr/53
[4] https://nucleuscoffeetools.kr/40/?idx=209
[5] https://www.instagram.com/p/DFCLnPXTvBk/
[6] https://gomguk.tistory.com/201
[7] https://docs.omniverse.nvidia.com/nucleus/latest/features.html
[8] https://github.com/centrica-engineering/nucleus-docs
[9] https://translate.google.com/translate?u=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FNucleus_RTOS&sl=en&tl=ko&client=srp
[10] https://www.irsglobal.com/bbs/rwdboard/23151
[11] https://nucleussec.com/platform/
[12] https://api-docs.nucleussec.com/nucleus/docs/
[13] https://www.instagram.com/reel/DFpsGPKzeIC/
[14] https://a-platform.tistory.com/110
[15] https://docs.omniverse.nvidia.com/nucleus/latest/index.html
[16] https://www.nucleus-cms.com/documentation/
[17] https://www.instagram.com/p/DDmIb9cToXO/
[18] https://naro-security.tistory.com/20
[19] https://nucleussec.com
[20] https://github.com/atlassian/nucleus/blob/master/docs/API.md
