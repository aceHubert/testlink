# @acehubert/testlink-mcp

TestLink MCP Server，提供测试用例、测试套件、测试计划、构建、执行结果和需求的常用操作。

## 使用

```bash
yarn install
yarn build
```

环境变量：

```bash
TESTLINK_URL=http://localhost/testlink
TESTLINK_API_KEY=your-testlink-api-key
```

MCP Server：

```bash
npx @acehubert/testlink-mcp
```

CLI：

```bash
testlink projects list
testlink cases view --testCaseId GPDL-1
testlink --version
```
