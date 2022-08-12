---
layout: post
title:  "Framework笔记06 - LIPO篇"
date:   2022-08-10 19:01:00 +0800
categories: Framework
---

### lipo命令  
lipo命令主要用来查看库支持的架构以及合并、拆分库。  

1. 查看库
```
$ lipo -info XXXX.framework/framework_file
```

2. 合并库
```
$ lipo -create AAA.framework/framework_file1 BBB.framework/framework_file2 -output CCC.framework
```

3. 拆分库
```
$ lipo AAA.framework -thin x86_64 -output BBB.framework
```

### 库的资源  
在framework中加载图片资源的时候，使用`imageNamed:`可能会导致找不到图片资源，因为`imageNamed:`默认是会从MainBundle中的`Asset Catalog`中查找资源，而framework的资源可能不在这里。  
可以通过创建`bundle`来将资源放入其中，需要注意的是bundle并不会被打进库中，而是要单独添加到工程：在主工程`Copy Bundle Resources`中添加bundle。  
```
# 代码中可以通过以下两种方法加载：
NSBundle *bundle = [NSBundle bundleForClass:self.class];
NSString *bundlePath = [bundle.resourcePath stringByAppendingPathComponent:@"XXX.bundle"];
// 方法1
[UIImage imageNamed:[bundlePath stringByAppendingPathComponent:@"Images/logo.png"]];
// 方法2
[UIImage imageWithContentsOfFile:[bundlePath stringByAppendingString:@"/Images/logo.png"]];
```
