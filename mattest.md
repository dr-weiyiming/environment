接口配置问题
http://blog.csdn.net/lee_j_r/article/details/52693724

（3）测试接口。输入 make mattest

这里可能报错：caffe_.mexa64: undefined symbol:
_ZN2cv8imencodeERKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERKNS_11_InputArrayERSt6vectorIhSaIhEERKSB_IiSaIiEE

错误原因是Matlab自带的库和Ubuntu的系统库之间发生了冲突，一言不和就废掉Matlab的自带库，使用Ubuntu系统库，呵呵。

PS：只替换库libstdc++.so.6是不行的，要解决此问题需要多替换几个库。输入终端命令：

export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libopencv_highgui.so.2.4:/usr/lib/x86_64-linux-gnu/libopencv_imgproc.so.2.4:/usr/lib/x86_64-linux-gnu/libopencv_core.so.2.4:/usr/lib/x86_64-linux-gnu/libstdc++.so.6:/usr/lib/x86_64-linux-gnu/libfreetype.so.6

export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/
