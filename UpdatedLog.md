## 1.1.4
### Fixes and Improvements:
1. 修复: 不能加载部分第三方的 AuthDefaultRequest 的 bug

## 1.1.3
### Fixes and Improvements:
1. 优化: Auth2RequestHolder.getAuth2DefaultRequest(..) 与 Auth2RequestHolder.getProviderIdBySource(..) 方法.
2. 优化: 解决 log 中异常信息不详细.

## 1.1.2
### Fixes and Improvements:
1. 改进: 通过适配器模式对 AuthDefaultRequest 子类进行适配取代对 AuthDefaultRequest 子类的逐个继承的方式. 因 CSDN 与 FEISHU 不支持第三方授权登录故删除此第三方的支持.
2. 优化: 日志重复记录异常调用链的问题

## 1.1.1
### Fixes and Improvements:
1. 优化: 第三方授权登录获取授权链接时, 如果请求的第三方不在应用支持第三方服务商范围内, 跳转授权失败处理器处理.

## 1.1.0
### Fixes and Improvements:
1. 改进: 考虑到很多应用都有自己的定时任务应用, 提取 Executor 配置放入 executor 包, 从定时任务 RefreshAccessTokenJob 中拆分出 RefreshAccessTokenJobHandler
, RefreshTokenJob 接口的实现已注入 IOC 容器, 方便自定义定时任务接口时调用.
2. 依赖: 依赖升级到 spring-security:5.4.1, spring-boot:2.3.5.RELEASE.
3. 优化: 删除不必要的属性: ums.oauth.enabled.

## 1.0.12
### Fixes and Improvements:
1. 修复: 不能对部分通过 Filter 实现的逻辑进行 MDC 日志链路追踪的 bug, 如: 第三方授权登录, 因为 interceptor 拦截在 Filter 之后.

## 1.0.11
### Fixes and Improvements:
1. 修复: 第三方授权登录时, 缓存到 redis 时, 设置 state 缓存时间时少个时间单位, 变成 offset错误的 bug. 感谢: 永生的灯塔水母

## 1.0.10
### Fixes and Improvements:
1. 修复: enableRefreshTokenJob 属性不能控制是否开启定时刷新 accessToken 任务的 bug.

## 1.0.9
### Fixes and Improvements:
1. 修复: 示例功能, 当 signUpUrl=null 时, 成功处理器未生效的bug
2. 特性: 支持基于 SLF4J MDC 机制的日志链路追踪功能

## 1.0.8
### Fixes and Improvements:
1. 修复: 生成 userConnectionUpdateExecutor 时 maximumPoolSize 小于 corePoolSize 的 bug. 感谢: 永生的灯塔水母
2. 优化: 修改 signUpUrl 相关的注释,文档, 示例, 增加 signUp.html 提示页面

## 1.0.7
### Fixes and Improvements:
1. 修复: AuthStateRedisCache.java containsKey(key) 方法的 bug. 感谢: 永生的灯塔水母
2. 优化: signUpUrl 的处理方式: 增加如果 signUpUrl == null 时不跳转, 直接由开发者在成功处理器上自己处理.

## 1.0.6
### Fixes and Improvements:
1. 增强: RememberMeAuthenticationToken 的反序列化, 优化反序列化配置(Auth2Jackson2Module).

## 1.0.5
### Fixes and Improvements:
1. 增强: 添加了一些 Authentication 与 UserDetails 子类的反序列化器, 以解决 redis 缓存不能反序列化此类型的问题.
具体配置 redis 反序列器的配置请看 RedisCacheAutoConfiguration.getJackson2JsonRedisSerializer() 方法

## 1.0.4
### Fixes and Improvements:
1. 增强: 添加在不支持自动注册时, 创建临时用户 TemporaryUser 后跳转 signUpUrl, signUpUrl 可通过属性设置, 再次获取 TemporaryUser 通过 SecurityContextHolder.getContext().getAuthentication().getPrincipal().
2. 优化: 添加 UmsUserDetailsService.generateUsernames(AuthUser authUser) 接口默认实现方法, 便于开发者对与用户命名规则的自定义.
3. 优化: 更改接口 UmsUserDetailsService 的方法名称: existedByUserIds -> existedByUsernames. 更新方法说明.
4. 优化: 更新时序图, 更新 example 与 README.
5. 兼容: 去除对 org.apache.commons.lang3.StringUtils 依赖.
6. 改进: 更新 JustAuth 到 1.15.8.

## 1.0.2
### Improvements:
1. 兼容: 去除 JDK11 API, 增加对 JDK8的兼容性, 使用 JDK8 编译项目

## 1.0.0
### Fixes and Improvements:
1. 集成 JustAuth, 支持所有 JustAuth 支持的第三方授权登录，登录后自动注册或绑定。
2. 支持定时刷新 accessToken, 支持分布式定时任务。
3. 支持第三方授权登录的用户信息表与 token 信息表的缓存功能。
4. 支持第三方绑定与解绑及查询接口(top.dcenter.ums.security.core.oauth.repository.UsersConnectionRepository).
5. 支持线程池配置
6. 添加抑制非法反射警告, 适用于jdk11, 通过属性可以开启与关闭
7. 添加 spring cache 对 @Cacheable 操作异常处理, 缓存穿透处理(TTL随机20%上下浮动), 缓存击穿处理(添加对null值的缓存, 新增与更新时更新null值)
8. 移除 fastJson, 改成 Jackson.