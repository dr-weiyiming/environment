## 1. 接口配置问题
 
Invalid MEX-file '/home/wei/caffe/matlab/+caffe/private/caffe_.mexa64':
/home/wei/caffe/matlab/+caffe/private/caffe_.mexa64: undefined symbol:
_ZN2cv8imencodeERKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERKNS_11_InputArrayERSt6vectorIhSaIhEERKSB_IiSaIiEE

出错 caffe.set_mode_cpu (line 5)
caffe_('set_mode_cpu');

出错 caffe.run_tests (line 6)
caffe.set_mode_cpu();

## 1. 网上解决方案
export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libopencv_highgui.so.2.4:/usr/lib/x86_64-linux-gnu/libopencv_imgproc.so.2.4:/usr/lib/x86_64-linux-gnu/libopencv_core.so.2.4:/usr/lib/x86_64-linux-gnu/libstdc++.so.6:/usr/lib/x86_64-linux-gnu/libfreetype.so.6

export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/

再执行测试：make mattest
可能会在测试开始时出现如下提示，但不影响最后测试结果

    malloc: unknown:0: assertion botched
    free: called with unallocated block argument
    last command: (null)
    Aborting…find: `bash’ terminated by signal 6

## 2. matlab 文件权限问题

An error was encountered while saving the command history

java.io.FileNotFoundException: /home/userA/.matlab/R2014b/History.xml (Permission denied) 
    
    at java.io.RandomAccessFile.open(Native Method)
    at java.io.RandomAccessFile.<init>(Unknown Source)
    at com.mathworks.mde.cmdhist.AltHistoryCollection$CommandSaver.run(AltHistoryCollection.java:1212)
    at java.lang.Thread.run(Unknown Source)
## 2. 解决方案： sudo chmod -R 777 .matlab

