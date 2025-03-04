# 从 v3 迁移

## 包名变更

::: tip 重要
请开发者直接依赖 koishi 而非 @koishijs/core 进行开发。
:::

- koishi-core 与 node 解耦后更名为 @koishijs/core
- koishi-utils 与 node 解耦后更名为 @koishijs/utils
- 配置文件加载相关功能独立为 @koishijs/loader
- koishi 为上述库加上 node 相关代码的整合
- **现存的官方插件都改为 @koishijs/plugin-xxx**
- **所有官方适配器也调整为插件**，名称与上一条一致
- koishi-test-utils 被拆分为多个部分：
  - 数据库测试相关代码移至 @koishijs/database-tests
  - 测试工具重构后成为 @koishijs/plugin-mock

### 插件拆分

为了提供更细粒度的插件功能，我们将部分插件拆分为了多个包进行发布。

- 从核心拆分出下列插件：
  - [@koishijs/plugin-help](../plugins/common/help.md)
  - [koishi-plugin-rate-limit](https://common.koishi.chat/plugins/rate-limit.html)
- koishi-plugin-assets 被拆分为多个插件：
  - [koishi-plugin-assets-local](https://assets.koishi.chat/plugins/local.html)
  - [koishi-plugin-assets-remote](https://assets.koishi.chat/plugins/remote.html)
  - [koishi-plugin-assets-smms](https://assets.koishi.chat/plugins/smms.html)
  - 由于这些插件实现了同一个服务，你只需安装其中的一个即可
- koishi-plugin-common 被拆分为多个插件：
  - [@koishijs/plugin-admin](../plugins/common/admin.md)
  - [@koishijs/plugin-bind](../plugins/common/bind.md)
  - [@koishijs/plugin-broadcast](../plugins/common/broadcast.md)
  - [@koishijs/plugin-callme](../plugins/common/callme.md)
  - [@koishijs/plugin-echo](../plugins/common/echo.md)
  - [koishi-plugin-feedback](https://common.koishi.chat/plugins/feedback.html)
  - [koishi-plugin-forward](https://common.koishi.chat/plugins/forward.html)
  - [koishi-plugin-recall](https://common.koishi.chat/plugins/recall.html)
  - [koishi-plugin-repeater](https://common.koishi.chat/plugins/repeater.html)
  - [koishi-plugin-respondent](https://common.koishi.chat/plugins/respondent.html)
  - [koishi-plugin-sudo](https://common.koishi.chat/plugins/sudo.html)
  - [koishi-plugin-verifier](https://common.koishi.chat/plugins/verifier.html)
- koishi-plugin-webui 被拆分为多个插件：
  - @koishijs/client (构建工具)
  - [@koishijs/plugin-console](../plugins/console/index.md)
  - [@koishijs/plugin-analytics](../plugins/console/analytics.md)
  - [@koishijs/plugin-insight](../plugins/console/insight.md)
  - [@koishijs/plugin-status](../plugins/console/status.md)
  - 我们还引入了更多控制台插件，请继续阅读下面的介绍

### 新增的包

- 开发相关：
  - @koishijs/plugin-hmr：提供插件级别的热重载功能
- 命令行相关：
  - create-koishi：可结合 npm init 或 yarn create 使用，用于快速搭建项目
  - @koishijs/scripts：用于模板项目的命令行工具
- 适配器相关：
  - [@koishijs/plugin-adapter-dingtalk](../plugins/adapter/dingtalk.md)：钉钉适配器
  - [@koishijs/plugin-adapter-lark](../plugins/adapter/lark.md)：飞书适配器
  - [@koishijs/plugin-adapter-line](../plugins/adapter/line.md)：LINE 适配器
  - [@koishijs/plugin-adapter-mail](../plugins/adapter/mail.md)：邮件适配器
  - [@koishijs/plugin-adapter-matrix](../plugins/adapter/matrix.md)：Matrix 适配器
  - [@koishijs/plugin-adapter-qq](../plugins/adapter/qq.md)：QQ 适配器
  - [@koishijs/plugin-adapter-slack](../plugins/adapter/slack.md)：Slack 适配器
  - [@koishijs/plugin-adapter-wechat-official](../plugins/adapter/wechat-official.md)：微信公众号适配器
  - [@koishijs/plugin-adapter-wecom](../plugins/adapter/wecom.md)：企业微信适配器
  - [@koishijs/plugin-adapter-whatsapp](../plugins/adapter/whatsapp.md)：WhatsApp 微信适配器
- 数据库相关：
  - [@koishijs/plugin-database-memory](../plugins/database/memory.md)：基于内存的数据库实现
  - [@koishijs/plugin-database-sqlite](../plugins/database/sqlite.md)：SQLite 数据库实现
- 控制台相关 (部分插件也可脱离控制台使用)：
  - [@koishijs/plugin-auth](../plugins/console/auth.md)：用户登录
  - [@koishijs/plugin-commands](../plugins/console/commands.md)：指令管理
  - [@koishijs/plugin-config](../plugins/console/config.md)：插件配置
  - [@koishijs/plugin-dataview](../plugins/console/dataview.md)：数据库操作
  - [@koishijs/plugin-logger](../plugins/console/logger.md)：日志管理
  - [@koishijs/plugin-market](../plugins/console/market.md)：插件市场
  - [@koishijs/plugin-sandbox](../plugins/console/sandbox.md)：沙盒调试
- 杂项：
  - [@koishijs/plugin-inspect](../plugins/common/inspect.md)：会话信息

### 移除的包

下列包由于使用场景和用途的限制，不再进行官方维护。这些包会继续留在 Koishi 组织中。

- [koishi-plugin-chess](https://github.com/koishijs/koishi-plugin-chess) (社区维护)
- [koishi-plugin-image-search](https://github.com/koishijs/koishi-plugin-image-search) (社区维护)
- ~~[koishi-plugin-tomon](https://github.com/koishijs/koishi-plugin-tomon)~~ (已归档)
- [koishi-plugin-tools](https://github.com/koishijs/koishi-plugin-tools) (拆分后由社区维护)
- ~~[koishi-plugin-monitor](https://github.com/koishijs/koishi-plugin-monitor)~~ (已归档)

## 核心功能变更

### 概念用词变更

所有涉及「群组」的概念，对应英文单词从 group 更改为 guild。下面是一些例子：

```diff
- session.groupId
+ session.guild.id
- bot.getGroupMember()
+ bot.getGuildMember()
- ctx.on('group-request')
+ ctx.on('guild-request')
```

这样修改是为了提供更好的兼容性，减轻 group 本身在多种场合使用所带来的二义性。

### 插件变更

- 移除了 before-connect 和 before-disconnect 事件，请直接使用 ready 和 dispose 事件代替
- 新增了 [Schema API](../api/utils/schema.md)，用于描述插件的配置项，下面是一个例子：

```ts
// @errors: 2749

export const name = 'foo'

export const Config: Schema<Config> = Schema.object({
  bar: Schema.string().default('baz').description('这是一个配置项'),
})

export function apply(ctx: Context, config: Config) {
  config.bar // string
}
```

我们强烈建议开发者在 Koishi v4 插件的开发中为自己的每一个公开插件提供 schema 字段，基于下面的两点好处：

1. 能够在插件被加载前就对插件的配置项进行类型检查，并提供缺省值和更多预处理
2. 如果你希望自己的插件能够**在插件市场被动态安装**，那 schema 会作为网页控制台中呈现的配置表单

### 适配器变更

适配器现在通过插件的形式导入了：

```ts title=koishi.ts
// @errors: 2528

// before
export default {
  bots: [ /* 机器人配置项 */ ],
  onebot: { /* 适配器配置项 */ },
}

// after
export default {
  plugins: {
    onebot: {
      bots: [ /* 机器人配置项 */ ],
      /* 适配器配置项 */
    },
  },
}
```

同时我们也调整了一些机器人配置项，并支持了一些全新的特性。下面举一些例子：

```yaml title=koishi.yml
plugins:
  onebot:
    # 如果只有一个 bot，你仍然可以像 v3 一样直接写在这里，不用专门提供 bots 数组
    protocol: http      # 相当于过去的 type: 'onebot:http'
    disabled: true      # 不启动，可以配合网页控制台动态控制运行状态
    platform: qq        # 此时账户信息将从 user.qq 而非 user.onebot 访问
                        # 你还可以对同一个适配器下的多个 bot 实例设置多个不同的平台
```

### 应用变更

- 调整了 app.bots 接口的部分用法（参见文档）
- 新增了 `ctx.http` 接口，移除了所有的 `axiosConfig` 配置

除此以外，如果你使用 @koishijs/cli，那么有一些额外的配置项变更：

- 新增 `logger` 配置项，包含了过去的 `logLevel` 等一系列配置，同时支持将输出日志写入本地文件

### 数据库变更

- 接口变更
  - 新增了方法 `db.set(table, query, updates)`
  - `db.update()` 更名为 `db.upsert()`，语法不变
- 数据结构变更
  - channel 表使用 `platform`+`id` 复合主键进行索引，这意味着 `channel.id` 语义将发生变化，同时新增了 `channel.platform`
- 全局接口变更
  - ORM 相关接口现使用 `ctx.model` 实现

### 事件变更

- connect → ready (原命名依然可用)
- before-connect → ready
- disconnect → dispose (原命名依然可用)
- before-disconnect → dispose
- before-command → command/before-execute (原命名依然可用)

## 其他变更

### @koishijs/core

- `ctx.all()` 更名为 `ctx.any()`，同时新增了 `ctx.never()`
- 移除了 `processMessage` 配置项，即取消了内置的将中文字符替换为简体字的机制
- 废弃了 `Command.userFields()` 和 `Command.channelFields()` 方法，请使用对应的事件 `command/before-attach-user` 和 `command/before-attach-channel` (注意这里废弃的只是静态方法，实例方法依然可用)

### @koishijs/utils

- 移除了 `Random.uuid()` 方法，新增了 `Random.id()` 方法
- 移除了 `simplify()` 和 `traditionalize()` 方法，请使用 [simplify-chinese](https://www.npmjs.com/package/simplify-chinese) 这个包
- Observer API 改动：所有 `_` 前缀替换为 `$` 前缀，例如 `session.user.$update()`

### @koishijs/plugin-adapter-onebot

- `server` 配置项更名为 `endpoint`
- 由于快速响应已经不属于 OneBot 标准，我们移除了对快速响应的支持
