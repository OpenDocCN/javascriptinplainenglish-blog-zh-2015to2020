# 角度贬低——迷宫

> 原文：<https://javascript.plainenglish.io/angular-deprecations-the-maze-7498de672514?source=collection_archive---------8----------------------->

## 开发人员需要关注版本更新的不合理性。

![](img/927d27a334348c560207defb84cb3af4.png)

Photo by [Susan Yin](https://unsplash.com/@syinq?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

软件版本升级——这也是每个开发人员所期待的。他们可能受困于过时的软件或缺乏他们煞费苦心编写的解决方法，或者甚至可能是一些人对最新版本的状态问题。(相信我，这些人确实存在)。

下面是 Angular 的反对页面的摘录。

让我们来看看[这里提到的一些贬损](https://angular.io/guide/deprecations#deprecated-apis-and-features)。这些可能使版本更新变得容易，或者使代码与标准同步，这样，当我们为版本升级更新代码时，它看起来是无缝的。

## **当前管道**:

这是其中最常见的一个。在 V9 中，默认货币代码“USD”已被弃用，以允许选择本地货币代码。

```
If you need the previous behavior then set it by creating a [DEFAULT_CURRENCY_CODE](https://angular.io/api/core/DEFAULT_CURRENCY_CODE) provider in your application [NgModule](https://angular.io/api/core/NgModule):{provide: DEFAULT_CURRENCY_CODE, useValue: ‘USD’}
```

## **视图封装:**

组件装饰器中的封装选项通常选择下列值之一。

*   `[ViewEncapsulation.Native](https://angular.io/api/core/ViewEncapsulation#Native) (Deprecated)`
*   `[ViewEncapsulation.Emulated](https://angular.io/api/core/ViewEncapsulation#Emulated)`
*   `[ViewEncapsulation.None](https://angular.io/api/core/ViewEncapsulation#None)`
*   `[ViewEncapsulation.ShadowDom](https://angular.io/api/core/ViewEncapsulation#ShadowDom)`

```
[ViewEncapsulation.Native](https://angular.io/api/core/ViewEncapsulation#Native) will be replaced by [ViewEncapsulation.ShadowDom](https://angular.io/api/core/ViewEncapsulation#ShadowDom)// I did dig into the angular 4/5 documentation for Native option. It is the same as ShadowDom option. — "creating a ShadowRoot for Component’s Host Element."
```

## **/deep/、> > >和::ng-deep**

每当我们设置组件时，Angular 有助于确保组件样式不会重叠。我们可以使用/deep/、>>>和::ng-deep 来用我们自己的样式覆盖深度嵌套的样式。但是，在这样做的时候要小心。可能有这样的情况，这可以作为一个通用的覆盖，我们可能不希望这样。

## **具有反应形式的 ng model**

```
<input [formControl]="control" [(ngModel)]="value">
```

我们都会看到的。这是一段令人困惑的代码。作为反应式表单或模板驱动表单的一部分进行编码总是更好。

```
// reactive forms
<input [formControl]="control">
this.control.setValue('some value');// template driven forms
<input [(ngModel)]="value">
this.value = 'some value';
```

我们现在可以通过在导入 ReactiveFormsModule 时设置 config 选项来选择显示/隐藏此用法的警告

```
// You can use 'never' / 'always' option to suppress/display the warnings.imports: [
  ReactiveFormsModule.withConfig({warnOnNgModelWithFormControl: 'never'});
]
```

## **激活路由**

我们使用 ActivatedRoute 和 ActivatedRouteSnapshot 来获取当前的路线信息。

*params*和*query params*属性已被弃用，而分别支持 paramMap 和 queryParamMap。

## **懒惰加载模块**

当一个应用程序使用延迟加载模块时，我们在路由器中设置引用，如下所示

```
const routes: Routes = [{
  path: 'lazy',
  loadChildren: './lazy/lazy.module#LazyModule'
}];
```

加载模块的新方式遵循动态模块导入语法。

```
const routes: Routes = [{
  path: 'lazy',
  loadChildren: () => import('./lazy/lazy.module').then(m => m.LazyModule)
}];
```

## **@ViewChild 和@ContentChild 查询**

我们在应用程序中都使用它来获取对元素或指令的引用。改变的解释可以在[这里](https://angular.io/guide/static-query-migration)找到(我推荐阅读全文)。简而言之，下面是需要的改变

```
// query results available in ngOnInit
[@ViewChild](http://twitter.com/ViewChild)('foo', {static: true}) foo: ElementRef;OR// query results available in ngAfterViewInit
[@ViewChild](http://twitter.com/ViewChild)('foo', {static: false}) foo: ElementRef;
```

这些只是让我们开始的一些反对意见。我们可以随时回头参考[的弃用指南](https://angular.io/guide/deprecations) / [更新指南](https://update.angular.io/)。当运行 *ng update* 命令时，有一些不推荐使用的特性得到了解决。最好是确保在我们的代码中解决所有的反对意见，以便轻松地进行迁移。

移民快乐！