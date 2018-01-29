# Ubuntu

#### Nividia Driver
    sudo gedit /etc/modprobe.d/blacklist.conf
    最后一行加上 blacklist nouveau
    sudo update-initramfs -u 
    重启电脑
    sudo service lightdm stop
    sudo ./NVIDIA-Linux-x86_64-375.66.run --no-opengl-files --no-x-check --no-nouveau-check
        --no-opengl-files 只安装驱动文件，不安装OpenGL文件。这个参数最重要, 防止循环登录
        --no-x-check 安装驱动时不检查X服务
        --no-nouveau-check 安装驱动时不检查nouveau
        后面两个参数可不加。
    sudo nvidia-smi

#### [nvidia-docker安装](https://github.com/NVIDIA/nvidia-docker) vs [教程](https://devblogs.nvidia.com/nvidia-docker-gpu-server-application-deployment-made-easy/) 
    根据需要，选择下载所需cuda版本以及cudnn版本,tag要正确！！
        nvidia-docker run --rm -ti nvidia/cuda:8.0-cudnn5-devel nvcc --version 

#### [Docker安装](https://docs.docker.com/install/linux/docker-ce/ubuntu/) 
    免输入sudo
        sudo groupadd docker
        sudo usermod -aG docker $USER
        注销并重新登录用户
    查看镜像 
        docker images
    进入容器 
        docker run -t -i <容器> /bin/bash
    文件挂载：
        例： docker run -it -v /home/wei/下载:/weiyiming nvidia:cuda8.0-cudnn5 /bin/bash
        通过-v参数，冒号前为宿主机目录，必须为绝对路径，冒号后为镜像内挂载的路径
    更新容器：
        docker commit <容器id> <镜像名称>
    容器镜像删除
        停止所有的container，这样才能够删除其中的images：
            docker stop $(docker ps -a -q)
        如果想要删除所有container的话再加一个指令：
            docker rm $(docker ps -a -q)
        通过image的id来指定删除谁
            docker rmi <image id>
        想要删除untagged images，也就是那些id为<None>的image的话可以用
            docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
        要删除全部image的话
            docker rmi $(docker images -q)
    镜像无法删除解决方案
        https://github.com/moby/moby/issues/17304
        常见问题：
        多个镜像ImageID一样，解决方法
            wei@wei-PC:~$ docker images
            REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
            nvidia/cuda         latest              e2b6067aaeb5        11 days ago         2.18GB
            nvidia/cuda         7.0                 a60a3c83aeed        2 weeks ago         0B
            nvidia/cuda         8.0                 a60a3c83aeed        2 weeks ago         0B
            hello-world         latest              f2a91732366c        2 months ago        1.85kB
            wei@wei-PC:~$ docker images ubuntu | tail -n +2 | awk '{ print $1 ":" $2}' | xargs docker rmi nvidia/cuda:8.0
            Untagged: nvidia/cuda:8.0
            wei@wei-PC:~$ docker images ubuntu | tail -n +2 | awk '{ print $1 ":" $2}' | xargs docker rmi nvidia/cuda:7.0
            Untagged: nvidia/cuda:7.0



#### [cudnn下载](https://developer.nvidia.com/rdp/cudnn-download)

#### opencv:
    opencv-2.4.13
    mkdir release
    cd release
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D CUDA_GENERATION=Kepler ..
    sudo make -j8 #多核编译
    sudo make install
  
#### [gcc版本切换](http://blog.csdn.net/robertchenguangzhi/article/details/47837445)

#### matlab创建快捷方式
    ~/.bashrc中添加 export PATH=/usr/local/MATLAB/R2014a/bin:$PATH最终直接在终端输入’matlab’就可以打开Matlab
   
#### cafe
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

#### [sougou安装教程](http://blog.csdn.net/leijiezhang/article/details/53707181)
