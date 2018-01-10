# GG客户端非社区接口
**Author**: mengyu

**Date**: 2016/10/14

[TOC]

## 更新
#### 2017-02-28
#### gg4.3
    1. 增加接口(27)获取池子游戏数据 
    2. 游戏详情页接口(17)增加字段 hasGift 表示是否有礼包
    3. 增加获取广告配置接口(14)
    4. 所有二级页面(卡片组装)的广告数据字段变更(通用定义3)
        page 下面原来的adCount去掉；新增字段adConfig, 例如{"position": "default", "count": 5},通过position 在广告配置中取pid，count表示广告数量
    5. 增加接口(28)根据渠道获取来源相关游戏验证信息
    6. 增加接口(29)获取悬浮窗数据


#### 2017-02-15
#### gg4.2
    1. 增加接口(25)获取可以开小号游戏列表
    2. 删除接口(8)之前的开小号接口，不再适用
    3. 修改接口(4)
        * 增加开小号的游戏信息(之前只下发安装到魔盒的游戏)
    4. 修改接口(6)
        * 游戏信息中删除plugins字段
        * 游戏信息中增加字段expiredPlugins(已失效mod),oldPlugins(可继续使用mod),newPlugins(新mod)
    5. 增加接口(26)获取游戏列表
 
###  2017-01-07
#### gg4.2
    1.增加接口(19)，获取存档加密用的key
    2.接口(6) 游戏详情中增加插件信息
    3.所有游戏详情中包含supportBox的地方增加supportBackup字段(bool),表示是否支持备份
    4.魔盒插件信息中
        插件列表中的第一条插件是推荐插件
        增加字段payedPlugins，内容为用户为当前游戏买过的插件id
        增加字段 downloadLinks
        增加字段latestPluginUpdateTime 最新插件的更新时间
        增加字段updateInfo: 
              {
                  "isShow": 控制帖子详情页是否显示更新信息,
                  "title": 更新信息项的名称,
                  "instruction": 更新信息项的具体内容,
              }
              有数据项示例:
                  "updateInfo":{
                      "instruction":"测试更新说明内容",
                      "isShow":true,
                      "title":"更新说明"
                  },
            
              无数据项示例:
                  "updateInfo":{
                      "instruction":"",
                      "isShow":false,
                      "title":"更新说明"
                  },
    5. 升级接口(6)增加字段
        "withoutModInfo": "各路大神正在紧锣密鼓制作MOD，请耐心等待。"
    6. 获取视频插件升级信息增加超时 字段
            "timeoutSohu": 15 * 1000,
            "timeoutLetv": 15 * 1000,
            "timeoutYouku": 15 * 1000,
            "timeoutBilibili": 15 * 1000,
        

#### gg4.1.5
    1. 增加上传用户安装游戏包接口(20)
    2. 游戏详情页增加相关帖子字段relativeGames，接口(17)
    3. 下载mod积分不足弹窗文案 ScoreNotEnoughTitle、ScoreNotEnoughLabel， 接口(12)
    4. 动态打包逻辑
    5. 修改悬浮窗接口(24) page id=9时， card24 增加字段 showIndexs:[0, 1, 2, 3, 4] 修改字段
    6. 升级接口增加字段，                   
        "updateDesc":  
        "plugins": [{插件信息}]


## 通用定义
本页所有请求都是post请求
### 服务器
    开发环境服务:https://ggapi.ggzhushou.cn:442
    测试服务：https://ggapi.ggzhushou.cn:444
    正式服务：https://ggapi.ggzhushou.cn
### 通用参数(所有接口都必须包含的参数，以下接口不再单列)
    
        "caller": {
            "id": "ggclient",
            "channel": "B200",
            "verCode": 309,
            "pkgName": "com.iplay.assistant",
            "token": "username||\u592a\u53e4&state||0&telephone||17090883230&adfree||1&secret||2|1:0|10:1468984985|6:secret|200:eyJ1aWQiOiAxMDEwMzUzLCAiY3VycmVudEFuZHJvaWRJZCI6ICIzMmUyZWRjNTI2YWNmNWU0IiwgImNyZWF0ZV90aW1lIjogMTQ2ODk4NDk4NSwgImFkZnJlZV9leHBpcmUiOiAxNDY5MDM0NzU1LCAibG9naW5faXAiOiAibG9naW5faXAiLCAiZ2FtZV9pZCI6IDB9|1f59fa0ca531b191655221f800563c427a39d8cf3f6a91c2bc1afab14a45c929&expire||0&type||0"
        },
        "device": {
            "product": "t03gxx",
            "gpuVendor": "Mali-400 MP",
            "fingerprint":
            "samsung/t03gxx/t03g:4.4.2/KOT49H/N7100XXUFNH2:user/release-keys",
            "heightPixels": 1280,
            "model": "GT-N7100",
            "density": 320,
            "widthPixels": 720,
            "vendor": "samsung",
            "imei": "352304060320363",
            "mac": "78:4B:87:6B:25:6A",
            "sdk": 19
        },
        "token":""

### 通用返回json定义
    
1.下载信息 downloadInfo

* 描述
        
    |   参数名   |类型| 说明  |
    | :--------  |--:--| :-    |
    |gameId |string|游戏id|
    |gameName|string| 游戏名|
    |pkgName|string| 包名|
    |verCode|int| 游戏版本号 |
    |ggVerCode|int| GG游戏版本号 |
    |verName|string| 游戏版本 |
    |iconUrl|string| 游戏图标链接|
    |gpuRenderers|list| 一个数字数组[1]|
    |minSdk|int|支持最低Android版本|
    |downloadLinks|json|下载地址列表|
    |downloadType|int|下载方式 1 直接下载 2 webview下载|
    |size|int|文件大小|
    |dependGooglePlay|  bool | 需要使用内置的免google play的插件|
    |dependCheck|   bool | 需要依赖内置的免验证插件|
    |supportBox|    bool | 是否支持直接安装到魔盒|
    |supportAccount|    bool | 是否支持账号中心插件登录|
    |supportCenterPlugin|   bool | 是否支持账号中心插件|
    |supportBackup| bool | 是否支持备份|
    |supportDuplicate|  bool | 是否支持双开|
    |pluginCount|   int | 游戏可用插件数|
    
* 示例
        
        {
            "gameId": "43ce4dac",
            "gameName": "牛仔对抗 UFO",
            "pkgName": "air.com.thumbstar.cowboyvsufos",
            "verCode": 1014000,
            "ggVerCode": 1479091699,
            "verName": "1.14",
            "iconUrl": "http://7xleaj.com1.z0.glb.clouddn.com/-50012b8a72ab4620",
            "gpuRenderers": [1],
            "downloadUrl": "https://ggapi.ggzhushou.cn:442/view/dl?id=3283eca4d3dfac56cf1eb2863c4f5819",
            "size": 44362151,
            
            "dependCheck": true,
            "supportPlugin": true,
            "supportBox": true,
            "supportBackup": false,
            "supportCenterPlugin": true,
            "dependGooglePlay": true,
            "supportAccount": true,
            "supportDuplicate": false,
            "pluginCount": 0,
            "downloadType": 1,
            "downloadLinks": {
                "webview": [],
                "client": [],
                "direct": [
                    {
                        "url": "https://ggapi.ggzhushou.cn:442/view/dl?id=82699740ceb42105a851bbe622fe5f24",
                        "name": "直接下载",
                        "desc": ""
                    }
                ]
            },
        }

2.跳转信息 action

* 描述
        
    |   参数名   |类型| 说明  |
    | :--------  |--:--| :-    |
    |actionType |string|跳转类型|
    |actionTarget|string| 跳转url|
    |actionData|json| 打点参数 |

* 示例
        
        "action": {
            "actionType": 1,
            "actionData": {
                "param": "e4426044"
            },
            "actionTarget": "/view/page?id=2000&pa=e4426044",
        }

3.二级页面广告配置

* 获取pid流程说明

    启动应用时会调用“获取广告配置接口(14)”取到广告数据，例如

        {
            "data": {
                "baidu": {
                    "name": "百度",
                    "aid": "f3b488ba",
                    "pids": {
                        "picture": "3024325",
                        "leave": "3350747",
                        "video": "3350750",
                        "listItem": "3350746",
                        "install": "3350753",
                        "banner": "3350753",
                        "default": "3350746"
                    }
                },
                "gdt": {
                    "name": "广点通",
                    "aid": "1105921915",
                    "pids": {
                        "default": "5050516968326629"
                    }
                },
                "adsource": "baidu"
            }
        }
    page广告配置是

        "adConfig": {
            "position": "default",
            "count": 5
        }
    那么要使用的pid 就是   广告规则数据["data"]["baidu"]["pids"]["default"] "3350746";
    如果position的值为video那么要使用的pid 就是   广告规则数据["data"]["baidu"]["pids"]["video"]        "3350750"。


* 描述
    
    |   参数名   |类型| 说明  |
    | :--------  |--:--| :-    |
    |position |string|广告位置，用来获取pid|
    |count|int| 当前页面广告数量|

* 示例

        "adConfig": {
            "position": "default",
            "count": 5
        }


## 1.页面
###uri: /view/page
### GET参数：
    id：
    pa：
    pg：
## 2.获取升级信息
### url: /api/get/latest-version-info
    
## 3.获取视频插件升级信息
### uri： /api/get/plugin-version-info
    
###     返回值：
    {
        "msg": "ok",
        "data": {
            "url": "http://ggapi.ggzhushou.cn/videotools4.0.apk",
            "verCode": 2,
            "timeoutSohu": 15 * 1000,
            "timeoutLetv": 15 * 1000,
            "timeoutYouku": 15 * 1000,
            "timeoutBilibili": 15 * 1000,
            
        },
        "rc": 0
    }
## 4.获取魔盒游戏
### url：/api/get/box-games
### POST参数
    "apks":
        [
            {
                "pkgName": "com.invictus.impossiball",
                "verCode": "132",
                "signatureSf":"sddddddddddasdffdsafdsfasf",
                "signatureMf":"sddddddddddasdffdsafdsfasf",
            },
            {
                "pkgName": "com.moe.niceshot.moli"
                "verCode": "132",
                "signatureSf":"sddddddddddasdffdsafdsfasf",
                "signatureMf":"sddddddddddasdffdsafdsfasf",
            },
            {
                "pkgName": "com.yileweb.cytl.sf"
                "verCode": "132",
                "signatureSf":"sddddddddddasdffdsafdsfasf",
                "signatureMf":"sddddddddddasdffdsafdsfasf",
            }
        ],
### 返回值：
    {
        "msg": "ok",
        "data": {
            "boxGames": [
                {
                    "verCode": 17,
                    "supportPlugin": true,
                    "gameName": "蒙特祖玛的宝藏3国服",
                    "iconUrl": "http://qiniuggic.yyhudong.com/a57c63327676383965cda3bd68efa0d6",
                    "supportBox": true,
                    "supportAccount": true,
                    "pkgName": "com.alawar.treasuresofmontezuma3",
                    "ggVerCode": 1479188898,
                    "size": 101506092,
                    "subTitle": "2款MOD",
                    "dependGooglePlay": true,
                    "payedPlugins": [],
                    "dependCheck": true,
                    "gpuRenderers": [
                        1
                    ],
                    "downloadLinks": {
                        "webview": [],
                        "client": [],
                        "direct": [
                            {
                                "url": "https://ggapi.ggzhushou.cn:442/view/dl?id=5f817ad939a1dfbe01df0ab588de3170",
                                "name": "直接下载",
                                "desc": ""
                            }
                        ]
                    },
                    "gameId": "35ef9004",
                    "downloadType": 1,
                    "supportDuplicate": false,
                    "supportCenterPlugin": true,
                    "desc": "极品三消游戏，挑战你的手指和你的眼睛",
                    "minSdk": 9,
                    "supportBackup": false,
                    "verName": "1.1.0",
                    "action": {
                        "actionType": 1,
                        "actionTarget": "/view/page?id=2000&pa=35ef9004",
                        "param": "35ef9004"
                    },
                    latestPluginUpdateTime:1480669359,
                    "plugins": [
                        "updateTime": 1480669359,
                        "authorLevel": 2,
                        "relatedGameId": "48173a91",
                        "id": 491,
                        "size": 45906,
                        "author": "KK小神",
                        "verCode": "200",
                        "authorid": 1010358,
                        "status": 1,
                        "pkgName": "com.gameassist.plugin.minimetro.frankpi",
                        "payedUsers": [
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1072858&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1000013&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1000279&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1000014&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1000017&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1000023&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=16777480&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1000290&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1000291&size=big",
                            "https://bbs.ggzhushou.cn:444/user/avatar?uid=1010333&size=big"
                        ],
                        "downloadCountShow": "16人下载",
                        "testModels": [
                            "Xiaomi MI 4C"
                        ],
                        "price": 0,
                        "gameVerCode": "14",
                        "action": {
                            "actionType": 102,
                            "actionTarget": "/forum_app/topic?topic_id=706304"
                        },
                        "vername": "1.200",
                        "desc": "车速加倍，车站容量翻倍，解锁全部城市。",
                        "authorAvatar": "https://bbs.ggzhushou.cn:444/avatar//001/01/03/58_avatar_big.jpg",
                        "name": "上帝模式补丁",
                        "showTime": "2016-12-02",
                        "gender": 0,
                        "downloadCount": 16
                        "isPayed": true     # 是否已经购买过  true|false
                        updateInfo: 
                          {
                              "isShow": 控制帖子详情页是否显示更新信息,
                              "title": 更新信息项的名称,
                              "instruction": 更新信息项的具体内容,
                          }            
                    ],
                    payedPlugins:[491],
                    "action": {
                        "actionType": 1,
                        "actionTarget": "/view/page?id=2000&pa=d0f819be",
                        "param": "d0f819be"
                    },
                    "size": 42834217
                }
            ],
            "showCount": 4,
            "rmInstalled": true,
            "moreUrl": "/view/page?id=2009",            # 点击更多时跳转页面 
            "emptyPicUrl": "http://7xleam.com1.z0.glb.clouddn.com/50.jpg", #没有魔盒游戏时的背景图
            "recommend": [
                "subTitle": "4款插件",
                游戏信息

            ]
        },
        "rc": 0
    }


## 6.获取可升级游戏
### url： /api/get/upgrade_games
### POST参数
    "apks":
        [
            {
                "pkgName": "com.moe.niceshot.moli",
                "signatureSf":"sddddddddddasdffdsafdsfasf",
                "signatureMf":"sddddddddddasdffdsafdsfasf",
                "ver_code": 7
                "gg_ver_code": 12312323
            },
            {
                "pkgName": "com.yileweb.cytl.sf",
                "signatureSf":"sddddddddddasdffdsafdsfasf",
                "signatureMf":"sddddddddddasdffdsafdsfasf",
                "ver_code": 200
            }
        ],
### 返回值
    {
        "msg": "ok",
        "data": {
            "upgradeGames":[{
                    "dependCheck": false,
                    "gameId": "25516ca7",
                    "downloadType": 1,
                    "minSdk": 8,
                    "supportPlugin": false,
                    "gameName": "妹型杀器【BT版】",
                    "verCode": 7,
                    "supportDuplicate": true,
                    "iconUrl": "http://7xufo0.com1.z0.glb.clouddn.com/mxsq0.png",
                    "supportBox": false,
                    "supportBackup": false,
                    "supportCenterPlugin": false,
                    "verName": "1.0.9",
                    "subTitle": "0款MOD",
                    "supportAccount": false,
                    "pkgName": "com.moe.niceshot.moli",
                    "gpuRenderers": [
                        1
                    ],
                    "ggVerCode": 1469158424,
                    "plugins": [],
                    "dependGooglePlay": false,
                    "downloadLinks": {
                        "webview": [],
                        "client": [],
                        "direct": [
                            {
                                "url": "https://ggapi.ggzhushou.cn:444/view/dl?id=0d2b6ac822d797151c770ec43eaaf1ff",
                                "name": "直接下载",
                                "desc": ""
                            }
                        ]
                    },
                    "size": 186084109,
                    "updateDesc": "",
                  redPlugins": [{
                        "isPayed": false,
                        "id": 458,
                        "priceInfo": "20积分",
                        "name": "3倍修改补丁"
                    }]
                    "oldPlugins": [{
                        "isPayed": false,
                        "id": 458,
                        "priceInfo": "20积分",
                        "name": "3倍修改补丁"
                    }]
                    "newPlugins": [{
                        "isPayed": false,
                        "id": 458,
                        "priceInfo": "20积分",
                        "name": "3倍修改补丁"
                    }]
                },...]
            "rmInstalled": True,
            "showCount": 4,
            "recommend": [推荐游戏信息],
            "withoutModInfo": "各路大神正在紧锣密鼓制作MOD，请耐心等待。"
        },
        "rc": 0
    }
## 7.获取推荐游戏(暂仅用于下载页面的推荐)
### url： /api/get/recommend-games
### 返回值
    {
        "msg": "ok",
        "data": {
            "rmInstalled": True,
            "showCount": 6,
            "recommend": [推荐游戏信息]
        },
        "rc": 0
    }
<!-- ## 8.获取可以开小号的游戏包名 -->
<!-- `注：开小号游戏需要计算签名，该接口告诉客户端需要计算哪些游戏的签名` -->
<!-- ###uri: /api/get/support_market_games -->
<!-- ### POST参数(同魔盒接口) -->
    <!-- "apks":[{"pkgName":"com.invictus.impossiball"}, -->
            <!-- {u'pkgName': u'com.moe.niceshot.moli'}, -->
            <!-- {u'pkgName': u'com.yileweb.cytl.sf'}], -->

<!-- ### 返回值： -->
    <!-- { -->
        <!-- "msg": "ok", -->
        <!-- "data": { -->
            <!-- "pkgs": [ -->
                <!-- "com.moe.niceshot.moli", -->
                <!-- "com.yileweb.cytl.sf", -->
                <!-- "com.invictus.impossiball" -->
            <!-- ], -->
        <!-- }, -->
        <!-- "rc": 0 -->
    <!-- } -->
    
## 9. 获取百度网盘升级信息
### url：/api/get/baidupan-version-info
### 返回值
    {
        "msg": "ok",
        "data": {
            "url": "http://ggapi.ggzhushou.cn/videotools4.0.apk",
            "verCode": 2
        },
        "rc": 0
    }
## 10.获取搜索框中的默认搜索词
### url：/api/get/default-keywords
### 返回值
    {
        "msg": "ok",
        "data": "御龙在天",
        "rc": 0
    }

## 11.获取搜索页推荐的游戏列表
### url: /api/get/hot-games
### 返回值
    {
        "msg": "ok",
        "data": {
            "items": {
                "poolId": "MP_1",
                "rmInstalled": true,
                "poolItems": [
                    {游戏信息}，
                    {游戏信息}，。。。
                ]
            },
            "title": "热搜"
        },
        "rc": 0
    }

## 12.获取协议内容
### url：/api/get/agreement
### 返回值
    {
        "msg": "成功",
        "data": {
            "WarningUnsupportGame": "游戏MOD无法支持你当前安装的{game_name}，请先卸载后从GG安装即可下载游戏MOD",
            "WarningUninstallGame": "请安装游戏后再下载游戏MOD~",
            "ScoreNotEnoughLabel": "赞助送积分",
            "pluginTitle": "提示条款",
            "showMsg": {
                "msg": "",
                "isShow": false
            },
            "ScoreNotEnoughTitle": "积分不足，无法完成下载"
        },
        "rc": 0
    }
## 13.搜索页面推荐热门搜索词
### url：/api/get/hot-keywords
### 返回值
    {
        "msg": "ok",
        "data": [
            "乖离性",
            "孤胆车神",
            "永不言弃",
            "模拟城市",
            "侠盗猎车手",
            "内购破解版",
            "炉石传说",
            "上帝模式",
            "侠盗猎车手",
            "孤胆车神：拉斯维加斯 免内购版"
        ],
        "rc": 0
    }
## 14.获取广告位信息
### /api/get/ad-config
接口用于获取广告位信息，打开客户端时调用一次

* url
    
    /api/get/ad-config

* method
    
    POST
    
* input 参数

    |   参数名   |类型|  是否必须 | 说明  |
    | :--------  |--:--| --------:| :--:    |
    | device     |json|  Y       |  常规参数             |
    | caller     |json|  Y       |  常规参数     |

* input 示例

        {
            "caller": {
                "id": "ggclient",
                "channel": "B2",
                "verCode": 441,
                "pkgName": "com.iplay.assistant",
                "token": "username||tt003&state||0&character||0&telephone||2016288277&adfree||1&secret||2|1:0|10:1485241972|6:secret|188:eyJ1aWQiOiAxMzgzNTA0LCAiY3VycmVudEFuZHJvaWRJZCI6ICI3NDRhYzk1Nzk2ZGJhZjFkIiwgImNyZWF0ZV90aW1lIjogMTQ4NTI0MTk3MiwgImFkZnJlZV9leHBpcmUiOiAwLCAibG9naW5faXAiOiAibG9naW5faXAiLCAiZ2FtZV9pZCI6IDB9|ca6778048bc3f3cf9c2534dff554d85f0956bf15ba0f791af00c657c329c4cc7&expire||7&create_time||1485241972&type||0"
            },
            "device": {
                "product": "t03gxx",
                "gpuVendor": "Mali-400 MP",
                "fingerprint":
                "samsung/t03gxx/t03g:4.4.2/KOT49H/N7100XXUFNH2:user/release-keys",
                "heightPixels": 1280,
                "model": "GT-N7100",
                "density": 320,
                "widthPixels": 720,
                "vendor": "samsung",
                "imei": "868746027415404",
                "mac": "78:4B:87:6B:25:6A",
                "sdk": 19
            },
        }

* output 
    
    |   参数名   |类型|  说明  |
    | :--------  |--:--| --------:--|
    | baidu     |json|  百度广告规则             |
    | gdt     |json|  广点通广告规则    |
    | adsource |string| 当前使用哪一套广告配置，现在值是 baidu 或者 gdt |

    广告规则(百度和广点通一样)

    |   参数名   |类型|  说明  |
    | :--------  |--:--| --------:--|
    | name     |string|  规则名字    |
    | aid     |string|  广告app id    |
    | pids |json| 各个位置的广告pid|

    pids

    |   参数名   |类型|  说明  |
    | :--------  |--:--| -----:--| 
    | picture     |string|  游戏详情截图最后一张    |
    | leave     |string|  退出广告    |
    | video     |string|  视频结束广告    |
    | listItem     |string|  列表条目    |
    | install     |string|  安装界面的广告   |
    | banner     |string|  banner大图    |
    | default     |string| 如果卡片中广告中的key 在pids的参数名|

* output 示例

    {
        
        "data": {
            "baidu": {
                "name": "百度",
                "aid": "f3b488ba",
                "pids": {
                    "picture": "3024325",
                    "leave": "3350747",
                    "video": "3350750",
                    "listItem": "3350746",
                    "install": "3350753",
                    "banner": "3350753",
                }
            },
            "gdt": {
                "name": "广点通",
                "aid": "1105921915",
                "pids": {
                    "default": "5050516968326629"
                }
            },
            "adsource": "baidu"
        }
    }
<!--    {-->
        <!-- "msg": "成功", -->
        <!-- "data": { -->
            <!-- "picture": "3024325", -->
            <!-- "leave": "3024319", -->
            <!-- "video": "3024316", -->
            <!-- "listItem": "3024324", -->
            <!-- "install": "3024326", -->
            <!-- "banner": "3024326" -->
        <!-- }, -->
        <!-- "rc": 0 -->
    <!-- } -->
<!-- ## 15.客户端打点接口 -->
<!-- ### url：/api/put/statistics -->
<!-- ### post参数： -->
    <!-- 'data'{ -->
             <!-- 'target': { -->
                    <!-- 'pageId': '必须有值', -->
                    <!-- 'pageParam': '游戏id,文章id,视频id,帖子id,搜索词,专题id,分类id  或者为 空字符串', -->
                    <!-- 'pageNum': '列表页的分页id 或者为 空字符串' -->
                <!-- }, -->
             <!-- 'from': { -->
                <!-- 'fromPage': '来源页面', -->
                <!-- 'fromParam': '来源的: 游戏id,文章id,视频id,帖子id,搜索词,专题id,分类id 或者为 空字符串', -->
                <!-- 'fromCard': '卡片id 或者为 空字符串', -->
                <!-- 'fromCardPosition': '卡片的位置（非卡片的）或者为 空字符串', -->
                <!-- 'fromPosition': '单个卡片内容的位置 或者为 空字符串', -->
            <!-- } -->
    <!-- } -->
<!-- ### 返回值 -->
    <!-- { -->
        <!-- "msg": "ok", -->
        <!-- "rc": 0 -->
    <!-- } -->

## 16.游戏时长打点
``
### url： /api/put/game_statistics
###post参数：
    
    'data': {}
### 返回值：
    {
        "msg": "ok",
        "rc": 0
    }

## 17. 游戏详情页
### url: /view/page
### post参数:
    id: 2000        # 固定
    pa: game_id     # 游戏id
### 返回值：
    
    {
        "msg": "成功",
        "data": {
            "page": {   
                "status": 0,        # 0;1表示已下架
                "updateTime": "2016.05.24更新",
                "relativeTopics":[{发现页卡片}],
                "relativeGames": [
                    {
                        "title": "王者荣耀",
                        "action": {
                            "position": 0,
                            "actionType": 1,
                            "actionTarget": "/view/page?id=2000&pa=9c77eaa4&rp=2000&rpa=9c77eaa4&rc=118&rcpo=6&rpo=0",
                            "param": "9c77eaa4"
                        },
                        "fileSize": "213.9MB",
                        "downloadInfo": {
                            "gameId": "9c77eaa4",
                            "pkgName": "com.tencent.tmgp.sgame",
                            "minSdk": 9,
                            "iconUrl": "http://img.wdjimg.com/mms/icon/v1/f/79/3fad5c8db2111be7df3db5d0dfbe779f_256_256.png",
                            "downloadType": 1,
                            "supportPlugin": false,
                            "gameName": "王者荣耀",
                            "verCode": 1,
                            "supportAccount": false,
                            "supportBox": false,
                            "supportBackup": false,
                            "supportCenterPlugin": false,
                            "verName": "1.3.6.3",
                            "dependGooglePlay": false,
                            "supportDuplicate": true,
                            "dependCheck": false,
                            "gpuRenderers": [
                                1
                            ],
                            "ggVerCode": 1510281124,
                            "downloadLinks": {
                                "webview": [],
                                "client": [],
                                "direct": [
                                    {
                                        "url": "http://ggapi.ggzhushou.cn/view/dl?id=d8b91b8d1a27819f985cb5ddbacbc205&rp=2000&rc=118&rpa=9c77eaa4&rcc=B2",
                                        "name": "直接下载",
                                        "desc": ""
                                    }
                                ]
                            },
                            "size": 224284529
                        },
                        "icon": "http://img.wdjimg.com/mms/icon/v1/f/79/3fad5c8db2111be7df3db5d0dfbe779f_256_256.png"
                    },
                ],
                "downloadType": 1,
                "minSdk": 9,
                "developerId": 0,
                "feature": [    # 游戏特色
                    {
                        "text": "不需要谷歌框架也可游戏。"
                    }
                ],
                "iconUrl": "http://img.wdjimg.com/mms/icon/v1/f/79/3fad5c8db2111be7df3db5d0dfbe779f_256_256.png",
                "supportBox": false,
                "supportBackup": false,
                "plugins": [],
                "supportAccount": false,
                "pkgName": "com.tencent.tmgp.sgame",
                "adCount": {
                    "GDTTotalCount": 0,
                    "GDTId": "6050303615747691"
                },
                "developer": "腾讯电商数码（深圳）有限公司",
                "category": "动作射击",
                "dependGooglePlay": false,
                "supportDuplicate": true,
                "pageTitle": "游戏详情",
                "source": "腾讯游戏",
                "minAndroidVersion": "Android 2.3",
                "colorLabels": [],
                "versionCode": 1,
                "dependCheck": false,
                "preview": [
                    {
                        "picture": "http://7xqj0m.com2.z0.glb.qiniucdn.com/feature-1.jpg",
                        "text": "1v1、3v3、5V5、闯关、探索等丰富游戏模式，让你随时随地战起来！"
                    },
                    {
                        "picture": "http://7xqj0m.com2.z0.glb.qiniucdn.com/feature-2.jpg",
                        "text": "上下千年的英雄乱斗，演绎缭乱时空的冒险史诗！"
                    },
                    {
                        "picture": "http://7xqj0m.com2.z0.glb.qiniucdn.com/feature-3.jpg",
                        "text": "跨服匹配秒开局，好友组队战排位，不靠装备、没有等级，更公平、更爽快，不靠RMB，超神也容易！"
                    },
                    {
                        "picture": "http://7xqj0m.com2.z0.glb.qiniucdn.com/feature-4.jpg"
                    }
                ],
                "downloadLinks": {
                    "webview": [],
                    "client": [],
                    "direct": [
                        {
                            "url": "http://ggapi.ggzhushou.cn/view/dl?id=d8b91b8d1a27819f985cb5ddbacbc205",
                            "name": "直接下载",
                            "desc": ""
                        }
                    ]
                },
                "gameLabelSource": 2,
                "topicId": 327789,
                "notice": [
                    {
                        "text": "<html><body>可能会出现断网重玩的情况、多进几次即可<br/>游戏需挂载VPN下载数据包</body></html>"
                    }
                ],
                "gameId": "9c77eaa4",
                "pageId": 2000,
                "relativeVideos": [
                    {
                        "title": "王者荣耀",
                        "pic": "http://7xqj0m.com2.z0.glb.qiniucdn.com/wzry.jpg",
                        "url": "",
                        "videoUrlType": 1,  # 视频链接类型，0网页链接，1裸链
                    }
                ],
                "hotComments": [
                    {
                        "comment": "哦呵呵你女兔兔",
                        "topicId": 327789,
                        "nickIcon": "",
                        "name": "~Lily~",
                        "avatarUrl": "https://ggauth-1663115772.cn-north-1.elb.amazonaws.com.cn:444/avatar/noavatar_big.jpg",
                        "image": "",
                        "likedCount": 0,
                        "commentCount": 0,
                        "isMan": 0,
                        "is_tipoff": 0,
                        "timestrap": "2016-08-03 14:12",
                        "lv": 4,
                        "device": "Redmi Note 3",
                        "postId": 629458,
                        "groupId": 52,
                        "isLike": 0,
                        "isMine": 0
                    },
                ],
                "gpuVendor": [
                    1
                ],
                "screenshotUrls": [
                    "http://img.wdjimg.com/mms/screenshot/9/1b/224a4fb0544eb200b526ee51d20b91b9_320_534.jpeg",
                    "http://img.wdjimg.com/mms/screenshot/1/35/f8dfbc057c30f43ce665b90f0dc87351_320_534.jpeg",
                    "http://img.wdjimg.com/mms/screenshot/c/6c/2ad274dc1a8cf6dbc468a0c5936546cc_320_534.jpeg",
                    "http://img.wdjimg.com/mms/screenshot/2/06/dd8c81efd42fb7c372a08232945fc062_320_534.jpeg",
                    "http://img.wdjimg.com/mms/screenshot/5/df/2803c05360475ba5ebf42e5f55cdbdf5_320_534.jpeg",
                    "http://qiniuimg.yyhudong.com/GG%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg"
                ],
                "supportCenterPlugin": false,
                "fileSize": 224284529,
                "versionName": "1.3.6.3",
                "vercodeByGg": 1510281124,
                "desc": "热血竞技、抗塔强杀！MOBA第一真竞技手游《王者荣耀》拥有极致的英雄实时对战体验，领略爽快连招、推塔三杀、团灭超神的酣畅淋漓！",
                "name": "王者荣耀",
                "language": "中文",
                "detailInfo": [
                    {
                        "groupType": 0,
                        "content": [
                            {
                                "text": "<html><body>可能会出现断网重玩的情况、多进几次即可<br/>游戏需挂载VPN下载数据包</body></html>"
                            }
                        ],
                        "groupTitle": "注意事项",
                        "groupBarColor": "#2aad60"
                    },
                    {
                        "groupType": 0,
                        "content": [
                            {
                                "text": "<html><body>Nexus5(4.4)完美运行<br/>华为荣耀3X(4.1)完美运行<br/>小米4（开发5.1）完美运行<br/>红米note2(5.0)完美运行</body></html>"
                            }
                        ],
                        "groupTitle": "实测机型",
                        "groupBarColor": "#2aad60"
                    },
                    {
                        "groupType": 0,
                        "content": [
                            {
                                "text": "<html><body>哈拉顿运动会来临！<br/>参加一场史诗级世界赛事！<br/>特殊任务和每日赛事<br/>挑战闯关<br/>在新的单人模式中击退一波又一波的怪物！<br/>在三处独特地点战斗，赢取史诗奖励！<br/>新经典副本<br/>战斗并解锁制作史诗装备的新材料！<br/>全新传家宝装备<br/>一套装备从1级一直用到45级！<br/>新改进和新内容<br/>全新游戏菜单<br/>新传承武器等级<br/>新坐骑、随从和幻化！<br/>更新各职业平衡性</body></html>"
                            }
                        ],
                        "groupTitle": "更新内容",
                        "groupBarColor": "#2aad60"
                    }
                ],
                "shareData": {
                    "url": "http://ggzhushou.cn/game/detail/1875aa91",
                    "message": "分享一个游戏",
                    "icon": "http://7xleaj.com1.z0.glb.clouddn.com/-1caa0d5843fd63c6",
                    "title": "分享游戏"
                },
                "supportPlugins": false,
                "isPortrait": false,
                "isConcern": false, # 是否关注
                "downloadCount": 1049948,
                "hasGift": true
            }
        },
        "rc": 0
    }
## 17. 获取内置插件升级信息
### url: /api/get/buildin-plugin-version-info
### post参数:
    pkgName: com.iplay.assistant
    verCode: 406
### 返回值：

    {
        "msg": "成功",
        "data": {
            "showMsg": {
                "msg": "",
                "isShow": false
            }
            "url": "https://cdn2.yyhudong.com/ggclient4.0/4.0.5011/GGAssistant_Rel_4.0.5011.apk",
            "verCode": 406,
            "pkgName": "com.iplay.assistant",
            "name": "游戏助手",
            "verName": "4.0.5011"
        },
        "rc": 0
    }
    没有更新版本时数据：
    {
        "msg": "成功",
        "data": {
               "showMsg": {
                "msg": "",
                "isShow": false
           }
        },
        "rc": 0
    }


## 18. 获取静态图片资源
### url: /api/get/client-source
### 返回值

    {
        "msg": "成功",
        "data": {
            "charge_effect": "http://oikt4avk3.bkt.clouddn.com/charge_effect.png",
            "invite_friends": "http://oikt4avk3.bkt.clouddn.com/invite_friends.jpg",
            "charge_bg": "http://oikt4avk3.bkt.clouddn.com/charge_bg.png",
            "noboximg": "http://oikt4avk3.bkt.clouddn.com/noboximg.png",
            "showMsg": {
                "msg": "",
                "isShow": false
            }
        },
        "rc": 0
    }
## 19. 获取存档加密用的key和规则
### 正常返回条件
    1. 必须登录，且token有效
    2. caller中必须有imei
### url: /api/get/backup_key
### 参数
通用参数： device 和 caller

其他参数：

    pkgName: "存档的游戏包名"
    verCode: "游戏版本号"
    
### 返回值

    {
        "msg": "成功",
        "data": {
            "imei": "352304060320363",
            "showMsg": {
                "msg": "",
                "isShow": false
            },
            "backUpRule": {},
            "key": "996072aa2137eb5165c8e9f46d2d4440",
            "uid": 1000278
        },
        "rc": 0
    }


## 20. 更新用户安装应用包名
### url: /api/put/installed_pkg
### 参数：
    "pkgs":
        [
            {
                "pkgName": "com.invictus.impossiball",
                "verCode": "132",
            },
            {
                "pkgName": "com.moe.niceshot.moli"
                "verCode": "132",
            },
            {
                "pkgName": "com.yileweb.cytl.sf"
                "verCode": "132",
            }
        ]
    "pkgsInBox":
        [
            {
                "pkgName": "com.invictus.impossiball",
                "verCode": "132",
            },
            {
                "pkgName": "com.moe.niceshot.moli"
                "verCode": "132",
            },
            {
                "pkgName": "com.yileweb.cytl.sf"
                "verCode": "132",
            }
        ]
### 返回值

    {
        "msg": "成功",
        "data": {
            "showMsg": {
                "msg": "",
                "isShow": false
            },
        },
        "rc": 0
    }


## 21. 动态打包逻辑
    **动态打包用于**:
        1.邀请免填邀请码
        2.通过推广的游戏下载GG客户端，则该客户端打开后自动下载该游戏

    **邀请**:
        1.邀请会在META-INF/ 目录下生成i_{加密后的邀请码} 文件

    **游戏推广**:
        1. 会在META-INF/ 目录下生成gameId_000feac3 文件

    客户端首次打开请判断是否存在以上2个文件进行相应操作


## 22. 获取下载信息接口
### url: /api/get/download_info
### 参数：
    device + caller
### 返回值:
    {
        "msg": "成功",
        "data": {
            "showMsg": {
                "msg": "",
                "isShow": false
            },
            "download_url": "http://cdn2.yyhudong.com/ggclient4.0/4.1.5767/com.iplay.assistant.apk"
        },
        "rc": 0
    }
    没有取到url
    {
        "msg": "没有对应的下载地址",
        "data": {
            "showMsg": {
                "msg": "",
                "isShow": false
            },
            "download_url": ""
        },
        "rc": 4002
    }

## 23. 获取游戏下载信息
### url: /api/get/game_download_info
### 参数：
    game_id: "2bdec7e8"
### 返回值：
    {
        "msg": "成功",
        "data": {
            "dependCheck": false,
            "gameId": "2bdec7e8",
            "pkgName": "com.netease.onmyoji",
            "minSdk": 9,
            "supportPlugin": false,
            "gameName": "阴阳师",
            "verCode": 5,
            "supportDuplicate": true,
            "iconUrl": "http://img.wdjimg.com/mms/icon/v1/2/33/b9cc8a6cf4f9775e7d882ef9e6b33332_256_256.png",
            "supportBox": false,
            "supportCenterPlugin": false,
            "verName": "1.0.6",
            "dependGooglePlay": false,
            "supportAccount": false,
            "downloadType": 1,
            "gpuRenderers": [
                1
            ],
            "ggVerCode": 1473384397,
            "showMsg": {
                "msg": "",
                "isShow": false
            },
            "downloadLinks": {
                "webview": [],
                "client": [],
                "direct": [
                    {
                        "url": "https://ggapi.ggzhushou.cn:444/view/dl?id=636e83da84708c6d8018ececbb8ef767",
                        "name": "直接下载",
                        "desc": ""
                    }
                ]
            },
            "size": 539910965
        },
        "rc": 0
    }

## 24. 获取悬浮窗信息接口
### url: /view/page?id=9
### 返回值：
    {
        "msg": "成功",
        "data": {
            "showMsg": {
                "msg": "",
                "isShow": false
            },
            "page": {
                "pageId": 9,
                "shareData": {
                    "url": "https://bbs.ggzhushou.cn/user/invite_page?invite_code=GGINV_1000278",
                    "message": "",
                    "title": "",
                    "icon": ""
                },
                "shareUrl": "https://bbs.ggzhushou.cn/user/invite_page?invite_code=GGINV_1000278",
                "pageTitle": "",
                "cards": [
                    {
                        "leftItem": {
                            "action": {
                                "actionType": 111,
                                "actionTarget": "/download_score/game_list"
                            },
                            "pic": "http://7xleaj.com1.z0.glb.clouddn.com/xxuanfu.png",
                            "showIndexs": [
                                0,
                                1,
                                2,
                                3,
                                4
                            ]
                        },
                        "cardId": 24,
                        "rightItem": {
                            "action": {
                                "actionType": 111,
                                "actionTarget": "/download_score/game_list"
                            },
                            "pic": "http://7xleam.com1.z0.glb.clouddn.com/kaixuefuli.png",
                            "showIndexs": [
                                0,
                                1,
                                2,
                                3,
                                4
                            ]
                        },
                        "isShowFoot": false
                    }
                ],
                "adCount": {
                    "GDTTotalCount": 0,
                    "GDTId": "3024324"
                }
            }
        },
        "rc": 0
    }
## 25.获取可以开小号游戏列表
### 25.1 /api/get/duplicate-game-list

接口用于获取手机安装的app中可以支持开小号的游戏列表

* url
    
    /api/get/duplicate-game-list

* method
    
    POST
    
* input 参数

    |   参数名   |类型|  是否必须 | 说明  |
    | :--------  |--:--| --------:| :--:    |
    | device     |json|  Y       |  常规参数             |
    | caller     |json|  Y       |  常规参数     |
    | apks       |list|  Y       |  安装的游戏apk信息的列表|
    
    apk 信息
    
    |   参数名   |类型|  是否必须 | 说明  |
    | :--------  |--:--| --------:| :--:    |
    | pkgName     |string|  Y       |  安装apk包名|
    | verCode     |int|  Y       |  安装的apk版本 |

* input 示例

        {
            "caller": {
                "id": "ggclient",
                "channel": "B2",
                "verCode": 441,
                "pkgName": "com.iplay.assistant",
                "token": "username||tt003&state||0&character||0&telephone||2016288277&adfree||1&secret||2|1:0|10:1485241972|6:secret|188:eyJ1aWQiOiAxMzgzNTA0LCAiY3VycmVudEFuZHJvaWRJZCI6ICI3NDRhYzk1Nzk2ZGJhZjFkIiwgImNyZWF0ZV90aW1lIjogMTQ4NTI0MTk3MiwgImFkZnJlZV9leHBpcmUiOiAwLCAibG9naW5faXAiOiAibG9naW5faXAiLCAiZ2FtZV9pZCI6IDB9|ca6778048bc3f3cf9c2534dff554d85f0956bf15ba0f791af00c657c329c4cc7&expire||7&create_time||1485241972&type||0"
            },
            "device": {
                "product": "t03gxx",
                "gpuVendor": "Mali-400 MP",
                "fingerprint":
                "samsung/t03gxx/t03g:4.4.2/KOT49H/N7100XXUFNH2:user/release-keys",
                "heightPixels": 1280,
                "model": "GT-N7100",
                "density": 320,
                "widthPixels": 720,
                "vendor": "samsung",
                "imei": "868746027415404",
                "mac": "78:4B:87:6B:25:6A",
                "sdk": 19
            },
            "apks":[
                    {"pkgName": "com.dreamsky.DiabloLOL", "verCode": 310},
                    {"pkgName": "com.sqage.Ogre.OgreInstance.uc", "verCode": 24}
                    {"pkgName": "com.windplay.mobius.dnv.uc", "verCode": 7},
            ]
        }

* output 参数说明
    
    游戏信息
    
    |   参数名   |类型| 说明  |
    | :--------  |--:--| --:    |
    |pkgName|string|apk包名|
    |gameId|string| 游戏id |
    |iconUrl|string| 游戏图标链接 |
    |gameName|string| 游戏显示名 |
    |gameId|string| 游戏id |
    |dependGooglePlay|  bool | 需要使用内置的免google play的插件|
    |dependCheck|   bool | 需要依赖内置的免验证插件|
    |supportBox|    bool | 是否支持直接安装到魔盒|
    |supportAccount|    bool | 是否支持账号中心插件登录|
    |supportCenterPlugin|   bool | 是否支持账号中心插件|
    |supportBackup| bool | 是否支持备份|
    |supportDuplicate|  bool | 是否支持双开|
    |signatureSf|   string | sf文件的md5|
    |signatureMf|   string | mf文件的md5|
    |checkParams|   list | 需要校验的字段 从["signatureSf", "signatureMf"]中取0-2 个|
    
    

* output 示例

    
        {
            "msg": "ok",
            "data": {
                "boxGames": [
                    {
                        "pkgName": "com.dreamsky.DiabloLOL",
                        "gameId": ,
                        "iconUrl": 
                        "gameName": 
                        "dependGooglePlay": true, # 需要使用内置的免google play的插件
                        "dependCheck": true, # 需要依赖内置的免验证插件
                        "supportBox": false # 是否支持直接安装到魔盒
                        "supportAccount": false # 是否支持账号中心插件登录
                        "supportCenterPlugin"   # 是否支持账号中心插件
                        "supportBackup" # 是否支持备份
                        "supportDuplicate" # 是否支持双开
                        "signatureSf":"sddddddddddasdffdsafdsfasf", # sf文件的md5
                        "signatureMf":"sddddddddddasdffdsafdsfasf", # mf文件的md5
                        "checkParams": ["signatureSf", "signatureMf"], # 需要校验的字段
                    }
                ]
            }，
           "rc": 0
        }


## 26.获取游戏列表
### 26.1 /api/get/game-list

接口用于获取游戏列表，有魔盒最新游戏列表，魔盒最热游戏列表

* url

    /api/get/game-list

* method

    POST
    
* input 参数

    |   参数名   |类型|  是否必须 | 说明  |
    | :--------  |--:--| --------:| :--:    |
    | device     |json|  Y       |  常规参数             |
    | caller     |json|  Y       |  常规参数     |
    | gameType       |string|  Y       |  游戏类型: 现在支持 boxGame|
    | orderType      |string|  Y       |  排序方式: 现在支持 latest, hottest|
    | pageNum        |int|  N       |  页码，表示第n页 默认为1|
    | pageSize       |int|  N       |  页面大小，表示一页取多少条数据, 默认20|


* input 示例

        {
            "caller": {
                "id": "ggclient",
                "channel": "B2",
                "verCode": 441,
                "pkgName": "com.iplay.assistant",
                "token": "username||tt003&state||0&character||0&telephone||2016288277&adfree||1&secret||2|1:0|10:1485241972|6:secret|188:eyJ1aWQiOiAxMzgzNTA0LCAiY3VycmVudEFuZHJvaWRJZCI6ICI3NDRhYzk1Nzk2ZGJhZjFkIiwgImNyZWF0ZV90aW1lIjogMTQ4NTI0MTk3MiwgImFkZnJlZV9leHBpcmUiOiAwLCAibG9naW5faXAiOiAibG9naW5faXAiLCAiZ2FtZV9pZCI6IDB9|ca6778048bc3f3cf9c2534dff554d85f0956bf15ba0f791af00c657c329c4cc7&expire||7&create_time||1485241972&type||0"
            },
            "device": {
                "product": "t03gxx",
                "gpuVendor": "Mali-400 MP",
                "fingerprint":
                "samsung/t03gxx/t03g:4.4.2/KOT49H/N7100XXUFNH2:user/release-keys",
                "heightPixels": 1280,
                "model": "GT-N7100",
                "density": 320,
                "widthPixels": 720,
                "vendor": "samsung",
                "imei": "868746027415404",
                "mac": "78:4B:87:6B:25:6A",
                "sdk": 19
            },
            "gameType": "boxGame",
            "orderType": "latest",
            "pageNum": 1,
            "pageSize": 20
        }

* output 参数说明
    
    data信息
    
    |   参数名   |类型| 说明  |
    | :--------  |--:--| --:    |
    |games |list|游戏信息列表|
    |hasMore|bool| 是否含有下一页 |

    游戏信息

    |   参数名   |类型| 说明  |
    | :--------  |--:--| --:    |
    |gameId |string|游戏id|
    |subTitle|string| 副标题,包含游戏大小和下载数 |
    |description|string| 游戏简短描述 |
    |actionUrl|string| 点击跳转页面url，这里是游戏详情页 |
    |title|string| 游戏显示名 |
    |iconUrl|string| 游戏图标链接|
    |superscriptId|int| 角标，0,无 1首发，2推荐，3热门，4送积分，5有mod，6免googleplay|
    |downloadInfo|json| 下载需要信息，具体见通用定义|

* output 示例

    
        {
            "msg": "ok",
            "data": {
                "games": [
                    {
                        "gameId": "3f0647e5",
                        "subTitle": "43.1MB | 下载人数 1,000,000+",
                        "description": "史上最酷炫的赛车游戏",
                        "actionUrl": "/view/page?id=2000&pa=3f0647e5",
                        "title": "狂野飙车8",
                        "iconUrl": "https://cdn2.yyhudong.com/ggziyuan/20161223/狂野飙车8.png",
                        "superscriptId": 6,
                        "downloadInfo": {
                            "dependCheck": true,
                            "gameId": "3f0647e5",
                            "minSdk": 9,
                            "supportPlugin": true,
                            "gameName": "狂野飙车8",
                            "verCode": 28033,
                            "supportBox": true,
                            "downloadUrl": "https://ggapi.ggzhushou.cn/view/dl?id=82699740ceb42105a851bbe622fe5f24",
                            "iconUrl": "https://cdn2.yyhudong.com/ggziyuan/20161223/狂野飙车8.png",
                            "supportBackup": false,
                            "supportCenterPlugin": true,
                            "verName": "2.8.0n",
                            "dependGooglePlay": true,
                            "supportAccount": true,
                            "pkgName": "com.gameloft.android.ANMP.GloftA8HM",
                            "supportDuplicate": false,
                            "ggVerCode": 1482481482,
                            "gpuRenderers": [
                                1
                            ],
                            "size": 45183446
                        }
                    }
                    .........
                ],
                "hasMore": true
            }，
           "rc": 0
           "showMsg": {
                "msg": "",
                "isShow": false
            }
        }


## 27.获取池子游戏数据 
### 27.1 /api/get/pool-game-cards
接口用于获取换一换中的下一页数据

* url

    /api/get/pool-game-cards

* method
    
    POST
    
* input 参数

    |   参数名   |类型|  是否必须 | 说明  |
    | :--------  |--:--| --------:| :--:    |
    | device     |json|  Y       |  常规参数             |
    | caller     |json|  Y       |  常规参数     |
    | poolId         |int|  Y       |  池子id|
    | pageNum        |int|  N       |  页码，表示第n页 默认为1|
    | pageSize       |int|  N       |  页面大小，表示一页取多少条数据, 默认20|

* input 示例

        {
            "caller": {
                "id": "ggclient",
                "channel": "B2",
                "verCode": 441,
                "pkgName": "com.iplay.assistant",
                "token": ""
            },
            "device": {
                "product": "t03gxx",
                "gpuVendor": "Mali-400 MP",
                "fingerprint":
                "samsung/t03gxx/t03g:4.4.2/KOT49H/N7100XXUFNH2:user/release-keys",
                "heightPixels": 1280,
                "model": "GT-N7100",
                "density": 320,
                "widthPixels": 720,
                "vendor": "samsung",
                "imei": "868746027415404",
                "mac": "78:4B:87:6B:25:6A",
                "sdk": 19
            },
            "poolId": 1,
            "pageNum": 1,
            "pageSize": 20
        }

* output 参数说明
    
    data信息
    
    |   参数名   |类型| 说明  |
    | :--------  |--:--| --:-    |
    |games |list|游戏信息列表|
    |hasMore|bool| 是否含有下一页 |
    |loadMoreUrl|string|下一页url|

    游戏信息

    |   参数名   |类型| 说明  |
    | :--------  |--:--| --:    |
    |gameId |string|游戏id|
    |subTitle|string| 副标题,包含游戏大小和下载数 |
    |description|string| 游戏简短描述 |
    |action|json| 跳转信息 见通用定义|
    |title|string| 游戏显示名 |
    |iconUrl|string| 游戏图标链接|
    |subheading|string|游戏大小|
    |superscriptId|int| 角标，0,无 1首发，2推荐，3热门，4送积分，5有mod，6免googleplay|
    |downloadInfo|json| 下载需要信息，具体见通用定义|

* output 示例


        {
            "msg": "成功",
            "data": {
                "hasMore": true,
                "games": [
                    {
                        "subTitle": "175.5MB | 下载人数 少于10,000",
                        "description": "日本国民级RPG最终幻想系列迎来了第3部",
                        "title": "最终幻想3",
                        "subheading": "175.5MB",
                        "iconUrl": "http://7xleaj.com1.z0.glb.clouddn.com/-83254fdec06a43e9",
                        "fileSize": "175.5MB",
                        "superscriptId": 6,
                        "downloadInfo": {
                            "supportPlugin": true,
                            "gameName": "最终幻想3",
                            "pluginCount": 0,
                            "iconUrl": "http://7xleaj.com1.z0.glb.clouddn.com/-83254fdec06a43e9",
                            "supportBox": false,
                            "supportAccount": true,
                            "pkgName": "com.square_enix.android_googleplay.FFIII_GP",
                            "ggVerCode": 1474522568,
                            "size": 183989936,
                            "dependGooglePlay": true,
                            "verCode": 12100,
                            "dependCheck": true,
                            "gpuRenderers": [
                                1
                            ],
                            "downloadLinks": {
                                "webview": [],
                                "client": [],
                                "direct": [
                                    {
                                        "url": "https://ggapi.ggzhushou.cn:442/view/dl?id=868262c7434a8ca0effbee5863476565",
                                        "name": "直接下载",
                                        "desc": ""
                                    }
                                ]
                            },
                            "gameId": "e4426044",
                            "downloadType": 1,
                            "supportCenterPlugin": true,
                            "minSdk": 8,
                            "downloadUrl": "https://ggapi.ggzhushou.cn:442/view/dl?id=868262c7434a8ca0effbee5863476565",
                            "supportBackup": false,
                            "verName": "1.2.1",
                            "supportDuplicate": false
                        },
                        "action": {
                            "actionType": 1,
                            "actionData": {
                                "param": "e4426044"
                            },
                            "actionTarget": "/view/page?id=2000&pa=e4426044",
                            "param": "e4426044"
                        }
                    },
                    ...
                ],
                "showMsg": {
                    "msg": "",
                    "isShow": false
                },
                "loadMoreUrl": "/api/get/pool-game-cards?poolId=1&pageNum=2&pageSize=4"
            },
            "rc": 0
        }



## 28.获取来源相关游戏验证信息
### 28.1 /api/get/source-game-signs
接口用于获取来源相关游戏验证信息, 例如用户是泰拉瑞亚盒子导过来的则下发泰拉瑞盒子 的包名以及各版本验证信息

* url

    /api/get/source-game-signs

* method
    
    POST
    
* input 参数

    |   参数名   |类型|  是否必须 | 说明  |
    | :--------  |--:--| --------:| :--:    |
    | device     |json|  Y       |  常规参数             |
    | caller     |json|  Y       |  常规参数     |

* input 示例

        {
            "caller": {
                "id": "ggclient",
                "channel": "B2",
                "verCode": 441,
                "pkgName": "com.iplay.assistant",
                "token": ""
            },
            "device": {
                "product": "t03gxx",
                "gpuVendor": "Mali-400 MP",
                "fingerprint":
                "samsung/t03gxx/t03g:4.4.2/KOT49H/N7100XXUFNH2:user/release-keys",
                "heightPixels": 1280,
                "model": "GT-N7100",
                "density": 320,
                "widthPixels": 720,
                "vendor": "samsung",
                "imei": "868746027415404",
                "mac": "78:4B:87:6B:25:6A",
                "sdk": 19
            },
        }

* output 参数说明
    
    data信息
    
    |   参数名   |类型| 说明  |
    | :--------  |--:--| --:-    |
    |signs |list|验证信息列表|

    游戏信息

    |   参数名   |类型| 说明  |
    | :--------  |--:--| --:    |
    |pkgName |string|包名|
    |verCode |string|版本号|
    |signatureSf|string| sf 验证信息 大写 |
    |signatureMf|string| mf 验证信息 大写 |
    |signatureRSA|string| rsa 验证信息 大写 |

* output 示例

        {
            "msg": "成功",
            "data": {
                "signs": [
                    {
                        "signatureMf": "32352C0E897115D83FE8314EAED55652",
                        "pkgName": "com.and.games505.TerrariaPaid",
                        "verCode": 12785,
                        "signatureRSA": "041EE1EDF85B4CEEEE4FCEFDACEE6685",
                        "signatureSf": "C5D9A373CF021D346A2764088A6A3246"
                    },
                    {
                        "signatureMf": "32352C0E897115D83FE8314EAED55652",
                        "pkgName": "com.and.games505.TerrariaPaid",
                        "verCode": 12784,
                        "signatureRSA": "041EE1EDF85B4CEEEE4FCEFDACEE6685",
                        "signatureSf": "C5D9A373CF021D346A2764088A6A3246"
                    }
                ],
            },
            "rc": 0
        }


## 29.获取悬浮窗数据
### 29.1 /api/get/float
接口用于获取各个页面的悬浮窗图片以及action

* url

    /api/get/float

* method
    
    POST
    
* input 参数

    |   参数名   |类型|  是否必须 | 说明  |
    | :--------  |--:--| --------:| :--:    |
    | device     |json|  Y       |  常规参数             |
    | caller     |json|  Y       |  常规参数     |

* input 示例

        {
            "caller": {
                "id": "ggclient",
                "channel": "B2",
                "verCode": 441,
                "pkgName": "com.iplay.assistant",
                "token": ""
            },
            "device": {
                "product": "t03gxx",
                "gpuVendor": "Mali-400 MP",
                "fingerprint":
                "samsung/t03gxx/t03g:4.4.2/KOT49H/N7100XXUFNH2:user/release-keys",
                "heightPixels": 1280,
                "model": "GT-N7100",
                "density": 320,
                "widthPixels": 720,
                "vendor": "samsung",
                "imei": "868746027415404",
                "mac": "78:4B:87:6B:25:6A",
                "sdk": 19
            },
        }

* output 字段说明
    
    item

    |   字段名   |类型| 说明  |
    | :--------  |--:--| --:    |
    |action |json|跳转数据 具体见通用定义|
    |pic |string|图片链接|
    |showIndex|int| 展示位置 0-5|
    |isShow|bool|是否展示|


* output 示例


        {
            "msg": "成功",
            "data": {
                "items": [
                    {
                        "action": {
                            "actionType": 2,
                            "actionData": {
                                "param": "'http://ggzhushou.cn/game/sniper'"
                            },
                            "actionTarget": "'http://ggzhushou.cn/game/sniper'"
                        },
                        "pic": "http://7xleaj.com1.z0.glb.clouddn.com/zhuanpan.png",
                        "showIndex": 0,
                        "isShow": true
                    },
                    {
                        "action": {
                            "actionType": 2,
                            "actionData": {
                                "param": "'http://ggzhushou.cn/game/sniper'"
                            },
                            "actionTarget": "'http://ggzhushou.cn/game/sniper'"
                        },
                        "pic": "http://7xleaj.com1.z0.glb.clouddn.com/zhuanpan.png",
                        "showIndex": 1,
                        "isShow": true
                    },
                    {
                        "action": {
                            "actionType": 2,
                            "actionData": {
                                "param": "'http://ggzhushou.cn/game/sniper'"
                            },
                            "actionTarget": "'http://ggzhushou.cn/game/sniper'"
                        },
                        "pic": "http://7xleaj.com1.z0.glb.clouddn.com/zhuanpan.png",
                        "showIndex": 2,
                        "isShow": true
                    },
                    {
                        "action": {
                            "actionType": 2,
                            "actionData": {
                                "param": "'http://ggzhushou.cn/game/sniper'"
                            },
                            "actionTarget": "'http://ggzhushou.cn/game/sniper'"
                        },
                        "pic": "http://7xleaj.com1.z0.glb.clouddn.com/zhuanpan.png",
                        "showIndex": 3,
                        "isShow": true
                    },
                    {
                        "action": {
                            "actionType": 2,
                            "actionData": {
                                "param": "'http://ggzhushou.cn/game/sniper'"
                            },
                            "actionTarget": "'http://ggzhushou.cn/game/sniper'"
                        },
                        "pic": "http://7xleaj.com1.z0.glb.clouddn.com/zhuanpan.png",
                        "showIndex": 4,
                        "isShow": true
                    }
                ],
                "showMsg": {
                    "msg": "",
                    "isShow": false
                }
            },
            "rc": 0
        } 
