# 젯슨 SD 카드 Jetpack SDK 설치

## Jetpack SDK 설치
1. **SD 카드 포맷**: SD Card Formatter를 사용하여 64GB microSD 카드를 포맷
    - [SD Card Formatter 다운로드](https://www.sdcard.org/downloads/formatter/sd-memory-card-formatter-for-windows-download/)
    - 포맷 버튼을 클릭하여 포맷을 완료
    - ![이미지](https://github.com/hyKwon13/Jetson-nano_PaddleOCR_CUDA/assets/117807382/40fec450-6ca2-48fe-b878-b73b55925ef2)

2. **Jetpack SDK 4.6.1 설치**:
    - [Jetpack SDK 다운로드](https://developer.nvidia.com/embedded/jetpack-sdk-461)

3. **balenaEtcher 사용**: balenaEtcher 프로그램을 사용하여 microSD 카드에 이미지 설치.
    - [Etcher 다운로드](https://etcher.balena.io/)

# 젯슨 OpenCV CUDA 환경 설정


## 1. Python 3.7 설치 및 numpy 설치
```bash
sudo apt update
sudo apt install -y build-essential libssl-dev zlib1g-dev \
libncurses5-dev libncursesw5-dev libreadline-dev libsqlite3-dev \
libgdbm-dev libdb5.3-dev libbz2-dev libexpat1-dev liblzma-dev \
libffi-dev uuid-dev

cd /usr/src
sudo wget https://www.python.org/ftp/python/3.7.12/Python-3.7.12.tgz
sudo tar xzf Python-3.7.12.tgz
cd Python-3.7.12
sudo ./configure --enable-optimizations --enable-shared
sudo make altinstall
sudo ldconfig

sudo apt update
sudo apt install python3-pip

python3.7 -m pip install numpy
```
  
## 2. 필요한 의존성 설치
OpenCV를 빌드하기 전에 필요한 의존성을 설치합니다. 이 단계는 OpenCV의 기능을 모두 지원하기 위해 다양한 라이브러리를 설치합니다.

```bash
sudo apt-get update && sudo apt-get install -y build-essential cmake unzip pkg-config libjpeg-dev libpng-dev libtiff-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libgtk-3-dev libatlas-base-dev gfortran python3-dev
```

## 3. OpenCV 소스 코드 다운로드 및 빌드
OpenCV와 OpenCV Contrib 모듈의 최신 버전을 다운로드하고 빌드합니다.

```bash
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.9.0.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.9.0.zip
unzip opencv.zip
unzip opencv_contrib.zip
cd opencv-4.9.0/
mkdir build
cd build
```

## 4. CMake를 사용한 OpenCV 구성
CMake를 사용하여 OpenCV를 CUDA와 함께 빌드하도록 구성합니다. 주의할 점은 모든 경로가 정확하고, CUDA 관련 옵션을 올바르게 설정하는 것입니다.

```bash
cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D INSTALL_PYTHON_EXAMPLES=ON \
      -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-4.9.0/modules \
      -D ENABLE_NEON=ON \
      -D BUILD_TESTS=OFF \
      -D WITH_CUDA=ON \
      -D CUDA_ARCH_BIN="5.3" \
      -D CUDA_ARCH_PTX="" \
      -D WITH_CUBLAS=ON \
      -D WITH_LIBV4L=ON \
      -D BUILD_opencv_python3=TRUE \
      -D PYTHON3_EXECUTABLE=/usr/local/bin/python3.7 \
      -D PYTHON3_INCLUDE_DIR=/usr/local/include/python3.7m \
      -D PYTHON3_LIBRARY=/usr/local/lib/libpython3.7m.so \
      -D PYTHON3_PACKAGES_PATH=/home/visionsensor/.local/lib/python3.7/site-packages \
      -D BUILD_opencv_python2=OFF \
      -D BUILD_EXAMPLES=ON ..
```

## 5. 빌드 및 설치

```bash
make -j$(nproc) # 1~2시간 정도 소요됩니다.
sudo make install
sudo ldconfig
```

## 6. 환경 변수 설정

~/.bashrc 파일 열기:

```bash
vim ~/.bashrc
```

환경 변수 추가: 맨 아래로 이동하여 다음 줄을 추가합니다.

```bash
export PYTHONPATH=/usr/local/lib/python3.7/site-packages:$PYTHONPATH
```

파일 저장 및 종료: 편집이 완료되면 Esc 키를 눌러 명령 모드로 돌아간 후, 다음 명령어를 입력해 저장하고 종료합니다.

```bash
:wq
```

변경 사항 적용:

```bash
source ~/.bashrc
```


## 7. 설치 확인
Python에서 OpenCV가 올바르게 설치되었는지 확인합니다.

```bash
import cv2
print(cv2.__version__)
print(cv2.cuda.getCudaEnabledDeviceCount())
```


# Paddlepaddle-GPU 설치
Python 3.7에 paddlepaddle-gpu를 설치하기 위해 whl 파일을 직접 다운로드하여 설치하였습니다.

1. **paddlepaddle-gpu 다운로드**: [다운로드 링크](https://forums.developer.nvidia.com/t/paddlepaddle-for-jetson/242765)
    - ![이미지](https://github.com/hyKwon13/Jetson-nano_PaddleOCR_CUDA/assets/117807382/fd80b418-f0a8-4eb9-ad90-5e9ecf788fbb)


2. **whl 파일 전송**

   데스크탑에서 ssh를 통해 jetson nano를 사용중이므로 cmd에서 pscp 명령어를 사용하여 whl 파일을 jetson nano로 전송합니다.
   ```bash
    pscp C:\temp\* root@192.168.2.222:/temp
    ```

4. **whl 파일 설치**
    ```bash
    python3.7 -m pip install paddlepaddle_gpu-2.4.1-cp37-cp37m-linux_aarch64.whl
    ```

5. **오류 해결**
    ```bash
    Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-apmufue2/paddle-bfloat/
    ```

    이 오류는 paddle-bfloat 패키지를 설치하는 과정에서 발생한 문제로, 주로 필요한 빌드 의존성 패키지가 누락되었거나, 설치 과정에서 문제가 발생했을 때 나타납니다. 아래의 방법들을 통해 문제를 해결할 수 있습니다.

    1. **pip 및 setuptools 업그레이드**
        ```bash
        pip3 install --upgrade pip setuptools
        ```

    2. **numpy 미리 설치**
        ```bash
        pip3 install numpy
        ```

    3. **paddle-bfloat 패키지 직접 설치**
        ```bash
        pip install paddle-bfloat==0.1.7
        pip install paddlepaddle_gpu-2.4.1-cp37-cp37m-linux_aarch64.whl
        ```

# PaddleOCR 설치

1. **paddleocr 설치**
    ```bash
    pip3 install paddleocr
    ```

2. **오류 해결**
    ```bash
    Failed to build psutil
    ERROR: Could not build wheels for psutil, which is required to install pyproject.toml-based projects
    ```

    이 오류는 psutil 패키지를 빌드하는 과정에서 Python 헤더 파일을 찾을 수 없어서 발생한 문제입니다. 이를 해결하려면 필요한 Python 헤더 파일을 설치해야 합니다.

    ```bash
    sudo apt-get install gcc python3-dev
    pip3 install paddleocr
    ```

    문제가 해결되지 않으면 psutil 패키지를 빌드할 때 필요한 추가적인 의존성을 설치해보세요.
    ```bash
    sudo apt-get install build-essential
    pip install paddleocr
    ```

3. **여전히 오류 발생 시**

    python3-dev 패키지가 누락되었을 가능성이 있으므로, 해당 패키지를 다시 설치해보세요.
    ```bash
    sudo apt-get install python3.7-dev
    pip install psutil
    ```

4. **환경 변수 설정 확인**
    Python 헤더 파일을 찾지 못하는 경우, 환경 변수가 제대로 설정되지 않았을 수 있습니다. 아래 명령어를 사용하여 CPATH 환경 변수를 설정해 보세요.
    ```bash
    export CPATH=/usr/include/python3.7m
    ```

    다시 psutil 설치
    ```bash
    pip install psutil
    ```

5. **OpenCV 환경 변수 설정**
    ```bash
    ImportError: OpenCV loader: missing configuration file: ['config-3.7.py', 'config-3.py']. Check OpenCV installation.
    ```

    이 오류는 OpenCV가 설치된 경로가 올바르게 설정되어 있는지 확인하세요. 경우에 따라 환경 변수를 수동으로 설정해야 할 수도 있습니다. Python 3.7로 환경 변수를 설정합니다.
    ```bash
    export PYTHONPATH=~/project/paddle3.7/lib/python3.7/site-packages:$PYTHONPATH
    ```

6. **설치 확인 예제**
다음 예제 코드를 통해 PaddleOCR이 GPU와 TensorRT를 제대로 사용하고 있는지 확인할 수 있습니다. 이를 통해 onnxruntime보다 더 빠른 추론 속도를 얻을 수 있습니다. 다만, TensorRT를 사용할 경우 초기 구동 시 많은 메모리를 사용하여 시작 속도가 느려질 수 있습니다.

```bash
import cv2
from paddleocr import PaddleOCR

def draw_rectangle(event, x, y, flags, param):
    global drawing, top_left_pt, bottom_right_pt, rectangles
    if event == cv2.EVENT_LBUTTONDOWN:
        drawing = True
        top_left_pt = (x, y)
        bottom_right_pt = (x, y)
    elif event == cv2.EVENT_MOUSEMOVE:
        if drawing:
            bottom_right_pt = (x, y)
    elif event == cv2.EVENT_LBUTTONUP:
        drawing = False
        bottom_right_pt = (x, y)
        rectangles.append((top_left_pt, bottom_right_pt))

def perform_ocr(frame, top_left_pt, bottom_right_pt, ocr):
    if top_left_pt == (-1, -1) or bottom_right_pt == (-1, -1):
        return
    roi = frame[top_left_pt[1]:bottom_right_pt[1], top_left_pt[0]:bottom_right_pt[0]]
    if roi.size == 0:
        return
    result = ocr.ocr(roi, cls=False)
    result = result[0]
    if result is not None:
        for line in result:
            print(line[1][0])
            cv2.putText(frame, line[1][0], (top_left_pt[0], bottom_right_pt[1]+21), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 255), 2)

cv2.namedWindow('paddleocr')
cv2.setMouseCallback('paddleocr', draw_rectangle)
cap = cv2.VideoCapture(0, cv2.CAP_DSHOW)

drawing = False
top_left_pt, bottom_right_pt = (-1, -1), (-1, -1)
rectangles = [] 

ocr = PaddleOCR(
    use_gpu=True,
    use_tensorrt=True,
    use_angle_cls=False,
    lang='en',
    show_log=False,
)


            
for _ in range(30):
    _, _ = cap.read()

while True:
    ret, frame = cap.read()

    for rect in rectangles:
        cv2.rectangle(frame, rect[0], rect[1], (0, 255, 0), 2)
    
    if drawing and top_left_pt != (-1, -1) and bottom_right_pt != (-1, -1):
        cv2.rectangle(frame, top_left_pt, bottom_right_pt, (0, 255, 0), 2)
    if not drawing and top_left_pt != (-1, -1) and bottom_right_pt != (-1, -1):
        perform_ocr(frame, top_left_pt, bottom_right_pt, ocr)
    cv2.imshow('paddleocr', frame)
    if cv2.waitKey(1) & 0xFF == 27:
        break

cap.release()
cv2.destroyAllWindows()
```

7. **TIP**
Jetson Nano에서 더 큰 모델이나 데이터를 실행하려면 스왑 메모리 공간을 늘려야 할 수도 있습니다. 스왑 메모리 공간을 8GB로 늘리는 것이 좋습니다. 스왑 메모리 공간을 늘리려면 다음 단계를 참조하십시오.

```bash
sudo fallocate -l 8G /var/swapfile8G
sudo chmod 600 /var/swapfile8G
sudo mkswap /var/swapfile8G
sudo swapon /var/swapfile8G
sudo bash -c 'echo "/var/swapfile8G swap swap defaults 0 0" >> /etc/fstab'
```

fallocate 오류 발생시

```bash
fallocate: fallocate failed: Text file busy
```

기존 스왑 파일 비활성화
```bash
sudo swapoff -a
```

새 스왑 파일 생성
```bash
sudo fallocate -l 8G /var/swapfile8G
sudo chmod 600 /var/swapfile8G
sudo mkswap /var/swapfile8G
sudo swapon /var/swapfile8G
sudo bash -c 'echo "/var/swapfile8G swap swap defaults 0 0" >> /etc/fstab'
```
