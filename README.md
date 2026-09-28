# 阿里云开发者社区签到确认适配器

此仓库只发布独立适配器 `checkin.py`，不复制上游代码。它调用 [WillpowerJin/ql-scripts](https://github.com/WillpowerJin/ql-scripts) 的 `aliyun_dev/daily.py`，将执行范围限制为社区签到和待领积分，并在提交后轮询签到状态最多 60 秒；状态和最终积分未确认时返回失败。

## 安装

先按上游仓库说明安装依赖和 `aliyun_dev/daily.py`。在青龙环境中设置原脚本需要的账号变量（例如 `ALIYUN_COOKIE`），再设置 `ALIYUN_DAILY_PATH` 为上游 `daily.py` 的绝对路径：

```bash
export ALIYUN_DAILY_PATH=/path/to/ql-scripts/aliyun_dev/daily.py
python3 checkin.py
```

本适配器不包含 Cookie、账号、上游脚本或定时配置。CookieCloud 仅负责传递已有登录态，不能保证服务端持续接受它。日志会包含积分变动，不要公开真实运行日志。

## 边界

适配器对上游类的方法做运行时覆盖，因此需检查上游更新是否仍提供 `AliyunDevClient`、`TASK_GROUPS` 和这些方法。重复请求只用于读取状态；超时后的签到写入不会自动重放。当前版本需要上游脚本才能执行，`python3 -m py_compile checkin.py` 仅验证语法。
