## 前言
[Flutter macos下载地址]([https://flutter.dev/docs/get-started/install/macos](https://flutter.dev/docs/get-started/install/macos)
)
在Mac下安装完`flutter`之后,如何设置`flutter`命令为全局命令.

##  pit1
先说比较坑的一点吧.
在执行官网上给出的命令:
```
$  export PATH="$PATH:`pwd`/flutter/bin"
```
的时候,开始有些看不懂什么意思.后来才理解到原来就是按照下面这个格式输入:
```
export PATH=$PATH:/Users/lindaidai/app/flutter/bin
```
首先一点,你得清楚你的`flutter`是下载安装到什么目录.
比如我这里`flutter`是安装在一个叫做`app`的文件目录下:
![image.png](https://upload-images.jianshu.io/upload_images/7190596-e654365a3fc1b010.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
所以我后面跟着的路径就是`/app/flutter/bin`
执行好之后,输入`flutter doctor`是可以执行的.

## pit 2

执行完上面的操作,我们配置`flutter`为全局命令貌似好像成功了.
但是如果你关闭当前这个终端,再次打开终端输入`flutter doctor`的时候,它又会提示:
```
-bash: flutter: command not found
```
也就是说你上面的全局命令配置失败了.
尬...😅
这是因为我们之前设置环境变量的时候，是直接在命令行通过export命令进行的，并没有全局设置上。
### 解决办法
#### 1. 执行打开`.bash_profile`文件的命令:
```
open -e .bash_profile
```
如果你看到的是下面这条错误提示:
```
.bash_profile does not exist.
```
那就说明你还没有这个文件,此时就需要创建一个,若是有的这个文件的小伙则可以直接进入步骤3.
#### 2. 创建`.bash_profile`文件
1.进入当前用户的home目录:
```
cd ~
```
2.创建`.bash_profile文件:
```
touch .bash_profile
```
#### 3. 设置`flutter`环境变量
创建完成之后,再次执行前面的`open`命令,打开了我们的`.bash_profile`文件.
要是你之前有这个文件的话,应该会看到一些`export PATH=${PATH}...`之类的配置项.
此时直接在最末尾处追加设置flutter bin目录路径为环境变量:
```
export PATH=${PATH}:/Users/lindaidai/app/flutter/bin
```
**注意**⚠️
这里的指令和上面的还是有区别的,第二个`PATH`要用`{}`包裹着,不然是没有效果的.
如下图:
![image.png](https://upload-images.jianshu.io/upload_images/7190596-1ff2dd2b036a38a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
保存关闭`.bash_profile`文件之后,需要执行以下命令,更新环境变量:
 ```
source ~/.bash_profile
 ```
在打开终端执行`flutter doctor`就发现有效果了.