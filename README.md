# 搜书吧自动签到

使用 GitHub Actions 每天北京时间 05:17 自动执行论坛空间动态任务。实际启动可能因 GitHub 排队而延后，电脑和手机无需保持开机。

## 已配置的任务

- **Run soushuba qiandao**：登录后记录初始余额，按两分钟间隔发布五条空间动态，再记录最终余额和差额。
- **Keep scheduled tasks active**：每月两次提交一条不修改文件的维护记录，保持仓库活动，降低公开仓库定时任务因长期不活跃而停用的风险。此任务不读取论坛账号密码。

## 加密配置

论坛账号只保存在仓库 Settings → Secrets and variables → Actions 的 Repository secrets 中，代码不含账号密码。

| Secret | 说明 |
| --- | --- |
| SOUSHUBA_HOSTNAME | 论坛地址发布页域名，当前配置为 `www.soushu2035.com` |
| SOUSHUBA_USERNAME | 论坛用户名 |
| SOUSHUBA_PASSWORD | 论坛密码 |

## 运行与排错

在 Actions 中查看 **Run soushuba qiandao**。绿色只表示这次程序完成；应同时看日志中的发布成功数量、初始余额、最终余额和余额差额。论坛积分规则或当日上限可能影响实际收益。

程序确认登录认证 cookie 后才发布。发布不完整会标记失败；只自动重试请求尚未发送时的连接失败，不重试已发送请求的读取失败，也不自动重跑整个任务，避免重复发布。

失败时先检查已成功的条数。手动 Run workflow 时，可在 completed_posts 中选择当天已经确认成功的数量，任务仅补发剩余条数。每天的定时运行默认从 0 开始。

## 登录地址限制

账号密码仅允许提交到已核实的 `nyt6y.bew2459m.com`，强制 HTTPS 证书校验并禁止携带会话的请求自动重定向。发布页跳转到其他域名时，程序会停止而不是向新站发送账号密码。

如果论坛正式更换地址，需要先核实新地址，再更新 `soushuba.py` 中的 `TRUSTED_FORUM_HOST`。不要仅为通过报错关闭证书校验或移除地址限制。

如需暂停，在 Actions 中选择对应任务，然后通过菜单选择 Disable workflow。维护任务和签到任务可分别暂停。

本仓库基于 [orivee/qiandao](https://github.com/orivee/qiandao)。
