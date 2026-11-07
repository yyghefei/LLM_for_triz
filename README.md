# Triz Copilot

[English](#english) | [中文](#中文)

Triz Copilot is an AI-assisted TRIZ workspace built with Next.js. It helps you turn a plain problem statement into a structured, TRIZ-based analysis.

---

## English

### Overview

Triz Copilot is a full-stack Next.js application that applies the TRIZ (Theory of Inventive Problem Solving) methodology to problems you write down. Each case you save is sent to a large language model, which returns:

- the primary and secondary contradictions,
- the TRIZ principles that apply,
- innovative solution ideas,
- anticipated benefits, and
- risks and challenges.

You can also ask free-form questions, answered from all of your saved cases.

### Features

- **Authentication** — sign-up and sign-in with Clerk, including prebuilt `/sign-in` and `/sign-up` pages.
- **TRIZ cases** — create, edit (with autosave) and delete entries.
- **AI analysis** — every save re-runs a GPT-3.5 analysis through LangChain and stores the result in the database.
- **Ask questions** — a retrieval-based Q&A box answers questions across all of your entries.
- **Per-user storage** — entries and analyses are scoped to the signed-in user in MySQL via Prisma.

### Tech stack

- Next.js 14 (App Router) + React 18 + TypeScript
- Tailwind CSS
- Clerk for authentication
- Prisma ORM with MySQL
- LangChain, OpenAI `gpt-3.5-turbo` and Zod for the AI analysis
- react-autosave for editor autosaving

### Prerequisites

- Node.js 18.17 or later
- A MySQL database with the target database already created
- A Clerk application (publishable and secret keys)
- An OpenAI API key

### Getting started

1. Clone the repository and install dependencies:

   ```bash
   git clone https://github.com/yyghefei/LLM_for_triz.git
   cd LLM_for_triz
   npm install
   ```

2. Create a `.env` file in the project root (the Prisma CLI reads `.env`, and Next.js loads it too):

   ```bash
   DATABASE_URL="mysql://USER:PASSWORD@localhost:3306/triz"
   OPENAI_API_KEY="sk-..."
   NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_..."
   CLERK_SECRET_KEY="sk_..."
   ```

3. Create the database tables:

   ```bash
   npx prisma generate
   npx prisma db push
   ```

   Use `npx prisma migrate dev` instead if you want to keep migration files.

4. Start the development server:

   ```bash
   npm run dev
   ```

   Open <http://localhost:3000> in your browser.

### Usage

1. Open the home page and click **Get started**.
2. Sign up or sign in with Clerk.
3. On the **Triz Study case** page, click **New Entry**.
4. Write your problem statement. The editor autosaves, and each save triggers the AI analysis shown in the right-hand panel.
5. Use the **Ask a question...** box to query all of your entries at once.
6. Use **Delete** to remove the current entry.

### Project structure

```text
app/                    App Router pages and API routes
  (dashboard)/triz/     Case list and case editor
  api/entry/            Create, update (and analyze) and delete entries
  api/question/         Q&A endpoint
components/             Client-side UI components
prisma/schema.prisma    Prisma schema (User, Account, trizEntry, Analysis)
util/                   Clerk auth helpers, Prisma client, AI chains, API calls
middleware.ts           Clerk middleware protecting app routes
```

### Available scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Start the development server |
| `npm run build` | Build for production |
| `npm run start` | Run the production build |
| `npm run lint` | Run ESLint via Next.js |

### Deployment

The app can be deployed to Vercel or any Node.js host. Set the same environment variables as above, and make sure `DATABASE_URL` points to a reachable MySQL instance.

### Notes and limitations

- AI analysis requires a valid `OPENAI_API_KEY`; requests fail without it.
- The model is `gpt-3.5-turbo` with `temperature: 0`. Results are still advisory — always review them.
- There are no automated tests in the repository yet.
- The Prisma schema targets MySQL; change the `provider` and `DATABASE_URL` if you use another database.

---

## 中文

### 项目简介

Triz Copilot 是一个基于 Next.js 的 AI 辅助 TRIZ 工作台，帮助你把自己写下的问题转化为结构化的 TRIZ 分析。每个保存的案例都会交给大语言模型，由模型给出：

- 主要矛盾与次要矛盾；
- 适用的 TRIZ 原理；
- 创新解决方案；
- 预期收益；
- 风险与挑战。

你还可以自由提问，应用会基于你保存的所有案例作答。

### 功能

- **用户认证** — 使用 Clerk 注册和登录，内置 `/sign-in` 与 `/sign-up` 页面。
- **TRIZ 案例** — 创建、编辑（自动保存）和删除条目。
- **AI 分析** — 每次保存都会通过 LangChain 调用 GPT-3.5 重新分析，并把结果保存到数据库。
- **提问** — 基于检索的问答框，可跨所有条目回答问题。
- **按用户隔离数据** — 条目和分析通过 Prisma 存储在 MySQL 中，并按登录用户隔离。

### 技术栈

- Next.js 14（App Router）+ React 18 + TypeScript
- Tailwind CSS
- Clerk 认证
- Prisma ORM + MySQL
- LangChain、OpenAI `gpt-3.5-turbo` 与 Zod 实现 AI 分析
- react-autosave 实现编辑器自动保存

### 环境要求

- Node.js 18.17 或更高版本
- 一个 MySQL 数据库，并已创建好目标数据库
- 一个 Clerk 应用（publishable key 和 secret key）
- 一个 OpenAI API key

### 快速开始

1. 克隆仓库并安装依赖：

   ```bash
   git clone https://github.com/yyghefei/LLM_for_triz.git
   cd LLM_for_triz
   npm install
   ```

2. 在项目根目录创建 `.env` 文件（Prisma CLI 会读取 `.env`，Next.js 也会加载它）：

   ```bash
   DATABASE_URL="mysql://USER:PASSWORD@localhost:3306/triz"
   OPENAI_API_KEY="sk-..."
   NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_..."
   CLERK_SECRET_KEY="sk_..."
   ```

3. 创建数据库表：

   ```bash
   npx prisma generate
   npx prisma db push
   ```

   如果希望保留迁移文件，请改用 `npx prisma migrate dev`。

4. 启动开发服务器：

   ```bash
   npm run dev
   ```

   然后在浏览器中打开 <http://localhost:3000>。

### 使用方式

1. 打开首页，点击 **Get started**。
2. 使用 Clerk 注册或登录。
3. 在 **Triz Study case** 页面点击 **New Entry**。
4. 写下你的问题描述。编辑器会自动保存，每次保存都会触发右侧面板中的 AI 分析。
5. 在 **Ask a question...** 输入框中提问，可一次查询你保存的所有条目。
6. 点击 **Delete** 删除当前条目。

### 项目结构

```text
app/                    App Router 页面与 API 路由
  (dashboard)/triz/     案例列表与案例编辑器
  api/entry/            创建、更新（并分析）和删除条目
  api/question/         问答接口
components/             客户端 UI 组件
prisma/schema.prisma    Prisma 模型（User、Account、trizEntry、Analysis）
util/                   Clerk 认证工具、Prisma 客户端、AI 链、API 调用
middleware.ts           Clerk 中间件，保护应用路由
```

### 可用脚本

| 命令 | 说明 |
| --- | --- |
| `npm run dev` | 启动开发服务器 |
| `npm run build` | 构建生产版本 |
| `npm run start` | 运行生产版本 |
| `npm run lint` | 通过 Next.js 运行 ESLint |

### 部署

应用可以部署到 Vercel 或任意 Node.js 主机。请配置与上面相同的环境变量，并确保 `DATABASE_URL` 指向可访问的 MySQL 实例。

### 说明与限制

- AI 分析需要有效的 `OPENAI_API_KEY`，否则请求会失败。
- 模型为 `gpt-3.5-turbo`，`temperature` 设为 0；生成结果仍仅供参考，请自行复核。
- 仓库目前没有自动化测试。
- Prisma schema 使用 MySQL；如需其他数据库，请修改 `provider` 和 `DATABASE_URL`。
