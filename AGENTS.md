# Repository Guidelines

## Project Structure & Module Organization

`src/` 是主代码目录：`index.ts` 提供 MCP Server 入口，`cli.ts` 提供命令行入口，`api.ts` 封装 TestLink XML-RPC 调用。`scripts/` 放发布辅助脚本，例如 `genChangelog.js`。`skills/` 存放配套技能说明。构建产物输出到 `dist/`，不要手改。环境变量示例在 `.env.example`。

## Build, Test, and Development Commands

- `yarn install`：安装依赖，仓库使用 Yarn 3。
- `yarn dev`：直接运行 `src/index.ts`，用于本地调试 MCP Server。
- `yarn build`：清空 `dist/` 后生成类型声明、ESM、CJS 和 CLI 产物。
- `yarn lint`：对 `src/**/*.ts` 运行 ESLint，要求 `--max-warnings=0`。
- `yarn lint:fix`：自动修复可修复的 lint 问题。
- `yarn typecheck`：运行 TypeScript 静态检查，不生成文件。
- `yarn changelog`：根据发布记录生成变更日志。

## Coding Style & Naming Conventions

统一使用 TypeScript、ES Module 和 2 空格缩进。Prettier 规则由 `prettier.config.cjs` 定义：保留分号、双引号、尾随逗号，`printWidth` 为 100。提交前运行 `yarn lint` 与 `yarn typecheck`。文件名使用小写短词，如 `api.ts`、`cli.ts`；导出类使用 PascalCase，例如 `TestLinkAPI`；函数和变量使用 camelCase。

## Commit & Pull Request Guidelines

仓库暂无历史提交，可直接遵循 `commitlint.config.cjs` 中的 Conventional Commits 规范，例如 `feat: add suite update command`、`fix: normalize external case id parsing`。已配置 `commit-msg` 和 `lint-staged` 钩子，提交前会校验消息并格式化改动文件。PR 请写清楚变更目的、影响范围、验证命令；如果改动 CLI 输出或接入方式，请附示例命令；如果涉及配置，说明所需环境变量，如 `TESTLINK_URL` 与 `TESTLINK_API_KEY`。

## Security & Configuration Tips

不要提交真实的 TestLink 地址、API Key 或其他敏感信息。新增配置项时，先更新 `.env.example` 和 `README.md`，再在代码中补齐缺省处理与报错信息。
