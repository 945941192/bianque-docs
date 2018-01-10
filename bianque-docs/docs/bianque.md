#联运流程
**Author**: liuke

**Date**: 2016/09/22
## 1.场景:
1. GG需要和一些游戏做联运，所以需要有一个联运平台和联运sdk
2. 同时我们有很多重打包的游戏，对于这些游戏，我们会加入一些plugin(每个plugin会包含一些功能)，而这些功能的使用需要用户拥有不同的level,
   因此我们也作为一个CP(Content Provider)来接入GG联运平台发行这些游戏


## 2.解释

### CP
>Content Provider, 游戏开发商

### GGLY-Server
>游戏联运平台, 联运平台和游戏开发商合作发行游戏，同一个游戏开发商可以和多家联运平台合作发行游戏，联运平台参提供账号系统，充值功能等功能，参入游戏收入分成

### CPServer 和 3rd-CPServer
1. 游戏的发行方，为游戏提供数据支持，游戏内的各种逻辑，道具都是通过CPServer控制的
2. 在联运过程中，我们自己重打包的游戏也是以联运的方式发行，用以区分第三方游戏的CPServer，我们自己的叫做CPServer第三方的称为3rd-CPServer

### GGLY-SDK
1. 联运的SDK，进行联运的游戏需要集成该SDK
2. 该SDK和GGLY-Server交互
3. 该SDK提供登录注册、购买道具等功能


###commodity
游戏中的商品，会有以下几种情况

1. 自有重打包游戏的商品： eg. 无限金币，免内购，上帝模式
               这类商品是GGLY-SDK直接向GGLY-Server购买，由GGLY-Server扣除积分，并把购买结果通知CPServer的

2. 第三方游戏的商品: eg. 裤子，魔法，10个金币，20个钻石,
              其中 裤子，魔法这类商品是游戏向3rd-CPServer购买，对于GGLY-Server完全透明
              10个金币，20个钻石这类商品是游戏集成的GGLY-SDK向GGLY-Server购买，然后GGLY-Server通知3rd-CPServer购买成功的，类似于游戏内货币充值行为

###plugin 
1. 只是自有联运游戏才会有plugin的概念
2. 每个游戏会包含多个plugin，每个plugin会有一些功能点，这些功能点就是商品(commodity)

###game
>和GGLY-Server进行联运的游戏，game中集成了GGLY-SDK

###game_id
>对于自有game
>>game_id为，8位16进制的字符串,game_id已有，直接录入到CP系统中

>对于3rd-CP game
>> game_id为，9位16进制的字符串,以及其他形式的能够区分开自有游戏和联运游戏的方式，3rd-CP需要向GGLY-Server申请game_id

###game_token
1. GGLY-SDK向GGLY-Server登录成功后获取的game_token,由GGLY-Server生成，用于Game获取到当前游戏用户的subid
2. game_token有效期为5分钟，过期无效，需要重新申请
3. game_token的验证需要考虑:
>>a. 有效期
b. game_id
c. sub_user_id,
d. ip
e. device_info(optional)

###sub_user_id
>GG账号在对应游戏中的id

###secert_key
>>1.secert_key在GGLY-Server平台获取
 2.GGLY-Server向CPServer或3rd-CPServer通知商品购买时使用secert_key和其他业务数据作为参数hash生成签名，用于CPServer校验通知数据的真实性
3.签名生成算法参考 GG平台联运接入文档 V1.0.0


## 3.数据举例
 表1. 重打包游戏的commodity数据示例
![Alt text](./imgs/1.png)



  表2. 第三方联运游戏的commodity数据示例
![Alt text](./imgs/2.png)



表数据说明:

1. 现在的设计已经支持重打包游戏的commodity使用GG对应的内部货币购买，这个GG内部货币与积分不一样，可能是需要用人民币购买的

2. 第三方游戏commodity购买是否支持GG积分，取决于联运的具体合作形式，GGLY-Server平台支持该功能

##4.功能

### 4.1游戏上架，游戏商品录入

####4.1.1自有联运游戏上架流程
![Alt text](./imgs/1.png)
注:
1. GGLY-Server需要提供录入game信息, 商品(commodity_id, price_type, price)的功能
2.  CPServer需要提供录入game，商品详情信息的功能
3.  自有的game可以支持每个游戏secert_key和game_token校验地址相同或者每个不同，这个看具体业务需求


#### 4.1.2第三方联运游戏上架流程
![Alt text](./imgs/2.png)

注：

   1. 第三方游戏需要联运需要先申请game_id,同时GGLY平台为之分配secert_key, game_token验证地址

   2. GGLY-Server需要提供录入game信息, 商品(commodity_id, price_type, price)的功能


### 4.2注册(不区分CPServer和3rd-CPServer)

![Alt text](./imgs/3.png)

GGLY-SDK注册功能支持三种方式：

1. 手机号
2. 第三方QQ
3. 第三方weixin 


###4.3登录(不区分CPServer和3rd-CPServer)
![Alt text](./imgs/4.png)

GGLY-SDK登录功能支持四种方式:

1. 手机号
2. 第三方QQ
3. 第三方weixin 
4. 老账号密码

说明:

  1. 步骤1的GGLY-Server处理需要考虑 <subid, game_id, device, ip>
  2. 步骤3,4的参数都要带上： <subid, game_id, device, ip>

###4.4登录注册效果图
![Alt text](./imgs/5.png)



###4.5商品购买（不区分自有和3rd的联运游戏， 区分GG积分支付或RMB支付）
1. GGLY-Server并不关心commodity详情，只关心(subid, game_id, commodity_id, price_type, price)
2. 并且完成购买后向CPServer或3rd-CPServer通知(subid, game_id, commodity_id)购买成功
3. CPServer或3rd-CPServer保存commodity的详细信息和游戏用户是否购买对应的商品信息

####4.5.1GG积分购买commodity
![Alt text](./imgs/6.png)


####4.5.2RMB购买commodity
![Alt text](./imgs/7.png =2000*2000)


## Reference
1 [360移动开放平台游戏接入](http://dev.360.cn/wiki/index/id/24)

2 [UC九游开放平台游戏接入](http://game.open.uc.cn/document/doc/detail/19)

3 [小米开放平台游戏接入](http://dev.xiaomi.com/docs/gameentry/%E6%89%8B%E6%9C%BA&pad%E6%B8%B8%E6%88%8F%E6%8E%A5%E5%85%A5%E6%96%87%E6%A1%A3/%E5%BA%94%E7%94%A8%E5%86%85%E6%94%AF%E4%BB%98%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97/)

4 [微信支付文档](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_3)





