# JavaScript 单元测试最佳实践—名称和期望

> 原文：<https://javascript.plainenglish.io/javascript-unit-test-best-practices-names-and-expects-e02540dc670d?source=collection_archive---------7----------------------->

![](img/e14773d2f5c9403ccf103112faf93766.png)

Photo by [Twitter: @jankolario](https://unsplash.com/@jankolar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 正确命名我们的测试

我们应该在测试中使用简洁、明确和描述性的名称。

这样，我们知道我们在测试什么。

而不是写:

```
describe('employee app', () => {
  it('returns an array', () => {
  }); // ...
});
```

我们写道:

```
describe('employee app', () => {
  it('returns a list of employees when initialized', () => {
  }); it('should calculate the pay of an employee when initialized', () => {
  });// ...
});
```

我们有工作单元、场景或内容，以及预期的行为。

我们可以用这种格式:

```
describe('[unit of work]', () => {
  it('should [expected behaviour] when [scenario/context]', () => {
  });
});
```

或者:

```
describe('[unit of work]', () => {
  describe('when [scenario/context]', () => {
    it('should [expected behaviour]', () => {
    });
  });
});
```

所以我们可以写:

```
describe('employee app', () => {
  describe('when initialized', () => {
    it('returns a list of employees', () => {
    }); it('should calculate the pay of an employee', () => {
    });
  }); // ...
});
```

# 不要注释掉测试

我们不应该注释掉测试。

如果它们太慢或者产生错误的结果，那么我们应该修正测试，使其更快更可靠。

# 在我们的测试中避免逻辑

我们应该在测试中避免逻辑。

这意味着我们应该避免条件和循环。

条件句可以让它选择任何路径。

循环使我们的测试共享状态。

所以我们不应该写:

```
it('should get employee by id', () => {
  const employees = [
    { id: 1, name: 'james' },
    { id: 2, name: 'may' },
    { id: 3, name: 'mary' },
    { id: 4, name: 'john' },
    { id: 5, name: 'james' },
  ] for (const em of employees) {
    expect(getEmployee(em.id).name).toBe(em.name);
  }
});
```

相反，我们将它们分成各自的`expect`陈述:

```
it('should get employee by id', () => {
  expect(getEmployee(1)).toBe('james');
  expect(getEmployee(2)).toBe('may');
  expect(getEmployee(3)).toBe('mary');
  expect(getEmployee(4)).toBe('john');
  expect(getEmployee(5)).toBe('james');
});
```

我们有所有病例的清晰输出。

我们也可以写出所有的案例作为自己的测试:

```
it('should sanitize a string containing non-ASCII chars', () => {
  expect(sanitizeString(`Avi${String.fromCharCode(243)}n`)).toBe('Avion');
});it('should sanitize a string containing spaces', () => {
  expect(sanitizeString('foo bar')).toBe('foo-bar');
});it('should sanitize a string containing exclamation signs', () => {
  expect(sanitizeString('funny chars!!')).toBe('funny-chars-');
});it('should sanitize a filename containing spaces', () => {
  expect(sanitizeString('file name.zip')).toBe('file-name.zip');
});it('should sanitize a filename containing more than one dot', () => {
  expect(sanitizeString('my.name.zip')).toBe('my-name.zip');
});
```

# 不要写不必要的期望

我们不应该写不必要的期望。

如果有没用过的东西，那我们就不应该添加。

例如，我们可以写:

```
it('compute the number by multiplying and subtracting by 2', () => {
  const multiplySpy = spyOn(Calculator, 'multiple').and.callThrough();
  const subtractSpy = spyOn(Calculator, 'subtract').and.callThrough(); const result = Calculator.compute(22.5); expect(multiplySpy).toHaveBeenCalledWith(22.5, 2);
  expect(subtractSpy).toHaveBeenCalledWith(45, 2);
  expect(result).toBe(43);
});
```

我们不需要间谍，因为我们正在测试计算的结果。

所以我们可以写:

```
it('compute the number by multiplying and subtracting by 2', () => {
  const result = Calculator.compute(22.5);
  expect(result).toBe(43);
})
```

我们只是测试结果，因为这是我们关心的。

我们不想检查实现细节。

![](img/2cc6c498d83569fe3880ab7d3ef350b3.png)

Photo by [Wynand van Poortvliet](https://unsplash.com/@wwwynand?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该测试结果而不是实现细节。

此外，我们应该正确地命名测试。

## **简明英语 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**找到一切的链接 plainenglish.io**](https://plainenglish.io/) ！