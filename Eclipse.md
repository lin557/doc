# Eclipse 设置



## 基础设置

参考阿里的JAVA开发手册规范做以下设置

### 1. 设置UTF-8编码

```
Window->Preferences->General->Workspace 面板, 找到 Text file encoding->Other, 选择UTF-8
```



### 2. 设置Linux换行符

```
Window->Preferences->General->Workspace 面板, 找到 New text file line delimiter->Other, 选择Unix
```

对于已存在的文件, 可以对文件进行修改换行符

```
file—>Convert Line Delimiters To—>Unix/Windows
```



### 3. 显示不可见字符

```
window -> preferences -> general -> editors -> text editors, 勾选 Show whitespace characters
```



### 4. 空格代替TAB

```
# java文件
1. window -> preferences -> general -> editors -> text editors, 勾选 Insert spaces for tabs
2. window -> preferences -> java -> code style -> formatter, 点 New, 随便输个名称，在Tab policy中选取Spaces only, 点ok
```

```
# xml文件
window -> preferences -> xml -> xml files -> editor, 右侧勾选 Indent using spaces, 并将 Indentation size 设置为 4
```



### 5. 中文字体

```
window -> preferences -> general -> appearance -> colors and fonts

右侧树表中
basic - text font 点 edit, 更改字体为“Courier New”, 字号大小不变, 保存
```



## Java Jdk配置



### 1. 错误提示

```
The compiler compliance specified is 1.8 but a JRE 16 is used.
```

```
Build path specifies execution environment JavaSE-1.8. There are no JREs installed in the workspace that are strictly compatible with this environment.
```



### 2. 解决方法

```
1. window -> preferences -> java -> installed jres, 右边点 add, 在Add JRE窗口中选 Standard VM 然后next, JRE home后面点Directiory选本机上安装的Jdk路径, 如C:\Program Files\Java\jdk1.8.0_191或C:\Program Files\Java\jre1.8.0_191, 点Finish完成(Jre Name自动生成不用管)
2. 回到Installed JREs窗口, 勾选刚新增的jdk
```



### 3. 修改编译版本

```
window -> preferences -> java -> compiler, 将Compiler compliance level: 改成 1.8(我用的是1.8)
```





## Git安装配置

新版本集成git插件, 只需配置



### 1. 手动配置

```
# 注意: git是按项目配置的
window -> preferences -> version control -> git -> configuration, 选user settings 配置git用户名相关
```



### 2. 用git clone生成的项目

```
# 自动配置, 无需管理
```



## Maven

新版本集成maven插件, 只需配置

### 1. 默认配置

```
不需任何配置, 直接启动项目就会下载相关的jar包
```



### 2. 用户配置

```
window -> preferences -> maven -> user settings, 将global settings 与 user settings 全换成自己的配置, 如 D:\Program Files\apache-maven-3.3.9\conf\settings.xml 保存, 退出, 重启eclipse
```



## 安装插件



### 1. 阿里代码规范

```
Help >> Install New Software, 点 Add 后, Name 输入 p3c, location 输入 https://p3c.alibaba.com/plugin/eclipse/update, 勾选下方的 Ali-CodeAnalysis, 下一步进行安装, 安装完成, 重启ide
```

