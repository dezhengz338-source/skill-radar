# Skill Radar

互联网 Agent Skill 情报雷达：每天发现、核验、评分并解释值得关注的 Skills。

## 功能

- 扫描 OpenAI、Anthropic、NVIDIA、Vercel、Hermes 等公开 Skill 仓库。
- 按任务价值、需求、效率杠杆、质量、增长、维护和差异化评分。
- 同时评估权限、命令执行、联网、凭据、混淆和来源风险。
- 提供中文用途、适合人群、工作方式和使用时机说明。
- 保存每日快照，比较新发现、升温和降温项目。
- 本地模式支持确认后安装到 Codex 或 Hermes。

## 在线网站

GitHub Pages 部署后，网站每天读取 `assets/dashboard/data/current.json`。在线版是只读模式，不会写入访问者电脑。

启用方法：

1. 在仓库的 **Settings → Pages** 中将 Source 设为 **GitHub Actions**。
2. 运行一次 **Deploy Skill Radar to Pages** 工作流。
3. 网站将发布到 `https://<你的用户名>.github.io/skill-radar/`。

## 本地联网模式

Windows 用户双击：

```text
assets/dashboard/启动联网版.cmd
```

然后访问：

```text
http://127.0.0.1:8765
```

本地模式可联网刷新、创建 Windows 每日任务，并在风险确认后安装 Skill。

## 每日更新

`.github/workflows/daily-refresh.yml` 每天 00:00 UTC（北京时间 08:00）运行扫描器，并更新当前快照。也可以在 Actions 页面手动运行。

如遇 GitHub API 限流，可在仓库 Secrets 中添加具有最小权限的 `RADAR_GITHUB_TOKEN`。

## 安全边界

- 公网页面不暴露本地安装或系统任务接口。
- 本地服务只监听 `127.0.0.1`。
- 隔离候选禁止一键安装。
- 安装过程拒绝覆盖、路径穿越、符号链接和超大文件。
- Skill 只下载到临时目录；完整成功后才移动到目标目录。

## Skill 使用

仓库根目录本身也是一个 Codex Skill。安装后可这样调用：

```text
使用 $skill-radar 扫描过去 30 天值得关注的 Skills，并输出中文价值雷达。
```

## 许可证

MIT
