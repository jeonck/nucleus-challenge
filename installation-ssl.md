### NVIDIA Omniverse Enterprise Nucleus Server SSL 설정 절차서

---

#### 1. Nginx 웹서버 설치 및 시작

**목표:** Nginx 웹서버를 설치하여 SSL/TLS를 통한 보안 연결을 지원합니다.

**단계:**

1. **패키지 목록 업데이트**
   ```bash
   sudo apt update
   ```

2. **Nginx 설치**
   ```bash
   sudo apt install -y nginx
   ```

3. **Nginx 시작 및 자동 실행 설정**
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

4. **Nginx 상태 확인**
   ```bash
   sudo systemctl status nginx
   ```

---

#### 2. Nginx 기본 페이지 설정

**목표:** Nginx의 기본 페이지를 설정합니다.

**단계:**

1. **기본 페이지 파일 편집**
   ```bash
   sudo nano /var/www/html/index.html
   ```
   - 필요에 따라 기본 페이지 내용을 수정합니다.

2. **Nginx 재시작**
   ```bash
   sudo systemctl restart nginx
   ```

---

#### 3. Nucleus 환경 설정 파일 수정

**목표:** Nucleus Server의 환경 설정 파일을 SSL 설정에 맞게 수정합니다.

**단계:**

1. **환경 설정 파일 편집**
   ```bash
   sudo nano /opt/ove/base_stack/nucleus-stack.env
   ```

2. **설정 변경**
   - **서버 호스트명 설정**
     ```plaintext
     #SERVER_IP_OR_HOST=***.***.***.***
     SERVER_IP_OR_HOST=sample.com
     ```

   - **SSL Ingress 호스트 및 포트 설정**
     ```plaintext
     SSL_INGRESS_HOST=sample.com
     SSL_INGRESS_PORT=443
     ```

   - **DNS 리졸버 설정 (필요 시)**
     ```plaintext
     SSL_INGRESS_RESOLVER=
     ```

   - **SSL/TLS 인증서 경로 설정**
     ```plaintext
     SSL_CERT=/opt/ove/base_stack/secrets/treal_xyz_chain_crt.pem
     SSL_CERT_KEY=/opt/ove/base_stack/secrets/treal_xyz_key.pem
     ```

---

#### 4. Nginx 설정 파일 수정

**목표:** Nginx 설정 파일을 수정하여 SSL/TLS 및 리버스 프록시를 구성합니다.

**단계:**

1. **Nginx 기본 설정 파일 수정**
   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```
   - `sites-enabled` 디렉토리 포함을 주석 처리합니다.
     ```plaintext
     include /etc/nginx/conf.d/*.conf;
     # include /etc/nginx/sites-enabled/*;
     ```

2. **Ingress Router 설정 파일 생성 및 수정**
   ```bash
   sudo nano /etc/nginx/conf.d/nginx.ingress.router.conf
   ```
   - 다음 내용을 추가합니다:
     ```nginx
     resolver 8.8.8.8;

     server {
         listen 80;
         server_name sample.com;

         location = /_sys/canonical-name {
             default_type text/plain;
             add_header Access-Control-Allow-Origin *;
             return 200 'sample.com';
         }

         location / {
             return 302 https://sample.com$request_uri;
         }
     }

     server {
         client_max_body_size 0;
         listen 443 ssl;
         server_name sample.com;

         ssl_certificate /etc/ssl/certs/treal_xyz_chain_crt.pem;
         ssl_certificate_key /etc/ssl/certs/treal_xyz_key.pem;

         location = /_sys/canon-name {
             default_type text/plain;
             add_header Access-Control-Allow-Origin *;
             return 200 'sample.com';
         }

         proxy_buffering off;
         proxy_request_buffering off;

         location /omni/api {
             proxy_pass http://localhost:3019;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             proxy_set_header Connection "upgrade";
             proxy_set_header Host $http_host:3019;
         }

         location /omni/lft/ {
             proxy_pass http://localhost:3030/;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             proxy_set_header Connection "upgrade";
         }

         location /omni/discovery {
             proxy_pass http://localhost:3333;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             add_header Access-Control-Allow-Origin * always;
             add_header Access-Control-Allow-Headers * always;
             add_header Access-Control-Allow-Methods * always;
             proxy_set_header Connection "upgrade";
         }

         location /omni/auth {
             proxy_pass http://localhost:3100;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             proxy_set_header Connection "upgrade";
         }

         location /omni/auth/login {
             proxy_pass http://localhost:3180/;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             add_header Access-Control-Allow-Origin *;
             proxy_set_header Connection "upgrade";
         }

         location = / {
             return 302 https://sample.com/omni/web3;
         }

         location /omni/web3/ {
             proxy_pass http://localhost:8080/;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             proxy_set_header Connection "upgrade";
             proxy_set_header Host $http_host;
         }

         location ~* "^/omniverse:/(.*)$" {
             return 302 https://sample.com/omni/web3/omniverse://$1;
         }

         location /omni/tagging3 {
             proxy_pass http://localhost:3020;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             proxy_set_header Connection "upgrade";
         }

         location /omni/search3 {
             proxy_pass http://localhost:3400;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             proxy_set_header Connection "upgrade";
         }

         location /omni/ngsearch2 {
             proxy_pass http://localhost:3503;
             proxy_http_version 1.1;
             proxy_read_timeout 60s;
             proxy_set_header Upgrade $http_upgrade;
             proxy_set_header Connection "upgrade";
         }
     }
     ```

---

#### 5. SSL 인증서 복사

**목표:** SSL 인증서를 Nginx가 참조할 수 있는 위치로 복사합니다.

**단계:**

1. **인증서 복사**
   ```bash
   sudo cp /opt/ove/base_stack/secrets/treal_xyz_chain_crt.pem /etc/ssl/certs/
   sudo cp /opt/ove/base_stack/secrets/treal_xyz_key.pem /etc/ssl/certs/
   ```

---

#### 6. Nginx 설정 테스트 및 재시작

**목표:** Nginx 설정 파일의 문법을 검사하고, 설정을 적용합니다.

**단계:**

1. **Nginx 설정 테스트**
   ```bash
   sudo nginx -t
   ```
   - 설정 파일에 오류가 없으면 다음 메시지가 출력됩니다:
     ```plaintext
     nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
     nginx: configuration file /etc/nginx/nginx.conf test is successful
     ```

2. **Nginx 재시작**
   ```bash
   sudo systemctl restart nginx
   ```

---

#### 7. Nucleus SSL 서버 기동

**목표:** SSL 설정이 적용된 Nucleus Server를 Docker Compose로 실행합니다.

**단계:**

1. **Docker Compose로 Nucleus SSL 서버 실행**
   ```bash
   sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f /opt/ove/base_stack/nucleus-stack-ssl.yml up -d
   ```

---

#### 8. 접속 확인

**목표:** SSL이 적용된 Nucleus Server에 접속하여 정상 동작 여부를 확인합니다.

**단계:**

1. **웹 브라우저에서 접속**
   - URL: `https://sample.com`
   - 루트 경로(`/`)에 접속하면 `/omni/web3`로 리다이렉트됩니다.

2. **주요 경로 확인**
   - `/omni/web3/` → 포트 8080 (Omniverse Navigator)
   - `/omni/auth/login` → 포트 3180 (Auth Login Form)
   - `/omni/api` → 포트 3019 (API)
   - `/omni/auth` → 포트 3100 (Auth)
   - `/omni/discovery` → 포트 3333 (Discovery)
   - `/omni/tagging3` → 포트 3020 (Tagging)
   - `/omni/search3` → 포트 3400 (Search)
   - `/omni/ngsearch2` → 포트 3503 (NGSearch)
   - `/omni/lft/` → 포트 3030 (LFT)

---

### 요약

1. **Nginx 설치 및 시작**
   - 패키지 업데이트 및 Nginx 설치
   - Nginx 서비스 시작 및 자동 실행 설정

2. **Nginx 기본 페이지 설정**
   - `/var/www/html/index.html` 파일 편집
   - Nginx 재시작

3. **Nucleus 환경 설정 파일 수정**
   - `nucleus-stack.env` 파일에서 SSL 관련 설정 변경

4. **Nginx 설정 파일 수정**
   - `/etc/nginx/nginx.conf`에서 기본 설정 주석 처리
   - `/etc/nginx/conf.d/nginx.ingress.router.conf` 파일 생성 및 SSL/TLS 및 리버스 프록시 설정 추가

5. **SSL 인증서 복사**
   - 인증서 파일을 `/etc/ssl/certs/` 디렉토리로 복사

6. **Nginx 설정 테스트 및 재시작**
   - 설정 파일 문법 검사 및 Nginx 재시작

7. **Nucleus SSL 서버 기동**
   - Docker Compose로 SSL 설정이 적용된 Nucleus Server 실행

8. **접속 확인**
   - 웹 브라우저에서 `https://sample.com`로 접속하여 정상 동작 확인

---

### 참고
- SSL 인증서는 신뢰할 수 있는 CA(Certificate Authority)에서 발급받은 것이어야 합니다.
- Nginx 설정 파일 수정 시, 문법 오류가 없는지 반드시 확인하세요.
- 각 서비스의 포트 번호는 환경 설정에 따라 다를 수 있으므로, 실제 환경에 맞게 조정해야 합니다.
- NVIDIA 공식 문서를 참조하여 추가적인 설정이나 문제 해결 방법을 확인할 수 있습니다.
