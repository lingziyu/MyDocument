# Xcode下搭建OpenCV开发环境

### 安装homebrew

 在Terminal中输入

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Homebrew安装成功后，会自动创建目录 /usr/local/Cellar 来存放Homebrew安装的程序，可以在finder下点击shift+command+G进入/usr/local/Cellar文件夹

###安装cmake

 在Terminal中输入

```
 brew install cmake
```

### 安装opencv

 在Terminal中输入

```
 brew link --overwrite python
 brew install opencv
```

### 添加Search Path

创建新的Xcode项目（command line tool），语言选择C++。接着，在项目的Bulid Settings里面找到Header Search Paths和Library Search Paths两项，在Header Search Paths中加入

> /usr/local/Cellar/opencv/3.4.1_2/include

Library Search Paths中加入

> $(inherited) /usr/local/Cellar/opencv/3.4.1_2/lib

### 添加Linked Frameworks

在项目的General中找到Linked Frameworks and Libraries，点击‘+’号，添加如下三个文件

> libopencv_core.3.4.1.dylib  
>
> libopencv_highgui.3.4.1.dylib
>
>  libopencv_ml.3.4.1.dylib
>
> libopencv_imgcodecs.3.4.1.dylib.

添加dylib文件的方法是，点击add other，然后点击shift+command+G进入/usr/local文件夹，然后根据我们之前说的安装glew和glfw3的路径找到这两个文件夹，在这两个文件夹中找到这两个文件

### 测试代码

```c++
#include <iostream>

#include <opencv/highgui.h>

#include <opencv/cv.h>

int main(int argc, char** argv)

{
    
    cvNamedWindow("Image", CV_WINDOW_AUTOSIZE);
    
    //MARK: 文件路径要改为自己的
    IplImage *img=cvLoadImage("hello.jpg", CV_LOAD_IMAGE_ANYCOLOR);
    
    cvShowImage("image", img);
    
    cvWaitKey(0);
    
    cvReleaseImage(&img);
    
    cvDestroyWindow("image");
    
    return 0;
    
}
```

