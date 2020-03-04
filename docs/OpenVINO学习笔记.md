<!--
 * @描述: OpenVINO 学习笔记
 * @版本: V1_0
 * @作者: LiWanglin
 * @创建时间: 2020.03.02
 * @最后编辑人: LiWanglin
 * @最后编辑时间: 2020.03.02
 -->

# OpenVINO 学习笔记

## 一. OpenVINO 介绍

- 本节主要介绍 OpenVINO ，包括 OpenVINO 简介，OpenVINO 的组成等。

### 1.1 OpnVINO 简介

- OpenVINO 是英特尔基于自身现有的硬件平台开发的一种可以加快高性能计算机视觉和深度学习视觉应用开发速度工具套件，支持各种英特尔平台的硬件加速器上进行深度学习，并且允许直接异构执行。
- OpenVINO 工具包可通过基于英特尔架构的处理器（ CPU ）及核显（ Integrated GPU ）和深度学习加速器（ FPGA、Movidius VPU ）的深度学习加速芯片，增强视觉系统功能和性能。

### 1.2 OpenVINO 组成

- OpenVINO 主要由四个模块组成，分别是：推断引擎，模型优化器，预训练模型，数据代码。
![四个主要模块](./doc_images/four_modules.png)

## 二. OpenVINO 环境配置

- 本节主要讲解如何配置 OpenVINO ，包括如何安装 OpenVINO 以及如何在 VS2019 配置 OpenVINO 的开发环境。

### 2.1 准备工作

- 在安装 OpenVINO 工具包之前，需安装一下软件:
  - VS2015/VS2017/VS2019
  - CMake 3.4 及以上版本（如果是 VS2019 ，需安装 CMake 3.14 版本或更高）
  - Python 3.6.5 以上

- 我的软件安装版本:
  - VS2019 社区版
  - CMake 3.16.0
  - Python 3.6.9

### 2.2 安装以及配置 OpenVINO

- 安装 OpenVINO 主要步骤有：下载 OpenVINO ，安装 OpenVINO 工具包，初始化 OpenVINO ，运行测试 Demo

(1) 下载 OpenVINO

- 进入网站：[OpenVINO](https://software.seek.intel.com/openvino-toolkit?os=windows)，下载OpenVINO ，如果要注册才能下载，那先注册。选择 2019.R3 版本和 Full Package 下载。
![OpenVINO_Download](./doc_images/OpenVINO_Download.png)

(2) 安装 OpenVINO 工具包

- 下载完 OpenVINO ，双击软件包，进入安装页面![install_openvino_toolkit](./doc_images/install_openvino_toolkit.png)
- 注意安装路径以及选择全部的组件，然后一路点击 Next，最后点击 Finsh，组件安装完成。![openvino_toolkit_install_finish](./doc_images/openvino_toolkit_install_finish.png)

(3) 初始化 OpenVINO

- 打开 CMD ，然后进入以下目录

    ```C++
    C:\Program Files (x86)\IntelSWTools\openvino_2019.3.334\bin
    ```

- 输入 setupvars.bat ，执行初始化脚本
![initialzed_openvino](./doc_images/initialized_openvino.png)

(4) 运行测试 Demo

- 进入以下目录

    ```C++
    C:\Program Files (x86)\IntelSWTools\openvino_2019.3.334\deployment_tools\demo
    ```

- 输入 demo_security_barrier_camera.bat ，执行测试脚本
![run_demo](./doc_images/run_demo.png)

- 如果 OpenVINO 安装成功，则会出现以下结果
![demo_result](./doc_images/demo_result.png)

### 2.3 基于 VS2019 的开发环境配置

- 要想在 VS2019 上使用 OPenVINO 进行加速优化，我们需要进行一系列的配置，包括：配置 VC++ 的包含目录，VC++ 的库目录，附加依赖库，添加环境变量等。

(1) 新建项目

- 打开 VS2019 ，点击创建新项目，然后创建一个空项目。这里，我创建的项目名称为：openvino_project 。
![creat_project](./doc_images/creat_project.png)

(2) 编译 samples

- 编译 samples 是为了后面在 VS2019 里面添加库目录
- 打开 cmd ，进入目录：

  ```C++
  C:\Program Files (x86)\IntelSWTools\openvino_2019.3.334\inference_engine\samples
  ```

- 执行 build_samples_msvc.bat 文件，完成编译
![build_samples](./doc_images/build_samples.png)
- 编译完成后，会在
 
  ```C++
  C:\Users\XXX\Documents\Intel
  ```

  生成一个 OpenVINO 文件夹

- 然后进入 OpenVINO\inference_engine_samples_build 文件夹，然后使用 VS2019 打开 Samples.sln 文件。
![Samples_file](./doc_images/Samples_file.png)

- 打开文件后，右击解决方案，再点击生成解决方案
![build_solution](./doc_images/build_solution.png)

- 最后，我们会在

  ```C++
  C:\Users\LWL\Documents\Intel\OpenVINO\inference_engine_samples_build\intel64\Debug
  ```

  目录下生成一个，cpu_extension.lib 文件。这个文件我们会在后面用到。
  ![cpu_extension](./doc_images/cpu_extension.png)

(3) 设置 VS++ 目录

- 在 VS2019 里面点击视图，然后点击其他窗口，再点击属性管理器。点击完成后，我们可以在右侧小窗口中看到属性管理器窗口。
![attribute_set](./doc_images/attribute_set.png)
- 然后在属性管理器窗口中双击 Debug|x64 ，进入 Debug 属性页。然后点击 VC++ 目录。接后，我们主要设置 **包含目录** 和 **库目录** 。
![attribute_set_2](./doc_images/attribute_set_2.png)

- 配置包含目录

  ```C++
  C:\Program Files %28x86%29\IntelSWTools\openvino_2019.3.334\deployment_tools\inference_engine\src\extension
  C:\Program Files %28x86%29\IntelSWTools\openvino_2019.3.334\deployment_tools\inference_engine\include
  C:\Program Files %28x86%29\IntelSWTools\openvino_2019.3.334\deployment_tools\inference_engine\samples\common\format_reader
  C:\Program Files %28x86%29\IntelSWTools\openvino_2019.3.334\opencv\include\opencv2
  C:\Program Files %28x86%29\IntelSWTools\openvino_2019.3.334\opencv\include
  ```

  ![set_include_meun](./doc_images/set_include_meun.png)

- 配置库目录

  ```C++
  C:\Users\LWL\Documents\Intel\OpenVINO\inference_engine_samples_build\intel64\Debug
  C:\Program Files %28x86%29\IntelSWTools\openvino_2019.3.334\deployment_tools\inference_engine\lib\intel64\Debug
  C:\Program Files (x86)\IntelSWTools\openvino_2019.3.334\opencv\lib
  ```

  ![include_meun](./doc_images/include_meun.png)

(4) 配置链接器

- 我们需要添加一些附加依赖库
![set_link](./doc_images/set_link.png)

- 在附加依赖库添加

  ```C++
  inference_engined.lib
  cpu_extension.lib
  format_reader.lib
  opencv_calib3d412d.lib
  opencv_core412d.lib
  opencv_dnn412d.lib
  opencv_features2d412d.lib
  opencv_flann412d.lib
  opencv_gapi412d.lib
  opencv_highgui412d.lib
  opencv_imgcodecs412d.lib
  opencv_imgproc412d.lib
  opencv_ml412d.lib
  opencv_objdetect412d.lib
  opencv_photo412d.lib
  opencv_stitching412d.lib
  opencv_video412d.lib
  opencv_videoio412d.lib
  ```

  ![link_lib](./doc_images/link_lib.png)

(5) 添加环境变量

 ```C++
 C:\Program Files (x86)\IntelSWTools\openvino_2019.3.334\opencv\bin
 C:\Program Files (x86)\IntelSWTools\openvino_2019.3.334\inference_engine\bin\intel64\Debug
 C:\Program Files (x86)\IntelSWTools\openvino_2019.3.334\inference_engine\bin\intel64\Release
 C:\Users\LWL\Documents\Intel\OpenVINO\inference_engine_samples_build\intel64\Debug
 C:\Users\LWL\Documents\Intel\OpenVINO\inference_engine_samples_build\intel64\Release
 ```

 ![set_path_environment](./doc_images/set_path_environment.png)

(6) 代码测试

- 在测试代码前，我们需要重启 VS2019
- 在源文件下添加 main.cpp 文件，然后输入代码：

  ```C++
  #include <opencv2/opencv.hpp>
  #include <iostream>

  using namespace cv;
  using namespace std;

  int main(int argc, char** argv)
  {
    Mat src = imread("./lenacolor.png");
    imshow("input", src);
    waitKey(0);
    destroyAllWindows();
    return 0;
  }
  ```

  ![test_code](./doc_images/test_code.png)

- 如果运行成功，则代表我们的 OpenVINO 开发环境以及配置成功。

## 二. OpenVINO 使用

### 2.1 OpenCV 中使用 IE 模块加速