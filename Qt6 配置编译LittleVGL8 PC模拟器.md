

![](https://i.loli.net/2021/08/24/uNJrSE5byHkhL2Y.png)

------

## 一、配置说明

本配置文档是采用的相对最新的Qt和LittleVGL版本，可以点击以下链接查看Download相关。

- Qt 6.1.2  [Index of /official_releases/online_installers (qt.io)](https://download.qt.io/official_releases/online_installers/)
- LVGL v8 https://github.com/littlevgl/lv_sim_eclipse_sdl （Eclipse Simulator）

注意：必须采用上面提供的Eclipse模拟器仓库，使用其他的模拟器将不能成功配置。

## 二、配置步骤

1、获取littlevGL源码和模拟器（一个仓库就搞定）
https://github.com/littlevgl/lv_sim_eclipse_sdl

建议采用git方式下载：

clone仓库：

```shell
cd Desktop
```

```apl
git clone https://github.com/littlevgl/lv_sim_eclipse_sdl
```

很多情况下会提示一些部分项下载错误，是由于仓库里面的lv_drivers、lv_demos、lvgl属于远程仓库，我们应再次分别单独克隆进入。

```shell
//1.进入根目录
cd lv_sim_eclipse_sdl-master\lv_sim_eclipse_sdl-master

//2.进入lv_drivers，clone进去
cd lv_drivers
git clone https://github.com/lvgl/lv_drivers.git

//3.进入lv_demos，clone进去
cd lv_demos
git clone https://github.com/lvgl/lv_demos.git

//4.进入lv，clone进去
cd lvgl
git clone https://github.com/lvgl/lvgl.git
```

2、下载SDL 动态库

SDL用于实现PC模拟器，如果后期移植嵌入式设备上，只需要移植填加好对应的显示驱动即可。

下载链接：https://www.libsdl.org/download-2.0.php
只需要开发库版本即可，这里选择SDL2-devel-2.0.16-mingw.tar.gz (MinGW 32/64-bit)

![image-20210823232726125](https://i.loli.net/2021/08/24/4rgvU59uYimKoZM.png)

## 三、Qt6创建工程

#### 按照以下步骤建立工程:

![image-20210823234834042](https://i.loli.net/2021/08/24/rYO1ydcghNopHmz.png)


![image-20210823235132707](https://i.loli.net/2021/08/24/MoYIe8JFSOZTW5d.png)

![image-20210823235254802](https://i.loli.net/2021/08/24/TiZGRxsw1VbufEj.png)

![image-20210823235427507](https://i.loli.net/2021/08/24/SogfNaeI7wLMyXU.png)

![image-20210823235450706](https://i.loli.net/2021/08/24/rAv6HBnmYhZSq82.png)

## 四、复制所需文件到工程文件路径下

- ##### 将littlevGL源码中的文件（红色部分）复制到Qt工程目录下，main.c覆盖

![image-20210824000439626](https://i.loli.net/2021/08/24/7USy8xkgp6nQFc1.png)

- #### 复制SDL 相关文件到工程目录

##### 1.如果在创建工程时，kits选的是MinGW32-bit：

将SDL2-2.0.16\i686-w64-mingw32\include，文件夹下SDL2目录（**蓝色部分**）复制到上面工程路径中，

将SDL2-2.0.16\i686-w64-mingw32，文件夹下**lib目录**（**蓝色部分**）复制到上面工程路径下。

##### 2.如果在创建工程时，kits选的是MinGW32-bit：

将SDL2-2.0.16\x86_64-w64-mingw32\include，文件夹下**SDL2目录**（**蓝色部分**）复制到上面工程路径中，

将SDL2-2.0.16\x86_64-w64-mingw32，文件夹下**lib目录**（**蓝色部分**）复制到上面工程路径下。

## 五、添加.c 和.h文件到工程中,参与编译构建 

##### 1.添加.c 和.h文件到工程中

![image-20210824000841819](https://i.loli.net/2021/08/24/QFfy5gTVXiNumkE.png)

![image-20210824001200728](https://i.loli.net/2021/08/24/qgEAfOiF7WvjQRZ.png)

2.双击打开Pro_littlevGL.pro文件，添加SDL lib编译选项, 使SDL库能够参与编译中。

```apl
LIBS += -L$$PWD/lib/ -lmingw32 -lSDL2main -lSDL2
```

![image-20210824001355995](https://i.loli.net/2021/08/24/irLa8WsgQqv7dj1.png)

## 六、Ctrl+R 编译运行，会自动在同目录生成 build- 开头的文件夹。

1. 将 SDL2-2.0.16\x86_64-w64-mingw32\bin 目录下的 SDL2.dll拷贝到 build-...目录下。

（如果是MinGW-32bits 的kits, 就选择SDL2-2.0.16\i686-w64-mingw32\bin 目录下的 SDL2.dll）

![image-20210824001813492](https://i.loli.net/2021/08/24/OFdiC6VEHsoBuZ3.png)

2. ##### 再次Ctrl+R 编译运行，会依次报一大堆错误。

   ## 莫慌，小问题！！

   之所以会报错，是因为demo目录里面，有一些Demo的在该模拟器下没有支持，从为发生类似XXXX.h头文件找不到的情况。

   我们只需要保留main里面执行的demo例程就OK，这里采取一个简单快速的处理方法。

   ### ”**双击报错项，跳转进入该Demo文件下，全选 Ctrl + / 注释该文件。** “

​       **再次Ctrl+R 编译运行，利用这中方法，方可注释掉所有不支持的demo例程文件，错误信息全部消除。**

## 七、成功编译，最终效果

![](https://i.loli.net/2021/08/24/uNJrSE5byHkhL2Y.png)

#### 接下来就可插API,撸代码,设计你的GUI原型作品了！

LVGL最新API文档 ： [Welcome to the documentation of LVGL! — LVGL documentation](https://docs.lvgl.io/master/index.html)

