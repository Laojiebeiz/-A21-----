# 金融企业智能办公提效平台

2026「数字马力杯」浙江省大学生服务外包创新应用大赛 A21 赛题（恒生电子）参赛项目。

当前进度：第九阶段「测试与验收」已完成——113 个单元测试全部通过，JaCoCo 行覆盖率 87.15%（门槛 80%）。

## 测试与覆盖率

```bash
cd hs-office-server
mvn clean verify   # 运行全部测试并执行 JaCoCo 80% 覆盖率门槛校验
```

覆盖率报告：`hs-office-server/target/site/jacoco/index.html`。

## 前端启动

```bash
cd hs-office-web
npm install
npm run dev   # 开发地址 http://localhost:5173，接口已代理到 http://localhost:8080
```

登录后进入首页工作台，各板块独立加载，单个板块异常不影响其余板块渲染。

## 行业热点使用说明

- 新闻源配置：`app.news.rss-sources`，支持 http/https RSS 与本地 XML 文件路径；
- 本地示例源：`data/news/sample-finance-rss.xml`（离线演示可用），接入真实源时在配置中追加 `name/url`；
- 手动抓取：`POST /api/v1/news/crawl`；任务进度：`GET /api/v1/news/crawl/tasks/{taskNo}`；
- 异步开关 `app.news.async-enabled`：`false` 同步执行（当前 dev 默认），接入 RabbitMQ 后改 `true`；
- 定时抓取：`app.news-crawl.enabled=true` 后按 `app.news-crawl.cron` 周期执行。

## 技术栈

- 后端：JDK 17 LTS、Spring Boot 3.4.4、MyBatis-Plus 3.5.7、MySQL 8、Redis 7、RabbitMQ、Maven
- 前端：Vue 3、Vite、Element Plus、Pinia、Vue Router、Axios

## 目录说明

- `docs/`：架构、目录、数据库、接口、实施步骤设计文档
- `sql/`：建库建表与初始化数据脚本（第二阶段落地）
- `hs-office-server/`：后端 Spring Boot 工程
- `hs-office-web/`：前端 Vue 3 工程
- `docker/`：MySQL / Redis / RabbitMQ 本地环境编排（后续补充）

## 演示账号

- 系统管理员：`admin / Admin@123`
- 其余用户：`zhangsan、wangwu、lisi、zhaoliu、sunqi`，密码统一 `123456`

数据库连接见 `hs-office-server/src/main/resources/application-dev.yml`（当前 MySQL：`root / 1123`）。

## 消息集成使用说明

- 本地导入目录：`data/messages/`，按子目录区分消息源：`WECOM/`、`DINGTALK/`、`QQMAIL/`；
- 支持文件格式：CSV（带表头）、JSON 数组、JSONL，字段见 `data/messages` 内示例；
- 手动同步：`POST /api/v1/message-sync/trigger`；上传导入：`POST /api/v1/messages/import`；
- 异步开关 `app.message.async-enabled`：`true` 走 RabbitMQ，`false` 同步执行（本机无 Broker 时默认 false，接入 RabbitMQ 后改 true）；
- 大模型开关 `app.llm.enabled`：默认 false，使用规则摘要降级；开启需填写 `base-url / api-key / model`。

## 本地启动

环境要求：

- JDK 17 LTS（构建与运行均需 `JAVA_HOME` 指向 JDK 17）
- Maven 3.9+（本机未安装时可使用 IntelliJ IDEA 内置 Maven）
- Node.js 20+

```bash
# 后端（JDK 17）
cd hs-office-server
mvn spring-boot:run

# 前端
cd hs-office-web
npm install
npm run dev
```

后端默认端口 8080，接口文档地址：<http://localhost:8080/swagger-ui.html>。
