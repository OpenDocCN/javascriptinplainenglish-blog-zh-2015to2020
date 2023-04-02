# Goodist:一个简单的 Node.js 数组操作库

> 原文：<https://javascript.plainenglish.io/simple-array-manipulation-library-for-node-js-goodist-cb02175db504?source=collection_archive---------2----------------------->

![](img/2649506b389b7738201d2eca1ca54efb.png)

## 今天早上我花了两个多小时，在互联网上搜索一个简单易用的库来解决我在 Node 项目中面临的一个小问题，并使我的工作更快。

# 我找不到一个能完全满足我需求的。所以我决定写一个(goodist)来解决我的问题。

因为我认为它可能对其他人有所帮助，所以我将它发布到 npm，让开源社区可以使用。

你到底想达到什么目的？会是一个很好的问题。

让我用一个实际的例子来开始我的解释，这样事情可能看起来更清楚，更容易理解。

假设我有两个模式对象 employeeSchema 和 leaveSchema，如下面的步骤所示。

第一步

```
//Employee Schema
const mongoose = require('mongoose');
const Schema = mongoose.Schema;const employeeSchema = new Schema({
  firstName: {
    type: String,
    required: true
  }
  lastName: {
    type: String,
    required: true
  },
  leave: [{
    type: Schema.Types.ObjectId,
    ref: 'Leave',
    required: true
  }]
  resetToken: String,
  resetTokenExpiration: Date
}, { timestamps: true });module.exports = mongoose.model('Employee', employeeSchema); //Leave Schema
const mongoose = require('mongoose');
const Schema = mongoose.Schema;const leaveSchema = new Schema({
  startDate: {
    type: Date,
    required: true
  }
  endDate: {
    type: Date,
    required: true
  },
  leaveType: {
    type: String,
	enum: ['annual','sick']
    required: true
  },
  employee: {
    type: mongoose.Types.ObjectId,
    ref: 'Employee',
 	required: true
  }
}, { timestamps: true });module.exports = mongoose.model(‘Leave’, leaveSchema);
```

第二步

既然我的模式已经准备好了。

注意:在我上面的模式中，员工可以多次请求休假，换句话说就是一对多的关系。

而一个假期只能有一个员工，换句话说就是一对一的关系，视情况而定。

因此，每当一个雇员请求休假时，我希望将该休假 ID 添加到上面 employeeSchema 对象中该雇员的休假数组中，每当他删除该休假时，我也希望将其从休假数组中删除。这就是我的图书馆( [goodist](https://www.npmjs.com/package/goodist) )发挥巨大作用的地方。请参见代码片段。

第一件事首先安装库

```
npm i goodist
```

如下图所示导入它

```
const goodist = require(‘goodist’);//Create Leaveexports.create = async (req, res, next) => {
  const errors = validationResult(req);
  const { startDate, endDate, comment, leavetype } = req.body;
  const employeeID = req.userId; try { if (!errors.isEmpty()) {
      const error = new Error('Validation failed, entered data is incorrect.');
      error.statusCode = 422;
      throw error;
    } const leave = new Leave({
      startDate: startDate,
      endDate: endDate,
      leavetype: leavetype,
      employee: employeeID
    }); await leave.save();
    const employee = await Employee.findById(employeeID); //Add leave ID to employee leave array
   **goodist.pushOne(leave, employee.leave, employee);** res.status(201).json({
      success: true,
      message: 'Created successfully!',
      leave: leave
    }); } catch (err) {
    if (!err.statusCode) {
      err.statusCode = 500;
    }
    next(err);
  }
} //To Delete Leave Request
exports.delete = async (req, res, next) => {
  const errors = validationResult(req);
  const id = req.params.id;
  const employeeID = req.userId; try { if (!errors.isEmpty()) {
      const error = new Error('Validation failed, entered data is incorrect.');
      error.statusCode = 422;
      throw error;
    } const leave = await Leave.findById(id);

    if (!leave) {
      const error = new Error('Error fetching data');
      error.statusCode = 422;
      throw error;
    } if (employeeID.toString() !== leave.employee.toString()) {
      const error = new Error('Unauthorized!');
      error.statusCode = 401;
      throw error;
    } await Leave.findOneAndDelete(id);
    const employee = await Employee.findById(employeeID); //Remove leave from employee leave array    
   **goodist.pullOne(leave, employee.leave, employee);** res.status(200).json({
      success: true,
      message: 'Deleted successfully!'
    }); } catch (err) {
    if (!err.statusCode) {
      err.statusCode = 500;
    }
    next(err);
  }
}
```

> 总之 **goodist** 揭露 4 个有用的方法 **pushOne** 、 **pushMany** 、 **pullOne** 和 **pullMany** 。

I .按一键在数组中添加一个项目。

使用

```
const employee = await Employee.findById(employeeID);
goodist.pushOne(leave, employee.leave, employee);
```

二。**拉动**从数组中移除一个项目。

使用

```
const employee = await Employee.findById(employeeID);
goodist.pullOne(leave, employee.leave, employee);
```

三。 **pushMany** 一次在数组中添加多个项目。

使用

```
const employee = await Employee.findById(employeeID);
goodist.pushMany(leave, employee.leave, employee);
```

四。 **pullMany** 一次从数组中移除多个项目。

使用

```
const employee = await Employee.findById(employeeID);
goodist.pullMany(leave, employee.leave, employee);
```

注意:

**离开** :-代表你要推送的内容。在我的例子中，它是离开对象

**employee.leave** :-是一个数组，它代表你把项目推到了哪里。

**employee** :-是存放数组的对象，推送后必须重新保存。

使用 **pushOne** 和 **pullOne** 时，第一个参数必须是对象或字符串，而 **pushMany** 和 **pullMany** 期望第一个参数是数组。

谢谢你阅读我的文章。希望有帮助。