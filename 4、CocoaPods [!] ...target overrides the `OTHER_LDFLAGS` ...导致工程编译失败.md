### CocoaPods [!] ...target overrides the `OTHER_LDFLAGS` ...导致工程编译失败

> 背景: 
>
> 工程 采用 pod init 方式集成 CocoaPods,  pod install 之后报警告，未处理，工程编译失败。

```shell
[!] The `project_name [Debug]` target overrides the `OTHER_LDFLAGS` build setting defined in `Pods/Target Support Files/Pods-project_name/Pods-project_name.debug.xcconfig'. This can lead to problems with the CocoaPods installation
    - Use the `$(inherited)` flag, or
    - Remove the build settings from the target.

[!] The `project_name [Release]` target overrides the `OTHER_LDFLAGS` build setting defined in `Pods/Target Support Files/Pods-project_name/Pods-project_name.release.xcconfig'. This can lead to problems with the CocoaPods installation
    - Use the `$(inherited)` flag, or
    - Remove the build settings from the target.
```

很明显的，警告的原因是 工程 build setting 重置了在pod的一个配置文件里面已经声明了的`OTHER_LDFLAGS`参数，这个参数被工程重置了，会产生一些意料之外的错误。解决方案就是将工程中的这个配置的参数值设成 `$(inherited)`， 或者把`OTHER_LDFLAGS`参数从build setting中移除掉

这里试了试第二种方案，实践可行：

点击项目文件 `project.xcodeproj`，右键显示包内容，打开`project.pbxproj`，删除`OTHER_LDFLAGS`的地方，保存，回到 Xcode，编译通过。可知`project.pbxproj`应该就是`biuld setting`的源文件。