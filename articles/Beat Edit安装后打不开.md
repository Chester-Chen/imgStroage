2019/7/3 18:42:04 

# Beat Edit安装后打不开

win+r打开cmd窗口，输入regedit进入注册表，找到HKEY_CURRENT_USER->Software->Adobe->csxs文件

csxs可能会有多个，本人用的pr2019cc，修改的csxs.9

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/Beat%20Edit%E5%AE%89%E8%A3%85%E5%90%8E%E6%89%93%E4%B8%8D%E5%BC%80/1.png?raw=true)

修改方法：在这里右键新建字符串值，数值名称：PlayerDebugMode； 数据值：1；

![](https://github.com/Chester-Chen/imgStroage/blob/master/images/Beat%20Edit%E5%AE%89%E8%A3%85%E5%90%8E%E6%89%93%E4%B8%8D%E5%BC%80/2.png?raw=true)

**如果csxs.9不成功，请把其他的csxs都试一遍**