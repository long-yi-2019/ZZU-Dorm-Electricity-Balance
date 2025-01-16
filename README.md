# ZZU Dorm Electricity Balance

## 项目简介

这是一个宿舍电量监控系统，旨在实时监测宿舍照明和空调设备的电量余额。通过修改并整合 [TorCroft/ZZU-Electricity](https://github.com/TorCroft/ZZU-Electricity) 的监控逻辑，同时将原项目的 [ZZU-API](https://github.com/TorCroft/ZZU-API) 替换为更高效的 [ZZU.Py](https://github.com/Illustar0/ZZU.Py)，实现了宿舍电量余额的实时获取与监控。当电量低于用户设定的阈值时，系统会自动通过 **Server 酱** 和 **Telegram** 发送通知，提醒及时充值，避免晚上宿舍突然断电困扰。
### 核心功能
1. **实时电量监控**：通过 [ZZU.Py](https://github.com/Illustar0/ZZU.Py) 实时获取宿舍电量余额，监控照明和空调设备的用电情况。
2. **智能提醒**：当电量低于设定阈值时，自动通过 Server 酱和 Telegram 发送通知，确保用户及时充值。
3. **历史记录**：记录每月电量使用数据，支持查看和分析历史用电情况，帮助用户更好地管理电量。
4. **多平台通知**：支持 Server 酱和 Telegram 两种通知方式，满足不同用户的需求。

### 技术亮点
- 基于 [ZZU.Py](https://github.com/Illustar0/ZZU.Py) 实现高效、稳定的电量数据获取。
- 结合 [ZZU-Electricity](https://github.com/TorCroft/ZZU-Electricity) 的监控逻辑，实现电量余额的实时监控与提醒。
- 支持Server 酱和 Telegram 等多平台通知，确保用户不会错过重要提醒。

## 功能

- 每隔一段时间获取宿舍照明和空调的电量余额。
- 自动判断电量状态，并根据设定的阈值（默认为5.0）发送警告通知。
- 通过 Server 酱和 Telegram 通知宿舍住户电量状态。
- 提供一个简单的前端页面，显示电量数据和图表。

## 技术栈

- **Python**：主要用于数据采集、处理和发送通知。
- **ZZUPy**：郑州大学移动校园的 Python API 封装。
- **Telegram Bot API**：通过 Telegram 发送通知。
- **Server 酱**：通过 Server 酱发送通知。
- **ECharts**：用于前端展示电量数据图表。
- **GitHub Actions**：用于自动化构建和部署，自动检查更新并部署到 GitHub Pages。

## 配置

1. **Repository Secrets**：需要在GitHub Secrets中添加以下变量：

| 环境变量            | 描述                              |
|---------------------|-----------------------------------|
| `EMAIL`             | GitHub邮箱                         |
| `ACCOUNT`           | 郑州大学移动校园登录账户           |
| `PASSWORD`          | 郑州大学移动校园登录密码           |
| `lt_room`           | 照明电量账户                       |
| `ac_room`           | 空调电量账户                       |
| `TELEGRAM_BOT_TOKEN`| Telegram Bot Token                |
| `TELEGRAM_CHAT_ID`  | Telegram Chat ID                  |
| `SERVERCHAN_KEY`    | Server 酱 API 密钥                |
| `SERVERCHAN_KEY2`   | 多个Server 酱 API 密钥，          |
| `SERVERCHAN_KEY3`   | Server 酱 API 密钥，              |

2. **创建数据存储文件夹**：该项目会将数据保存在 `./page/data` 文件夹下，请确保该文件夹存在。

## 自动化工作流

该项目使用 GitHub Actions 进行自动化管理。工作流触发时会：

- **定时触发**：工作流通过 `cron` 表达式每天在 12:00、15:00、18:00、21:00、24:00  自动运行。
- **手动触发**：也可以通过 GitHub 界面手动触发工作流执行。

### 工作流步骤

1. **Checkout**：通过 `actions/checkout@v4` 拉取项目代码，默认深度为 2，以便获取历史提交记录。
2. **设置 Python 3.12 环境**：使用 `actions/setup-python@v5` 设置 Python 3.12 环境。
3. **安装依赖**：安装项目的 Python 依赖，确保 `requirements.txt` 文件存在时自动安装。
4. **执行 Python 脚本**：运行 `index.py` 和 `markdown.py` 脚本，生成更新并将输出添加到 GitHub Actions 的步骤摘要中。
5. **检查更改**：检查项目代码是否发生了变化。
6. **获取最后一次提交信息**：获取最近的一次提交信息，避免不必要的提交。
7. **提交更改**：如果代码有变化，提交并推送更改。如果提交信息以 "Updated at" 开头，则执行强制推送。
8. **设置 GitHub Pages**：如果有更新，将网站内容上传至 GitHub Pages。
9. **上传构建的页面**：将页面内容打包并上传，以供部署。
10. **部署到 GitHub Pages**：如果构建步骤成功，自动将页面部署到 GitHub Pages，确保每次更新都会自动发布。

## 示例通知

- **电量充足**：  
  🏠宿舍电量通报🏠  
  💡 照明剩余电量：25.0 度（充足）  
  ❄️ 空调剩余电量：50.0 度（充足）

- **电量不足**：  
  ⚠️宿舍电量预警⚠️  
  💡 照明剩余电量：4.5 度（⚠️警告）  
  ❄️ 空调剩余电量：3.0 度（⚠️警告）  
  ⚠️ 电量不足，请尽快充电！

## 常见问题

### 1. 登录失败怎么办？

请确保您提供的账号和密码正确，并且 `ZZUPy` 的 API 接口正常工作。如果仍然无法登录，请检查网络连接或参考 [`ZZUPy` 的文档](https://illustar0.github.io/ZZU.Py/))。

### 2. Telegram 通知无法发送？

请确保您设置了正确的 Telegram Bot Token 和 Chat ID，并且 Telegram Bot 有权向该 Chat ID 发送消息。

### 3. 数据没有更新？

请确保程序能正常读取 `./page/data` 文件夹中的 JSON 文件。如果该文件夹没有数据，请先运行程序获取初始数据。

## 贡献

如果您有任何想法、问题或建议，欢迎提交 Issue 或 Pull Request。我们欢迎社区的贡献。

## 许可证

本项目使用 [MIT 许可证](LICENSE)。

