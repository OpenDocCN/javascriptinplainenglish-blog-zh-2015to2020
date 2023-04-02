# 如何测试 Nest.js 应用程序

> 原文：<https://javascript.plainenglish.io/testing-nestjs-applications-546ab3e9fa13?source=collection_archive---------0----------------------->

在之前的帖子中，我写了很多测试代码来验证我们的应用程序是否如预期的那样工作。

Nestjs 提供了与 [Jest](https://github.com/facebook/jest) 和 [Supertest](https://github.com/visionmedia/supertest) 的集成，以及用于单元测试和端到端(e2e)测试的测试工具。

# Nestjs 测试线束

像 Angular 的`TestBed`一样，Nestjs 提供了一个类似的`Test`工具来为你的测试代码组装 Nestjs 组件。

```
beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        ...
      ],
    }).compile(); service = module.get<UserService>(UserService);
  });
```

类似于`@Module`装饰器中的属性，`creatTestingModule`定义了将在测试中使用的组件。

我们已经展示了在 Nestjs 应用程序中测试服务的方法，例如在`post.service.spec.ts`中。

要隔离服务中的依赖关系，有几种方法。

*   创建一个假服务来替换真服务，在`providers`中组装。
*   提供者:[ { provide: UserService，useClass: FakeUserService } ]，
*   请改用模拟实例。
*   `providers: [ provide: UserService, useValue: { send: jest.fn() } ],`
*   对于简单的服务提供者，您可以脱离 Nestjs 的束缚，创建一个简单的假依赖服务，并使用`new`在`setup`钩子中实例化您的服务。

也可以在`Test.createTestingModule`中导入一个模块。

```
Test.createTestingModule({
        imports: []
       })
```

要替换导入模块中的某些服务，您可以`override`它。

```
Test.createTestingModule({
        imports: []
       })
       .override(...)
```

# 笑话技巧和窍门

Nestjs 测试严重依赖 Jest 框架。我花了很多时间研究测试 Nestjs 应用程序中的所有组件。

# 模仿外部类或函数

例如，`mongoose.connect`将需要一个真正的 mongo 服务器来连接，以模仿`mongoose`的`createConnection`。

在导入之前设置模拟。

```
jest.mock('mongoose', () => ({
    createConnection: jest.fn().mockImplementation(
        (uri:any, options:any)=>({} as any)
    ),
    Connection: jest.fn()
}))//...
import { Connection, createConnection } from 'mongoose';
//
```

当数据库提供者被实例化时，断言`createConnection`被调用。

```
it('connect is called', () => {
    //expect(conn).toBeDefined();
    //expect(createConnection).toHaveBeenCalledTimes(1); // it is 2 here. why?
    expect(createConnection).toHaveBeenCalledWith("mongodb://localhost/blog", {
        useNewUrlParser: true,
        useUnifiedTopology: true,
        //see: [https://mongoosejs.com/docs/deprecations.html#findandmodify](https://mongoosejs.com/docs/deprecations.html#findandmodify)
        useFindAndModify: false
    });
})
```

# 通过原型模拟父类

看看本地的 auth guard 测试。

模仿父原型中的方法`canActivate`。

```
describe('LocalAuthGuard', () => {
  let guard: LocalAuthGuard;
  beforeEach(() => {
    guard = new LocalAuthGuard();
  });
  it('should be defined', () => {
    expect(guard).toBeDefined();
  });
  it('should return true for `canActivate`', async () => {
    AuthGuard('local').prototype.canActivate = jest.fn(() =>
      Promise.resolve(true),
    );
    AuthGuard('local').prototype.logIn = jest.fn(() => Promise.resolve());
    expect(await guard.canActivate({} as ExecutionContext)).toBe(true);
  });});
```

# 尽可能将功能提取到函数中

让我们来看看`user.model.ts`。将前置`save`钩子方法和自定义`comparePassword`方法提取到独立的函数中。

```
async function preSaveHook(next) { // Only run this function if password was modified
  if (!this.isModified('password')) return next(); // Hash the password
  const password = await hash(this.password, 12);
  this.set('password', password); next();
}UserSchema.pre<User>('save', preSaveHook);function comparePasswordMethod(password: string): Observable<boolean> {
  return from(compare(password, this.password));
}UserSchema.methods.comparePassword = comparePasswordMethod;
```

测试它们就像测试简单的函数一样容易。

```
describe('preSaveHook', () => {
    test('should execute next middleware when password is not modified', async () => {
        const nextMock = jest.fn();
        const contextMock = {
            isModified: jest.fn()
        };
        contextMock.isModified.mockReturnValueOnce(false);
        await preSaveHook.call(contextMock, nextMock);
        expect(contextMock.isModified).toBeCalledWith('password');
        expect(nextMock).toBeCalledTimes(1);
    }); test('should set password when password is modified', async () => {
        const nextMock = jest.fn();
        const contextMock = {
            isModified: jest.fn(),
            set: jest.fn(),
            password: '123456'
        };
        contextMock.isModified.mockReturnValueOnce(true);
        await preSaveHook.call(contextMock, nextMock);
        expect(contextMock.isModified).toBeCalledWith('password');
        expect(nextMock).toBeCalledTimes(1);
        expect(contextMock.set).toBeCalledTimes(1);
    });
});
```

# 端到端测试

Nestjs 集成 supertest 向服务器端发送请求。

使用`beforeAll`和`afterAll`启动和停止应用程序，使用`request`向服务器发送 http 请求并断言响应结果。

```
import * as request from 'supertest';
//...describe('API endpoints testing (e2e)', () => {
    let app: INestApplication;
    beforeAll(async () => {
        const moduleFixture: TestingModule = await Test.createTestingModule({
            imports: [AppModule],
        }).compile(); app = moduleFixture.createNestApplication();
        app.enableShutdownHooks();
        app.useGlobalPipes(new ValidationPipe());
        await app.init();
    }); afterAll(async () => {
        await app.close();
    }); // an example of using supertest request.
    it('/posts (GET)', async () => {
        const res = await request(app.getHttpServer()).get('/posts').send();
        expect(res.status).toBe(200);
        expect(res.body.length).toEqual(3);
    });
}
```

关于完整的 e2e 测试的更多细节，查看 Nestjs 的[测试文件夹](https://github.com/hantsy/nestjs-sample/tree/master/test)。