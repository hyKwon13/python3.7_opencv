# VisionSensor API 문서

## 목차

1. [기본 정보](#1-기본-정보)
2. [카메라 설정](#2-카메라-설정)
    - [포커스 설정](#포커스-설정)
    - [노출 설정](#노출-설정)
    - [자동 포커스 및 노출 설정](#자동-포커스-및-노출-설정)
3. [네트워크 설정](#3-네트워크-설정)
    - [장치 IP 설정](#장치-ip-설정)
    - [장치 존재 여부 확인](#장치-존재-여부-확인)
4. [FTP 설정](#4-ftp-설정)
    - [출력처 설정](#출력처-설정)
    - [접속 설정 업데이트](#접속-설정-업데이트)
    - [FTP 접속 테스트](#ftp-접속-테스트)
    - [전송 조건 설정](#전송-조건-설정)
5. [ROI 설정](#5-roi-설정)
    - [위치 보정 ROI 설정](#위치-보정-roi-설정)
    - [색 평균 ROI 설정](#색-평균-roi-설정)
    - [대조 ROI 설정](#대조-roi-설정)
    - [직경 ROI 설정](#직경-roi-설정)
    - [돌출 검출 ROI 설정](#돌출-검출-roi-설정)
    - [ROI 조회](#roi-조회)
    - [활성화된 기능 조회](#활성화된-기능-조회)
    - [기능 비활성화](#기능-비활성화)
6. [한계 값 설정](#6-한계-값-설정)
    - [한계 값 설정](#한계-값-설정)
    - [한계 값 조회](#한계-값-조회)
7. [GPIO 제어](#7-gpio-제어)
    - [출력 상태 설정](#출력-상태-설정)
    - [포트 상태 조회](#포트-상태-조회)
    - [입력 신호 확인](#입력-신호-확인)
    - [IO 포트 설정](#io-포트-설정)
    - [IO 포트 조회](#io-포트-조회)
8. [종합 판정 조건](#8-종합-판정-조건)
    - [종합 판정 조건 설정](#종합-판정-조건-설정)
    - [종합 판정 조건 조회](#종합-판정-조건-조회)
9. [마스터 이미지](#9-마스터-이미지)
    - [마스터 이미지 조회](#마스터-이미지-조회)
    - [마스터 이미지 설정](#마스터-이미지-설정)
10. [TRIG 조건](#10-trig-조건)
    - [TRIG 조건 설정](#trig-조건-설정)
    - [TRIG 조건 조회](#trig-조건-조회)
11. [AI 촬영](#11-ai-촬영)
    - [AI 촬영 시작](#ai-촬영-시작)
    - [AI 촬영 결과 조회](#ai-촬영-결과-조회)
    - [AI 촬영 상태 조회](#ai-촬영-상태-조회)
12. [기타 기능](#12-기타-기능)
    - [모든 설정 저장](#모든-설정-저장)
    - [모든 설정 불러오기](#모든-설정-불러오기)
13. [운전](#13-운전)

---

## 1. 기본 정보

- **Base URL:** `http://<서버 IP>:<포트>/`
- **기본 포트:** `8000` (기본값)
- **응답 형식:** JSON

---

## 2. 카메라 설정

### 포커스 설정

- **API:** `/set_focus`
- **HTTP 메서드:** `PUT`
- **설명:** 카메라의 포커스 값을 설정합니다.
- **요청 Body:**

    ```json
    {
        "focus": 100  // 설정할 포커스 값 (정수)
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "focus": 100
    }
    ```

### 노출 설정

- **API:** `/set_exposure`
- **HTTP 메서드:** `PUT`
- **설명:** 카메라의 노출 값을 설정합니다.
- **요청 Body:**

    ```json
    {
        "exposure": 200  // 설정할 노출 값 (정수)
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "exposure": 200
    }
    ```

### 자동 포커스 및 노출 설정

- **API:** `/set_auto_controls`
- **HTTP 메서드:** `PUT`
- **설명:** 카메라의 자동 포커스 및 자동 노출 기능을 설정합니다.
- **요청 Body:**

    ```json
    {
        "auto_focus": true,       // 자동 포커스 활성화 여부 (boolean)
        "auto_exposure": false    // 자동 노출 활성화 여부 (boolean)
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "auto_focus": true,
        "auto_exposure": false
    }
    ```

---

## 3. 네트워크 설정

### 장치 IP 설정

- **API:** `/set_device_ip`
- **HTTP 메서드:** `PUT`
- **설명:** 장치의 IP 주소, 포트, 서브넷 마스크를 설정합니다.
- **요청 Body:**

    ```json
    {
        "ip": "192.168.1.100",
        "port": 8080,
        "subnet_mask": "255.255.255.0"
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "ip": "192.168.1.100",
        "port": 8080,
        "subnet_mask": "255.255.255.0"
    }
    ```

### 장치 존재 여부 확인

- **API:** `/existence_device`
- **HTTP 메서드:** `GET`
- **설명:** 장치의 존재 여부를 확인하고, 장치 이름과 펌웨어 버전을 반환합니다.
- **요청 파라미터:** 없음
- **응답:**

    ```json
    {
        "device_name": "Visionsensor_0",
        "firmware_version": "0.0.1"
    }
    ```

---

## 4. FTP 설정

### 출력처 설정

- **API:** `/set_output_destination`
- **HTTP 메서드:** `PUT`
- **설명:** 이미지 출력처를 설정합니다.
- **요청 Body:**

    ```json
    {
        "feature": "FTP"  // "무효", "FTP", "SD카드" 중 하나
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "output_destination": "FTP"
    }
    ```
    
### 접속 설정 업데이트

- **API:** `/set_connection_settings`
- **HTTP 메서드:** `PUT`
- **설명:** FTP 서버의 접속 설정을 업데이트합니다.
- **요청 Body:**

    ```json
    {
        "ip": "ftp.example.com",
        "port": 21,
        "username": "user",
        "password": "pass"
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "connection_settings": {
            "ip": "ftp.example.com",
            "port": 21,
            "username": "user",
            "password": "pass"
        }
    }
    ```

### FTP 접속 테스트

- **API:** `/test_connection`
- **HTTP 메서드:** `PUT`
- **설명:** 설정된 FTP 서버에 접속 테스트를 수행합니다.
- **요청 Body:**

    ```json
    {
        "host": "ftp.example.com",
        "port": 21,
        "username": "user",
        "password": "pass"
    }
    ```

- **응답:**
    - **성공:**

        ```json
        {
            "status": "success",
            "message": "FTP 접속에 성공했습니다."
        }
        ```

    - **실패:**

        ```json
        {
            "status": "failure",
            "message": "접속 실패 원인"
        }
        ```

### 전송 조건 설정

- **API:** `/set_transfer_condition`
- **HTTP 메서드:** `PUT`
- **설명:** FTP로 이미지 전송 조건을 설정합니다.
- **요청 Body:**

    ```json
    {
        "transfer_condition": "OK"  // "OK", "NG", "NOT" 중 하나
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "condition": "OK"
    }
    ```

---

## 5. ROI 설정

### 위치 보정 ROI 설정

- **API:** `/set_roi`
- **HTTP 메서드:** `PUT`
- **설명:** 위치 보정을 위한 ROI(Region of Interest)를 설정합니다.
- **요청 Body:**

    ```json
    {
        "top_left": [100, 100],       // ROI의 좌상단 좌표 (x, y)
        "bottom_right": [200, 200]    // ROI의 우하단 좌표 (x, y)
    }
    ```

- **응답:**

    ```json
    {
        "status": "Position ROI set",
        "top_left": [100, 100],
        "bottom_right": [200, 200]
    }
    ```

### 색 평균 ROI 설정

- **API:** `/set_color_average_roi`
- **HTTP 메서드:** `PUT`
- **설명:** 색 평균 계산을 위한 ROI를 설정합니다.
- **요청 Body:**

    ```json
    {
        "top_left": [150, 150],
        "bottom_right": [250, 250]
    }
    ```

- **응답:**

    ```json
    {
        "status": "Color average ROI set successfully",
        "roi_index": 0
    }
    ```

### 대조 ROI 설정

- **API:** `/set_contrast_roi`
- **HTTP 메서드:** `PUT`
- **설명:** 대조 계산을 위한 ROI를 설정합니다.
- **요청 Body:**

    ```json
    {
        "top_left": [300, 300],
        "bottom_right": [400, 400]
    }
    ```

- **응답:**

    ```json
    {
        "status": "Contrast ROI set successfully",
        "roi_index": 0
    }
    ```

### 직경 ROI 설정

- **API:** `/set_diameter_roi`
- **HTTP 메서드:** `PUT`
- **설명:** 직경 측정을 위한 ROI를 설정합니다.
- **요청 Body:**

    ```json
    {
        "top_left": [500, 500],
        "bottom_right": [600, 600]
    }
    ```

- **응답:**

    ```json
    {
        "status": "Diameter ROI set successfully",
        "reference_diameter": 50.0,
        "roi_index": 0
    }
    ```

### 돌출 검출 ROI 설정

- **API:** `/set_protrusion_roi`
- **HTTP 메서드:** `PUT`
- **설명:** 돌출 검출을 위한 ROI와 HSV 범위를 설정합니다.
- **요청 Body:**

    ```json
    {
        "top_left": [700, 700],
        "bottom_right": [800, 800],
        "hsv_lower": [50, 100, 100],
        "hsv_upper": [70, 255, 255]
    }
    ```

- **응답:**

    ```json
    {
        "status": "Protrusion ROI and HSV range set successfully",
        "roi_index": 0
    }
    ```

### ROI 조회

- **API:** `/get_active_rois`
- **HTTP 메서드:** `GET`
- **설명:** 활성화된 모든 ROI 목록을 조회합니다.
- **응답:**

    ```json
    {
        "rois": [
            {
                "top_left": [100, 100],
                "bottom_right": [200, 200],
                "type": "position_correction"
            },
            {
                "top_left": [150, 150],
                "bottom_right": [250, 250],
                "type": "color_average"
            }
            // 기타 ROI들...
        ]
    }
    ```

### 활성화된 기능 조회

- **API:** `/get_active_features`
- **HTTP 메서드:** `GET`
- **설명:** 활성화된 기능 목록과 각 기능의 ROI 개수를 조회합니다.
- **응답:**

    ```json
    {
        "active_features": ["position_correction", "color_average", "contrast"],
        "roi_counts": {
            "color_average": 2,
            "contrast": 1,
            "diameter": 1,
            "protrusion_detection": 0
        }
    }
    ```

### 기능 비활성화

- **API:** `/disable_feature`
- **HTTP 메서드:** `PUT`
- **설명:** 특정 기능을 비활성화합니다.
- **요청 Body:**

    ```json
    {
        "feature": "color_average"  // 비활성화할 기능 이름
    }
    ```

- **응답:**

    ```json
    {
        "status": "color_average feature disabled"
    }
    ```

    **또는**

    ```json
    {
        "status": "color_average feature is not active"
    }
    ```

---

## 6. 한계 값 설정

### 한계 값 설정

- **API:** `/set_tolerance`
- **HTTP 메서드:** `PUT`
- **설명:** 특정 도구의 한계 값을 설정합니다.
- **요청 Body:**

    ```json
    {
        "tool": "color_average",  // "position_correction", "color_average", "contrast", "diameter", "protrusion_detection"
        "roi_index": 0,           // 설정할 ROI의 인덱스
        "value": 50.0             // 한계 값 (0 ~ 100)
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "tolerance_settings": {
            "color_average": {
                "0": 50.0
            }
            // 기타 한계 값 설정...
        }
    }
    ```

### 한계 값 조회

- **API:** `/get_tolerance`
- **HTTP 메서드:** `GET`
- **설명:** 현재 설정된 모든 한계 값을 조회합니다.
- **응답:**

    ```json
    {
        "tolerance_settings": {
            "position_correction": 50,
            "color_average": {
                "0": 50,
                "1": 40
            },
            "contrast": {
                "0": 50
            },
            "diameter": {
                "0": 5
            },
            "protrusion_detection": {
                "0": 50
            }
        }
    }
    ```

---

## 7. GPIO 제어

### 출력 상태 설정

- **API:** `/set_output_state`
- **HTTP 메서드:** `PUT`
- **설명:** 특정 출력 포트의 상태를 설정합니다.
- **요청 Body:**

    ```json
    {
        "feature": "OUT1_ON"  // 형식: "포트이름_동작", 예: "OUT1_ON", "OUT2_OFF"
    }
    ```

    **참고:** `feature` 값은 "OUT1_OFF", "OUT1_ON", "OUT2_OFF" 등과 같은 형식이어야 합니다.

- **응답:**

    ```json
    {
        "status": "success",
        "out_name": "OUT1",
        "action": "ON"
    }
    ```

### 포트 상태 조회

- **API:** `/get_port_states`
- **HTTP 메서드:** `GET`
- **설명:** 현재 모든 출력 포트의 상태를 조회합니다.
- **응답:**

    ```json
    {
        "port_states": {
            "OUT1": "ON",
            "OUT2": "OFF",
            // ...
            "OUT8": "OFF"
        }
    }
    ```

### 입력 신호 확인

- **API:** `/check_input_signal`
- **HTTP 메서드:** `GET`
- **설명:** 특정 입력 포트(IN1)에서 신호를 감지하여 반환합니다.
- **응답:**

    ```json
    {
        "input1_signal": true  // 신호 감지 여부 (true: HIGH, false: LOW)
    }
    ```

### IO 포트 설정

- **API:** `/set_io_ports`
- **HTTP 메서드:** `PUT`
- **설명:** 입력 포트 설정을 업데이트합니다.
- **요청 Body:**

    ```json
    {
        "ports": ["외부TRIG(UP)", "OFF", "HIGH", "LOW"]  // 각 입력 포트(IN1~IN8)의 설정
    }
    ```

    **설명:**
    - 각 포트는 "외부TRIG(UP)", "외부TRIG(DOWN)", "OFF", "HIGH", "LOW" 중 하나로 설정할 수 있습니다.
    - `ports` 리스트의 순서는 IN1부터 IN8까지의 순서입니다.

- **응답:**

    ```json
    {
        "status": "success",
        "ports": ["외부TRIG(UP)", "OFF", "HIGH", "LOW"]
    }
    ```

### IO 포트 조회

- **API:** `/get_io_ports`
- **HTTP 메서드:** `GET`
- **설명:** 현재 모든 I/O 포트의 상태를 조회합니다.
- **응답:**

    ```json
    {
        "port_states": {
            "IN1": "외부TRIG(UP)",
            "IN2": "OFF",
            "IN3": "HIGH",
            "IN4": "LOW",
            // ...
            "IN8": "OFF"
        }
    }
    ```

---

## 8. 종합 판정 조건

### 종합 판정 조건 설정

- **API:** `/set_judgement_condition`
- **HTTP 메서드:** `PUT`
- **설명:** 종합 판정 조건을 설정합니다.
- **요청 Body:**

    ```json
    {
        "feature": "ALL_OK"  // "ALL_OK", "ONE_OK" 중 하나
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "judgement_condition": "ALL_OK"
    }
    ```

### 종합 판정 조건 조회

- **API:** `/get_judgement_condition`
- **HTTP 메서드:** `GET`
- **설명:** 현재 설정된 종합 판정 조건을 조회합니다.
- **응답:**

    ```json
    {
        "judgement_condition": "ALL_OK"
    }
    ```

---

## 9. 마스터 이미지

### 마스터 이미지 조회

- **API:** `/get_master_image`
- **HTTP 메서드:** `GET`
- **설명:** 마스터 이미지를 반환합니다.
- **요청 파라미터:** 없음
- **응답:**
    - **성공 시:** `master_image.jpg` 파일
    - **이미지 없음:**

        ```json
        {
            "status": "no_image"
        }
        ```

### 마스터 이미지 설정

- **API:** `/set_master_image`
- **HTTP 메서드:** `PUT`
- **설명:** 마스터 이미지를 설정합니다.
- **요청 Body:** `multipart/form-data` 형식의 이미지 파일
- **응답:**

    ```json
    {
        "status": "success"
    }
    ```

---

## 10. TRIG 조건

### TRIG 조건 설정

- **API:** `/set_trig_condition`
- **HTTP 메서드:** `PUT`
- **설명:** TRIG 상태를 설정합니다.
- **요청 Body:**

    ```json
    {
        "feature": "내부 TRIG"  // "내부 TRIG", "외부 TRIG" 중 하나
    }
    ```

- **응답:**

    ```json
    {
        "status": "success",
        "trig_condition": "내부 TRIG"
    }
    ```

### TRIG 조건 조회

- **API:** `/get_trig_condition`
- **HTTP 메서드:** `GET`
- **설명:** 현재 설정된 TRIG 상태를 조회합니다.
- **응답:**

    ```json
    {
        "trig_condition": "내부 TRIG"
    }
    ```

---

## 11. AI 촬영

### AI 촬영 시작

- **API:** `/start_ai_capture`
- **HTTP 메서드:** `PUT`
- **설명:** AI 촬영 프로세스를 시작합니다.
- **요청 Body:** 없음
- **응답:**

    ```json
    {
        "status": "AI capture started"
    }
    ```

### AI 촬영 결과 조회

- **API:** `/get_ai_capture_results`
- **HTTP 메서드:** `GET`
- **설명:** AI 촬영 결과를 반환합니다.
- **요청 파라미터:** 없음
- **응답:**

    ```json
    {
        "status": "success",
        "results": [
            {
                "method": "Histogram",
                "exposure": 100,
                "score": null,
                "image": "<base64 인코딩된 이미지 데이터>"
            }
            // 기타 촬영 결과...
        ]
    }
    ```

### AI 촬영 상태 조회

- **API:** `/ai_capture_status`
- **HTTP 메서드:** `GET`
- **설명:** AI 촬영의 진행 상태를 반환합니다.
- **응답:**

    ```json
    {
        "in_progress": false
    }
    ```

---

## 12. 기타 기능

### 모든 설정 저장

- **API:** `/save_all_settings`
- **HTTP 메서드:** `PUT`
- **설명:** 모든 설정을 파일에 저장합니다.
- **요청 파라미터:** 없음
- **응답:**

    ```json
    {
        "status": "success",
        "message": "All settings saved"
    }
    ```

### 모든 설정 불러오기

- **API:** `/load_all_settings`
- **HTTP 메서드:** `GET`
- **설명:** 모든 설정을 파일에서 불러옵니다.
- **응답:**

    ```json
    {
        "status": "success",
        "output_destination": "FTP",
        "connection_settings": {
            "ip": "ftp.example.com",
            "port": 21,
            "username": "user",
            "password": "pass"
        },
        "transfer_condition": "OK"
    }
    ```

---

## 13. 운전

- **API:** `/get_correction_frame`
- **HTTP 메서드:** `GET`
- **설명:** 이미지 처리를 수행하고 결과 이미지를 반환합니다.
- **요청 파라미터:** 없음
- **응답:**

    ```json
    {
        "image": "ffd8ffe000104a46494600010101004800480000ffdb004300...",
        "roi_statuses": [
            {
                "feature": "position_correction",
                "idx": 0,
                "status": "OK"
            },
            {
                "feature": "color_average",
                "idx": 1,
                "status": "NG"
            }
            // 기타 ROI 상태...
        ],
        "judgement_condition": "ALL_OK",
        "overall_status": "OK",
        "measurement_values": [
            {
                "feature": "position_correction",
                "idx": 0,
                "value": 95.0
            },
            {
                "feature": "color_average",
                "idx": 1,
                "value": 45.0
            }
            // 기타 측정값...
        ]
    }
    ```

    **설명:**
    - `image`: 위치 보정 후의 결과 이미지가 JPEG 형식으로 인코딩된 16진수 문자열입니다. 클라이언트는 이 문자열을 디코딩하여 이미지를 복원할 수 있습니다.
    - `roi_statuses`: 각 ROI의 상태를 나타내는 리스트입니다.
    - `judgement_condition`: 현재 설정된 종합 판정 조건입니다.
    - `overall_status`: 전체 ROI 상태에 따른 종합 판정 결과입니다 ("OK" 또는 "NG").
    - `measurement_values`: 각 ROI의 측정값을 나타내는 리스트입니다.

    **예제 응답:**

    ```json
    {
        "image": "ffd8ffe000104a46494600010101004800480000ffdb004300...",
        "roi_statuses": [
            {
                "feature": "position_correction",
                "idx": 0,
                "status": "OK"
            },
            {
                "feature": "color_average",
                "idx": 1,
                "status": "NG"
            }
        ],
        "judgement_condition": "ALL_OK",
        "overall_status": "OK",
        "measurement_values": [
            {
                "feature": "position_correction",
                "idx": 0,
                "value": 95.0
            },
            {
                "feature": "color_average",
                "idx": 1,
                "value": 45.0
            }
        ]
    }
    ```

---

## 14. 서버 시작

### 서버 시작 방법

이 애플리케이션을 실행하려면 다음 명령어를 사용합니다:

```bash
python your_script.py
