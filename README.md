# Ubuntu




## opencv:

  编译错误/无法通过
  
  cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D CUDA_GENERATION=Kepler ..

## matlab:  

  创建快捷方式
  
  ~/.bashrc中添加 export PATH=/usr/local/MATLAB/R2014a/bin:$PATH最终直接在终端输入’matlab’就可以打开Matlab

## sougou：

  安装教程 http://blog.csdn.net/leijiezhang/article/details/53707181

## Docker:
1. 文件挂载：
    例： docker run -it -v /home/dock/Downloads:/usr/Downloads ubuntu64 /bin/bash
    通过-v参数，冒号前为宿主机目录，必须为绝对路径，冒号后为镜像内挂载的路径

2. 镜像/容器删除：

3. 更新容器：
    docker commit 
