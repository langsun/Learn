###一、获取ANR日志信息

#####1.获取日志信息的名字
1. adb shell
2. cd /data/anr
3. ls 
     
**注意：**此时就出现ANR日志信息的文件，可能是anr_2019-11-27-14-29-06-371或traces.txt形式
#####2.获取日志信息

1. 在桌面创建一个文件夹anr
2. mkdir ANR
3. cd ANR/
4. adb pull /data/anr/anr_2019-11-27-14-28-45-636
5. adb: error: failed to copy '/data/anr/anr_2019-11-27-14-28-45-636' to './anr_2019-11-27-14-28-45-636': remote open failed: Permission denied
6. ANR sun$ adb bugreport
7. /data/user_de/0/com.android.shell/files/bugreports/bugreport-PD...5-10-49.zip: 1 file pulled. 15.6 MB/s (5880371 bytes in 0.361s)

**注意：**第4步已经将文件复制到了ANR文件夹中，如果出现第5步的错误，那就执行第六步，然后回出现第七步的提示，这时就在ANR文件夹中生成了一个.zip文件，解压得到--》FS--》anr—-》anr_2019-11-27-14-28-45-636，就可以进行分析了
