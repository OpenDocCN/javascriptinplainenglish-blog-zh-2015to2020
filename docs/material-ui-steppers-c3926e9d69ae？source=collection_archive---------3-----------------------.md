# 材料用户界面—步进器

> 原文：<https://javascript.plainenglish.io/material-ui-steppers-c3926e9d69ae?source=collection_archive---------3----------------------->

![](img/3aeb9db166195250fce5c9446f136dcc.png)

Photo by [Jem Sahagun](https://unsplash.com/@jemsahagun?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何添加带有材质 UI 的步进器。

# 跳舞者

步进器让我们展示用户完成一个过程必须经历的步骤。

要使用它，我们可以使用`Stepper`组件添加步进器:

```
import React from "react";
import Stepper from "[@material](http://twitter.com/material)-ui/core/Stepper";
import Step from "[@material](http://twitter.com/material)-ui/core/Step";
import StepLabel from "[@material](http://twitter.com/material)-ui/core/StepLabel";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";function getSteps() {
  return ["step 1", "step 2", "step 3"];
}function getStepContent(step) {
  switch (step) {
    case 0:
      return "do step 1";
    case 1:
      return "do step 2";
    case 2:
      return "do steo 3";
    default:
      return "unknown step";
  }
}export default function App() {
  const [activeStep, setActiveStep] = React.useState(0);
  const [skipped, setSkipped] = React.useState(new Set());
  const steps = getSteps(); const isStepOptional = step => {
    return step === 1;
  }; const isStepSkipped = step => {
    return skipped.has(step);
  }; const handleNext = () => {
    let newSkipped = skipped;
    if (isStepSkipped(activeStep)) {
      newSkipped = new Set(newSkipped.values());
      newSkipped.delete(activeStep);
    } setActiveStep(prevActiveStep => prevActiveStep + 1);
    setSkipped(newSkipped);
  }; const handleBack = () => {
    setActiveStep(prevActiveStep => prevActiveStep - 1);
  }; const handleSkip = () => {
    if (!isStepOptional(activeStep)) {
      throw new Error("You can't skip a step that isn't optional.");
    } setActiveStep(prevActiveStep => prevActiveStep + 1);
    setSkipped(prevSkipped => {
      const newSkipped = new Set(prevSkipped.values());
      newSkipped.add(activeStep);
      return newSkipped;
    });
  }; const handleReset = () => {
    setActiveStep(0);
  }; return (
    <div>
      <Stepper activeStep={activeStep}>
        {steps.map((label, index) => {
          const stepProps = {};
          const labelProps = {};
          if (isStepOptional(index)) {
            labelProps.optional = "optional";
          }
          if (isStepSkipped(index)) {
            stepProps.completed = false;
          }
          return (
            <Step key={label} {...stepProps}>
              <StepLabel {...labelProps}>{label}</StepLabel>
            </Step>
          );
        })}
      </Stepper>
      <div>
        {activeStep === steps.length ? (
          <div>
            All steps completed
            <Button onClick={handleReset}>Reset</Button>
          </div>
        ) : (
          <div>
            {getStepContent(activeStep)}
            <div>
              <Button disabled={activeStep === 0} onClick={handleBack}>
                Back
              </Button>
              {isStepOptional(activeStep) && (
                <Button
                  variant="contained"
                  color="primary"
                  onClick={handleSkip}
                >
                  Skip
                </Button>
              )} <Button variant="contained" color="primary" onClick={handleNext}>
                {activeStep === steps.length - 1 ? "Finish" : "Next"}
              </Button>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
```

我们用`getSteps`函数创建步骤标题。

每一步的内容都在`getStepContent`函数中。

当我们点击下一步按钮时，`handleNext`被调用。

然后我们添加添加到`skipped`状态的步骤。

点击后退按钮时运行`handleBack`。

我们只需将`activeStep`设置为上一步。

点击跳过按钮时，调用`handleSkip`函数。

我们检查活动步骤是否是可选的。

如果它是可选的，那么我们让他们跳过它，并将其添加到`skipped`集合中。

在 JSX 代码中，我们使用带有`Step`道具的`Stepper`组件来添加步骤。

然后我们添加步骤的内容和它下面的按钮。

内容根据`activeStep`值显示。

# 替代标签

我们可以用`StepLabel`组件将步骤标签更改为我们想要的。

例如，我们可以写:

```
import React from "react";
import Stepper from "[@material](http://twitter.com/material)-ui/core/Stepper";
import Step from "[@material](http://twitter.com/material)-ui/core/Step";
import StepLabel from "[@material](http://twitter.com/material)-ui/core/StepLabel";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";function getSteps() {
  return ["step 1", "step 2", "step 3"];
}function getStepContent(step) {
  switch (step) {
    case 0:
      return "do step 1";
    case 1:
      return "do step 2";
    case 2:
      return "do steo 3";
    default:
      return "unknown step";
  }
}export default function App() {
  const [activeStep, setActiveStep] = React.useState(0);
  const steps = getSteps(); const handleNext = () => {
    setActiveStep(prevActiveStep => prevActiveStep + 1);
  }; const handleBack = () => {
    setActiveStep(prevActiveStep => prevActiveStep - 1);
  }; const handleReset = () => {
    setActiveStep(0);
  }; return (
    <div>
      <Stepper activeStep={activeStep} alternativeLabel>
        {steps.map(label => (
          <Step key={label}>
            <StepLabel>{label}</StepLabel>
          </Step>
        ))}
      </Stepper>
      <div>
        {activeStep === steps.length ? (
          <div>
            All steps completed
            <Button onClick={handleReset}>Reset</Button>
          </div>
        ) : (
          <div>
            {getStepContent(activeStep)}<div>
              <Button disabled={activeStep === 0} onClick={handleBack}>
                Back
              </Button>
              <Button variant="contained" color="primary" onClick={handleNext}>
                {activeStep === steps.length - 1 ? "Finish" : "Next"}
              </Button>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
```

我们添加了`alternativeLabel`道具来覆盖标签。

然后在渲染子道具中，我们用`StepLabel`返回我们自己的`Step`组件来显示步骤号。

![](img/8f53a1748778e691a2596e4a52797674.png)

Photo by [Mike Meeks](https://unsplash.com/@mikemeex?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以用`Stepper`组件添加一个步进器。

我们可以让步骤可选并定制标签。

内容可以显示在它的下面。

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到一切的链接！