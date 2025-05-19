# GitHub/test Repository Overview

이 저장소는 스마트 주차 공간 탐지 시스템 "Parking\_Genius" 프로젝트 및 관련 실험 코드, 개별 팀원 작업 내용을 포함하는 전체 작업 공간입니다. Git을 사용하여 프로젝트의 변경 이력을 관리하고 팀원 간 협업을 진행합니다.

---

발표자료 링크(Canva) : [https://www.canva.com/design/DAGgqab_32s/t-m5z3zhj1ddI1swKuW0yw/view?utm_content=DAGgqab_32s&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=hcfbad3b837]

---

## 🎥 **Demo**

---

### 📌 **Project Demonstration**

<video width="600" height="400" controls>
  <source src="5조_프로젝트_시연.mp4" type="video/mp4">
</video>

---

### 📌 **Processed Test 01**

<video width="600" height="400" controls>
  <source src="processed_test01.mp4" type="video/mp4">
</video>

---

### 📌 **Processed Test 02**

<video width="600" height="400" controls>
  <source src="processed_test02.mp4" type="video/mp4">
</video>

---

## Repository Structure

```
test/
 ┣ .git/              # Git 버전 관리 시스템 파일
 ┣ LJH/               # 팀원 이장헌 작업 공간
 ┣ README.md          # 현재 파일: 저장소 전체 개요 및 Parking_Genius 프로젝트 상세 설명
 ┣ JKL/               # 팀원 이진규 작업 공간
 ┣ app/               # 프로젝트 메인 폴더
 ┗ wonjeonghwan/      # 팀원 원정환 작업 공간
```

---

# Parking\_Genius: 스마트 주차 공간 탐지 시스템

## 📌 **1️⃣ 프로젝트 개요**

### 🚀 **문제 정의**

* 마트나 대형 백화점 주차장에서 발생하는 비효율적인 주차 문제 해결
* 운전자가 빈 자리를 찾기 위해 배회하지 않도록 빠르게 안내

### 🎯 **핵심 해결 과제**

* CCTV 영상을 실시간으로 분석하여 주차 가능 공간을 탐지할 수 있는가?
* 운전자가 선호하는 구역을 기준으로 빠르게 빈 자리를 안내할 수 있는가?
* 웹 플랫폼을 통해 시각적으로 실시간 주차 정보를 제공할 수 있는가?

---

## 👥 **2️⃣ 팀원 및 역할 분담**

| 역할 | 이름                                                                            |
| -- | ----------------------------------------------------------------------------- |
| 팀장 | **이진규** \[[https://github.com/OnyX0000](https://github.com/OnyX0000)]         |
| 팀원 | **원정환** \[[https://github.com/wonjeonghwan](https://github.com/wonjeonghwan)] |
| 팀원 | **이장헌** \[[https://github.com/LJH0963](https://github.com/LJH0963)]           |

---

## 🛠️ **3️⃣ 프로젝트 구현 과정**

### ✅ **(1) 데이터 수집 및 전처리**

* Roboflow에서 주차 공간 탐지 데이터셋 확보
* Bounding Box 라벨링 검수 및 수정
* YOLOv11 모델 학습에 최적화된 형태로 전처리 진행

### ✅ **(2) 모델 학습 및 평가**

* YOLOv11n → YOLOv11l → YOLOv11x 순서로 학습
* 최적 모델로 YOLOv11x 선정 및 성능 평가
* DeepSORT을 활용한 차량 추적 기능 추가

### ✅ **(3) API 서버 구축**

* **FastAPI**를 사용하여 실시간 주차 공간 탐지 API 구성

| HTTP Method | Endpoint                      | 설명                           |
| ----------- | ----------------------------- | ---------------------------- |
| `POST`      | `/video/upload/`              | CCTV 영상 업로드 및 1초 프레임 미리보기 제공 |
| `POST`      | `/video/select_parking_spot/` | 클릭한 주차 좌표를 서버에 저장하고 분석 시작    |
| `GET`       | `/video/download/{filename}`  | 분석이 완료된 영상 결과 파일 다운로드        |
| `GET`       | `/video/preview/{video_id}`   | 업로드된 영상의 1초 프레임 미리보기 제공      |

#### 🚀 **API 상세 설명**

---

### `/video/upload/`

* **설명:**

  * 사용자가 업로드한 CCTV 영상 파일을 서버에 저장합니다.
  * 파일은 UUID를 기반으로 고유한 이름으로 저장되며, 저장 경로는 `app/resources/videos`입니다.
  * 1초 프레임을 추출하여 미리보기 이미지를 생성하고, 서버에 저장합니다.
  * 추출된 미리보기 이미지는 `/video/preview/{video_id}`를 통해 접근할 수 있습니다.

---

### `/video/select_parking_spot/`

* **설명:**

  * 미리보기 이미지에서 사용자가 클릭한 주차 좌표를 서버에 전달합니다.
  * 전달된 좌표와 `video_id`를 기반으로 YOLOv11 모델을 활용한 분석이 시작됩니다.
  * 분석이 완료되면 `/video/download/{filename}`을 통해 결과를 다운로드할 수 있습니다.

---

### `/video/download/{filename}`

* **설명:**

  * 분석이 완료된 CCTV 영상 파일을 다운로드할 수 있는 API입니다.
  * 파일 경로는 `app/resources/downloaded/`에 저장됩니다.

---

### `/video/preview/{video_id}`

* **설명:**

  * 업로드된 영상의 1초 프레임 이미지를 제공하는 API입니다.
  * 사용자는 `/video/upload/` 응답으로 받은 `preview_url`을 통해 접근할 수 있습니다.
  * 미리보기 이미지 경로는 `app/resources/videos`에 저장됩니다.

---

#### 🔍 **4️⃣ 주요 분석 결과**

* YOLOv11x 모델을 통한 높은 정확도의 주차 공간 탐지
* 빈 공간 탐지 정확도 95% 이상 달성
* DeepSORT로 동일 차량 추적 기능 추가

---

## 💻 **5️⃣ 기술 스택**

| 기술            | 설명                               |
| ------------- | -------------------------------- |
| **Python**    | 데이터 처리 및 모델 학습                   |
| **FastAPI**   | 모델 배포 및 API 서버 구성                |
| **YOLOv11x**  | 주차 공간 탐지 모델, 높은 성능을 보이는 최신 버전 사용 |
| **DeepSORT**  | 차량 추적 및 동일 차량 식별                 |
| **HTML & JS** | 웹 페이지 구성 및 시연 화면 생성              |
| **Roboflow**  | 데이터셋 관리 및 전처리                    |

---

## 🎯 **6️⃣ 프로젝트 결과 및 기대 효과**

* 마트 및 대형 백화점의 주차 공간 탐지 문제 해결
* FastAPI와 HTML을 활용한 실시간 탐지 시연 가능
* 빈자리 탐지 결과를 시각적으로 제공하여 주차 시간 단축

---

## 🚀 **7️⃣ 실행 방법**

1. **FastAPI 서버 실행(git clone 후 진행)**

   ```bash
   uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
   ```

2. **웹 페이지 접속**

   * [http://localhost:8000/static/index.html](http://localhost:8000/static/index.html)

---

## 📌 **8️⃣ 프로젝트 구조**

```plaintext
📦Parking_Genius
 ┣ 📂app
 ┃ ┣ 📜__init__.py             # 패키지 초기화 파일
 ┃ ┣ 📜main.py                 # FastAPI 서버 엔트리 포인트
 ┃ ┣ 📂models
 ┃ ┃ ┣ 📜__init__.py
 ┃ ┃ ┗ 📜model_loader.py       # YOLOv11x 모델 로딩 및 관리
 ┃ ┣ 📂resources               # 업로드된 파일 및 분석 결과 저장 폴더
 ┃ ┣ 📂routers
 ┃ ┃ ┣ 📜__init__.py
 ┃ ┃ ┗ 📜video.py              # Video 관련 API 엔드포인트 정의
 ┃ ┣ 📂services
 ┃ ┃ ┣ 📜__init__.py
 ┃ ┃ ┗ 📜video_service.py      # 영상 처리 로직 (프레임 추출, 분석 처리 등)
 ┃ ┗ 📂static
 ┃ ┃ ┗ 📜favicon.ico           # 웹 페이지 아이콘
 ┗ 📜requirements.txt          # 필요한 라이브러리 목록
```

---

## 📌 **9️⃣ 개선 고려사항**

* 실시간 스트리밍 연동 필요
* 다양한 주차 유형(사선 주차, 평행 주차) 학습 필요
* 다중 카메라 통합 시 전역 탐지 기능 확립
* 내비게이션 경로 안내 기능 추가 고려
