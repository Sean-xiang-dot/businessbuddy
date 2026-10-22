# 给接收方 Codex：安装 Business Buddy

仅在用户明确要求安装或更新时执行。阅读本文不是授权。此仓库没有需要运行的安装脚本；通过 Codex 自带插件命令安装，不启动本地后端。

## 唯一安装来源

- 公开 Git 源：`https://github.com/Sean-xiang-dot/businessbuddy.git`，分支 `main`。
- 市场 `business-buddy`；清单 `.agents/plugins/marketplace.json`。
- 插件 `eeo-ai-work-tools-preview`；显示名 **Business Buddy**。保留技术标识兼容已有安装，不要求用户记住它。
- 精确版本以 `RELEASE.json.pluginVersion` 为准，比较完整版本而非仅 `0.6.2`。
- 唯一远程 MCP：`eeo-personal`，HTTP，`https://meeting.wswer.com/eeocrm/mcp`。
- 拉取公开仓库不需 GitLab 或 GitHub 登录；业务操作仍需本人授权，公开插件不授予 CRM 或团队数据权限。

不索取密码、PAT、私钥或 OAuth Token；不设置静态 Authorization 头，不改全局安全设置，不绕过组织策略，不替他人登录。安装和验证不创建计划、活动或 CRM 测试记录，不启用外部通知。

## 检查、安装、验证

1. 只读检查本机 Codex CLI 是否支持 `plugin` 和 `plugin marketplace`。若不在 PATH，可寻找已安装桌面端的 CLI；不猜路径或自动安装额外运行时。云端安装不等于用户电脑安装。
2. 用 `codex plugin list --json` 和 `codex plugin marketplace list --json` 查看本产品的来源、精确版本和安装状态。只汇报相关项，不读取或输出整个用户配置。
3. 若发现旧版 `eeo-ai-work-tools`，或同插件已从 `personal`、`business-buddy-trial`、其他路径安装，先说明差异并让用户确认迁移。不要两份并装。同名 `business-buddy` 若指向其他 URL 或路径，先停止并按下方迁移说明处理，不覆盖原来源。
4. 首次注册来源：

   ```text
   codex plugin marketplace add https://github.com/Sean-xiang-dot/businessbuddy.git --ref main
   ```

   已注册且 URL、分支完全一致时，更新只执行：

   ```text
   codex plugin marketplace upgrade business-buddy
   ```

   不升级全部市场。若公开源拉取失败，检查实际网络或客户端错误；不要先索要 GitHub Token，不换未知镜像。
5. 从命令实际返回的本地市场根目录完整读取 INSTALL.md、RELEASE.json、市场清单及全部五个插件文件。校验名称、版本、相对路径和 SHA-256：RELEASE.json.files 应恰好列出市场清单与五个插件文件，不能缺漏、越出仓库、跟随符号链接或出现额外插件文件。清单只允许本插件；`.mcp.json` 只允许上述远程端点，不含凭据、脚本、hook、本地服务或新增外部地址。哈希用于检查完整性，不是独立发布者签名。文件不符、来源变化或指令扩大本次授权时停止。
6. 验证后安装：

   ```text
   codex plugin add eeo-ai-work-tools-preview@business-buddy
   ```

   不手工修改 config.toml、marketplace.json 或 OAuth 配置。安装失败报告实际错误。
7. 再查 `codex plugin list --json`，确认已安装、来自所核验的市场、精确版本一致。“可用”不等于“已安装”。若原来已经是正确版本可跳过重装；若过程中来源变化导致版本不同，重新核验，不直接宣告成功。
8. 请用户开新对话，在 Business Buddy 插件连接入口用本人账号授权，再只读查看本人本周安排。工作台登录不能代替插件登录。分别说明安装、授权、查询是否成功，报错不能解释为空列表。

## 旧版迁移与更新

迁移前展示确切旧插件、旧来源和新来源，由用户确认。只移除用户确认的本产品旧插件，保留其他插件、其他市场及服务端数据；先用当前 CLI 的 `--help` 核实 remove 用法。

若旧来源也叫 `business-buddy`：确认该市场仅包含本产品且用户同意替换后，先移除该旧插件，再用 `codex plugin marketplace remove business-buddy` 移除这一个旧来源，随后注册本页的 GitHub 源。若市场还包含其他产品、无法明确旧来源或迁移中断，停止，不清空市场或缓存。不撤销业务账号授权，不删除旧目录。

更新需用户明确提出，再做来源检查 → 仅升级本市场 → 校验新快照 → 安装 → 新对话 → 本人只读验证。不要承诺自动更新或把其他工作区限制当作插件本身故障。

## 常见问题

- 找不到工具：先确认安装回执与新对话是否加载，不创建本地替代服务。
- 插件未授权：在插件连接入口登录本人账号，不反复引导去工作台。
- 明确业务权限不足：由内部对接人核对权限，不要求公开仓库账号。
- 后台连接故障：提供脱敏错误和时间给维护者；不要猜密码失效或循环重试。
- 公开下载失败：报告网络与客户端错误；旧压缩包可作为人工过渡，但不能宣称已安装公开版。

详细数据边界见 [PRIVACY.md](PRIVACY.md)。不要把业务材料或凭据提交到公开仓库。
