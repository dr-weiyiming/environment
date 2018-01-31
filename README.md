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
#### tensorflow
    选用版本：
    nvidia/cuda:8.0-cudnn5-devel
    docker镜像内安装
        Anaconda3-5.0.1-Linux-x86_64
        pip install tensorflow_gpu==1.2
    nvidia-docker镜像内测试tesorflow_gpu
        测试：（libcudart.so错误：cudnn版本不对，或者是没切换到nvidia-docker环境中）
        $ python
        >>> import tensorflow as tf
        >>> hello = tf.constant('Hello, TensorFlow!')
        >>> sess = tf.Session()
        >>> print(sess.run(hello))
        
 #### python环境[cv2](https://anaconda.org/menpo/opencv3/files)
    文件下载：linux-64-opencv3-3.1.0-py27_0.tar.bz2
    conda install --channel menpo linux-64-opencv3-3.1.0-py27_0.tar.bz2
    卸载：conda uninstall opencv3
       
        
    
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

#### Matlab安装：
    sudo mkdir /iso
    sudo mount -o loop [path]MATHWORKS_R2014A.iso /iso
    cd /media/matlab 
    sudo ./install
    sudo cp libmwservices.so /usr/local/MATLAB/R2014A/bin/glnxa64
    sudo do umount /iso
    
    创建快捷方式
         ~/.bashrc中添加 export PATH=/usr/local/MATLAB/R2014a/bin:$PATH最终直接在终端输入’matlab’就可以打开Matlab
   
#### caffe
    依赖
    sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
    sudo apt-get install --no-install-recommends libboost-all-dev
    sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev
    sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
    
    sudo cp Makefile.config.example Makefile.config
    sudo gedit Makefile.config 
        USE_CUDNN := 1
        WITH_PYTHON_LAYER := 1
        INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
        LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial   
                        MATLAB_DIR := /usr/local/MATLAB/R2014a
    sudo gedit Makefile
        NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
        CXXFLAGS += -MMD -MP下面添加CXXFLAGS += -std=c++11
    sudo gedit /usr/local/cuda/include/host_config.h
    // #error-- unsupported GNU version! gcc versions later than 4.9 are not supported!
    sudo make all -j8
    sudo make test -j8
    sudo make runtest -j8 
    sudo make pycaffe -j8
                1. libhdf5_hl.so.100: cannot open shared object file: No such file or directory
                    进入/etc/ld.so.conf.d目录
                    创建anaconda2.conf
                    输入/home/user/anaconda2/lib
                    输入命令sudo ldconfig立即命令生效
                2. python caffe报错：No module named google.protobuf.internal
                    sudo chmod 777 -R anaconda2
                    pip install protobuf
    sudo make matcaffe -j8
    sudo make mattest -j8
 
###### 错误一：   
        Invalid MEX-file '/home/wei/caffe/matlab/+caffe/private/caffe_.mexa64':
        /usr/local/MATLAB/R2014a/bin/glnxa64/../../sys/os/glnxa64/libstdc++.so.6: version `CXXABI_1.3.8' not found (required by
        /home/wei/caffe/matlab/+caffe/private/caffe_.mexa64)

        Error in caffe.set_mode_cpu (line 5)
        caffe_('set_mode_cpu');

        Error in caffe.run_tests (line 6)
        caffe.set_mode_cpu();
###### [解决一](http://blog.csdn.net/l297969586/article/details/76590055)
        strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep 'CXXABI'
        ---------------------------------------------------------------
            CXXABI_1.3
            CXXABI_1.3.1
            CXXABI_1.3.2
            CXXABI_1.3.3
            CXXABI_1.3.4
            CXXABI_1.3.5
            CXXABI_1.3.6
            CXXABI_1.3.7
            CXXABI_1.3.8
            CXXABI_1.3.9
            CXXABI_TM_1
            CXXABI_FLOAT128
        ------------------------------------
        参考解决方案中的anaconda换成matlab的思路，替换matlab的libstdc++.so.6，把系统的动态库拷贝到matlab中
            #删除原来的libstdc++.so.6
            sudo rm -rf anaconda3/lib/libstdc++.so.6
            #拷贝新的动态库文件,注意自己的是哪个版本
            sudo cp /usr/lib/libstdc++.so.6.0.21 /home/ubuntu/anaconda3/lib/
        再建立软连接
            cd anaconda3/lib/
            sudo chmod +r libstdc++.so.6.0.21
            sudo ln -sf libstdc++.so.6.0.21 libstdc++.so.6
            sudo ldconfig
###### 错误二：
        Invalid MEX-file
        '/home/xw/caffeBuild/caffe-master/matlab/+caffe/private/caffe_.mexa64':
        /home/xw/caffeBuild/caffe-master/matlab/+caffe/private/caffe_.mexa64: undefined
        symbol:
        _ZN2cv8imencodeERKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERKNS_11_InputArrayERSt6vectorIhSaIhEERKSB_IiSaIiEE

        Error in caffe.set_mode_cpu (line 5)
        caffe_('set_mode_cpu');

        Error in caffe.run_tests (line 6)
        caffe.set_mode_cpu();
###### 解决二： 
        参考 https://github.com/BVLC/caffe/issues/3934
        root@test222:/usr/local/MATLAB/R2014a/bin/glnxa64# mv libopencv_imgproc.so.2.4 libopencv_imgproc.so.2.4.bak
        root@test222:/usr/local/MATLAB/R2014a/bin/glnxa64# mv libopencv_highgui.so.2.4 libopencv_highgui.so.2.4.bak
        root@test222:/usr/local/MATLAB/R2014a/bin/glnxa64# mv libopencv_core.so.2.4 libopencv_core.so.2.4.bak

        root@test222:/usr/local/MATLAB/R2014a/bin/glnxa64# sudo ln -sf /usr/lib/x86_64-linux-gnu/libopencv_core.so.2.4.9 libopencv_core.so.2.4
        root@test222:/usr/local/MATLAB/R2014a/bin/glnxa64# sudo ln -sf /usr/lib/x86_64-linux-gnu/libopencv_highgui.so.2.4.9 libopencv_highgui.so.2.4
        root@test222:/usr/local/MATLAB/R2014a/bin/glnxa64# sudo ln -sf /usr/lib/x86_64-linux-gnu/libopencv_imgproc.so.2.4.9 libopencv_imgproc.so.2.4



#### [sougou安装教程](http://blog.csdn.net/leijiezhang/article/details/53707181)
