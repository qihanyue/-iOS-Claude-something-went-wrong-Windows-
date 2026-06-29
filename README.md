# 解决iOS端Claude封号后卡"something went wrong"问题（Windows版）
>  教程来自苍梧月，仅发布于Linux.do及Github。
>  注意：请勿相信任何第三方"在线修复工具"，当于将自己手机大门打开，将所有信息都流到对方手中(包含但不限于所有软件记录、账号密钥等)，谨防信息泄露，最安全的方法只有自己动手操作。
## 前置条件

我默认你已有代理并已经在电脑下载安装Charles：  
https://www.charlesproxy.com/download/latest-release/

## 操作步骤

### 1. 配置手机代理

手机和电脑连接同一个Wi-Fi。  
进入手机的无线局域网设置，将代理改为**手动**。  
- 服务器：填写电脑的局域网IP  
- 端口：`8888`

> 如果IP填写正确，电脑端Charles会弹出提示，点击Allow允许手机连接。

---

### 2. 安装并信任证书

**此步骤请完整阅读后再操作。**

手机Safari浏览器访问：  
`chls.pro/ssl`  

下载安装证书后，依次进入：  
`设置` → `通用` → `关于本机` → `证书信任设置`  

手动将刚刚安装的证书的信任开关打开。

---

### 3. 配置SSL代理

回到电脑端Charles，点击顶部工具栏：  
`Proxy` → `SSL Proxying Settings`  

在Include中添加以下三条记录：  
- `*.anthropic.com`  
- `claude.com`  
- `claude.ai`  

> 注意：星号`*`不要漏掉。

然后将电脑代理切换为**虚拟网卡模式**。  
手机全程**不要开启任何代理软件**。

> 我一直拦截不成功，后来把全局模式改成了虚拟网卡就成功了。

![SSL代理配置成功示意图](https://github.com/qihanyue/-iOS-Claude-something-went-wrong-Windows-/blob/main/images/pic1.png?raw=true)

配置完成后，手机打开Claude，确保与其相关的网址（即上面添加的三条）前面显示**蓝色闪电**图标。  
如果仍显示🔒，说明上一步的手动信任证书没有做，或者手机连接时电脑没有点击Allow。



---

### 4. 配置Rewrite规则

点击顶部菜单栏：  
`Tools` → `Rewrite`  

勾选`Enable Rewrite`，然后点击左下角**➕**，Name处随便填写。  

随后点击右上框**➕**，依次在Host中添加以下三个网址，其他字段留空：  
![Rewrite规则添加示意图](https://github.com/qihanyue/-iOS-Claude-something-went-wrong-Windows-/blob/main/images/pic2.png?raw=true)

点击右下框的**➕**，按下图所示填写。  
（PS：Replace的Value填什么都可以，我胡乱打了一串字符）

![Rewrite规则填写示意图](https://github.com/qihanyue/-iOS-Claude-something-went-wrong-Windows-/blob/main/images/pic3.png?raw=true)

---

### 5. 完成配置

点击Done保存所有设置。

---

### 6. 验证效果

手机关闭Claude后台，重新打开应用。  
不出意外的话，这时就被踢回登录页面了。

---

### 7. 恢复网络设置

验证成功后，请复原所有网络设置：

- 手机无线局域网设置中，将代理改回**自动**。
- 电脑Charles中，进入`Tools` → `Rewrite`，取消勾选`Enable Rewrite`。
- 电脑关闭代理。
