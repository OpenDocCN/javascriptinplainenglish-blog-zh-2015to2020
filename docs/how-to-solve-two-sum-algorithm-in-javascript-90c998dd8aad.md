# 如何在 JavaScript 中求解二和算法

> 原文：<https://javascript.plainenglish.io/how-to-solve-two-sum-algorithm-in-javascript-90c998dd8aad?source=collection_archive---------7----------------------->

![](img/65e552dc0fa312232cc1e392d0855857.png)

我想我们都同意这一点:两和算法是一种经典的数据结构挑战。在我的博客中，我想分享两种解决方法。第一种是所谓的“暴力”方法，第二种代表了一种更优雅的解决方案

所以，我们的问题是:*给定一个数字数组和一个目标数，从数组中找出两个数字之和等于目标数。不能对同一指数求和两次。返回这些数字。*

1.  **创建一个嵌套循环的工作方案(时间复杂度:O(n ))**

这里，我使用嵌套的 for 循环来检查数组中所有可能的和。一旦我买到合适的，我就把它们退回去。如果没有匹配，则返回一个空数组。

```
function twoSum(nums, target) {
    let result = [];  

    for(let i = 0; i < nums.length; i++) {
        for(let j = i + 1; j < nums.length; j++) {
            if(nums[i] + nums[j] === target) {
            result.push(nums[i], nums[j])
            } 
        }
    }
    return result;
};
```

2.**使用基于对象的解决方案(时间复杂度:O(n))**

一种更复杂且时间复杂度友好的方法是通过使用空对象来解决这个问题。这将给我们 O(n)的时间复杂度。

```
function twoSum(nums, target){
//FIRST STEP: create an empty Object 

    let numObject = {}   

//SECOND STEP: use a for-loop to iterate through the array

    for(let eachNum in nums){
    let otherNum = target - nums[eachNum]   //we'll check for otherNum in the object and if it's there, we got it and can push in our result array.       if(otherNum in numObject){
        let resultArr = [];
        resultArr.push(otherNum, nums[eachNum])        
        return resultArr;
        } numObject[nums[eachNum]] = eachNum//NB! adding key/value has to go after the if-statement to avoid adding the same index twice. We add the value or a new pair on each iteration.
      }
     return "not found";
   }
```

希望对你有帮助！

**来源:**

1.  [解决两和问题](https://levelup.gitconnected.com/solving-the-two-sum-problem-in-javascript-three-ways-4d43067fcfc7)
2.  [两和问题](https://dev.to/yongliang24/solve-the-two-sum-problem-with-javascript-4jlk)