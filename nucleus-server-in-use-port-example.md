# Nucleus Server 포트 설명서

이 문서는 Nucleus Server가 사용 중인 포트에 대한 정보를 제공합니다. 아래의 내용은 `netstat` 및 `ss` 명령어를 통해 확인된 활성 인터넷 연결을 기반으로 작성되었습니다.

## 포트 목록 및 설명

| 포트 번호 | 프로토콜 | 상태   | 바인딩 주소       | 프로세스 이름               | 설명                                   |
|-----------|----------|--------|-------------------|-----------------------------|----------------------------------------|
| 6010      | TCP      | LISTEN | 127.0.0.1         | sshd: nucleus (PID: 32530) | Nucleus 서버의 SSH 연결을 위한 포트   |
| 8080      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 20931)  | 웹 애플리케이션 또는 API 서비스용 포트 |
| 8081      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 2860)   | 추가 웹 애플리케이션 또는 API 서비스용 포트 |
| 8000      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 20727)  | 웹 애플리케이션 또는 API 서비스용 포트 |
| 5300      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 3043)   | 특정 서비스 또는 애플리케이션용 포트  |
| 3333      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 20349)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3400      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 21006)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3106      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 21093)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3100      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 20689)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3180      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 20709)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3009      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 21057)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3010      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 20808)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3020      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 20227)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3019      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 21074)  | 특정 서비스 또는 애플리케이션용 포트  |
| 3030      | TCP      | LISTEN | 0.0.0.0           | docker-proxy (PID: 20768)  | 특정 서비스 또는 애플리케이션용 포트  |
| 443       | TCP      | LISTEN | 0.0.0.0           | nginx: master (PID: 22390) | HTTPS 웹 서버 포트                     |
| 22        | TCP      | LISTEN | 0.0.0.0           | sshd: /usr/sbin (PID: 900) | SSH 연결을 위한 포트                  |
| 80        | TCP      | LISTEN | 0.0.0.0           | nginx: master (PID: 22390) | HTTP 웹 서버 포트                     |
| 53        | UDP      | UNCONN | 127.0.0.53        | systemd-resolve (PID: 651) | DNS 서비스 포트                       |
| 9090      | TCP      | LISTEN | :::               | prometheus (PID: 799)      | Prometheus 모니터링 서비스 포트      |
| 9100      | TCP      | LISTEN | :::               | node_exporter (PID: 793)   | Node Exporter 서비스 포트             |
| 3000      | TCP      | LISTEN | :::               | grafana (PID: 776)         | Grafana 대시보드 포트                |

## 결론

위의 포트들은 Nucleus Server와 관련된 다양한 서비스 및 애플리케이션을 지원합니다. 각 포트의 사용 목적에 따라 적절한 보안 설정 및 접근 제어를 적용하는 것이 중요합니다.
