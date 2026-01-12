# 数学核心素养AI - Netlify 部署版

一个集成了 AI 对话（DeepSeek）、数学公式渲染（MathJax）和动态图形绘制（Desmos免费版）的数学学习助手。

## 📁 项目结构

```
math-ai-netlify/
├── index.html                    # 主页面（已安全化）
├── netlify.toml                 # Netlify 配置
├── package.json                 # 项目配置
├── .gitignore                   # Git 忽略文件
├── .env.example                 # 环境变量示例
├── netlify/
│   └── functions/
│       └── chat-stream.js       # 支持流式响应的 Serverless API
└── README.md                    # 本文件
```

## ✨ 核心功能

- 🤖 **AI 数学助手**：基于 DeepSeek API 的智能对话
- 📐 **公式渲染**：使用 MathJax 完美显示数学公式
- 📊 **动态图形**：集成 Desmos 免费计算器，实时绘制函数图像
- 💬 **流式响应**：实时显示 AI 回复，体验更流畅
- 🔒 **安全部署**：API key 存储在服务器端
- 📱 **响应式设计**：适配各种设备

## 🚀 快速部署

### 方法一：拖拽部署（最简单）⭐

1. 访问 https://app.netlify.com/drop
2. 将整个项目文件夹拖到页面上
3. 等待部署完成
4. 在 Site settings > Environment variables 添加 `DEEPSEEK_API_KEY`

### 方法二：Git 部署（推荐）⭐⭐⭐

1. **创建 Git 仓库**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

2. **推送到 GitHub**
   ```bash
   git remote add origin https://github.com/your-username/math-ai.git
   git push -u origin main
   ```

3. **连接 Netlify**
   - 访问 https://app.netlify.com
   - 点击 "New site from Git"
   - 选择你的仓库
   - Build settings:
     * Build command: 留空
     * Publish directory: `.`
   - 点击 "Deploy"

4. **配置环境变量**
   - Site settings > Environment variables
   - 添加：`DEEPSEEK_API_KEY` = `sk-b6c7f4dfb8224b3c934e1370f81d9c3f`
   - 保存后触发重新部署

### 方法三：CLI 部署

```bash
# 安装 Netlify CLI
npm install -g netlify-cli

# 登录
netlify login

# 初始化
netlify init

# 部署
netlify deploy --prod
```

## ⚙️ 环境变量配置

**必需配置**（否则应用无法工作）：

在 Netlify 网站设置中添加：

| 变量名 | 值 | 说明 |
|--------|-----|------|
| `DEEPSEEK_API_KEY` | 你的DeepSeek API密钥 | DeepSeek API 密钥 |

**如何获取DeepSeek API密钥**：
1. 访问 https://platform.deepseek.com/
2. 注册并登录账号
3. 进入 API Keys 页面
4. 创建新的 API 密钥
5. 复制密钥（格式类似：sk-xxxxxxxxxxxxxx）

**配置步骤**：
1. Netlify 网站 > Site settings > Environment variables
2. 点击 "Add a variable"
3. Key: `DEEPSEEK_API_KEY`
4. Value: 你的 DeepSeek API key
5. 保存后**必须重新部署**

## 🔒 安全性说明

### ✅ 已完成的安全措施

1. **API Key 隐藏**
   - 前端代码不包含 API key
   - 使用 Netlify serverless function 作为代理

2. **安全架构**
   ```
   用户浏览器 (无密钥)
       ↓
       请求: /.netlify/functions/chat-stream
       ↓
   Netlify Function (有密钥)
       ↓
       使用: process.env.DEEPSEEK_API_KEY
       ↓
   DeepSeek API
       ↓
       流式返回响应
       ↓
   用户浏览器
   ```

3. **HTTPS + CORS**
   - Netlify 自动提供 HTTPS
   - 正确配置 CORS 头

### ⚠️ 安全建议

1. **限制 API 使用**
   - 在 DeepSeek 控制台设置使用限额
   - 定期检查 API 使用情况

2. **监控部署**
   - 定期查看 Netlify Functions 日志
   - 关注异常流量

## 🛠️ 技术栈

- **前端**：HTML/CSS/JavaScript
- **AI API**：DeepSeek Chat API（流式响应）
- **数学渲染**：MathJax 3
- **图形绘制**：Desmos API
- **Markdown**：Marked.js
- **部署**：Netlify（静态站点 + Serverless Functions）
- **图标**：Font Awesome 6

## 📊 功能说明

### 1. AI 对话
- 支持数学问题解答
- 流式显示回复内容
- 保留对话历史

### 2. 公式渲染
- 行内公式：`$formula$`
- 显示公式：`$$formula$$`
- 支持矩阵、行列式等复杂结构
- 自动识别和渲染

### 3. Desmos 图形
- 使用免费的Desmos图形计算器
- 在对话中自动检测图形命令
- 实时绘制函数图像
- 支持多种数学函数
- 无需API密钥，完全免费使用

## 🧪 本地开发

```bash
# 安装依赖
npm install

# 启动本地服务器（包括 functions）
npm run dev

# 访问 http://localhost:8888
```

## 🐛 故障排查

### 问题 1：AI 不回复

**检查**：
- Netlify Functions 标签是否显示 `chat-stream` function
- 环境变量 `DEEPSEEK_API_KEY` 是否已设置
- 浏览器 Console 是否有错误信息

**解决**：
```bash
# 查看 function 日志
netlify functions:list

# 重新部署
netlify deploy --prod
```

### 问题 2：公式不显示

**检查**：
- MathJax CDN 是否加载成功
- 公式语法是否正确
- 浏览器 Console 是否有错误

### 问题 3：Desmos 不显示

**检查**：
- Desmos API key 是否有效
- 网络连接是否正常

## 📝 使用示例

### 数学问题
```
用户：解方程 x^2 + 5x + 6 = 0
AI：使用因式分解...
```

### 行列式计算
```
用户：计算行列式 |1 2; 3 4|
AI：[自动渲染行列式并计算]
```

### 函数图像
```
用户：画出 y = x^2 的图像
AI：[在 Desmos 中绘制抛物线]
```

## 🎯 后续优化

- [ ] 添加对话历史保存功能
- [ ] 支持导出对话记录
- [ ] 添加更多数学工具
- [ ] 支持用户自定义主题
- [ ] 添加语音输入功能

## 📄 许可

MIT License

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

**部署愉快！** 🚀
