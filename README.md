(PS:图片有点多,网速不好的请耐心等待一下。。手机用户点一下 View All of README.md 查看详情)

- 关于私有 pod ，其实原理大致相同，如果不能参悟的朋友,耐心等待，待我有空再研究一下私有 pod。
- 除了方便的管理一些自己写的分类，其实这个技能也是日后自己写第三方框架集成 Cocoapods 快速上手技能。
- 好记星不如烂笔头，对于这种不常用的技能，只能写一篇笔记，日后好快速恢复。
- TODO：
	- 增加一些常用的分类。
	- podspec 文件的一些细节。例如:如何添加依赖，如何在项目中导入分类文件夹下的某个文件夹
	- 。。。

---

##导航
1. [2015-10-27](https://github.com/Damonvvong/DWCategory#2015-10-27)

---

##<a id="2015-10-27"></a>借助GitHub托管Category,利用CocoaPods集成到项目中。

（如果觉得这个过程太过简洁，可以看看我昨天学习时写的[笔记](https://github.com/Damonvvong/DWLog)又长又臭=。=）
###1.在 GitHub 初始化一个分类仓库（DWCategory）

![Category.png](http://7xlv6p.com1.z0.glb.clouddn.com/Category_min.png)

###2.clone 到本地，配置文件，再上传

- 把 GitHub 上的项目克隆到本地。(打开终端，cd 到桌面)

``` 
git clone git@github.com:Damonvvong/DWCategory.git

```
![Category_Clone](http://7xlv6p.com1.z0.glb.clouddn.com/Category_Clone.png)

> 这里是利用 SSH 方式 clone,配置 SSH[教程传送门](https://help.github.com/articles/generating-ssh-keys/)

- 把自己的分类放入桌面的DWCategory文件夹中,如下。

![Dir](http://7xlv6p.com1.z0.glb.clouddn.com/Dir.png)

- 给仓库创建一个 podspec 文件

```

pod spec create DWCategory git@github.com:Damonvvong/DWCategory.git

```


![Catagory_result](http://7xlv6p.com1.z0.glb.clouddn.com/Catagory_result.png
)


- 编写 podspec 文件

```
Pod::Spec.new do |s|
  s.name         = "DWCategory"  #名字
  s.version      = "0.1.0"  #版本号
  s.summary      = "Custom Category" #简短的介绍
  s.homepage     = "https://github.com/Damonvvong/DWCategory"   #主页,这里要填写可以访问到的地址，不然验证不通过
  s.license      = "MIT"  #开源协议
  s.author             = { "Damonwong" => "coderonevv@gmail.com" }  #作者信息
  s.social_media_url   = "http://weibo.com/damonone"    #多媒体介绍地址
  s.platform     = :ios, "7.0"    #支持平台及版本
  s.source       = { :git => "https://github.com/Damonvvong/DWCategory.git", :tag => s.version }    #项目地址，这里不支持ssh的地址，验证不通过，只支持HTTP和HTTPS，最好使用HTTPS,
  s.source_files  = "DWCategory/**/*" #代码源文件地址，**/*表示Classes目录及其子目录下所有文件，如果有多个目录下则用逗号分开，如果需要在项目中分组显示，这里也要做相应的设置
  s.requires_arc = true   #项目是否使用 ARC
end

```
- 把当前版本 push 到 GitHub，并上传版本号（podspec 文件的 s.version）

```
// push to remote
git add .
git commit -m "UIView + frame"
git push origin master

//add tag 
git tag -m "first release" 0.1.0
git push --tags
```

![Commit](http://7xlv6p.com1.z0.glb.clouddn.com/Catagory_commit.png
)


- 检查 podspec 文件是否编写正确。

```
pod lib lint
```

![pod_lib_lint](http://7xlv6p.com1.z0.glb.clouddn.com/pod_lib_lint.png)

- 如果有错误，就把错误修改了，并上传到 GitHub 上

### 利用[Trunk](http://guides.cocoapods.org/making/getting-setup-with-trunk.html)把自己的 DWCategory.podspec 文件上传给 Cocoapods

#####1.注册

```
pod trunk register coderonevv@gmail.com 'Damonwong' --verbose
```
- coderonevv@gmail.com：自己的邮箱
- Damonwong：用户名（最好和.podspec 文件 中一样）

#####2.检查是否注册成功
- 登录邮箱，确认注册

- 检查注册情况：pod trunk me（看到类似下面，就是成功了）

```
pod trunk me
  - Name:     Damonwong
  - Email:    coderonevv@gmail.com
```

#####3.上传DWCategory.podspec 到 Cocoapods/repo
- 进入 文件所在文件夹
```
	cd /Users/damon/Desktop/DWCategory 
```
- 上传文件

```
pod trunk push DWCategory.podspec
```

#####4.上传完成，查找一下

```
pod search DWCategory
```

![Succeed](http://7xlv6p.com1.z0.glb.clouddn.com/Category_Succeed.png)

> 下面就可以通过编写Podfile 文件，利用 Cocoapods 集成自己写的文件了

```
platform :ios, "6.0"
pod 'DWCategory'
```

#Done！
