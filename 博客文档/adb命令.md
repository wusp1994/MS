## adb安装



## adb命令相关

```shell
#启动服务
adb start-server
#关闭服务
adb kill-server
#显示当前运行得全部模拟设备
adb devices
#多个设备运行时，指定某一虚拟设备运行 -s

#重启设备
adb reboot --指定虚拟设备
#查看日志
adb logcat 
#清除日志
adb logcat -c

#安装应用程序
adb install -r 124.apk
#卸载app
adb shell #进入shell模式
cd data/app
rm 124.apk
exit
adb uninstall 124.apk

```

### 文件操作

```shell
#从本地传文件到设备中 from <> to <>,复制动作
adb push <本地路径> <远程目录>
#从设备中拷贝文件到本地 from <> to <>,复制动作
adb pull <远程目录> <本地路径> 
```

### 模拟事件

#### 点击

adb shell input [touchscreen|touchpad] tap x y

touchscreen -触摸屏幕（默认）

touchpad - 触摸板

x,y – 要点击的位置的横纵轴坐标，以左上角为原点，下和右为正方向

#### 滑动

adb shell input [touchscreen|touchpad] swipe x1 y1 x2 y2

touchscreen -触摸屏幕（默认）

touchpad - 触摸板

x1 y1 x2 y2 – 滑动起始和终止位置的横纵轴坐标

```shell
#点击
adb shell input tap 500 500

#滑动
adb shell input swipe 450 66 110 66

#文本框输入，如将光标定位到一个输入框后执行下面的命令
adb shell input text bcoder.com

#触发按键
adb shell input keyevent ‘按键码值/按键名称’

```



| keycode | 含义                         |
| ------- | ---------------------------- |
| 3       | HOME键                       |
| 4       | 返回键                       |
| 5       | 打开拨号应用                 |
| 6       | 挂断电话                     |
| 24      | 增加音量                     |
| 25      | 降低音量                     |
| 26      | 电源键                       |
| 27      | 拍照（需要已经在相机应用里） |
| 64      | 打开浏览器                   |
| 82      | 菜单键                       |
| 85      | 播放/暂停                    |
| 86      | 停止播放                     |
| 87      | 播放下一首                   |
| 122     | 移动光标到行首或列表顶部     |
| 123     | 移动光标到行末或列表底部     |
| 164     | 静音                         |
| 223     | 系统休眠                     |
| 224     | 点亮屏幕                     |
| 231     | 打开语音助手                 |

## 使用python脚本自动运行cmd 命令，执行adb脚本

```python
import os
os.system('adb shell input tap 100 100');
```

也可以使用 subprocess.Popen，最简单使用方式如下，设置shell=True，就不会弹出cmd框

```python
import subprocess
process = subprocess.Popen('adb shell input tap 14 1402',shell=True)
```



例子，每隔一段时间点击屏幕

```python
import time
import subprocess
i = 0
#每次操作的间隔时间取决于手机配置，配置越高时间越短
sleep_time = 0.5
while 1:
  #用popen设置shell=True不会弹出cmd框
  process = subprocess.Popen('adb shell input tap 14 1402',shell=True)
  time.sleep(sleep_time)
  process = subprocess.Popen('adb shell input keyevent KEYCODE_BACK', shell=True)
  time.sleep(sleep_time)
  process = subprocess.Popen('adb shell input tap 375 1402', shell=True)
  time.sleep(sleep_time)
  process = subprocess.Popen('adb shell input keyevent KEYCODE_BACK', shell=True)
  time.sleep(sleep_time)
  #os.system('adb shell input tap 14 1402')
  #os.system('adb shell input keyevent KEYCODE_BACK')
  #os.system('adb shell input tap 375 1402')
  i+=1
  print( str(i) + 'clicks have been completed')
```



