필요한 패키지 설치

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
