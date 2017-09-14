## GraphQL 入门

首先，GraphQL 并不是一个图数据库查询语言，而是 API 的查询语言

### API 的查询语言

GraphQL 是基于数据的满足运行时查询的 API 查询语言，GraphQL 在 API 中提供了完整和易于理解的数据描述。这将使客户端有能力询问其所需要的精确数据，同时使随时增强 API 变得简单。

#### 精确描述你的数据

你的数据有那些，每种数据的格式又是怎样的，都需要由 GraphQL 严格定义。

```
type Project {
  name: String
  tagline: String
  contributors: [User]
}
```

### 你需要怎样的数据

确定你的查询条件，这将决定你会拿到怎样的数据

```graphql
{
  project(name: "GraphQL") {
    tagline
  }
}
```

#### 拿到你要的东西

你将拿到你需要的数据，如你所预料的，不多不少。

```json
{
  "project": {
    "tagline": "A query language for APIs"
  }
}
```

### GraphQL 的特征

#### 精确拿到你需要的数据

根据你的查询条件，你将拿到的数据是可被精确预测的。同时，GraphQL 也将保证高速和稳定，因为是由 GraphQL 控制数据，而不是服务器。

#### 在一个请求中获取多种资源

GraphQL 的查询不是简单获取一个资源，而是根据资源间的引用顺畅地获取多种资源。与传统的 REST 风格的 API 中总是需要从多个 URL 中加载数据不同的是，GraphQL 只需要从一个请求中就能加载所有的数据，这在较慢的网络情况下十分有用。

#### 使用类型系统描述数据

GraphQL API 中使用类型和字段来描述数据。例如；

```
type Species {
  name: String
  lifespan: Int
  origin: String
}
```

#### 不通过版本就可以增强你的 API

在不影响现有查询的条件下，你可以向你的 GraphQL API 中添加新的类型和字段来增强你的 API。持续性的增强也会使服务器代码易于维护。

#### 不局限于特定的服务器语言

GraphQL 不局限于特定的语言，GraphQL 支持很多语言。

### 误区和问题

- GraphQL 与图数据库无关，其可以和任何的数据库很好工作。
- GraphQL 向客户端提供了高度的查询能力，但这不意味着是危险的，至少不会比 REST 危险。
- GraphQL 应该与数据库的验证和授权是无关的，数据库的处理应完全在 GraphQL 之外。
- GraphQL 擅长查询，但是对于 CRUD，都是有解决方案和相关实践的。
- GraphQL 虽然能在很大程度上对网络请求进行优化，但是在数据库层面的查询还存在性能问题，一般处理起来不是那么容易。
