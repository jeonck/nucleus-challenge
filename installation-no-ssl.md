### NVIDIA Omniverse Enterprise Nucleus Server 설치 및 구성 절차서

---

#### 1. 도커(Docker) 환경 구성

**목표:** 도커 및 도커 컴포즈를 시스템에 설치합니다.

**단계:**

1. **도커 GPG 키 추가**
   ```bash
   sudo mkdir -p /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   ```

2. **도커 저장소 추가**
   ```bash
   echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

3. **패키지 목록 업데이트**
   ```bash
   sudo apt-get update
   ```

4. **도커 CE 버전 확인**
   ```bash
   sudo apt-cache madison docker-ce | awk '{ print $3 }'
   ```

5. **도커 및 도커 컴포즈 설치**
   ```bash
   VERSION_STRING=5:27.5.0-1~ubuntu.24.04~noble
   sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-compose-plugin
   ```

---

#### 2. 설치 패키지 다운로드 및 VM 전송

**목표:** NVIDIA Omniverse Enterprise Nucleus Server 설치 패키지를 다운로드하고 VM으로 전송합니다.

**단계:**

1. **라이선스 포털에서 패키지 확인**
   - [NVIDIA 라이선스 포털](https://ui.licensing.nvidia.com/software)에 접속하여 "NVIDIA Omniverse Enterprise Nucleus Server" 패키지를 찾습니다.
   - 버전: 2023.2.7
   - 다운로드 링크: [다운로드](https://ui.licensing.nvidia.com/software#:~:text=Nov%2026%2C%202024-,Download,-SHA256)

2. **패키지 다운로드**
   - 라이선스 포털에서 패키지를 다운로드합니다.
   - 예시 파일명: `nucleus-stack-2023.2.7+tag-2023.2.7.gitlab.20798950.d7a79764.tar.gz`

3. **PC에서 VM으로 패키지 전송**
   ```bash
   scp *.gz nucleus@ip_address:/home/nucleus/
   ```

---

#### 3. 설치 패키지 압축 해제

**목표:** 다운로드한 설치 패키지를 VM에서 압축 해제합니다.

**단계:**

1. **/opt/ove 디렉토리 생성**
   ```bash
   sudo mkdir -p /opt/ove
   ```

2. **압축 해제**
   ```bash
   sudo tar xzvf /home/nucleus/nucleus-stack-2023.2.7+tag-2023.2.7.gitlab.20798950.d7a79764.tar.gz -C /opt/ove --strip-components=1
   ```

3. **압축 해제 확인**
   ```bash
   cd /opt/ove
   ls
   ```
   - 예상 출력: `README.md`, `VERSION`, `base_stack`, `ssl`, `sso`, `templates`

---

#### 4. 환경 설정 파일(.env) 편집

**목표:** Nucleus Server의 환경 설정 파일을 수정하여 필요한 설정을 적용합니다.

**단계:**

1. **환경 설정 파일 편집**
   ```bash
   cd /opt/ove/base_stack
   sudo nano nucleus-stack.env
   ```

2. **설정 변경**
   - **EULA 수락 및 보안 검토 확인**
     ```plaintext
     ACCEPT_EULA=1
     SECURITY_REVIEWED=1
     ```

   - **서버 IP 또는 호스트명 설정**
     ```plaintext
     SERVER_IP_OR_HOST=myhost.mydomain.com
     ```

   - **Nucleus 비밀번호 설정**
     ```plaintext
     MASTER_PASSWORD=MY_NEW_PASSWORD
     SERVICE_PASSWORD=MY_NEW_PASSWORD
     ```

   - **Nucleus 데이터 저장 위치 설정**
     ```plaintext
     DATA_ROOT=/var/lib/omni/nucleus-data
     ```

   - **컨테이너 서브넷 설정**
     ```plaintext
     CONTAINER_SUBNET=192.168.2.0/26
     ```

   - **Reference Content 설정**
     ```plaintext
     REFERENCE_CONTENT_MOUNT_ENABLE=1
     REFERENCE_CONTENT_MOUNT_TARGET=/NVIDIA
     REFERENCE_CONTENT_SOURCE="content-production.omniverse.nvidia.com"
     REFERENCE_CONTENT_BUCKET=""
     REFERENCE_CONTENT_SECURE=1
     REFERENCE_CONTENT_NON_COMPLIANT_XML_SCHEMA=0
     ```

   - **Private Bucket 사용 시 추가 설정** (필요 시)
     ```plaintext
     REFERENCE_CONTENT_USE_CREDENTIALS=0
     REFERENCE_CONTENT_SOURCE_REGION=""
     REFERENCE_CONTENT_BUCKET_ACCESS_KEY_ID=""
     REFERENCE_CONTENT_BUCKET_SECRET_ACCESS_KEY=""
     ```

---

#### 5. 보안 키 및 토큰 생성

**목표:** Nucleus Server 운영에 필요한 보안 키와 토큰을 생성합니다.

**단계:**

1. **보안 키 및 토큰 생성 스크립트 실행**
   ```bash
   sudo ./generate-sample-insecure-secrets.sh
   ```

---

#### 6. Docker Compose로 Nucleus Server 실행

**목표:** Docker Compose를 사용하여 Nucleus Server를 실행합니다.

**단계:**

1. **Docker 이미지 풀(Pull)**
   ```bash
   sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f /opt/ove/base_stack/nucleus-stack-no-ssl.yml pull
   ```

2. **Docker Compose로 Nucleus Server 실행 (데몬 모드)**
   ```bash
   sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f /opt/ove/base_stack/nucleus-stack-no-ssl.yml up -d
   ```

   - **실행 확인 예시**
     ```plaintext
     [+] Running 12/12
      ✔ Container base_stack-nucleus-navigator-1       Running                             0.0s
      ✔ Container base_stack-nucleus-api-1             Running                             0.0s
      ...
     ```

---

#### 7. Nucleus Server 접속 및 동작 확인

**목표:** Nucleus Server와 Omniverse Navigator에 접속하여 정상 동작 여부를 확인합니다.

**단계:**

1. **Omniverse Navigator 접속**
   - 웹 브라우저에서 다음 주소로 접속합니다:
     ```plaintext
     http://<서버 IP 또는 호스트명>:8080/
     ```
   - 예시: `http://20.196.**.***:8080/`

2. **Nucleus Server 접속**
   - Navigator에서 설치한 Nucleus 서버 아이콘을 클릭하여 접속합니다.
   - **로그인 정보**
     - ID: `omniverse`
     - Password: `******`

---

#### 8. Nucleus 서버 상태 로그 확인

**목표:** Nucleus Server의 로그를 확인하여 상태를 모니터링합니다.

**단계:**

1. **특정 컨테이너 로그 확인**
   - 예시: `nucleus-auth` 컨테이너 로그 확인
   ```bash
   sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f /opt/ove/base_stack/nucleus-stack-no-ssl.yml logs nucleus-auth
   ```

2. **로그 실시간 모니터링**
   ```bash
   sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f /opt/ove/base_stack/nucleus-stack-no-ssl.yml logs -f nucleus-auth
   ```

   - **다른 컨테이너 로그 확인 예시**
     ```bash
     sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f /opt/ove/base_stack/nucleus-stack-no-ssl.yml logs nucleus-navigator
     sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f /opt/ove/base_stack/nucleus-stack-no-ssl.yml logs nucleus-api
     ```

---

### 요약

1. **도커 설치**
   - GPG 키 추가, 저장소 설정, 패키지 업데이트 및 설치

2. **Nucleus Server 패키지 다운로드 및 VM 전송**
   - 라이선스 포털에서 다운로드 후 `scp`로 전송

3. **패키지 압축 해제**
   - `/opt/ove` 디렉토리에 압축 해제

4. **환경 설정 파일 편집**
   - `nucleus-stack.env` 파일에서 필수 설정 변경

5. **보안 키 및 토큰 생성**
   - `generate-sample-insecure-secrets.sh` 스크립트 실행

6. **Docker Compose로 Nucleus Server 실행**
   - 이미지 풀(Pull) 후 데몬 모드로 실행

7. **Nucleus Server 및 Omniverse Navigator 접속**
   - 웹 브라우저 및 Navigator를 통해 접속 및 동작 확인

8. **Nucleus 서버 상태 로그 확인**
   - `docker compose logs` 명령어로 로그 확인 및 모니터링

---

### 참고
- 각 단계에서 발생하는 오류 메시지는 문제 해결에 중요한 단서가 될 수 있습니다.
- 보안 관련 설정은 실제 운영 환경에 맞게 적절히 조정해야 합니다.
- NVIDIA 공식 문서를 참조하여 추가적인 설정이나 문제 해결 방법을 확인할 수 있습니다.
