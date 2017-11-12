# environment


deep learning config:

1.opencv : 编译错误/无法通过, cuda --> CUDA_GENERATION=Kepler
  
  cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D CUDA_GENERATION=Kepler ..

2. matlab:  创建快捷方式
   ~/.bashrc中添加 export PATH=/usr/local/MATLAB/R2014a/bin:$PATH最终直接在终端输入’matlab’就可以打开Matlab
