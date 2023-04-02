# 在 Nest.js 中处理用户注册

> 原文：<https://javascript.plainenglish.io/handling-user-registration-19d5bd3c5995?source=collection_archive---------1----------------------->

在前面的文章中，用户样本数据在一个观察`OnMoudleInit`事件的服务中初始化。

![](img/155e8ffce096399e46512b1f870809aa.png)

在本帖中，我们将添加一个端点来处理用户注册请求，包括:

*   添加一个端点*/注册*来处理用户注册进程
*   用 bcrypt 散列密码
*   通过 SendGrid 邮件服务发送通知

# 注册新用户

生成寄存器控制器。

```
nest g controller user/register --flat
```

将以下内容填入`RegisterController`。

```
// user/register.controller.ts@Controller('register')
export class RegisterController {
    constructor(private userService: UserService) { } @Post()
    register(
        @Body() registerDto: RegisterDto,
        @Res() res: Response): Observable<Response> {
        const username = registerDto.username; return this.userService.existsByUsername(username).pipe(
            flatMap(exists => {
                if (exists) {
                    throw new ConflictException(`username:${username} is existed`)
                }
                else {
                    const email = registerDto.email;
                    return this.userService.existsByEmail(email).pipe(
                        flatMap(exists => {
                            if (exists) {
                                throw new ConflictException(`email:${email} is existed`)
                            }
                            else {
                                return this.userService.register(registerDto).pipe(
                                    map(user =>
                                        res.location('/users/' + user.id)
                                            .status(201)
                                            .send()
                                    )
                                );
                            }
                        })
                    );
                }
            })
        );
    }
}
```

在上面的代码中，我们将分别通过用户名和电子邮件检查用户是否存在，然后将用户数据保存到 MongoDB 数据库中。

在`UserService`中，添加缺少的方法。

```
@Injectable()
export class UserService {

  existsByUsername(username: string): Observable<boolean> {
    return from(this.userModel.exists({ username }));
  } existsByEmail(email: string): Observable<boolean> {
    return from(this.userModel.exists({ email }));
  } register(data: RegisterDto): Observable<User> { const created = this.userModel.create({
      ...data,
      roles: [RoleType.USER]
    }); return from(created);
  }
  //...
}
```

创建一个 DTO 类来表示用户注册请求数据。首先生成 DTO 骨架。

```
nest g class user/register.dto --flat
```

并填写以下内容。

```
import { IsEmail, IsNotEmpty, MaxLength, MinLength } from "class-validator";export class RegisterDto {
    @IsNotEmpty()
    readonly username: string; @IsNotEmpty()
    @IsEmail()
    readonly email: string; @IsNotEmpty()
    @MinLength(8, { message: " The min length of password is 8 " })
    @MaxLength(20, { message: " The password can't accept more than 20 characters " })
    // @Matches(/^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z]{8,20}$/,
    //     { message: " A password at least contains one numeric digit, one supercase char and one lowercase char" }
    // )
    readonly password: string; @IsNotEmpty()
    readonly firstName?: string; @IsNotEmpty()
    readonly lastName?: string;
}
```

以上代码中的`@IsNotEmpty()`、`@IsEmail`、`@MinLength()`、`@MaxLength()`、`@Matches()`均出自`class-validator`。如果您有一些 Java EE/Jakarta EE Bean 验证或 Hibernate 验证的经验，这些注释很容易理解。

*   `@IsNotEmpty()`检查给定值是否为空
*   `@IsEmail`验证输入字符串是否是有效的电子邮件格式
*   `@MinLength()`和`@MaxLength()`用于限制输入值的长度范围
*   `@Matches()`对于自定义正则表达式匹配是灵活的。

> *更多关于类验证器用法的信息，查看项目*[*typestack/类验证器*](https://github.com/typestack/class-validator) *的详细信息。*

在之前的文章中，我们已经在`main.ts`条目文件的`bootstrap`函数中应用了一个全局`ValidationPipe`。当用无效数据注册时，它将返回 404 错误。

```
$ curl [http://localhost:3000/register](http://localhost:3000/register) -d "{}" {"statusCode":400,"message":["username should not be empty","email must be an em ail","email should not be empty"," The password can't accept more than 20 charac ters "," The min length of password is 8 ","password should not be empty","first Name should not be empty","lastName should not be empty"],"error":"Bad Request"}
```

为`RegisterController`添加一个测试。

```
describe('Register Controller', () => {
  let controller: RegisterController;
  let service: UserService; beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [RegisterController],
      providers: [
        {
          provide: UserService,
          useValue: {
            register: jest.fn(),
            existsByUsername: jest.fn(),
            existsByEmail: jest.fn()
          },
        },
      ]
    }).compile(); controller = module.get<RegisterController>(RegisterController);
    service = module.get<UserService>(UserService);
  }); it('should be defined', () => {
    expect(controller).toBeDefined();
  }); describe('register', () => {
    it('should throw ConflictException when username is existed ', async () => {
      const existsByUsernameSpy = jest.spyOn(service, 'existsByUsername').mockReturnValue(of(true));
      const existsByEmailSpy = jest.spyOn(service, 'existsByEmail').mockReturnValue(of(true));
      const saveSpy = jest.spyOn(service, 'register').mockReturnValue(of({} as User)); const responseMock = {
        location: jest.fn().mockReturnThis(),
        json: jest.fn().mockReturnThis(),
        send: jest.fn().mockReturnThis()
      } as any;
      try {
        await controller.register({ username: 'hantsy' } as RegisterDto, responseMock).toPromise();
      } catch (e) {
        expect(e).toBeDefined();
        expect(existsByUsernameSpy).toBeCalledWith('hantsy');
        expect(existsByEmailSpy).toBeCalledTimes(0);
        expect(saveSpy).toBeCalledTimes(0)
      }
    }); it('should throw ConflictException when email is existed ', async () => {
      const existsByUsernameSpy = jest.spyOn(service, 'existsByUsername').mockReturnValue(of(false));
      const existsByEmailSpy = jest.spyOn(service, 'existsByEmail').mockReturnValue(of(true));
      const saveSpy = jest.spyOn(service, 'register').mockReturnValue(of({} as User)); const responseMock = {
        location: jest.fn().mockReturnThis(),
        json: jest.fn().mockReturnThis(),
        send: jest.fn().mockReturnThis()
      } as any;
      try {
        await controller.register({ username: 'hantsy', email: 'hantsy@example.com' } as RegisterDto, responseMock).toPromise();
      } catch (e) {
        expect(e).toBeDefined();
        expect(existsByUsernameSpy).toBeCalledWith('hantsy');
        expect(existsByEmailSpy).toBeCalledWith('hantsy@example.com');
        expect(saveSpy).toBeCalledTimes(0)
      }
    }); it('should save when username and email are available ', async () => {
      const existsByUsernameSpy = jest.spyOn(service, 'existsByUsername').mockReturnValue(of(false));
      const existsByEmailSpy = jest.spyOn(service, 'existsByEmail').mockReturnValue(of(false));
      const saveSpy = jest.spyOn(service, 'register').mockReturnValue(of({ _id: '123' } as User)); const responseMock = {
        location: jest.fn().mockReturnThis(),
        status: jest.fn().mockReturnThis(),
        send: jest.fn().mockReturnThis()
      } as any; const locationSpy = jest.spyOn(responseMock, 'location');
      const statusSpy = jest.spyOn(responseMock, 'status');
      const sendSpy = jest.spyOn(responseMock, 'send'); await controller.register({ username: 'hantsy', email: 'hantsy@example.com' } as RegisterDto, responseMock).toPromise(); expect(existsByUsernameSpy).toBeCalledWith('hantsy');
      expect(existsByEmailSpy).toBeCalledWith('hantsy@example.com');
      expect(saveSpy).toBeCalledTimes(1);
      expect(locationSpy).toBeCalled();
      expect(statusSpy).toBeCalled();
      expect(sendSpy).toBeCalled(); });
  });
});
```

在上述测试代码中，我们检查了所有条件，并确保命中了`RegisterController`中的所有代码块。

对于`UserService`中新增的方法，相应增加测试。这里我跳过测试代码，请自行检查[源代码](https://github.com/hantsy/nestjs-sample/tree/feat/user)。

# 哈希密码

在以前的文章中，我们使用纯文本来存储用户文档中的密码字段。在实际应用中，出于安全考虑，我们应该选择哈希算法来编码普通密码。

Bcrypt 是非常流行的散列密码。

首先安装`bcypt`。

```
npm install --save bcrypt
```

当保存一个新用户，散列密码，然后保存它。在`User`型号中添加一个预保存挂钩。

```
async function preSaveHook(next) { // Only run this function if password was modified
  if (!this.isModified('password')) return next(); // Hash the password
  const password = await hash(this.password, 12);
  this.set('password', password); next();
}UserSchema.pre<User>('save', preSaveHook);
```

在新用户数据被持久化到 MongoDB 之前，将调用`preSave`钩子。

当用户试图通过用户名和密码对登录时，应该检查密码是否与数据库中的密码匹配。

向`User`模型添加一个方法。

```
function comparePasswordMethod(password: string): Observable<boolean> {
  return from(compare(password, this.password));
}UserSchema.methods.comparePassword = comparePasswordMethod;
```

改变`AuthService`的`validateUser`方法，检查密码是否匹配。

```
flatMap((user) => {
    const { _id, password, username, email, roles } = user;
    return user.comparePassword(pass).pipe(map(m => {
        if (m) {
            return { id: _id, username, email, roles } as UserPrincipal;
        }else {
            throw new UnauthorizedException('username or password is not matched')
        }
    }))
```

测试`User`模型的挂钩有点困难，为了简化测试工作，这里我将挂钩提取到独立的函数，并在测试中模拟调用上下文。

```
// see: [https://stackoverflow.com/questions/58701700/how-do-i-test-if-statement-inside-my-mongoose-pre-save-hook](https://stackoverflow.com/questions/58701700/how-do-i-test-if-statement-inside-my-mongoose-pre-save-hook)
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

在 [user.mdoel.sepc.ts](https://github.com/hantsy/nestjs-sample/blob/master/src/database/user.mdoel.spec.ts) 中探索`comparePasswordMethod`等的其他测试。

现在运行应用程序，查看控制台中关于用户初始化的日志，因为您看到存储在 MongoDB 中的密码被散列。

```
(UserModule) is initialized...
[
  {
    roles: [ 'USER' ],
    _id: 5f477055fb9a2b3fa4cb1c21,
    username: 'hantsy',
    password: '$2b$12$/spjKM3Vdf5vRJE9u2cHaulIAWzKMbNVSyHjMp9E9PifbSEHTQrJy',
    email: 'hantsy@example.com',
    createdAt: 2020-08-27T08:35:33.800Z,
    updatedAt: 2020-08-27T08:35:33.800Z,
    __v: 0
  },
  {
    roles: [ 'ADMIN' ],
    _id: 5f477055fb9a2b3fa4cb1c22,
    username: 'admin',
    password: '$2b$12$kFhASRJPkb/WD99J4uZrf.ZkkeKghpvf/6pgVGQArGiIgXu5aNMe.',
    email: 'admin@example.com',
    createdAt: 2020-08-27T08:35:33.801Z,
    updatedAt: 2020-08-27T08:35:33.801Z,
    __v: 0
  }
]
```

# 注册欢迎通知

通常，在真实的应用程序中，当注册成功完成时，应该向新注册的用户发送一封欢迎电子邮件。

NodeJS 应用中有几个模块可以用来发送邮件，例如`nodemailer`等。还有一些邮件的云服务，比如 [SendGrid](https://sendgrid.com/) 。有一个现有的 Nestjs 模块将 SendGrid 集成到 Nestjs 中，请检查[ntegral/Nestjs-send grid](https://github.com/ntegral/nestjs-sendgrid)项目。

在这个示例中，我们将不使用现有的模块，而是为这个应用程序创建一个新的家用模块。

首先安装 sendgrid npm 包。

```
npm i @sendgrid/mail
```

生成 sendgrid 模块和 sendgrid 服务。

```
nest g mo sendgrid
nest g s sendgrid
```

将以下内容添加到`SendgridService`中。

```
@Injectable()
export class SendgridService { constructor(@Inject(SENDGRID_MAIL) private mailService: MailService) { } send(data: MailDataRequired): Observable<any>{
        //console.log(this.mailService)
        return from(this.mailService.send(data, false))
    }}
```

创建一个提供者来公开来自`@sendgrid/mail`包的`MailService`。

```
export const sendgridProviders = [
    {
      provide: SENDGRID_MAIL,
      useFactory: (config: ConfigType<typeof sendgridConfig>): MailService =>
        {
            const mail = new MailService();
            mail.setApiKey(config.apiKey);
            mail.setTimeout(5000);
            //mail.setTwilioEmailAuth(username, password)
            return mail;
        },
      inject: [sendgridConfig.KEY],
    }
  ];
```

相应地，为 sendgrid 添加一个配置。

```
//config/sendgrid.config.ts
export default registerAs('sendgrid', () => ({
  apiKey: process.env.SENDGRID_API_KEY || 'SG.test',
}));
```

> *注册 SendGrid 并为您的应用程序生成一个 API 密匙来发送电子邮件。*

在`SendgridModule`中声明与 sendgrid 相关的配置、提供商和服务。

```
@Module({
  imports: [ConfigModule.forFeature(sendgridConfig)],
  providers: [...sendgridProviders, SendgridService],
  exports: [...sendgridProviders, SendgridService]
})
export class SendgridModule { }
```

改变`UserService`中的寄存器功能。

```
const msg = {
      from: 'hantsy@gmail.com', // Use the email address or domain you verified above
      subject: 'Welcome to Nestjs Sample',
      templateId: "d-cc6080999ac04a558d632acf2d5d0b7a",
      personalizations: [
        {
          to: data.email,
          dynamicTemplateData: { name: data.firstName + ' ' + data.lastName },
        }
      ] };
    return this.sendgridService.send(msg).pipe(
      catchError(err=>of(`sending email failed:${err}`)),
      tap(data => console.log(data)),
      flatMap(data => from(created)),
    );
```

> *template Id 是 SendGrid 管理的模板的 id。SendGrid 有很棒的 web UI，可以让你编写和管理电子邮件模板。*

理想情况下，用户注册过程应该分为两个步骤。

*   验证注册表单中的用户输入数据，并将其持久化到 MongoDB 中，然后发送一个验证号来验证注册的电话号码、电子邮件等。在此阶段，用户帐户将被暂停验证。
*   注册用户收到电子邮件中的验证号码或链接，在验证页面中提供或直接点击电子邮件中的链接，并得到验证。在此阶段，用户帐户将被激活。

从我的 github 中抓取[源代码，切换到分支](https://github.com/hantsy/nestjs-sample)[专长/用户](https://github.com/hantsy/nestjs-sample/blob/feat/user)。