# 什么是 Redis，如何与 Nest 配合使用？射流研究…

> 原文：<https://javascript.plainenglish.io/what-is-redis-and-how-to-use-it-with-nest-js-3cd1de0fe13b?source=collection_archive---------1----------------------->

## 使用码头集装箱化

![](img/804967fcc41d241d166e246515b10151.png)

Photo by [Jan Antonin Kolar](https://unsplash.com/@jankolar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

今天我们将谈论 Redis。一个非常强大和易于使用的数据库。它在缓存方面非常流行。

## 先决条件

*   对缓存的基本理解
*   NestJS 的经验

## 问题是

我们公司正在构建一个物业管理解决方案，我们必须维护许多用户，他们可以根据自己的角色拥有不同的权限。

因此，每次成功登录后，客户端都会请求获取用户的详细资料以及权限。由于查询非常复杂，需要连接多个表，因此需要很长时间才能加载。

并且用户必须等待很长时间才能在他们的屏幕上看到任何东西，这是非常糟糕的用户体验。

## 解决方案

因此，在挖掘了一些可能的解决方案后，我们发现**缓存**是我们的答案。主要因为 2 个原因

*   这个 API 被调用的非常频繁。
*   用户权限不会经常改变

因此，经过一番挖掘，很明显 Redis 可能是我们的最佳选择。并且 **Redis** 是缓存的好选择

## 那么为什么是 Redis 呢？

*   Redis 真的真的很快。它是用 C 语言写的，所以非常高效。
*   非常好用。它几乎就像我们浏览器的本地存储，但功能更强大。您只需用一个键设置一些数据，然后用同一个键取回这些数据。
*   所有主流语言/框架都支持 Redis。
*   它非常受欢迎，被许多大公司使用，所以稳定性根本不是问题

NestJS 使得使用 Redis 变得非常容易。现在我们来看看如何使用它。

## 第一步。安装依赖项

安装以下依赖项

```
yarn add cache-manager cache-manager-redis-store
```

## 第二步。添加缓存模块

然后创建一个用于缓存的模块

```
nest g module redis-cache
```

然后将下面的代码添加到您的`redis-cache.module.ts`文件中

```
import { CacheModule, Module } from '@nestjs/common';
import { RedisCacheService } from './redis-cache.service';
import { ConfigModule, ConfigService } from '@nestjs/config';
import * as redisStore from 'cache-manager-redis-store';

@Module({
    imports: [
        CacheModule.*registerAsync*({
            imports: [ConfigModule],
            inject: [ConfigService],
            useFactory: async (configService: ConfigService) => ({
                store: redisStore,
                host: configService.get('REDIS_HOST'),
                port: configService.get('REDIS_PORT'),
                ttl: configService.get('CACHE_TTL'),
                max: configService.get('MAX_ITEM_IN_CACHE')
            })
        })
    ],
    providers: [RedisCacheService],
    exports: [RedisCacheService]
})
export class RedisCacheModule {}
```

这里需要注意一些事情…

`REDIS_HOST`:指定我们 Redis 数据库的主机(例如:localhost)

`REDIS_PORT`:默认端口值为 6479

`CACHE_TTL`:指定一个值失效前的时间(秒)

`MAX_ITEM_IN_CACHE`:指定缓存中应该保存的最大项数。

## 第三步。创建服务

创建新的缓存服务

```
nest g service redis-cache
```

然后在`redis-cache.service.ts`文件中添加以下代码

```
import { ***CACHE_MANAGER***, Inject, Injectable } from '@nestjs/common';
import { Cache } from 'cache-manager';

@Injectable()
export class SomeService {
    constructor(@Inject(***CACHE_MANAGER***) private readonly cache: Cache) {}

    async get(key): Promise<any> {
        return await this.cache.get(key);
    }

    async set(key, value) {
        await this.cache.set(key, value, 1000);
    }

    async reset() {
        await this.cache.reset();
    }

    async del(key) {
        await this.cache.del(key);
    }
}
```

这些是使用缓存的实用函数。

## 第四步。使用缓存服务

在任何服务内部，现在都可以将 RedisCacheModule 导入到不同的模块中。

```
@Module({
    imports: [RedisCacheModule],
    controllers: [UserController],
    providers: [UserService],
    exports: [UserService]
})
export class SomeModule {}
```

然后在该模块内的任何服务中使用它

```
@Injectable()
export class UserService {
    constructor(private cacheManager: RedisCacheService) {} async setSomeValue(KEY , value){
      await this.cacheManager.set(KEY , value);
   } async getSomeValue(KEY){
      await this.cacheManager.get(KEY);
   }}
```

## 额外收获:将整个申请归档

如果您现在运行您的应用程序，您将遇到一个错误，因为您没有启动和运行 Redis 数据库。你有两个选择

*   在本地机器上安装并运行 Redis
*   使用 docker 从 docker hub 中提取 Redis 映像。

现在我们将看看以后如何做。

主 docker 文件是任何 nodeJS 应用程序的典型文件

```
FROM node:12-alpine
WORKDIR /app
ADD package.json /app/package.json
RUN yarn install
ADD . /app
EXPOSE 3000
CMD ["yarn", "start"]
```

我们必须添加另一个和`docker-compose.yml`文件

```
version: '3'
services:
  redis:
    image: 'redis:alpine'
  node-app:
    build: .
    ports:
      - "3000:3000"
```

这里，我们从 docker hub 中提取官方的 Redis 映像并运行它，以便我们的应用程序可以访问它。

> 如果你从 docker 镜像运行 Redis，你必须将`REDIS_HOST`值(在步骤 2 中使用)改为`redis`(在 docker-compose 文件中使用)

现在您应该对在您的下一个项目中使用 Redis 更加熟悉了。这确实是一个非常有用的工具，可以放在开发人员的腰带下面。

今天到此为止。编码快乐！:D

**通过** [**LinkedIn**](https://www.linkedin.com/in/56faisal/) **或我的** [**个人网站**](https://www.mohammadfaisal.dev/) **与我取得联系。**