---

---





![图片](https://gitee.com/wanwanzh/imagebed/raw/master/pictures/640-20210407130938116)

点击链接观看安装视频：https://youtu.be/6835OZT0Y5Y

### 设置Xcode

打开终端并执行

*sudo xcode-select --install*

### 安装HomeBrew(原生Apple Silicon M1)

打开终端，逐个写入

这将为M1芯片安装最新的Brew

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

echo “export PATH=/opt/homebrew/bin:$PATH” >> ~/.zshrc

 
//Restart The Terminal

brew install gcc

brew install cmake

brew install wget
```

### 安装Miniforge，设置Conda环境

点击下面的链接下载(Apple Silicon)版本

https://github.com/conda-forge/miniforge

![图片](https://gitee.com/wanwanzh/imagebed/raw/master/pictures/640-20210407131947989) 

打开终端并执行以下操作

```shell
// If the Downloaded File Stored in Download
cd Downloads

bash Miniforge3-MacOSX-arm64.sh

//After Installation Completes Restart Terminal

//Creating Conda Environment named ml You can use any name in place of "ml"

conda create --name ml

conda install -y python==3.8.6

conda install -y pandas matplotlib scikit-learn jupyterlab
```

### 安装Tensorflow

单击下面的链接并下载文件

https://github.com/apple/tensorflow_macos/releases

Am在2021年3月3日为M1使用最新的TF alpha x版本。

![图片](https://gitee.com/wanwanzh/imagebed/raw/master/pictures/640-20210407132101050) 

```shell
//if Download Directory is Downloads
cd Downloads
tar xvf tensorflow_macos-0.1alpha2.tar.gz
cd tensorflow_macos/arm64

//Dont Forget To Activate Conda Environment 

conda activate ml

// Install specific pip version and some other base packages
pip install --force pip==20.2.4 wheel setuptools cached-property six

// Install all the packages provided by Apple but TensorFlow
pip install --upgrade --no-dependencies --force numpy-1.18.5-cp38-cp38-macosx_11_0_arm64.whl grpcio-1.33.2-cp38-cp38-macosx_11_0_arm64.whl h5py-2.10.0-cp38-cp38-macosx_11_0_arm64.whl tensorflow_addons_macos-0.1a2-cp38-cp38-macosx_11_0_arm64.whl

// Install additional packages
pip install absl-py astunparse flatbuffers gast google_pasta keras_preprocessing opt_einsum protobuf tensorflow_estimator termcolor typing_extensions wrapt wheel tensorboard typeguard

// Install TensorFlow
pip install --upgrade --force --no-dependencies tensorflow_macos-0.1a2-cp38-cp38-macosx_11_0_arm64.whl
```

安装额外的包

```shell
pip install matplotlib
conda install -c conda-forge scikit-learn
pip install keras
pip install notebook
```

### 编译和安装OpenCV

```shell
//I Suggest To Do all this Inside miniforge3 dir for that
//  cd miniforge3

 wget -O opencv.zip https://github.com/opencv/opencv/archive/4.5.0.zip
 
 wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.5.0.zip
 
 unzip opencv.zip
 
 unzip opencv_contrib.zip
 
 cd opencv-4.5.0

mkdir build && cd build

//Here Take Care Of Paths of OPENCV_EXTRA_MODULES_PATH and   
//    PYTHON3_EXECUTABLE If you're Beginner watch the YouTube  video
//And If Inside miniforge3 just place your <username>.

cmake \
-DCMAKE_SYSTEM_PROCESSOR=arm64 \
-DCMAKE_OSX_ARCHITECTURES=arm64 \
-DWITH_OPENJPEG=OFF \
-DWITH_IPP=OFF \
-D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D OPENCV_EXTRA_MODULES_PATH=/Users/<username>/miniforge3/opencv_contrib-4.5.0/modules \
-D PYTHON3_EXECUTABLE=/Users/<username>/miniforge3/envs/ml/bin/python3 \
-D BUILD_opencv_python2=OFF \
-D BUILD_opencv_python3=ON \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D OPENCV_ENABLE_NONFREE=ON \
-D BUILD_EXAMPLES=ON ..

make -j8
//"8" is the number of cores To be used(This Step Takes Time)

sudo make install

//Linking OpenCV To Conda Environment 

mdfind cv2.cpython
//From the output Copy the Path similar to the below one 

"/usr/local/lib/python3.8/site-packages/cv2/python-3.8/cv2.cpython-38-darwin.so cv2.so"

cd 

cd miniforge3/envs/dev/lib/python3.8/site-packages

ln -s PasteYourCopiedPathHere
```



