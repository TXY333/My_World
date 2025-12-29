## 传统方法
https://www.bilibili.com/video/BV1i4421Z7aM/?share_source=copy_web&vd_source=ca4e02b93a63f6b61653cafdecdfc29f
![[1751811398994.png]]

---
如上图  一般来说，深度学习项目在运行的时候是多个参数同时传入的，而且是以命令行的形式传参数的，但是debug的时候无法以命令行的形式传参数？
### 那么问题就是
==如何再debug的时候把命令行调出来？或者如何从一开始将所有参数传入？==
#### 1.创建json文件
![[1751811870266.png]]			

#### 2.选择带有参数的文件
![[1751811939974 2.png]]
#### 3.具体参数介绍
![[1751812048218.png]]
“program“：选择当前选中的文件
”cwd"：设置当前工作目录，有以下两种：
          filedirname：设置工作目录为以下当前打开文件所在的目录
          workspaceFolder:设置当前工作空间的根目录为工作目录
 ”args“：有以下两种
		 1. debug前把参数传入进去：把参数复制过去
		 2. ${command:pickArgs}：用户在debug前，会有一个交互输入能够手动输入参数



## 优雅方法
[https://www.bilibili.com/video/BV1wt421V718/?share_source=copy_web&vd_source=ca4e02b93a63f6b61653cafdecdfc29f](https://www.bilibili.com/video/BV1wt421V718/?share_source=copy_web&vd_source=ca4e02b93a63f6b61653cafdecdfc29f)

