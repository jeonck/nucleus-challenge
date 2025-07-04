```python
import streamlit as st
import docker
import os

# Nucleus 서버 정보
NUCLEUS_HOST = "XX.XXX.XX.XXX"  # IP 주소 마스킹 처리
NUCLEUS_PORT = "3009"
NUCLEUS_USER = "omniverse"
NUCLEUS_PASSWORD = "omniverse123!"

# Docker 클라이언트 설정
client = docker.from_env()

def upload_usd(file_path, destination_path):
    # Nucleus 도구 이미지
    image_name = "nvcr.io/nvidia/omniverse/nucleus-tools:1.2.2"
    
    # 파일 경로와 목적지 경로 설정
    mount_path = os.path.abspath("temp_upload")
    os.makedirs(mount_path, exist_ok=True)
    
    # Docker 컨테이너 실행
    try:
        container = client.containers.run(
            image=image_name,
            command=f"upload /files/ {NUCLEUS_HOST} {destination_path} -u {NUCLEUS_USER} -p {NUCLEUS_PASSWORD}",
            volumes={mount_path: {'bind': '/files', 'mode': 'rw'}},
            environment=["ACCEPT_EULA=Y"],
            detach=True
        )
        
        # 컨테이너 로그 확인
        for line in container.logs(stream=True):
            st.text(line.strip().decode())
        
        container.stop()
        container.remove()
        st.success("파일 업로드가 완료되었습니다.")
    except Exception as e:
        st.error(f"업로드 중 오류가 발생했습니다: {e}")

def main():
    st.title("Nucleus 서버로 USD 파일 업로드")
    
    # 파일 업로드
    uploaded_file = st.file_uploader("업로드할 USD 파일 선택", type=["usd", "usda", "usdc"])
    
    if uploaded_file is not None:
        with open(f"./temp_upload/{uploaded_file.name}", "wb") as f:
            f.write(uploaded_file.getbuffer())
        
        destination_path = st.text_input("업로드할 Nucleus 서버의 목적지 경로", "/Projects/TEST_project_rename")
        
        if st.button("업로드 시작"):
            if destination_path:
                upload_usd(f"./temp_upload/{uploaded_file.name}", destination_path)
            else:
                st.warning("목적지 경로를 입력해주세요.")

if __name__ == "__main__":
    main()
```

### 코드 흐름 설명

1. **라이브러리 임포트**: 
   - `import streamlit as st`, `import docker`, `import os`를 통해 Streamlit, Docker, os 라이브러리를 임포트하여 웹 애플리케이션과 Docker 컨테이너를 관리할 준비를 합니다.

2. **Nucleus 서버 정보 설정**: 
   - Nucleus 서버의 IP 주소, 포트, 사용자 이름 및 비밀번호를 설정합니다. IP 주소는 보안을 위해 마스킹 처리하였습니다.

3. **Docker 클라이언트 초기화**: 
   - `docker.from_env()`를 사용하여 현재 환경에서 Docker 클라이언트를 초기화하여 컨테이너를 관리할 수 있도록 합니다.

4. **파일 업로드 함수 정의**: 
   - `upload_usd(file_path, destination_path)` 함수를 정의하여 USD 파일을 Nucleus 서버에 업로드하는 기능을 수행합니다.

   4.1 **도구 이미지 설정**: 
   - `image_name`에 업로드를 위해 사용할 Docker 이미지의 이름을 설정합니다.

   4.2 **경로 설정**: 
   - `os.makedirs(mount_path, exist_ok=True)`를 통해 임시 파일을 저장할 디렉토리를 생성하고, 경로를 설정합니다.

   4.3 **컨테이너 실행**: 
   - `client.containers.run()`을 사용하여 Nucleus 서버에 파일을 업로드하기 위해 Docker 컨테이너를 실행합니다.

   4.4 **로그 확인 및 종료**: 
   - `container.logs`를 통해 컨테이너의 로그를 출력하고, 작업이 완료되면 `container.stop()`과 `container.remove()`를 통해 컨테이너를 중지하고 제거합니다.

5. **메인 함수 정의**: 
   - `main()` 함수를 정의하여 Streamlit 애플리케이션의 제목을 설정하고, 사용자 인터페이스를 구성합니다.

   5.1 **파일 업로드 UI**: 
   - `st.file_uploader()`를 사용하여 사용자가 업로드할 USD 파일을 선택할 수 있는 파일 업로더를 생성합니다.

   5.2 **파일 저장 및 업로드**: 
   - 사용자가 선택한 파일을 저장하고, 목적지 경로를 입력받아 업로드를 시작합니다.

6. **애플리케이션 실행**: 
   - `if __name__ == "__main__": main()`을 통해 스크립트가 직접 실행될 때 `main()` 함수를 호출하여 Streamlit 애플리케이션을 시작합니다.
[1] https://forums.developer.nvidia.com/t/save-usd-to-external-nucleus-from-python/255617
[2] https://manual.reallusion.com/Omniverse-Plug-in/Content/ENU/iC-7.9/03-Working-with-Omniverse/Exporting-USD/USD-to-Server-for-Collaboration.htm
[3] https://forums.developer.nvidia.com/t/communicate-between-python-scripts-using-nucleus-server/246303
[4] https://forums.developer.nvidia.com/t/nucleus-file-manipulation-in-python/261622
[5] https://docs.omniverse.nvidia.com/services/latest/services/usd-search/deploy.html
[6] https://github.com/NVIDIA-Omniverse/connect-samples/blob/main/README.md
[7] https://github.com/isaac-sim/IsaacLab/issues/285
[8] https://www.markiiisys.com/blog/reinforcement-learning-ai-using-nvidia-isaac-for-robotics/
[9] https://github.com/NVIDIA-Omniverse/usd_scene_construction_utils
[10] https://mtw75.medium.com/omniverse-kit-cheat-sheet-9d2a7cce9fb
[11] https://medium.com/@anais.druart/converting-obj-mtl-files-to-usd-files-in-omniverse-a7ce963f51de
[12] https://knxwledge.tistory.com/7
[13] https://aws.amazon.com/blogs/spatial/deploying-nvidia-omniverse-nucleus-on-amazon-ec2/
[14] https://www.ngene.org/wsw.html
[15] https://docs.robotsfan.com/isaacsim/latest/utilities/asset_browser.html
[16] https://docs.informatica.com/integration-cloud/data-integration/current-version/transformations/data-masking-transformation/ip-address-masking.html
[17] https://www.pollux.ai/ko/tech-blog
[18] https://extra-ordinary.tv/2024/04/11/accessing-the-omniverse-message-bus-from-python/
[19] https://stackoverflow.com/questions/68594897/use-python-ip-address-to-mask-ip-addresses
[20] https://docs.python.org/3/library/ipaddress.html
[21] https://python-forum.io/thread-3266.html
[22] https://docs.python.org/ko/dev/library/ipaddress.html
[23] https://github.com/orgs/community/discussions/123688
[24] https://scrapfly.io/blog/how-to-hide-your-ip-address-while-scraping/
[25] https://www.reddit.com/r/learnpython/comments/t6u7h6/ip_network_subnet_mask_code_for_python/
[26] https://www.geeksforgeeks.org/python/how-to-manipulate-ip-addresses-in-python-using-ipaddress-module/
[27] https://superuser.com/questions/1619465/how-to-use-a-web-proxy-python-program-to-hide-ip-address-while-web-browsing
[28] https://medium.com/opsops/wildcard-masks-operations-in-python-16acf1c35683
[29] https://quickscraper.co/how-to-hide-your-ip-address-for-web-scraping/
