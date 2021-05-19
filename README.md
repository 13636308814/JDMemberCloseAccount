# JDMemberCloseAccount

学习python操作selenium的一个🌰，用来 全自动/半自动 退出加入的所有店铺会员

* 全自动：短信验证码全自动，图形验证码用 [超级鹰打码](https://www.chaojiying.com/) ，费用是1块=1000积分，一次扣15积分

* 半自动：短信验证码全自动，图形验证码手动

## 要求

1. 有一定的电脑知识 or 有耐心爱折腾
   
2. chrome驱动(只在chrome测试了，故只留了chrome)
   
3. 操作系统(只在mac上测试了，非M1)
   
4. 关于手机短信验证码同步到浏览器中，本人采用了websocket来传递验证码
   
5. 关于如何在手机传递到浏览器，这点只说一下我的方式(达到目的即可)
   
   * 安卓端：利用tasker软件监听，一旦监听到就立即通过websocket推送过来
   
   * 安卓端：利用macrodroid软件监听，一旦监听到就立即通过websocket推送过来
   
   * 关于 `tasker` 和 `macrodroid` 配置均在 [extra](https://github.com/yqchilde/JDMemberCloseAccount/tree/main/extra) 目录下

## 安装方法

1. 克隆到本地

    ```shell
    git clone https://github.com/yqchilde/JDMemberCloseAccount.git
    ```

2. 安装所需要的包

    ```shell
    pip3 install -r requirements.txt
    ```

3. 下载对应的浏览器驱动放到项目的`drivers`文件夹下面
    * `chrome`请访问`chrome://version/`查看浏览器的版本，然后去 [chromedriver](http://chromedriver.storage.googleapis.com/index.html) 下载对应的版本/系统驱动

4. 配置`config.json`

    ```json
    {
        "browserType": "Chrome",
        "headless": false,
        "binary": "",
        "cjy_validation": false,
        "cjy_username": "",
        "cjy_password": "",
        "cjy_soft_id": "",
        "cjy_kind": 9101,
        "ws_conn_url": "ws://localhost:5201/subscribe",
        "mobile_cookie": "",
        "users": {}
    }
    ```
   
    * `browserType`: 浏览器类型
    
    * `headless`: 无头模式，建议默认设置
    
    * `binary`: 可执行路径，如果驱动没有找到浏览器的话需要你手动配置
    
    * `cjy_validation`: 是否开启超级鹰验证图形验证码
    
    * `cjy_username`: 超级鹰账号，仅在 cjy_validation 为 True 时需要设置
    
    * `cjy_password`: 超级鹰密码，仅在 cjy_validation 为 True 时需要设置
    
    * `cjy_soft_id`: 超级鹰软件ID，仅在 cjy_validation 为 True 时需要设置
    
    * `cjy_kind`: 超级鹰验证码类型，仅在 cjy_validation 为 True 时需要设置，且该项目指定为 `9101`
    
    * `ws_conn_url`: websocket链接地址
    
    * `mobile_cookie`: 手机端cookie，是pt_key开头的那个
    
    * `users`: web端cookie，通过add_cookie.py添加


5.  添加`cookie`

    * web端cookie：请在项目目录下执行`python3 add_cookie.py`， 在打开的浏览器界面登录你的京东，此时你可以看到`config.json`已经有了你的用户信息（**请不要随意泄露你的cookie**）
      
    * 手机端cookie：在 `config.json` 中写入 `mobile_cookie` 项，注意是pt_key开头的那个（**请不要随意泄露你的cookie**）

6.  执行主程序

    在项目目录下执行`python3 main.py`，等待执行完毕即可

## websocket服务端运行

1. 手动运行 `go run ./cmd/jd_wstool`

2. 下载 [jd_wstool](https://github.com/yqchilde/JDMemberCloseAccount/releases)，选择自己的电脑系统对应的压缩包，解压运行

![测试图](https://github.com/yqchilde/JDMemberCloseAccount/blob/main/screenshots/test_img3.png)

## 手机端短信如何传递给电脑端

1. 安卓端，我是用了tasker监听，总是随便一个可以监听到的，然后请求接口就行，接口如下

2. 安卓端，用 `Macrodroid监听`，原理一样

* 关于 `tasker` 和 `macrodroid` 配置均在 [extra](https://github.com/yqchilde/JDMemberCloseAccount/tree/main/extra) 目录下

```bash
http://同局域网IP:5201/publish?smsCode=短信验证码

例如：
http://192.168.2.100:5201/publish?smsCode=12345

同局域网IP会在运行 `./jd_wstool 或 jd_wstool.exe` 时提示出来，例如：
listening on http://192.168.2.100:5201
```

## ScreenShots

![测试图1](https://github.com/yqchilde/JDMemberCloseAccount/blob/main/screenshots/test_img1.gif)

![测试图2](https://github.com/yqchilde/JDMemberCloseAccount/blob/main/screenshots/test_img2.gif)

# Thanks

感谢以下作者开源JD相关项目供我学习使用

[@AntonVanke](https://github.com/AntonVanke/JDBrandMember)

