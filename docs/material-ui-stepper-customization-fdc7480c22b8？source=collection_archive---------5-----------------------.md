# 材料用户界面—非线性步进器

> 原文：<https://javascript.plainenglish.io/material-ui-stepper-customization-fdc7480c22b8?source=collection_archive---------5----------------------->

![](img/f92da38821703de73ac3feed38f3065f.png)

Photo by [Ernesto Bruschi](https://unsplash.com/@brucekee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何用材质 UI 定制步进器。

# 非线性步进机

我们可以添加非线性步进器来添加步进器，允许用户导航到他们想要的任何步骤。

例如，我们可以写:

```
import React from "react";
import Stepper from "[@material](http://twitter.com/material)-ui/core/Stepper";
import Step from "[@material](http://twitter.com/material)-ui/core/Step";
import StepButton from "[@material](http://twitter.com/material)-ui/core/StepButton";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";function getSteps() {
  return ["step 1", "step 2", "step 3"];
}function getStepContent(step) {
  switch (step) {
    case 0:
      return "do step 1";
    case 1:
      return "do step 2";
    case 2:
      return "do step 3";
    default:
      return "unknown step";
  }
}export default function App() {
  const [activeStep, setActiveStep] = React.useState(0);
  const [completed, setCompleted] = React.useState({});
  const steps = getSteps(); const totalSteps = () => {
    return steps.length;
  }; const completedSteps = () => {
    return Object.keys(completed).length;
  }; const isLastStep = () => {
    return activeStep === totalSteps() - 1;
  }; const allStepsCompleted = () => {
    return completedSteps() === totalSteps();
  }; const handleNext = () => {
    const newActiveStep =
      isLastStep() && !allStepsCompleted()
        ? steps.findIndex((step, i) => !(i in completed))
        : activeStep + 1;
    setActiveStep(newActiveStep);
  }; const handleBack = () => {
    setActiveStep(prevActiveStep => prevActiveStep - 1);
  }; const handleStep = step => () => {
    setActiveStep(step);
  }; const handleComplete = () => {
    const newCompleted = completed;
    newCompleted[activeStep] = true;
    setCompleted(newCompleted);
    handleNext();
  }; const handleReset = () => {
    setActiveStep(0);
    setCompleted({});
  };return (
    <div>
      <Stepper nonLinear activeStep={activeStep}>
        {steps.map((label, index) => (
          <Step key={label}>
            <StepButton
              onClick={handleStep(index)}
              completed={completed[index]}
            >
              {label}
            </StepButton>
          </Step>
        ))}
      </Stepper>
      <div>
        {allStepsCompleted() ? (
          <div>
            All steps completed - you&apos;re finished
            <Button onClick={handleReset}>Reset</Button>
          </div>
        ) : (
          <div>
            {getStepContent(activeStep)} <div>
              <Button disabled={activeStep === 0} onClick={handleBack}>
                Back
              </Button>
              <Button variant="contained" color="primary" onClick={handleNext}>
                Next
              </Button>
              {activeStep !== steps.length &&
                (completed[activeStep] ? (
                  `Step {activeStep + 1} already completed`
                ) : (
                  <Button
                    variant="contained"
                    color="primary"
                    onClick={handleComplete}
                  >
                    {completedSteps() === totalSteps() - 1
                      ? "Finish"
                      : "Complete Step"}
                  </Button>
                ))}
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
```

我们添加了`nonLinear`道具，让用户点击他们想要的任何步骤。

下一步按钮调用`handleNext`进入下一步或未完成的步骤。

`handleBack`是点击后退按钮时运行的。

它将该步骤设置为上一步。

`handleStep`当我们点击一个步骤时运行。

我们还设置了`completed`道具来指示哪个步骤已经完成。

点击复位按钮时`handleReset`运行。

它将`activeStep`状态设置为 0，并将`completed`状态设置为空对象，以清除已完成的步骤。

例如，我们可以写:

```
import React from "react";
import Stepper from "[@material](http://twitter.com/material)-ui/core/Stepper";
import Step from "[@material](http://twitter.com/material)-ui/core/Step";
import StepButton from "[@material](http://twitter.com/material)-ui/core/StepButton";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";function getSteps() {
  return ["step 1", "step 2", "step 3"];
}function getStepContent(step) {
  switch (step) {
    case 0:
      return "do step 1";
    case 1:
      return "do step 2";
    case 2:
      return "do step 3";
    default:
      return "unknown step";
  }
}export default function App() {
  const [activeStep, setActiveStep] = React.useState(0);
  const [completed, setCompleted] = React.useState(new Set());
  const [skipped, setSkipped] = React.useState(new Set());
  const steps = getSteps();const totalSteps = () => {
    return getSteps().length;
  };const isStepOptional = step => {
    return step === 1;
  };const handleSkip = () => {
    if (!isStepOptional(activeStep)) {
      throw new Error("You can't skip a step that isn't optional.");
    }setActiveStep(prevActiveStep => prevActiveStep + 1);
    setSkipped(prevSkipped => {
      const newSkipped = new Set(prevSkipped.values());
      newSkipped.add(activeStep);
      return newSkipped;
    });
  };const skippedSteps = () => {
    return skipped.size;
  };const completedSteps = () => {
    return completed.size;
  };const allStepsCompleted = () => {
    return completedSteps() === totalSteps() - skippedSteps();
  };const isLastStep = () => {
    return activeStep === totalSteps() - 1;
  };const handleNext = () => {
    const newActiveStep =
      isLastStep() && !allStepsCompleted()
        ? steps.findIndex((step, i) => !completed.has(i))
        : activeStep + 1;setActiveStep(newActiveStep);
  };const handleBack = () => {
    setActiveStep(prevActiveStep => prevActiveStep - 1);
  };const handleStep = step => () => {
    setActiveStep(step);
  };const handleComplete = () => {
    const newCompleted = new Set(completed);
    newCompleted.add(activeStep);
    setCompleted(newCompleted);if (completed.size !== totalSteps() - skippedSteps()) {
      handleNext();
    }
  };const handleReset = () => {
    setActiveStep(0);
    setCompleted(new Set());
    setSkipped(new Set());
  };const isStepSkipped = step => {
    return skipped.has(step);
  };function isStepComplete(step) {
    return completed.has(step);
  }return (
    <div>
      <Stepper alternativeLabel nonLinear activeStep={activeStep}>
        {steps.map((label, index) => {
          const stepProps = {};
          const buttonProps = {};
          if (isStepOptional(index)) {
            buttonProps.optional = `Optional`;
          }
          if (isStepSkipped(index)) {
            stepProps.completed = false;
          }
          return (
            <Step key={label} {...stepProps}>
              <StepButton
                onClick={handleStep(index)}
                completed={isStepComplete(index)}
                {...buttonProps}
              >
                {label}
              </StepButton>
            </Step>
          );
        })}
      </Stepper>
      <div>
        {allStepsCompleted() ? (
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
                Next
              </Button>
              {isStepOptional(activeStep) && !completed.has(activeStep) && (
                <Button
                  variant="contained"
                  color="primary"
                  onClick={handleSkip}
                >
                  Skip
                </Button>
              )} {activeStep !== steps.length &&
                (completed.has(activeStep) ? (
                  `Step ${activeStep + 1} already completed`
                ) : (
                  <Button
                    variant="contained"
                    color="primary"
                    onClick={handleComplete}
                  >
                    {completedSteps() === totalSteps() - 1
                      ? "Finish"
                      : "Complete Step"}
                  </Button>
                ))}
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
```

我们有我们的`Step`和`StepButton`来添加步进按钮。

我们应用所有的道具，并允许用户点击每个链接，而不管其状态如何。

`buttonProps.optioal`让我们呈现一个字符串或组件来表示该步骤是可选的。

我们也可以将`stepProps`的`completed`属性设置为`false`。

![](img/4968d983604ab23efb20b8e1bad74f93.png)

Photo by [Frenjamin Benklin](https://unsplash.com/@frenjaminbenklin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以根据自己的喜好定制非线性步进器。

标签和内容可以用各种组件进行更改。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到所有内容的链接！