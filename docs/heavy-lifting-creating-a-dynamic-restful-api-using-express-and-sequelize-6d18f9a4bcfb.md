# 使用 Express 和 Sequelize 创建动态 RESTful API

> 原文：<https://javascript.plainenglish.io/heavy-lifting-creating-a-dynamic-restful-api-using-express-and-sequelize-6d18f9a4bcfb?source=collection_archive---------1----------------------->

## 什么是 API，RESTful 意味着什么，以及如何使用 Node.js 实现这些

![](img/c499320836fc52c53193e0c25608aab1.png)

# 介绍

在本文中，我们将讨论什么是 API，RESTful 意味着什么，以及如何使用 Node.js 实现这些。我们将使用的 Node.js 包将是用于 API 端点的 Express 和用于查询数据库的 Sequelize。

学习如何创建一个应用编程接口是一个微妙的过程。开发人员只是想快速构建端点，这样他们就可以快速为网页做好消费准备。然而，学习如何使事情 RESTful 将使您的 API 更加一致、可预测和可伸缩。

本文假设您知道如何创建一个 Express 服务器并使用 Sequelize 连接到一个数据库。

如果您只想进入代码，完整的代码示例是可用的。

[](https://github.com/andrewgbliss/tutorial-sequelize-router) [## andrewgbliss/教程-sequelize-路由器

### 通过在 GitHub 上创建一个帐户，为 andrewgbliss/tutorial-sequelize-router 开发做出贡献。

github.com](https://github.com/andrewgbliss/tutorial-sequelize-router) 

# 术语

让我们定义一下构建 RESTful API 的含义。

创建一个 API ( **A** 应用程序 **P** 编程 **I** 接口)是我们如何设置逻辑动作以在应用程序中执行某些功能。例如，我们可以设置一个名为“创建客户”的函数，它将为我们提供一个界面，所以我们不需要理解它将如何实际创建客户，我们需要知道的只是如何调用该函数。然后由程序员来实现对 API 的要求。

创建 RESTful(**RE**presentation**S**state**T**transfer)API 意味着我们将遵循如何通过使用 HTTP 方法在逻辑上设置端点的模式。例如，当您在浏览器中键入要转到的网页时，它将调用 HTTP GET 方法来检索要显示的网页。我们将在后面讨论创建 RESTful API 所需要知道的所有方法。

# 入门指南

在我们开始之前，让我们计划一下构建我们的 API 需要什么。例如，假设我们有一个反应应用程序，它有一个客户页面。在此页面上，用户可以创建、显示所有客户、更新和删除客户。然后，反应应用程序将对我们的 Express 服务器进行 API 调用，反过来，API 将对我们的数据库执行操作。我们还需要用版本号作为 API 端点的前缀(如果您需要构建更多的 API 但需要维护旧版本，这很有用)。

# RESTful 端点

## GET/API/v1/客户

这将返回我们数据库中的一系列客户。

## GET/API/v1/customers/id:

这会返回一个 *:id* 参数指定的客户。

## POST/API/v1/客户

这将在我们的数据库中创建一个客户。

## PUT/API/v1/customers/id:

这会更新 *:id* 参数指定的客户。

## DELETE/API/v1/customers/id:

这将删除由: *id* 参数指定的客户。

[](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) [## HTTP 请求方法

### HTTP 定义了一组请求方法来指示对给定资源要执行的操作。虽然…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) 

上面的链接介绍了每个 HTTP 方法是什么以及它的意图是什么。让我们快速浏览一下我们将使用的 HTTP 方法。

**GET:** 这个 HTTP 方法将返回位于服务器上的资源。

**POST:** 这个 HTTP 方法将创建一个资源。

这个 HTTP 方法将更新一个资源。

**删除:**这个 HTTP 方法将删除一个资源。

遵循 HTTP 方法及其目的，意味着我们可以开始构建一个可预测的 RESTful API。然而，我们不希望每次创建新的数据库资源时都编写全新的路由器。让我们写一些代码来帮我们完成所有繁重的工作。

# 快速路由器

创建一个 Express 路由器是直接的和非个人化的，所以我们希望保持这种灵活性，但同时我们不希望一遍又一遍地编写相同类型的路由器。

因此，让我们来看一个路由器，我们可以为我们的客户编写这个 RESTful API。

```
import express, { Request, Response } from 'express';
import db from '../../db';
const model = db.Customers;
const router = express.Router();const getCustomers = async (req: Request, res: Response) => {
  const results = await model.findAll();
  res.json(results);
};const getCustomer = async (req: Request, res: Response) => {
  const results = await model.findByPk(req.params.id);
  res.json(results);
};const createCustomer = async (req: Request, res: Response) => {
  const results = await model.create(req.body);
  res.json(results);
};const updateCustomer = async (req: Request, res: Response) => {
  const results = await model.update(req.body, {
    where: {
      id: req.params.id,
    },
  });
  res.json(results);
};const deleteCustomer = async (req: Request, res: Response) => {
  const results = await model.destroy({
    where: {
      id: req.params.id,
    },
  });
  res.json(results);
};router.get('/', getCustomers);
router.get(`/:id`, getCustomer);
router.post('/', createCustomer);
router.put(`/:id`, updateCustomer);
router.delete(`/:id`, deleteCustomer);export default router;
```

这种方法没有错，如果您需要做额外的事情，比如发送电子邮件或进行其他 api 调用，您可以拥有充分的灵活性。但是，您可以从这个示例中看到，它几乎是一个样板路由器，您可以复制并粘贴到另一个路由器中，只需将 customer 的名称更改为其他资源的名称。

因此，让我们制作一个中间件，它可以为我们完成所有这些繁重的工作。

首先，让我们创建一些助手中间件来帮助处理 async /await 函数，并以 JSON 格式返回我们的调用。

## 助手中间件

```
const asyncEndpoint = (endpoint) => {
  return async (req: Request, res: Response, next: NextFunction) =>
  {
    try {
      await endpoint(req, res, next);
    } catch (e) {
      next(e);
    }
  };
};
```

这将使一个路由能够被包装在一个 async / await 函数中，如果发生任何错误，我们可以捕捉它并将其发送到下一个 Express 函数。

```
const toJson = (req: Request, res: Response) => {
  res.status(200).json(req.results);
};
```

这将被放置在每个路由的末尾，如果路由成功，它将返回 JSON 形式的响应。

```
import asyncEndpoint from './asyncEndpoint';export const validateSchema = (...schemas) => {
  return asyncEndpoint(async (req, res, next) => {
    for (let schemaItem of schemas) {
      const { schema, path } = schemaItem;
      let validation = schema.validate(req[path], {
        abortEarly: false,
      });
      if (validation.error) {
        let messages = validation.error.details.map((i) => i.message);
        let errMessage = `Validation errors: ${messages.join(', ')}`;
        throw {
          status: 400,
          message: errMessage,
        };
      }
    }
    next();
  });
};
```

这将是我们的验证中间件，将使用 hapi / joi 包。

```
export const withSequelize = (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const db = req.app.get('db');
  const { Sequelize } = db;
  const {
    page = 0,
    limit = 100,
    order = '',
    attributes = ['id'],
    include,
  }: any = req.query;
  let options: SequelizeOptions = {
    offset: page === 0 ? 0 : parseInt(page) * parseInt(limit),
    limit: parseInt(limit),
  };
  let conditions = {}; if (order && isString(order)) {
    const [column, direction = 'ASC'] = order.split(',');
    options.order = [[Sequelize.col(column), direction]];
  } else if (order && isArray(order)) {
    options.order = order.map((orderGroup = '') => {
      const [column, direction = 'ASC'] = orderGroup.split(',');
      return [Sequelize.col(column), direction];
    });
  } if (attributes && isString(attributes)) {
    options.attributes = attributes.split(',');
  } else if (attributes && isArray(attributes)) {
    options.attributes = attributes;
  } if (attributes && isString(attributes)) {
    options.attributes = attributes.split(',');
  } else if (attributes && isArray(attributes)) {
    options.attributes = attributes;
  } if (include && isArray(include)) {
    options.include = include.map((includeModel) => {
      const { model, as, attributes, ...rest }: any = qs.parse(
        includeModel,
        ';',
        '='
      );
      const include: Include = {
        model: db[model],
      };
      if (as) {
        include.as = as;
      }
      if (attributes) {
        include.attributes = attributes.split(',');
      }
      const otherColumns = omit(rest, [
        'page',
        'limit',
        'order',
        'attributes',
        'include',
      ]);
      if (otherColumns) {
        include.where = otherColumns;
      }
      return include;
    });
  } const otherColumns = omit(req.query, [
    'page',
    'limit',
    'order',
    'attributes',
    'include',
  ]); if (otherColumns) {
    conditions = {
      where: otherColumns,
    };
  } req.sequelize = {
    options,
    conditions,
  };
  return next();
};
```

如果有任何你应该放入你的军火库的中间件，它将是这个。您可以设置任何读取路径，使其具有分页、条件，并只选择您需要的属性，而不是整个资源。

在我上面链接的 Github repo 中有一些 Typescript 接口可以查看。这篇文章已经太长了。

## 创建资源

```
export const create = (props) => {
  const route = async (req: Request, res: Response, next: NextFunction) => {
    const db = req.app.get('db');
    const model = db[props.model];
    if (!model) {
      throw {
        status: 404,
        message: 'Model not found',
      };
    }
    const results = await model.create(req.body);
    req.results = results;
    next();
  };
  return [asyncEndpoint(route), toJson];
};
```

这个中间件将接受一个带有模型名称的属性对象。然后，它将从我们的 db(数据库)对象中查找模型。如果你没有正确设置模型，它会抛出一个错误。否则，它将执行与普通路由器中的路由相同的操作。

## 阅读资源

```
export const read = (props) => {
  const route = async (req: Request, res: Response, next: NextFunction) => {
    const db = req.app.get('db');
    const model = db[props.model];
    if (!model) {
      throw {
        status: 404,
        message: 'Model not found',
      };
    }
    let results = await model.findAll({
      ...req.sequelize.conditions,
      ...req.sequelize.options,
    });
    req.results = results;
    next();
  };
  return [withSequelize, asyncEndpoint(route), toJson];
};
```

这与 create 函数具有相同的功能，只是因为我们从数据库中读取数据，所以我们可以使用带有 Sequelize 中间件的*来帮助控制分页和条件。*

例如，如果我们运行这个 API 调用:

```
[http://localhost:3000/api/v1/customers?attributes=id,first_name&first_name=John](http://localhost:3000/api/v1/customers?attributes=id,first_name&first_name=John)
```

API 将只返回名字为 John 的客户。

## 通过 Id 查找一个资源

```
export const findByPk = (props) => {
  const { id } = props;
  const route = async (req: Request, res: Response, next: NextFunction) => {
    const db = req.app.get('db');
    const model = db[props.model];
    if (!model) {
      throw {
        status: 404,
        message: 'Model not found',
      };
    }
    const results = await model.findByPk(req.params[id], {
      ...req.sequelize.conditions,
      ...req.sequelize.options,
    });
    req.results = results;
    next();
  };
  return [withSequelize, asyncEndpoint(route), toJson];
};
```

这类似于 read 中间件，除了它只会找到一条记录并返回一个对象，而不是一个对象数组。

## 更新资源

```
export const update = (props) => {
  const { key, path, fields } = props;
  const route = async (req: Request, res: Response, next: NextFunction) => {
    const db = req.app.get('db');
    const model = db[props.model];
    if (!model) {
      throw {
        status: 404,
        message: 'Model not found',
      };
    }
    const results = await model.update(req.body, {
      where: {
        [key]: get(req, path),
      },
      fields,
    });
    req.results = results;
    next();
  };
  return [asyncEndpoint(route), toJson];
};
```

您可以开始看到，我们正在从上面的普通路由器构建相同功能的通用版本。您可以在您的路由器中使用其中任何一种或其中一种。如果你不需要做什么特别的事情，这些中间件会大大加快你构建一个 API 的时间。

## 删除资源

```
export const destroy = (props) => {
  const { key, path } = props;
  const route = async (req: Request, res: Response, next: NextFunction) => {
    const db = req.app.get('db');
    const model = db[props.model];
    if (!model) {
      throw {
        status: 404,
        message: 'Model not found',
      };
    }
    const results = await model.destroy({
      where: {
        [key]: get(req, path),
      },
    });
    req.results = results;
    next();
  };
  return [asyncEndpoint(route), toJson];
};
```

现在我们有了一些中间件，可以为我们构建 RESTful API 路由，让我们以创建一个序列路由器来结束，它可以接受所有这些中间件并为我们构建一个动态 API。

## 顺序路由器

```
import express from 'express';
import { validateSchema } from './validateSchema';
import { create, read, findByPk, update, destroy } from './sequelize';const sequelizeRouter = (props) => {
  const { model, key = 'id', schemas } = props;
  const router = express.Router();

  router.get('/', read({ model }));
  router.get(`/:${model}Id`, findByPk({ model, id: `${model}Id` }));
  router.post('/', validateSchema(schemas.create), create({ model }));
  router.put(
    `/:${model}Id`,
    validateSchema(schemas.update),
    update({
      model,
      key,
      path: `params.${model}Id`,
    })
  );
  router.delete(
    `/:${model}Id`,
    destroy({
      model,
      key,
      path: `params.${model}Id`,
    })
  );
  return router;
};export default sequelizeRouter;
```

现在我们有了这个最终的中间件，我们可以像这样重写我们的普通客户路由器:

```
import joi from '@hapi/joi';
import sequelizeRouter from '../../../../../middleware/sequelizeRouter';const createSchema = {
  path: 'body',
  schema: joi.object().keys({
    first_name: joi.string().required(),
    last_name: joi.string().required(),
  }),
};const updateSchema = {
  path: 'body',
  schema: joi.object().keys({
    first_name: joi.string().required(),
    last_name: joi.string().required(),
  }),
};const router = sequelizeRouter({
  model: 'Customers',
  schemas: { create: createSchema, update: updateSchema },
});export default router;
```

您现在可以看到，我们可以通过中间件完成所有繁重的工作，现在我们唯一需要关心的是允许创建和更新的模式，以及模型的名称。如果您不关心模式验证，那么您只需要:

```
const router = sequelizeRouter({
  model: 'Customers',
});
```

因此，通过传递模型名，我们可以用几行代码构建一个完整的 RESTful API。

如果你确实需要做一些更健壮或更灵活的事情，你可以很容易地脱离这些功能，做你自己的事情。然而，通过保持 API 的可预测性、一致性和可伸缩性，您可以在此基础上构建更多，例如添加缓存和事件处理。

## 结论

当你一次又一次地使用相同的模式时，编写完成所有繁重工作的代码是有益的。创建动态中间件来为您生成代码使得跨 API 实现新特性变得如此容易。我们创建了每个中间件来处理 HTTP 方法，并为我们构建了一个 Express 路由器。

让我知道你对这种方法的看法。你一直喜欢手工编码普通路由器吗？你遇到过不同的举重方法吗？