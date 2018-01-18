# Ubuntu

## Nividia Driver

    安装(注意 参数)

    sudo ./NVIDIA-Linux-x86_64-375.20.run --no-opengl-files --no-x-check --no-nouveau-check

    --no-opengl-files 只安装驱动文件，不安装OpenGL文件。这个参数最重要, 防止循环登录
    --no-x-check 安装驱动时不检查X服务
    --no-nouveau-check 安装驱动时不检查nouveau
    后面两个参数可不加。


## opencv:
  opencv-2.4.13

  mkdir release
  
  cd release
  
  cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D CUDA_GENERATION=Kepler ..
  
  sudo make -j8 #多核编译
  
  sudo make install
  
## gcc版本切换
http://blog.csdn.net/robertchenguangzhi/article/details/47837445

## matlab:  

  创建快捷方式
  
  ~/.bashrc中添加 export PATH=/usr/local/MATLAB/R2014a/bin:$PATH最终直接在终端输入’matlab’就可以打开Matlab
  
## cafe
  依赖
    
    sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
    
    sudo apt-get install --no-install-recommends libboost-all-dev
    
    sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev
    
    sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
    
    sudo gedit /etc/modprobe.d/blacklist.conf 加入blacklist nouveau
    sudo update-initramfs -u 重启
    
    sudo service lightdm stop
    cd ~
    sudo ./NVIDIA-Linux-x86_64-375.66.run --no-opengl-files --no-x-check --no-nouveau-check
    sudo nvidia-smi

## sougou：

  安装教程 http://blog.csdn.net/leijiezhang/article/details/53707181

## Docker:
1. 文件挂载：
    例： docker run -it -v /home/dock/Downloads:/usr/Downloads ubuntu64 /bin/bash
    通过-v参数，冒号前为宿主机目录，必须为绝对路径，冒号后为镜像内挂载的路径

2. 镜像/容器删除：

3. 更新容器：
    docker commit 
