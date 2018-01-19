接口配置问题
wei@wei-PC:~/caffe$ make mattest
cd matlab; /usr/local/MATLAB/R2015b/bin/matlab -nodisplay -r 'caffe.run_tests(), exit()'

                            < M A T L A B (R) >
                  Copyright 1984-2015 The MathWorks, Inc.
                   R2015b (8.6.0.267246) 64-bit (glnxa64)
                              August 20, 2015

 
要开始，请键入以下项之一: helpwin、helpdesk 或 demo。
有关产品信息，请访问 www.mathworks.com。
 
Invalid MEX-file '/home/wei/caffe/matlab/+caffe/private/caffe_.mexa64':
/home/wei/caffe/matlab/+caffe/private/caffe_.mexa64: undefined symbol:
_ZN2cv8imencodeERKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERKNS_11_InputArrayERSt6vectorIhSaIhEERKSB_IiSaIiEE

出错 caffe.set_mode_cpu (line 5)
caffe_('set_mode_cpu');

出错 caffe.run_tests (line 6)
caffe.set_mode_cpu();

http://blog.csdn.net/lee_j_r/article/details/52693724


## 网上解决方案

#1
测试接口。输入 make mattest

这里可能报错：caffe_.mexa64: undefined symbol:
_ZN2cv8imencodeERKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERKNS_11_InputArrayERSt6vectorIhSaIhEERKSB_IiSaIiEE

错误原因是Matlab自带的库和Ubuntu的系统库之间发生了冲突，一言不和就废掉Matlab的自带库，使用Ubuntu系统库，呵呵。

PS：只替换库libstdc++.so.6是不行的，要解决此问题需要多替换几个库。输入终端命令：

export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libopencv_highgui.so.2.4:/usr/lib/x86_64-linux-gnu/libopencv_imgproc.so.2.4:/usr/lib/x86_64-linux-gnu/libopencv_core.so.2.4:/usr/lib/x86_64-linux-gnu/libstdc++.so.6:/usr/lib/x86_64-linux-gnu/libfreetype.so.6

export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/

#2 http://blog.csdn.net/babytang008/article/details/78631776
测试matlab接口
为避免链接库错误，需先在终端输入如下命令：
（包括以后每次需要使用matcaffe接口时，都要在终端输入如下命令，然后在该终端启动matlab）

export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libopencv_highgui.so.2.4:/usr/lib/x86_64-linux-gnu/libopencv_imgproc.so.2.4:/usr/lib/x86_64-linux-gnu/libopencv_core.so.2.4:/usr/lib/x86_64-linux-gnu/libstdc++.so.6:/usr/lib/x86_64-linux-gnu/libfreetype.so.6:$LD_PRELOAD
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH 


再执行测试：

make mattest

    1

可能会在测试开始时出现如下提示，但不影响最后测试结果

    malloc: unknown:0: assertion botched
    free: called with unallocated block argument
    last command: (null)
    Aborting…find: `bash’ terminated by signal 6

打开Matlab手动测试接口：（参考）
1下载bvlc_reference_caffenet.caffemodel
链接：http://dl.caffe.berkeleyvision.org/bvlc_reference_caffenet.caffemodel
下载后放入文件夹/caffe-master/models/bvlc_reference_caffenet 这是因为一会运行的demo要使用这个模型。
2 从终端启动matlab，切换到目录 ~/caffe/matlab/demo/（很重要）
3输入命令 run('classification_demo.m')或者双击打开classification_demo.m点击上面的“Run”即可。
4输出是一个1000×1的矩阵，因为ImageNet数据集有1000个类别。

## matlab 文件权限问题

An error was encountered while saving the command history
java.io.FileNotFoundException: /home/userA/.matlab/R2014b/History.xml (Permission denied)
    at java.io.RandomAccessFile.open(Native Method)
    at java.io.RandomAccessFile.<init>(Unknown Source)
    at com.mathworks.mde.cmdhist.AltHistoryCollection$CommandSaver.run(AltHistoryCollection.java:1212)
    at java.lang.Thread.run(Unknown Source)
## 解决方案： sudo chmod -R 777 .matlab

