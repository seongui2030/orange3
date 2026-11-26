## orange3 데이터 마이닝

[오렌지3](https://github.com/biolab/orange3)

### 오렌지 설치 절차
- **깃저장소 만들기:** https://github.com/seongui2030/orange3.git
- **파워쉘 실행: > git clone https://github.com/seongui2030/orange3.git >  cd orange3 폴더로 이동
- **orange3 설치 명령어:** uv tool install -p 3.12 -w PyQt6,PyQt6-WebEngine orange3
    - **가상환경 생성 및 활성화:** 
    ```bash
    python -m venv orange_env
    orange_env\Scripts\activate
    ```
    - **활성화된 (orange_env) 가상환경 내부에 직접 설치:**
    ```bash
    uv pip install PyQt6 PyQt6-WebEngine orange3
    ```
    - **python -m 명령어로 실행 (차단 우회).exe 실행 파일 대신 가상환경의 python.exe로 캔버스를 직접 호출:**
    ```bash
    python -m Orange.canvas
    ```
    - **Windows 보안(Windows Security) 알림 창이 뜨면:**
    ```bash
    Get-ChildItem -Path "D:\orange3\orange_env" -Recurse -Include *.pyd,*.dll | Unblock-File
    ```
---