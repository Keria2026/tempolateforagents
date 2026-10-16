# AGENTS.md

## 1. 技术栈

Frontend: TypeScript Strict + Svelte 5 + SvelteKit + Bun + Vite
Database: PostgreSQL
Backend: Rust Stable + SQLx + Axum
package manager:
 - frontend：Bun
 - Backend：Cargo

Database: PostgreSQL

CI/CD: GitHub Actions + Docker

## 2.原则

**做长期正确的事。做长期正确的事。做长期正确的事。**

- 长期主义，面向维护，新增改动应降低未来开发成本，禁止引入技术债
- 结构性问题用重构解决，禁止补丁、特例绕过
- 遵循KISS原则，选最短且长期最佳实现路径，架构导致冗余就简化架构
- 禁止任何类型的硬编码
- 迭代修改而非增量修改，重写/重构后删旧代码
- 遵循可执行最佳、现代的实践
- 先看代码，再询问信息和需求，不直接开发
- 除非特别要求，不做兼容，不保留历史包袱，面向长期干净整洁，减少复杂度
- 精准手术刀式的修正，不做过度兜底

## 2.开发规范
- 保持最小必要依赖，不引入大型依赖，禁止重复功能依赖
- 优先调用语言、运行时和框架原生能力
- 及时删除无效代码，发现不符合原则或规范的代码顺手优化
### 2.1开发过程
- 不编写只有单一调用、没有复用或抽象价值的私有 Helper 函数
- 密钥出现在私有的仓库中是可以的
- 禁止编写注释
- 文档要保持索引和信息职责和单一信息，不能复述已经存在的逻辑，比如已有的代码逻辑，应该是指向相关的代码，而不是用文字将代码逻辑复述一遍，避免形成分叉，避免浪费上下文。文档保持精简。
- 开发中，不要运行全量测试，这是为了缩短开发时间
- 优先修改优化，而不是增量
- 修改业务逻辑必须补充测试

### 2.2 质量门禁
Frontend:
- bun run lint
- bun run check
- bun test
- bun run build

Backend:
- cargo fmt --all -- --check
- cargo clippy --workspace --all-targets -- -D warnings
- cargo test --workspace

所有检查必须通过，失败必须修复。
## Git与CI/CD
- feature/* → develop → master
- 只有合并给master进行自动部署到生产环境。
- CI 执行静态检查、测试、构建和安全扫描
- CD 仅部署经过验证的不可变容器镜像
- 禁止直接修改生产环境代码
## 项目结构规范

project/
├── AGENTS.md
├── Cargo.lock
├── .github/
│   ├── CODEOWNERS
│   └── workflows/
│       ├── ci.yml
│       ├── staging.yml
│       ├── release.yml
│       └── security.yml
│
├── apps/
│   ├── web/                 # SvelteKit + Bun
│   │   ├── src/
│   │   ├── tests/
│   │   ├── package.json
│   │   └── bun.lock
│   │
│   └── api/                 # Rust + Axum
│       ├── src/
│       │   ├── main.rs
│       │   ├── config.rs
│       │   ├── error.rs
│       │   └── modules/
│       ├── tests/
│       ├── migrations/
│       └── Cargo.toml
│
├── packages/
│   └── api-types/           # 自动生成 TS 类型
│
├── infra/
│   ├── compose.yml
│   ├── Caddyfile
│   └── scripts/
│       ├── deploy.sh
│       ├── healthcheck.sh
│       └── rollback.sh
│
├── docs/
│   ├── architecture.md
│   ├── api.md
│   └── deployment.md
│
├── Cargo.toml
└── .gitignore 

