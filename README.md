# qtt# 趣头条

> 代码已同时兼容 Surge & QuanX, 使用同一份签到脚本即可

> QuanX 需要: v1.0.6-build195 及以后版本 (TestFlight)

> 感谢 [@GideonSenku](https://github.com/GideonSenku) Commit

> 2020.04.07 添加签到脚本

> 2020.04.08 添加视频广告奖励、添加每时段签到奖励

> 2020.04.09 添加幸运转盘抽奖

> 2020.04.13 简化Cookie获取方式

> 2020.04.14 添加阅读篇数,添加首页金币奖励,优化通知

> 2020.04.16 阅读篇数奖励自动获取

> 2020.04.19 添加睡觉领金币

> 2020.04.22 添加幸运转盘额外奖励

## 配置 (QuanX)

```properties

https://raw.githubusercontent.com/sss1016/qtt/main/qtt.js

[MITM]
api.1sapp.com

[rewrite_local]

# [商店版] QuanX v1.0.6-build194 及更早版本
^https:\/\/api\.1sapp\.com\/sign\/info? url script-request-header https://raw.githubusercontent.com/sss1016/qtt/main/qtt.cookie.js
^https:\/\/api\.1sapp\.com\/content\/readV2? url script-request-header https://raw.githubusercontent.com/sss1016/qtt/main/qtt.cookie.js
^https:\/\/api\.1sapp\.com\/x\/feed\/getReward? url script-request-header https://raw.githubusercontent.com/sss1016/qtt/main/qtt.cookie.js
# [TestFlight] QuanX v1.0.6-build195 及以后版本
^https:\/\/api\.1sapp\.com\/sign\/info? url script-request-header https://raw.githubusercontent.com/sss1016/qtt/main/qtt.cookie.js
^https:\/\/api\.1sapp\.com\/content\/readV2? url script-request-header https://raw.githubusercontent.com/sss1016/qtt/main/qtt.js
^https:\/\/api\.1sapp\.com\/x\/feed\/getReward? url script-request-header https://raw.githubusercontent.com/sss1016/qtt/main/qtt.js

[task_local]
1 0 * * * qtt.js
```

## 配置 (Surge)

```properties
[MITM]
api.1sapp.com

[Script]
http-request ^https:\/\/api\.1sapp\.com\/sign\/info? script-path=https://raw.githubusercontent.com/chavyleung/scripts/master/qtt/qtt.cookie.js
http-request ^https:\/\/api\.1sapp\.com\/content\/readV2? script-path=https://raw.githubusercontent.com/chavyleung/scripts/master/qtt/qtt.cookie.js
http-request ^https:\/\/api\.1sapp\.com\/x\/feed\/getReward? script-path=https://raw.githubusercontent.com/chavyleung/scripts/master/qtt/qtt.cookie.js

cron "10 0 0 * * *" script-path=https://raw.githubusercontent.com/chavyleung/scripts/master/qtt/qtt.js
```



## 说明

1. 先把`api.1sapp.com`加到`[MITM]`
2. 再配置重写规则:
   - Surge: 把两条远程脚本放到`[Script]`
   - QuanX: 把`qtt.cookie.js`和`qtt.js`传到`On My iPhone - Quantumult X - Scripts` (传到 iCloud 相同目录也可, 注意要打开 quanx 的 iCloud 开关)
3. 打开 APP 进入签到:  `右上角` > `签到`
4. 系统提示: `获取Cookie: 成功`
5. 把获取 Cookie 的脚本注释掉
6. 运行一次脚本, 如果提示重复签到, 那就算成功了!
7. 建议将`task`执行次数改成每小时执行防止错过奖励
8. 阅读篇数获取Cookie:`小视频`中播放一段时间视频即可获取,具体的阅读篇数奖励请到应用内手动点击
9. 首页金币奖励:此Cookie在首页的推荐中随机出现,随机获取,并不一定会出现。
10. 其他问题请看日志报错,日志提示权限错误代表cookie失效
11. 幸运转盘达到次数可自己手动去获取累计奖励,有机率每周五脚本能够再帮你获取一回奖励
12. 赞赏:趣头条邀请码`A1040276307`,农妇山泉 -> 有点咸
> 第 1 条脚本是用来获取 cookie 的, 用浏览器访问一次获取 cookie 成功后就可以删掉或注释掉了, 但请确保在`登录成功`后再获取 cookie.

> 第 2 条脚本是签到脚本, 每天`00:00:10`执行一次.

## 常见问题

1. 无法写入 Cookie

   - 检查 Surge 系统通知权限放开了没
   - 如果你用的是 Safari, 请尝试在浏览地址栏`手动输入网址`(不要用复制粘贴)

2. 写入 Cookie 成功, 但签到不成功

   - 看看是不是在登录前就写入 Cookie 了
   - 如果是，请确保在登录成功后，再尝试写入 Cookie

3. 为什么有时成功有时失败

   - 很正常，网络问题，哪怕你是手工签到也可能失败（凌晨签到容易拥堵就容易失败）
   - 暂时不考虑代码级的重试机制，但咱有配置级的（暴力美学）：

   - `Surge`配置:

     ```properties
     # 没有什么是一顿饭解决不了的:
     cron "10 0 0 * * *" script-path=xxx.js # 每天00:00:10执行一次
     # 如果有，那就两顿:
     cron "20 0 0 * * *" script-path=xxx.js # 每天00:00:20执行一次
     # 实在不行，三顿也能接受:
     cron "30 0 0 * * *" script-path=xxx.js # 每天00:00:30执行一次

     # 再粗暴点，直接:
     cron "* */60 * * * *" script-path=xxx.js # 每60分执行一次
     ```

   - `QuanX`配置:

     ```properties
     [task_local]
     1 0 * * * xxx.js # 每天00:01执行一次
     2 0 * * * xxx.js # 每天00:02执行一次
     3 0 * * * xxx.js # 每天00:03执行一次

     */60 * * * * xxx.js # 每60分执行一次
     ```

## 感谢

[@NobyDa](https://github.com/NobyDa)

[@lhie1](https://github.com/lhie1)

[@ConnersHua](https://github.com/ConnersHua)

[@GideonSenku](https://github.com/GideonSenku)
