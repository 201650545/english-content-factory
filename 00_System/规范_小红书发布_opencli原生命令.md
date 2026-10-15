# 小红书发布 · opencli 原生命令规范（v2 · 桌面路线）

> 生效：2026-10-15 ｜ 取代 v1 手机通道方案（手机路线已放弃，保留在文末作废案）
> 适用范围：本机桌面 Chrome + opencli 的 xiaohongshu 适配器

---

## 一、一条命令发布（图文笔记）

```powershell
$body = (Get-Content '正文.txt' -Raw).TrimEnd()
$dir  = 'D:\Work\English-Content-Factory\02_Projects\HN-ENGLISH-666\03_Products\03_小红书封面'
$imgs = @("$dir\688_cover_v4.png","$dir\xhs_p2_表格A页.png","$dir\xhs_p3_表格B页.png","$dir\xhs_p4_表格M页.png","$dir\xhs_p5_数据汇总页.png") -join ','
$topics = '湖南中考,中考英语,中考英语单词,初三家长,长沙中考,英语学习打卡,中考复习,免费资料'

opencli xiaohongshu publish $body `
  --title '湖南中考英语，背这688个词就够了' `
  --images $imgs `
  --topics $topics `
  --draft true `
  -f yaml
```

- `--draft true` = **只存草稿，不发布**（发布前审核用这个）
- 去掉 `--draft` 才是真发布
- 话题**不带 `#`**，逗号分隔，最多 9 张图
- 成功输出：`status: ✅ 暂存成功` + 图片数 + 话题清单

---

## 二、配套的 opencli 命令

| 用途 | 命令 |
| :--- | :--- |
| 看草稿箱 | `opencli xiaohongshu drafts -f yaml` |
| 删草稿（默认 dry-run） | `opencli xiaohongshu draft-delete <id> --execute true` |
| 看账号 | `opencli xiaohongshu whoami` |
| 数据看板 | `opencli xiaohongshu creator-stats` |
| 笔记列表 | `opencli xiaohongshu creator-notes` |
| 诊断桥 | `opencli doctor` |
| 重启桥 | `opencli daemon restart` |

---

## 三、踩过的四个坑（都已在 override 里修好）

| # | 现象 | 根因 | 修法 |
| :--- | :--- | :--- | :--- |
| 1 | `Page.fileChooserOpened not received within 5s` | 扩展的 `set-file-input` 走文件选择框事件，被创作页反自动化挡住 | override 里改成**页面自己 fetch 本机 HTTP** 拼 `File`，绕开选择框 |
| 2 | `CDP method not permitted: DOM.setFileInputFiles` | 扩展有 CDP 方法白名单，禁了这个方法 | 不再依赖裸 CDP，走 ① 的 fetch 路线 |
| 3 | `sendCommand: max attempts exhausted`（4.1MB base64） | 把图片 base64 塞进 `page.evaluate` 超过桥上限 | override 内置**临时 HTTP 服务**（随机端口）伺服图片，载荷只剩 5 个文件名 |
| 4 | `Could not attach topic "X": no real topic entity appeared after selection` | 话题下拉是竞态，首次常常丢高亮 | override 里**每个话题重试 3 次**，失败先 undo 再试 |

**override 文件位置**（opencli 官方本地覆盖机制）：

```
%USERPROFILE%\.opencli\clis\xiaohongshu\publish.js
```

它在 `opencli adapter status` 里会显示为 `xiaohongshu [override]`。
**升级 opencli 后若发布报图片错，先检查这个 override 是否还在。**

---

## 四、话题要用小红书的真名

话题名必须命中平台真实话题，写错会整单失败。实测：

| 想写 | 实际真名 |
| :--- | :--- |
| 英语学习 | **英语学习打卡** |
| 高考英语单词 / 四六级单词 / 高职高考英语 | ⚠️ 这些是推荐榜默认项，**中考人群不要用**（用户明确否掉） |

> 注意：**纯文本写 `#话题` 不会变成可点话题胶囊**，必须走 `--topics` 参数走平台话题实体。

---

## 五、标签页为什么"一下子就没了"

opencli 的**标签页租约（tab lease）**机制：适配器跑完会释放租约并关掉自己开的标签页。
草稿是**存在平台服务端的**，标签页关掉不影响草稿。

要把它**固定在那儿**：

```powershell
# 让适配器别关标签
opencli xiaohongshu <命令> --keep-tab true --site-session persistent

# 或者自己开一个并占住（这个租约不会被适配器带走）
opencli browser xhs open 'https://creator.xiaohongshu.com/publish/publish?source=official'
opencli browser xhs tab list     # 确认还在
```

⚠️ `opencli daemon restart` 会清掉所有租约，标签页随之消失。

---

## 六、删草稿要小心

`draft-delete` 默认是 **dry-run**，必须显式 `--execute true`。
草稿删除不可恢复 —— 2026-10-15 有一条 8 话题的完整草稿被误删，用第一节那条命令
**1 分钟就重建了**（这就是把流程固化成一条命令的价值）。

---

## 七、作废案：手机通道（v1，不要再走）

2026-10-15 曾用「手机 Kiwi + 桌面 CDP 接管 + adb 隧道」绕过桌面硬墙，链路能通、能发图，
但每次掉线要重建 `adb forward/reverse`，综合比桌面 opencli 原生命令慢得多，**已放弃**。

留档价值仅两点：
1. 手机页 `input.files = DataTransfer.files` 是生效的（桌面改走 fetch 路线的思路来源）
2. 小红书自研组件只认真鼠标事件（`Input.dispatchMouseEvent`），JS `.click()` 无效

工具残留在 `D:\小浣熊\_phone_cdp\`，未删除，仅供考古。
