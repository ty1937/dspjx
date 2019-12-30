## 『短视频解析』开放平台接口协议


### 1 准备
#### 1.1 平台编码
短视频解析为各支持平台定义了平台编码，如下：

平台 | 编码
---|---
抖音| 抖音解析
快手| 快手解析
火山| 火山解析
陌陌| 陌陌解析
微视| 微视解析
美拍| 美拍解析
秒拍| 秒拍解析
最右| 最右解析
小咖秀| 小咖秀解析
皮皮虾| 皮皮虾解析
TikTok| TikTok解析
全民小视频| 全民小视频解析
全民K歌| 全民K歌解析
西瓜视频| 西瓜视频解析
开眼| 开眼解析
小红书| 小红书解析
映客| 映客解析
哗哩哗哩| 哗哩哗哩解析
网易云 | 网易云解析
UC小视频 | UC小视频解析
QQ看点 | QQ看点解析
刷宝 |刷宝短视频解析
AcFun |AcFun解析


#### 1.2 接口签名算法
每一次接口调用，都需要加上接口签名（sign）字段，服务器用该字段值鉴定接口确实由授权客户端发起。

签名算法涉及到以下两个参数：

参数|意义|长度|是否必填
---|---|---|---
clientId | 客户端密钥| 32 |是
timestamp | 当前时间戳 | 10 | 是
type | 解析视频平台 | 不定| 否
url | 解析视频地址 | 不定| 是

提示：
最好带入type字段，不带入会有判断类型，接口返回时间加长！

将这四个参数及**具体接口指定参与排序的字段的值**以如下方式拼接后，取MD5值，即：
```
将四个参数以字母顺序排序使用&连接，取MD5使用&cc=连接,总体再次MD5
带有type类型：
MD5(clientId=xxx&timestamp=xxx&type=xxx&url=xxx&cc=MD5(clientId=xxx&timestamp=xxx&type=xxx&url=xxx))
不带有type类型：
MD5(clientId=xxx&timestamp=xxx&url=xxx&cc=MD5(clientId=xxx&timestamp=xxx&url=xxx))

提示：
MD5中文编码为‘UTF-8’，许多开发语言默认‘gb2312’
标准为：
https://md5jiami.51240.com/
```


### 2 接口
#### 2.1 视频解析接口

###### 接口地址：
```
https://www.helloty.cn/api/jx
```

###### 接口方式：POST
###### 接口Content-Type：application/x-www-form-urlencoded
###### 接口形式：x-www-form-urlencoded

###### 本接口参与接口加密的字段为：
```
clientId、timestamp、url、type
```

###### 接口入参：

参数|意义|长度|是否必填
---|---|---|---
clientId|客户ID|32|是
timestamp|当前时间戳|10|是
sign|接口加密值||是
url | 视频页面地址 （不得出现URL编码符）||是
type | 视频平台编码||否

##### 接口示例
```
clientId=DCC5051B51155FB4249758AF1F3EA701&timestamp=1565850803&type=西瓜视频解析&url=https://www.ixigua.com/i6698226996385677832/&sign=ebe34e849ce2bbd545c6e3cec7071748
```

###### 接口出参：
参数|意义|类型|长度|是否必填
---|---|---|---|---
Status | 处理结果编码 | 字符串 | |是
msg | 处理结果消息 | 字符串| | 否
Frequency| 剩余解析次数| 数字 ||是
VideoBean | 处理结果数据 | 对象 | |成功时是
VideoBean.voidename| 视频标题 | 字符串| |否
VideoBean.photo| 视频封面URL| 字符串| |否
VideoBean.voideurl| 视频文件URL| 字符串| |成功时是

