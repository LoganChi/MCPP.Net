# MCPP.Net

<div align="center">
  <img src="https://raw.githubusercontent.com/microsoft/modelcontextprotocol/main/Logo.png" width="200" alt="MCPP.Net Logo"/>
  <h3>基于Model Context Protocol的.NET开发工具扩展</h3>
</div>

## 简介

MCPP.Net是一个基于[Model Context Protocol (MCP)](https://github.com/microsoft/modelcontextprotocol)的.NET应用程序，它允许您通过简单的方式动态导入Swagger/OpenAPI规范并自动将其转换为MCP工具，使大型语言模型（LLMs）能够通过标准接口调用这些API。

该项目使API调用变得简单，无需手动编写代码即可为LLMs提供工具访问能力。

## 特性

✨ **动态Swagger导入** - 通过提供Swagger URL或本地文件，自动生成MCP工具
✨ **即时可用** - API导入后立即可用，无需重启服务
✨ **标准兼容** - 完全符合Model Context Protocol规范
✨ **可扩展架构** - 易于添加新的工具类型和功能
✨ **易于集成** - 简单的API接口用于管理和使用工具

## 快速开始

### 安装

```bash
# 克隆仓库
git clone https://github.com/yourusername/MCPP.Net.git
cd MCPP.Net

# 构建项目
dotnet build

# 运行项目
dotnet run
```

### 导入Swagger API

API运行后，您可以使用以下方式导入Swagger API：

```bash
curl -X POST "https://localhost:5001/api/Import/Import" \
  -H "Content-Type: application/json" \
  -d '{
    "swaggerUrl": "https://petstore.swagger.io/v2/swagger.json",
    "nameSpace": "MCPP.Net.PetStore",
    "className": "PetStoreApi"
  }'
```

或通过Swagger UI进行导入：

1. 打开浏览器访问 `https://localhost:5001/swagger`
2. 找到 `/api/Import/Import` 端点
3. 填写导入参数并执行

<div align="center">
  <img src="https://mermaid.ink/img/pako:eNp1kMFqwzAMhl_F6NRB82wO4y1ZYdttsCvsIG9Qkw0TQxzLuFk72rv3dCnbTtlBIH193_-D5V4jVawTm8DeyPYjyRToCT0qcvdY_1KRPWdv2kAHZD4GgzD5-QgSQGhMcQvnc1m-MzuChXHCrY4G33D3CL_KPtVbMjRTn15PplN20O1P_WkGsj4s3C7LZgbCzB96SxtywF2eYH0B3Qr0OGRKnAWLxeSFWWZPXuMAK2-zsjjQ2YBUgVZLIhIbmz_c5X9nDbmEa-b9eNwfDTRCpR7uKBOqxBhYG_MwVGE-_pO9F3nFvAilyJvTtqqruq0bl3f1tuCc5bpQcrt1JW_yhteC662q63LztQdSj3R5" width="600" alt="MCPP.Net工作流程"/>
</div>

## 项目结构

```
MCPP.Net
├── Controllers/             # API控制器
│   └── ImportController.cs  # Swagger导入控制器
├── Models/                  # 数据模型
│   └── SwaggerImportModels.cs # Swagger导入相关模型
├── Services/                # 服务层
│   ├── ImportedToolsService.cs    # 导入工具管理服务
│   ├── McpServerMethodRegistry.cs # MCP服务方法注册
│   └── SwaggerImportService.cs    # Swagger导入服务
├── Tools/                   # MCP工具实现
│   └── TestTool.cs          # 测试工具示例
└── Program.cs               # 应用程序入口
```

## API参考

### 导入API

**POST** `/api/Import/Import`

导入Swagger API并动态注册为MCP工具

请求体：
```json
{
  "swaggerUrl": "https://api.example.com/swagger.json",
  "nameSpace": "MCPP.Net.Example",
  "className": "ExampleApi"
}
```

响应：
```json
{
  "success": true,
  "apiCount": 10,
  "toolClassName": "ExampleApi",
  "importedApis": ["getUsers", "createUser", "updateUser", ...]
}
```

### 获取已导入工具

**GET** `/api/Import/GetImportedTools`

获取当前已导入的所有API工具

响应：
```json
[
  {
    "nameSpace": "MCPP.Net.Example",
    "className": "ExampleApi",
    "apiCount": 10,
    "importDate": "2023-10-15T12:34:56",
    "swaggerSource": "https://api.example.com/swagger.json"
  }
]
```

### 删除工具

**DELETE** `/api/Import/DeleteImportedTool?nameSpace=MCPP.Net.Example&className=ExampleApi`

删除已导入的API工具

## 技术架构

<div align="center">
  <img src="https://mermaid.ink/img/pako:eNp1kU9PwzAMxb9KlFPR1qh3EAqHwRDsNgkJITdxF7Nu02SJkTJV_e64HVD2h1xy8vN7z078SNSI4SzvlQFnxeoq-e7gDQUIIvfI_ik2ZSQtTdAOMYGpd0ogxIfxCBRA7JRfwtVVWT6CaQGmLvlJTxQ-4PYBnpWLdE6TYFenlyM0Qnfk_rE_G4GcJh3YIWQTgWh6hL52CRr4zxfYvoJuBK5M8FXTXLCQvHqGQhSgyXbAZWR-2N-aQODqmqJWFO1Df_9JsSn32zprxJXVJRQgMcfIuH2CvTg-y5CUm_OMPjPYbFMhBsIWCcuDHc651gHW4PxMpIdqYUJU4nCXFe9W62x8HXOoI0-nPmLGBsGDNeHXVGM-_Fd7z3jBeJ4PBs8W_ZoP2XKR8Ww-n7KMzfmiKBb0oZqxOVvMs9mMrYuC5TxfHJ7-AJf1kQU" width="600" alt="MCPP.Net架构图"/>
</div>

## 贡献指南

欢迎贡献！请遵循以下步骤：

1. Fork 项目
2. 创建功能分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 打开 Pull Request

## 许可证

本项目采用 MIT 许可证 - 详情请参阅 [LICENSE](LICENSE) 文件 