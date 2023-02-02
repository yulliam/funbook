# 反调试方法
1、判断是否有调试器连接： Debug.isDebuggerConnected()；
2、判断是否被其他进程跟踪（使用Ptrace方式跟踪一个进程时，目标进程会记录自己被谁跟踪，通过查看 /proc/pid/status 中 tracerPid 是否为 0）；
3、每个进程同一时刻只能被1个调试进程ptrace，主动ptrace本进程可以使得其他调试器无法调试；
4、检测是否在非 Debug 编译模式下，进行了调试操作；
5、一般调试都是在设备被 root条件下，所以可以检测设备是否被 root ，如果存在风险就退出应用。
