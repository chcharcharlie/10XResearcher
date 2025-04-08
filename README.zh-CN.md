# 10XResearcher

[English](./README.md) | [中文](./README.zh-CN.md)

仅需每月$20，通过将Claude转变为具有本地数据存储功能的强大研究代理，在您的桌面上进行类似ChatGPT和Manus的深度研究。

## 什么是10XResearcher？

10XResearcher将Claude转变为系统化的研究代理，它遵循迭代方法论，本地存储所有发现，并产出高质量的研究成果。

### 主要优势

- **成本效益高**：仅需Claude Desktop的$20/月订阅
- **本地存储**：所有研究文件和发现都存储在本地
- **系统化方法**：交替进行讨论和研究阶段
- **循证为基础**：所有声明都链接到存储的参考资料
- **可恢复研究**：只需引用项目名称，即可在多个对话中暂停和继续研究

## 系统要求

- Claude Desktop订阅
- Python 3.10+
- Node.js 14+
- Git

## 安装

### 1. 安装所需软件

1. 从[python.org](https://python.org)安装Python
2. 从[nodejs.org](https://nodejs.org)安装Node.js
3. 从[anthropic.com](https://www.anthropic.com/claude)订阅Claude Desktop

### 2. 安装依赖项

```bash
# 安装uvx以获取MCP服务器功能
pip install uvx

# 对于desktop commander，请按照GitHub仓库中的选项5进行操作：
# 1. 克隆仓库：git clone https://github.com/wonderwhy-er/DesktopCommanderMCP.git
# 2. 导航到目录：cd DesktopCommanderMCP
# 3. 运行：npm install
# 4. 构建：npm run build
```

### 3. 配置MCP服务器

1. 打开Claude Desktop → 设置 → MCP服务器
2. 添加以下配置（根据您的系统更新路径）：

```json
{
  "mcpServers": {
    "git": {
      "timeout": 60,
      "command": "/path/to/python/bin/uvx",
      "args": [
        "mcp-server-git",
        "--repository",
        "/path/to/your/research/directory"
      ],
      "transportType": "stdio"
    },
    "browser-use": {
      "disabled": false,
      "timeout": 60,
      "command": "uvx",
      "args": [
        "mcp-browser-use"
      ],
      "transportType": "stdio"
    },
    "desktop-commander": {
      "timeout": 60,
      "command": "node",
      "args": [
        "/path/to/your/DesktopCommanderMCP/dist/index.js"
      ],
      "transportType": "stdio"
    }
  }
}
```

**重要提示：**
- 将`/path/to/python/bin/uvx`替换为您的实际uvx路径
- 将`/path/to/your/research/directory`替换为您希望存储研究的位置
- 将`/path/to/your/DesktopCommanderMCP/dist/index.js`替换为您本地构建的DesktopCommanderMCP的dist/index.js文件的路径

我们使用以下MCP实现：
- 浏览器：[mcp-browser-use](https://github.com/vinayak-mehta/mcp-browser-use)
- 桌面命令：[DesktopCommanderMCP](https://github.com/wonderwhy-er/DesktopCommanderMCP)
- Git：[mcp-server-git](https://github.com/modelcontextprotocol/servers/tree/main/src/git)

#### 找到您的Python路径

在macOS/Linux上：
```bash
which uvx
```

在Windows上：
```
where uvx
```

### 4. 创建研究目录

```bash
mkdir -p ~/claude_space/research
```

## 使用Claude项目

### 设置研究项目

1. 打开Claude Desktop → 项目 → 新建项目
2. 基于您的研究主题命名项目
3. 点击"设置项目指令"
4. 将[prompt_instructions.md](./prompt_instructions.md)的全部内容复制到该字段中
5. 点击"保存"

### 开始研究

1. 打开您的项目
2. 在该项目中开始新对话
3. 描述您的研究请求
4. Claude将从讨论阶段开始，了解您的需求

### 恢复研究

1. 打开同一个项目
2. 开始新对话
3. 引用您的研究项目名称（例如，"继续我的可再生能源市场研究"）
4. Claude将读取现有的仓库并从您上次结束的地方继续

### 示例提示

开始研究：
```
我需要研究可再生能源市场趋势以进行潜在投资
```

恢复研究：
```
继续我的可再生能源市场研究。当前状态如何？
```

### 最佳实践

- 将所有研究对话保持在同一个项目中
- 开始每个新对话时，明确引用您的研究项目名称
- 让Claude在您打断之前完成每个阶段
- 定期查看本地仓库文件以查看存储的研究内容

## 工作原理

10XResearcher实现了一种结构化的研究方法：

1. **讨论阶段**：Claude收集需求并制定研究计划
2. **研究阶段**：Claude进行网络研究，并进行全面的文档记录
3. **重复**：该过程在讨论和研究阶段之间循环，直到完成

所有研究都存储在本地git仓库中，并有详细的文档。

## 故障排除

### MCP服务器连接问题
- 验证配置中的路径是否正确
- 检查是否安装了所有依赖项
- 确保Claude Desktop有执行命令的权限

### 仓库访问问题
- 确认仓库路径存在且可访问
- 检查目录的文件权限
- 验证git是否已安装且可访问

### 浏览器问题
- 确认已启用browser-use MCP服务器
- 检查您的系统是否有兼容的浏览器安装

## 已知问题

1. **截断的响应**：在生成冗长内容时，Claude可能会中途停止。输入"继续"让它恢复。

2. **长对话限制**：在多次使用"继续"提示后，您可能会看到关于使用限制的警告。这表示可能会受到限制，Claude意外停止的风险增加。为减轻这种情况，在适当的检查点开始新的聊天，并引用您的项目名称来恢复。

3. **MCP授权**：在打开新聊天时，您需要至少授权每个MCP操作一次。虽然此设置也适用于Claude (Cline)，但自动批准功能可能无法一致地工作。

4. **其他客户端的令牌成本**：使用Cursor或Cline进行此设置将产生令牌成本，这与固定费用的Claude Desktop不同。

## 其他资源

- [prompt_instructions.md](./prompt_instructions.md) - 核心提示系统
- [example_project](./example_project/) - 示例研究项目结构

## 许可证

本项目根据MIT许可证授权 - 详见[LICENSE](./LICENSE)文件。