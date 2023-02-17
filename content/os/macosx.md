# macosx

esxi5.5最多只能安装macOS10.13版本

#### ios-simulator

手动安装下载好的模拟器\(dmg模式\)

1. dmg用安装器安装后，会在释放一个目录到根Contents
2. 创建目录sudo mkdir -p  /Library/Developer/CoreSimulator/Profiles/Runtimes/iOS 7.1.simruntime \(模拟器版本\)
3. 移动/Contents到/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS 7.1.simruntime下

#### close\_nsurlsessiond

```
#!/bin/sh
launchctl unload /System/Library/LaunchAgents/com.apple.nsurlstoraged.plist
launchctl unload /System/Library/LaunchAgents/com.apple.nsurlsessiond.plist
sudo launchctl unload /System/Library/LaunchDaemons/com.apple.nsurlsessiond.plist
sudo launchctl unload /System/Library/LaunchDaemons/com.apple.nsurlstoraged.plist
```

[下载macOS镜像，速度快](https://github.com/munki/macadmin-scripts)

**制作启动盘**

```
1、创建一个空dmg
hdiutil create -o ~/Desktop/macOS10.14.6 -size 6.5g -layout SPUD -fs HFS+J

2、挂载新建的dmg
hdiutil attach ~/Desktop/macOS10.14.6.dmg -noverify -mountpoint /Volumes/install_build

3、将下载的安装镜像复制到新建的dmg
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/install_build --nointeraction

4、卸载新建的dmg
hdiutil detach /Volumes/install_build

5、压缩
hdiutil convert ~/Desktop/macOS10.14.6.dmg -format UDZO -o ~/Desktop/macOS10.14.6-z.iso

6、将dmg转换为iso(可选)
hdiutil convert ~/Desktop/macOS10.14.6.dmg -format UDTO -o ~/Desktop/macOS10.14.6.iso
```



