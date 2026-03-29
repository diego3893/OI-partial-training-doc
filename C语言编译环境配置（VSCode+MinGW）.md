# C语言编译环境配置（VSCode+MinGW）

## 1 VSCode 配置

- 前往官网下载并安装VSCode [Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)

- 安装下列扩展
  - Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code
  - C/C++
  - Code Runner

## 2 MinGW配置

- 官网下载并安装MinGW [MinGW - Minimalist GNU for Windows download | SourceForge.net](https://sourceforge.net/projects/mingw/)

  - **记住你的安装路径，要用**
  - 安装时勾选所有包![](https://ask.qcloudimg.com/http-save/yehe-8223537/8da9be5fb60d0bf88e460b2d5f051ad3.png)

- 环境变量配置

  - 右键点击win徽标，进入设置，搜索环境变量并进入配置界面，点击右下角**环境变量**，左键双击系统变量中的`Path`，点击**新建**，输入你的MinGW安装目录+bin
  - 例：我的安装目录为`D:\MinGW`，则新建环境变量为`D:\MinGW\bin`

  ![image-20250925222339694](C:\Users\30130\AppData\Roaming\Typora\typora-user-images\image-20250925222339694.png)

- 检查配置

  - 快捷键win+R，输入cmd进入终端，输入`gcc -v`，如果出现类似图示的输出则进入下一步![](C:\Users\30130\Desktop\0.png)

## 3 Code Runner 配置

- 进入扩展设置（点那个小齿轮）

![image-20250925222706419](C:\Users\30130\AppData\Roaming\Typora\typora-user-images\image-20250925222706419.png)

- 勾选图示两项

![image-20250925222736443](C:\Users\30130\AppData\Roaming\Typora\typora-user-images\image-20250925222736443.png)

- code runner安装后运行和调试可以直接点击vscode UI右上角的横向小箭头进行，不用从终端输指令

## 4 其他

- 配好后建议写个hello world检验一下

附代码

```c
#include<stdio.h>
int main(){
    printf("hello world");
}
```



- 调试配置参见ppt这里不赘述