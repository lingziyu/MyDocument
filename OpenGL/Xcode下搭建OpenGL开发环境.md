# Xcode下搭建OpenGL开发环境

###命令行安装homebrew

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Homebrew安装成功后，会自动创建目录 /usr/local/Cellar 来存放Homebrew安装的程序，可以在finder下点击shift+command+G进入/usr/local/Cellar文件夹

###命令行安装glew和glfw

```
brew install glew
brew install glfw3
```

link编译链接（如果已经链接会提示Warning）

```
brew link glew
brew link glfw3
```

###添加Custom Paths

在Xcode中找到Peference菜单项，这个一般在File菜单项左边的那个Xcode项目中，然后在里面找到Locations项，再点击Custom Paths，添加四项

> Name            Display Name    Path
> glew_header        glew_header        /usr/local/Cellar/glew/2.0.0/include
> glew_lib        glew_lib        /usr/local/Cellar/glew/2.1.0/lib
> glfw_header        glfw_header        /usr/local/Cellar/glfw3/3.2.1/include
> glfw_lib        glfw_lib        /usr/local/Cellar/glfw3/3.2.1/lib

###添加Search Path

创建新的Xcode项目（command line tool），语言选择C++。接着，在项目的Bulid Settings里面找到Header Search Paths和Library Search Paths两项，在Header Search Paths中加入

> /usr/local/Cellar/glew/2.1.0/include
>
> /usr/local/Cellar/glfw/3.2.1/include

Library Search Paths中加入

> /usr/local/Cellar/glfw/3.2.1/lib
>
> /usr/local/Cellar/glew/2.1.0/lib

###添加Linked Frameworks

在项目的General中找到Linked Frameworks and Libraries，点击‘+’号，添加如下三个文件

> OpenGL.framework    libGLEW.2.1.0.dylib    libglfw3.2dylib

添加两个dylib文件的方法是，点击add other，然后点击shift+command+G进入/usr/local文件夹，然后根据我们之前说的安装glew和glfw3的路径找到这两个文件夹，在这两个文件夹中找到这两个文件

###测试代码

```c++
#include <iostream>
#include <GL/glew.h>
#include <GLFW/glfw3.h>

void Render(void)
{
    glClearColor(0.0f, 0.0f, 0.0f, 1.0f);
    glClear(GL_COLOR_BUFFER_BIT);
    glBegin(GL_TRIANGLES);
    {
        glColor3f(1.0,0.0,0.0);
        glVertex2f(0, .5);
        glColor3f(0.0,1.0,0.0);
        glVertex2f(-.5,-.5);
        glColor3f(0.0, 0.0, 1.0);
        glVertex2f(.5, -.5);
    }
    glEnd();
}

int main(int argc, const char * argv[]) {
    GLFWwindow* win;
    if(!glfwInit()){
        return -1;
    }
    win = glfwCreateWindow(640, 480, "OpenGL Base Project", NULL, NULL);
    if(!win)
    {
        glfwTerminate();
        exit(EXIT_FAILURE);
    }
    if(!glewInit())
    {
        return -1;
    }
    glfwMakeContextCurrent(win);
    while(!glfwWindowShouldClose(win)){
        Render();
        glfwSwapBuffers(win);
        glfwPollEvents();
    }
    glfwTerminate();
    exit(EXIT_SUCCESS);
    return 0;
}
```

运行结果

<img src="http://ww4.sinaimg.cn/large/65e4f1e6jw1f97bsbd9l0j219i0vi42g.jpg">

