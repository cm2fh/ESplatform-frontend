### 项目介绍

基于 Spring Boot + Elastic Stack + Vue 3 的一站式信息聚合搜索平台。用户可在同一页面集中搜索出不同来源、不同类型的内容（文章、图片、用户），提升搜索体验。

### **主要工作**

#### 后端

1.  基于自己二次开发的 Spring Boot 初始化模板 + MyBatis X 插件，快速生成基本数据源的增删改查。
2.  数据源获取：
    -   使用 HttpClient 请求离线获取外部网站的文章，并使用 Hutool 的 JSONUtil 解析和预处理文章，最终入库。
    -   使用 jsoup实时请求 bing 搜索接口获取图片，并使用 CSS Selector 语法解析和预处理图片信息，最终返回给前端。
3.  为实现多类数据源的整体搜索，使用门面模式在后端对各类数据源的搜索结果进行聚合，统一返回给前端，减少了前端请求次数以及前端开发复杂度。并通过 CompletableFuture 并发搜索各数据源进一步提升搜索接口性能。
4.  为提高聚合搜索接口的通用性，首先通过定义数据源接口来实现统一的数据源接入标准；当新数据源（比如视频）要接入时，只需使用适配器模式对其数据查询接口进行封装、以适配数据源接口，无须修改原有代码，提高了系统的可扩展性。
5.  为减少代码的圈复杂度，使用注册器模式代替 if else 来管理多个数据源对象，调用方可根据名称轻松获取对象。
6.  为解决文章搜不出的问题，自主搭建 Elasticsearch 来代替 MySQL 的模糊查询，并通过为索引绑定 ik 分词器实现了更灵活的分词搜索。
7.  构建 ES 文章索引时，采用动静分离的策略，只在 ES 中存储要检索的、修改不频繁字段（比如文章）用于检索，而修改频繁的字段（比如点赞数）从数据库中关联查出，从而减少了 ES 数据更新和同步的成本、保证数据一致性。
8.  为了更方便地管理 Elasticsearch 中的数据，自主搭建 Kibana 并配置 index pattern 和看板，实现对文章数据的可视化管理。
9.  开发搜索功能时，使用 Kibana DevTools + DSL 调试 ES 的搜索效果，并使用 Spring Data Elasticsearch 的 QueryBuilder 组合查询条件，实现对 ES 内文章的灵活查询。
10.  自主搭建 Logstash 实现同步 MySQL 的文章数据到 ES，并通过指定 tracking\_column 为更新时间字段解决重复更新的问题。

#### 前端

1.  基于 Vue 3 + Ant Design Vue 实现响应式页面开发，使用 Tab 组件 + Vue Router 动态路由实现统一页面布局，通过用户点击 Tab 时更改路由来切换各类数据的搜索结果，并选用不同的组件进行展示。
2.  使用 TypeScript + ESLint + Prettier + Husky 保证项目编码和提交规范，提高项目的质量。

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```
