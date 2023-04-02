# Angular 中基于装饰器的反应式表单——一种有效的表单验证方法

> 原文：<https://javascript.plainenglish.io/decorator-based-reactive-forms-in-angular-an-efficient-way-to-validate-forms-1cdc11a0918d?source=collection_archive---------4----------------------->

![](img/77f776250ec6a79401b9583870006ebb.png)

Image is taken from [burst](https://burst.shopify.com/)

在 Angular 中处理表单时，我们有两种选择:-

*   模板驱动的表单
*   基于模型的**反应形式**

根据表单的复杂程度，每种表单都有自己的用例。

*   模板驱动的表单在登录和注册这样简单的表单时很有用，但是它们没有反应式表单那么大。如果您有非常基本和简单的需求，请使用模板驱动的表单。这里的主要缺点是，没有组件内部的表单模型，单元测试是相当复杂的。
*   **反应式表单**更加健壮，因为它们更具可伸缩性、可重用性和可测试性。大部分繁重的工作都是通过将输入字段的实例放在模板上从组件类中完成的，这使得单元测试更容易。如果您使用具有复杂业务逻辑的大型面向数据的表单，请始终使用反应式表单。

在本文中，我们将讨论基于 decorator 的验证的反应式表单方法。但是在开始之前，我先解释一下如何在没有装饰器的情况下工作。

# **不带装饰器**

## 1)导入反应式模块

首先，我们需要在 app.module.ts 中导入 [ReactiveFormsModule](https://angular.io/docs/ts/latest/api/forms/index/ReactiveFormsModule-class.html) 。

app.module.ts

## 2)模板:formGroup，formControlName & ngSubmit

对于反应式表单，大部分工作被委托给组件类，因此模板文件更加简单明了。

reactive-form.component.html

## **3)组件类别**

**NgSubmit** —这是提交表单时将触发的事件。

**表单控件**—`[FormControl](https://angular.io/docs/ts/latest/api/forms/index/FormControl-class.html)`方便了单个表单控件，跟踪值和验证状态，还表示视图中的输入。第一个参数是默认值，第二个是验证器数组。

**FormGroup** — A `FormGroup`将每个子`[FormControl](https://angular.io/api/forms/FormControl)`的值聚集到一个对象中，每个控件名作为键。它通过减少其子节点的状态值来计算其状态。例如，如果一个组中的一个控件无效，则整个组都将无效。如果我们想创建一个复杂的值，我们也可以嵌套 FormGroup。

**FormControlName** —每个表单域应该有一个`formControlName`指令，其值将是组件类中使用的名称。

然后，让我们在`ngOnInit()`的控制器中创建表单。

reactive-form.component.ts

**OnSubmit** 输出显示我们如何访问表单的有效性和表单控制值。

## **4)当前表单验证的问题**

*   反应模块不是强类型的。
*   在创建表单组时，我们不能通过验证将模型对象值直接映射到反应式表单。
*   我们必须手动配置每个属性的验证器。
*   如果有多个验证器，编写太多代码来管理 FormControl 的模板验证。

> **注:-** 欲了解更多关于反应式表单的详细信息，请访问[https://angular.io/api/forms](https://angular.io/api/forms)

# **带装饰器**

我们将把`[**@rxweb/reactive-form-validators**](https://www.npmjs.com/package/@rxweb/reactive-form-validators)` angular 库用于基于装饰器的验证方法。

下面是使用这个库的一些优点

1.  使用基于模型的方法来构造反应式表单，只需在类属性上添加像`@alpha()`、`@required()`等装饰器。
2.  模板端不需要额外的代码，组件中也不需要任何形式的自定义验证。

# **实施**

1) npm 安装`[@rxweb/reactive-form-validators](https://www.npmjs.com/package/@rxweb/reactive-form-validators)`

2)还需要在 app.module 中导入`**RxReactiveFormsModule**`

3)使用 **app.module.ts** 中的`**ReactiveFormConfig**` ，使用 set 方法传递验证消息对象。

4)用各自的装饰者创建一个用户模型:-

`**@required()**`:所有字段都应该是必填的。如果您想要一个定制消息，那么在装饰器中将`message`作为一个参数传递。

```
@required({message: 'Last Name is required'})lastName: string;
```

`**@alpha()**`:userName 属性只接受字母。如果你想允许空白，那么在装饰器中把`allowWhitespace`作为一个参数传递。

```
@required()@alpha({ allowWhiteSpace: true })userName: string;
```

`**@minLength({ value: 8 })**`:密码属性的最小字符串长度必须为 8 个字符。

`**@compare({ fieldName: 'password' })**`:比较属性值应该与密码属性值相同。

> **注意:-** 这些只是一些装饰器，仅用于演示目的。更多可用装饰者的详细列表，请访问:-[**https://github . com/rx web/rx web/tree/master/packages/reactive-form-validators/decorators**](https://github.com/rxweb/rxweb/tree/master/packages/reactive-form-validators/decorators)

5)在`**ngOnInit**`钩子中创建组件类并初始化用户模型实例，然后将其传递给`**RxFormBuilder**`的`**FormGroup**`。

6)使用`formGroup`、`formControlName`、&、`ngSubmit`编写模板文件

# **结论**

`[@rxweb/reactive-form-validators](https://www.npmjs.com/package/@rxweb/reactive-form-validators)`是一个非常健壮的库，可以满足 angular 中的所有表单验证需求。它不仅支持反应式表单，还通过**指令**支持基于模板的验证。

下面是库文档中一个验证器的例子。

## `allOf`

该验证将检查用户是否输入了给定字段的所有值。

> ***无功形式验证***

```
this.employeeInfoFormGroup = this.formBuilder.group({skills: [RxwebValidators.allOf({
  matchValues: ["MVC", "AngularJS","AngularJS 5","C#"]
})],});
```

> ***模板表单验证***

```
<input **id**="skills" **name**="skills" **type**="checkbox" **value**="{{tag.name}}" [(**ngModel**)]="employee.skills" [**allOf**]='{"matchValues":"MVC","AngularJS","AngularJS","C#"}'/>
```

> ***装饰器基础验证***

```
@allOf({matchValues**:** ["MVC", "AngularJS","Angular 5","C#"]}) skills**:** string
```

如果你想要完整的代码。这里是项目的 [GitHub 库](https://github.com/kaushiksamanta/angular-decorator-validation)。

# 资源

1.  [https://www . npmjs . com/package/@ rx web/reactive-form-validators](https://www.npmjs.com/package/@rxweb/reactive-form-validators)
2.  [https://medium . com/@ oojhaajay/angular-reactive-form-validation-with-rx web-validation-decorators-e 943 fa 85d 67 c](https://medium.com/@oojhaajay/angular-reactive-form-validation-with-rxweb-validation-decorators-e943fa85d67c)
3.  [https://angular.io/api/forms](https://angular.io/api/forms)