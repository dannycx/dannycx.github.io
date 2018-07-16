# Android开机动画
## 目录
- 源码目录下的 /device/softwinner/octopus-f1/media/ 目录
- 开机动画配置文件路径 /device/softwinner/octopus-f1/octopus_f1.mk 搜关键字 bootanimation
## 相关文件介绍
- 动画相关目录：part0，part1，两组开机动画，图片格式为 png，分辨率需与设备分辨率一致

- desc.txt目录为开机动画配置文件
- 700 860 15  开机动画在屏幕先以多少解析度显示，超出设备分辨率，显示不全 每秒播放帧数
- p 1 0 part0 前两个参数代表只播放一次，空指针，播放part0图片
- p 0 0 part1 前两个参数代表重复播放，空指针，反复播放part1图片

## 修改开机动画
- 将 part0 或 part1 中图片替换为自己图片，注意分辨率，然后压缩
- 修改后执行 adb push bootanimation.zip system/media 重启即可
