# 文游 Bright UI

一个纯前端、浏览器本地运行的角色扮演聊天界面，支持多会话管理、角色卡编辑、本地持久化、Markdown 渲染，以及可选的生图能力。

## 功能特性

- 多会话对话管理
  - 新建、切换、删除、清空、导入、导出会话
  - 每个会话都会保存：
    - 消息记录
    - 设置快照
    - 角色卡快照
- 角色卡编辑器
  - 基础信息
  - 场景
  - 作者注释
  - 自定义条目
  - 世界书
  - 正则规则
  - 支持角色卡 JSON 导入 / 导出
  - 支持通过已配置的文字模型“一键生成角色卡”
- 聊天能力
  - 支持流式输出
  - 支持非流式输出
  - 本地保存聊天记录
  - 支持 Markdown 渲染
  - 支持安全彩字标记：
    - `[color=#7c3aed]文字[/color]`
- 生图能力
  - 手动指令触发：
    - `/img 英文提示词`
    - `/image 英文提示词`
    - `生成图片：英文提示词`
  - 也支持助手回复尾部自动触发：
    - `[[IMAGE: ENGLISH_PROMPT]]`
  - 支持两类接口：
    - OpenAI 风格 `/v1/images/generations`
    - NovelAI 风格 `/ai/generate-image`
- 外观自定义
  - 主题色
  - 背景色
  - 卡片 / 描边 / 文字颜色
  - 背景图 URL 或上传
  - 背景图透明度
- 所有主要数据都保存在浏览器 `localStorage`
- 响应式抽屉和弹窗界面

## 技术栈

- 原生 JavaScript
- HTML
- CSS
- 无需构建工具

## 文件结构
.
├── index.html
├── styles.css
└── app.js

## 运行方式

这是一个纯前端项目。

你只需要用浏览器打开 `index.html`，然后在界面中填写 API 设置即可使用。

本项目会将数据保存到浏览器本地，包括：

- 设置
- 会话列表
- 角色卡
- 壁纸资源
- 角色生成描述草稿

## 快速开始

1. 下载或克隆本项目
2. 确保以下文件位于同一目录：
   - `index.html`
   - `styles.css`
   - `app.js`
3. 用现代浏览器打开 `index.html`
4. 点击左侧或抽屉中的 **设置**
5. 填写文字模型相关配置：
   - 中转 API BaseURL
   - API Key
   - 模型
6. 如需生图，再额外配置：
   - 生图 API BaseURL
   - 生图 API Key
   - 生图接口路径
   - 生图模型
7. 保存后即可开始聊天

## 接口兼容说明

### 文本聊天接口

项目默认按 OpenAI 兼容格式调用文字接口：

- `GET /v1/models`
- `POST /v1/chat/completions`

支持：

- SSE 流式输出
- 普通 JSON 非流输出

### 生图接口

#### OpenAI 风格生图接口

默认使用：

- `POST /v1/images/generations`

返回中支持以下任一字段：

- `data[0].url`
- `data[0].b64_json`

#### NovelAI 风格生图接口

例如：

- `POST /ai/generate-image`

项目内置了严格的 NovelAI 请求体构造逻辑，兼容这类模型：

- `nai-diffusion-3`
- `nai-diffusion-4-curated-preview`
- `nai-diffusion-4-full`
- `nai-diffusion-4-5-curated`
- `nai-diffusion-4-5-full`

## 会话快照机制

每个会话都会保存完整快照，包括：

- 当前角色卡
- 当前设置
- 当前消息记录

当你切换会话时，程序会自动恢复该会话对应的：

- API 设置
- 已选模型
- 系统提示词
- 生成参数
- 外观设置
- 角色卡
- 消息列表

因此每个会话都可以视为一个独立工作区。

## 角色卡格式

项目内部角色卡结构如下：
{
"basic": {
"name": "夜航诗人",
"avatar": "",
"playerAvatar": "",
"shortDesc": "",
"persona": "",
"greeting": "",
"talkativeness": 0.4
},
"detail": {
"scenario": "",
"creatorNotes": "",
"custom": []
},
"worldbook": [],
"regex": []
}

同时也兼容较扁平的导入格式，例如：

{
"name": "夜航诗人",
"shortDesc": "",
"persona": "",
"greeting": "",
"scenario": "",
"creatorNotes": "",
"worldbook": [],
"regex": []
}

## 会话导入格式

会话 JSON 导入支持以下三类结构：

### 1）整库格式
{
"v": 1,
"activeId": "...",
"convs": [...]
}

### 2）单会话格式

{
"id": "...",
"title": "...",
"messages": [...]
}

### 3）旧格式消息数组

[
{ "side": "right", "text": "你好" },
{ "side": "left", "text": "你好，有什么想聊的？" }
]

## 生图提示词规则

当启用“助手自动插画”时，系统提示词会要求模型在每次回复末尾输出：

[[IMAGE: <ENGLISH_PROMPT>]]


注意：

- 必须是纯英文
- 必须单行
- 不能包含中文字符
- 应描述当前这一轮 NPC 的画面

如果检测到中文，会直接拒绝生图请求。

## 本地存储键名

项目主要使用以下 `localStorage` 键：

- `wy_settings_v1`
- `wy_ui_assets_v1`
- `wy_chat_history_v1`
- `wy_conversations_v1`
- `wy_role_card_v2`
- `wy_role_gen_desc_v1`

## 重要说明

- 本项目是纯前端工具模板，不包含后端。
- API Key 会直接保存在浏览器 `localStorage` 中。
- 不建议在公共设备或不可信环境下使用真实密钥。
- 壁纸如果使用大图转 base64，可能占用较多本地存储空间。
- 会话数量和单会话消息数都有限制。

当前代码默认限制：

- 最多保存会话数：`30`
- 每个会话最多保存消息数：`200`

## 浏览器要求

推荐使用最新版：

- Chrome
- Edge
- Firefox
- Safari

项目依赖的浏览器能力包括：

- `localStorage`
- `fetch`
- `ReadableStream`
- `FileReader`
- `crypto.randomUUID`
  - 若不可用，代码中已有降级方案

## 可自定义项

你可以直接在 `app.js` 中修改默认配置，例如：

- 默认设置
- 默认系统提示词
- 最大会话数
- 最大消息数
- 默认外观色
- 默认图片尺寸
- 默认角色卡

常见可改常量：

- `DEFAULT_SETTINGS`
- `DEFAULT_ROLE`
- `CHAT_MAX_ITEMS`
- `CONV_MAX`

## 安全提示

- 数据主要保存在浏览器本地。
- API Key 未加密。
- Markdown 渲染为项目内自定义实现，功能有限，但导入未知 JSON 时仍建议谨慎。
- 本项目更适合作为个人使用、本地工具或界面模板。

## License

请在此处补充你的许可证信息。

## 致谢

这是一个偏“亮色主题 + 文游 / 角色扮演 + 本地优先”的轻量模板，集成了：

- 角色卡编辑
- 多会话历史
- Markdown 消息展示
- 生图接入
- 本地持久化工作流
