## orange3 데이터 마이닝

[오렌지3 github](https://github.com/biolab/orange3)

### 오렌지3 설치 절차
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
### 오렌지3(Orange3) 주요 영문-한글 위젯 대조표

| 카테고리 | 위젯 영문명 | 한국어 의미 | 주요 역할 및 설명 |
| :--- | :--- | :--- | :--- |
| **Data** | `File` | 파일 | CSV, Excel(`.xlsx`), `.tab` 등 로컬 데이터 파일 또는 URL 데이터 불러오기 |
| **Data** | `CSV File Import` | CSV 파일 가져오기 | 구분자, 인코딩(UTF-8/CP949) 등을 세밀하게 지정하여 CSV 불러오기 |
| **Data** | `Datasets` | 내장 데이터셋 | 실습용 기본 데이터셋(Iris, Titanic 등) 온라인 다운로드 및 로드 |
| **Data** | `Data Table` | 데이터 표 | 불러온 원본 데이터나 모델 예측/군집 결과를 스프레드시트 표로 확인 |
| **Data** | `Data Info` | 데이터 정보 | 행(Instance)·열(Feature) 개수, 결측치 여부, 변수 유형 요약 확인 |
| **Data** | `Select Columns` | 열 선택 | 분석에 사용할 입력 변수(Feature), 목표 변수(Target), 메타 변수 지정 |
| **Data** | `Select Rows` | 행 선택 | 특정 조건(필터)에 맞는 데이터 행만 추출 |
| **Data** | `Preprocess` | 전처리 | 결측치 대체, 정규화(표준화), 변수 선택, 이산화 등 데이터 정제 |
| **Data** | `Save Data` | 데이터 저장 | 전처리 또는 분석(군집·예측)이 완료된 데이터를 `.csv`, `.xlsx`, `.tab`으로 내보내기 |
| **Visualize** | `Scatter Plot` | 산점도 | 두 수치형 변수(X, Y축)의 분포와 군집·클래스별 색상 구분 시각화 |
| **Visualize** | `Box Plot` | 상자 그림 | 변수별 중앙값, 사분위수, 이상치 및 그룹 간 분포 차이 비교 |
| **Visualize** | `Distributions` | 분포도 | 단일 변수의 히스토그램 및 막대그래프 분포 확인 |
| **Visualize** | `Heat Map` | 히트맵 | 데이터 값의 크기나 군집 패턴을 색상 농도로 시각화 |
| **Model** | `Tree` | 의사결정트리 | 규칙
---
## 오렌지3(Orange3) 군집화 실습 절차 (`evcharging_2.csv`)

### 1. 데이터셋 개요
* **파일명:** `evcharging_2.csv` (99개 행, 결측치 0건)
* **변수 구성:** `latitude`(위도, `37.4610 ~ 37.5677`), `longitude`(경도, `127.0280 ~ 127.1784`) — 모두 수치형(`numeric`) 피처

---

### 2. 오렌지3 위젯 연결 및 군집화 실행 절차

| 단계 | 사용 위젯 (카테고리) | 위젯 연결 방법 (포트 연결) | 구체적 설정 및 조작 방법 | 확인 기준 (정상 작동 여부) |
| :--- | :--- | :--- | :--- | :--- |
| **1단계** | `File`<br>*(Data)* | 캔버스에 단독 배치 | 위젯을 더블클릭 후 폴더 아이콘을 눌러 `evcharging_2.csv` 선택 → 하단 `Columns`에서 `latitude`, `longitude`의 `Type`이 `numeric`, `Role`이 `feature`인지 확인 후 창 닫기 | 창 좌측 하단에 `99`가 표시됨 |
| **2단계** | `Data Table`<br>*(Data)* | `File` 오른쪽 출력(반원) → `Data Table` 왼쪽 입력(반원) 드래그 | 두 위젯 간 회색 실선이 생기도록 간격을 띄워 연결한 뒤, `Data Table`을 더블클릭하여 원본 좌표 데이터 확인 | 상단에 `99 instances`와 2개 열(`latitude`, `longitude`) 표 출력 |
| **3단계** | `k-Means`<br>*(Unsupervised)* | `File` 오른쪽 출력 → `k-Means` 왼쪽 입력 드래그 | 위젯 더블클릭 → `Number of Clusters`에서 **Fixed** 선택 후 `3`(실루엣 점수 최고 `0.5844`) 또는 `4` 입력 → `Initialization`은 **k-Means++** 유지 (창 닫기 시 자동 실행) | `k-Means` 창 하단에 실루엣 점수(Silhouette score)가 계산되어 표시됨 |
| **4단계** | `Data Table (1)`<br>*(Data)* | `k-Means` 오른쪽 출력(`Annotated Data`) → 새 `Data Table` 입력 드래그 | 군집 번호가 부여된 결과를 확인하기 위해 위젯을 더블클릭하여 열 구성 확인 | 기존 위도·경도 열 옆에 `Cluster`(`C1`, `C2`, `C3`) 및 `Silhouette` 열 추가 확인 |
| **5단계** | `Scatter Plot`<br>*(Visualize)* | `k-Means` 오른쪽 출력 → `Scatter Plot` 왼쪽 입력 드래그 | 위젯 더블클릭 → **Axis X:** `longitude`, **Axis Y:** `latitude` 선택 → 좌측 속성 패널의 **Color:** 항목을 `Cluster`로 지정 | 2차원 평면 위에 3개 군집(`C1~C3`)이 서로 다른 색상 점으로 구분되어 표시됨 |
| **6단계** | `Save Data`<br>*(Data)* | `k-Means` 오른쪽 출력 → `Save Data` 왼쪽 입력 드래그 | 위젯 더블클릭 → `Determine type from the file extension` 체크 확인 → **Save as...** 클릭 후 파일명을 `evcharging_cluster.csv`로 입력하여 저장 | 지정 경로에 군집 라벨(`Cluster`)이 포함된 `.csv` 파일 생성 완료 |

---

### 3. 전체 워크플로 연결 구조 요약

```text
[ File (evcharging_2.csv) ] ──┬──> [ Data Table ] (원본 데이터 99행 확인)
                              │
                              └──> [ k-Means (k=3) ] ──┬──> [ Data Table (1) ] (군집 번호·실루엣 점수 확인)
                                                       ├──> [ Scatter Plot ]   (위도·경도 군집 시각화)
                                                       └──> [ Save Data ]      (결과 CSV 내보내기)

> 💡 **실전 팁**
> * 군집 개수를 **4개**로 확정해 분석하려면 `Fixed: 4`로 두고 진행하시면 되며, **99개 데이터 전체에 `C1~C4` 중 하나의 군집 번호가 빠짐없이 붙습니다.**
> * 만약 일부 외곽 데이터는 군집에 억지로 넣지 않고 **이상치(Outlier / 노이즈)**로 따로 구분하고 싶다면 K-Means 대신 **DBSCAN** 같은 밀도 기반 군집화를 사용해야 합니다.

---
## 4. 가상환경 및 불필요한 파일 제외하고 Git 푸시하기 (`.gitignore`)

현재 `orange3` 폴더 안에 가상환경 폴더인 `orange_env/`(`Include`, `Lib`, `Scripts`, `share`, `.lock`, `pyvenv.cfg` 포함)가 함께 들어 있고, 왼쪽 소스 제어 아이콘에 **10K** 개가 넘는 변경 사항이 잡혀 있는 것도 가상환경 파일들이 추적 대상에 포함되었기 때문입니다[cite: 9]. Git 푸시 시 가상환경 폴더와 파이썬 캐시·임시 파일을 제외하려면 `orange3` 루트 폴더에 `.gitignore` 파일을 만들어 아래 내용을 넣으면 됩니다[cite: 9].

### `.gitignore` 작성 내용

```gitignore
# 가상환경 폴더 전체 제외 (Lib, Scripts, Include, pyvenv.cfg 등 포함)
orange_env/

# 파이썬 캐시 및 빌드 산출물
__pycache__/
*.py[cod]
*$py.class
*.egg-info/
dist/
build/

# 환경 변수 및 에디터/OS 임시 파일
.env
.DS_Store
Thumbs.db
```

### 이미 추적 중인 `orange_env` 정리 및 푸시 명령어

```bash
git rm -r --cached orange_env
# 작업 파일을 D:\orange3 루트로 옮기기
Move-Item -Path .\orange_env\K-means_전기차충전소군집.ows, .\orange_env\K-means.png, -Destination D:\orange3\ -ErrorAction SilentlyContinue

# 루트에 있는 작업 파일 스테이징하기
git add .gitignore K-means_전기차충전소군집.ows K-means.png README.md

# 커밋 및 푸시 완료하기
git commit -m "Add .gitignore and exclude virtual environment"
git push
```