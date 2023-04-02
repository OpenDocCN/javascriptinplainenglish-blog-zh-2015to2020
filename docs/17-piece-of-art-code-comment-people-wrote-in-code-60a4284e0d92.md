# 人们曾经用代码写了 30 多条艺术代码注释。

> 原文：<https://javascript.plainenglish.io/17-piece-of-art-code-comment-people-wrote-in-code-60a4284e0d92?source=collection_archive---------7----------------------->

## 谁说软件开发人员不幽默？

![](img/d1c2f4b45e06f857114e9140ddebc9b0.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

代码注释！有人说其丑陋，有人说其标准和做法做得好。这是大多数人相信我的猜测

> "好的代码是自文档化的."

好吧，这篇文章将向你展示代码注释也是一件糟糕的艺术品。我列出了一些人们在生产代码中遇到的艺术代码注释。

注:这篇是 [**25+无用代码注释的续篇，人们实际上是在他们的代码中写的。**](https://medium.com/javascript-in-plain-english/25-useless-code-comments-people-actually-wrote-in-their-code-6e55c370d562)

**TL；博士这里没什么严重的！读这个只是为了好玩！**

注意:我在 github gist 中添加了大部分注释，因为它们破坏了 medium 上的标准代码格式。

# 评分史上最佳评论(IMHO)

**速率 5.0/5.0**

```
// hack for ie browser (assuming that ie is a browser)
```

**速率 4.5/5.0**

```
/**
 * Always returns true.
 */
public boolean isAvailable() {
 return false;
}
```

**速率 4.6/5.0**

```
/*
This isn’t the right way to deal with this, but today is my last day, Ron
just spilled coffee on my desk, and I’m hungry, so this will have to do…
*/return 12; // 12 is my lucky number
```

**速率 4.0/5**

```
/***
 * Dear maintainer:
 *
 * Once you are done trying to 'optimize' this routine,
 * and have realized what a terrible mistake that was,
 * please increment the following counter as a warning
 * to the next guy:
 *
 * total_hours_wasted_here = 42
 */
/***
 * 亲爱的维护者：
 *
 * 如果你尝试了对这段程序进行'优化'
 * 下面这个计数器的个数用来对后来人进行警告
 *
 * 浪费在这里的总时间 = 42h
 */
```

**速率 5.0 /5.0**

```
/***
 * When I wrote this, only God and I understood what I was doing
 * Now, God only knows
 */
/***
```

**比率 4.5/5**

```
/***
 * I dedicate all this code, all my work, to my wife, Darlene, who will
 * have to support me and our three children and the dog once it gets
 * released into the public.
 */
```

**比率 4.8/5**

```
// Magic. Do not touch.
// 麻鸡。勿动。
```

**4.9/5**

```
// I'm sorry.
```

**速率 4.9/5**

```
// This code sucks, you know it and I know it.  
// Move on and call me an idiot later.
```

**速率 5.0/5.0**

```
// If this comment is removed the program will blow up
```

**速率 5.0/5.0**

```
// if i ever see this again i'm going to start bringing guns to work
```

**速率 5.0/5.0**

```
// I am not responsible of this code.
// They made me write it, against my will.
```

**速率 4.5/5.0**

```
//if you get here then you really f**ked
```

**速率 4.8/5.0**

```
// sometimes I believe compiler ignores all my comments
```

**速率 4.9/5.0**

```
#define TRUE FALSE 
//Happy debugging suckers
```

**速率 4.5/5.0**

```
// I have to find a better job
```

**速率 5.0/5.0**

```
Catch (Exception e) {
 //who cares?
}
```

**速率 5.0/5.0**

```
// If this code works, it was written by Paul DiLascia. If not, I //don’t know
// who wrote it
```

**速率 4.8/5.0**

```
/**
 * Always returns true.
 */
public boolean isAvailable() {
 return false;
}
```

**速率 5.0+/5.0**

```
// Joe is sorry
Few Hundred Lines Later
// Harry is sorry too
```

**速率 5.0/5.0**

```
// All bugs added by David S. Miller
```

**速率 4.8/5.0**

```
//This is a kind of magic.
```

**速率 5.0/5.0**

```
public GetRandomNumber()
{
    // Chosen by a fairly rolen dice
    return 12;
}
```

**速率 4.5/5.0**

```
// *** drunk -- fix later ***
```

**速率 4.8/5.0**

```
// For the sins I am about to commit, may James Gosling forgive me
```

**速率 4.6/5.0**

```
//I'm sorry, but our princess is in another castle.
```

**速率 5.0/5.0**

```
//If you even THINK of changing this code, you may have already gone //too far
```

**速率 4.9/5.0**

```
 .='  ' .`/,/!(=)Zm.           
                     .._,,._..  ,-`- `,\ ` -` -`\\7//WW.         
                ,v=~/.-,-\- -!|V-s.)iT-|s|\-.'   `///mK%.        
              v!`i!-.e]-g`bT/i(/[=.Z/m)K(YNYi..   /-]i44M.       
            v`/,`|v]-DvLcfZ/eV/iDLN\D/ZK@%8W[Z..   `/d!Z8m       
           //,c\(2(X/NYNY8]ZZ/bZd\()/\7WY%WKKW)   -'|(][%4\.      
         ,\\i\c(e)WX@WKKZKDKWMZ8(b5/ZK8]Z7%ffVM,   -.Y!bNMi      
         /-iit5N)KWG%%8%%%%W8%ZWM(8YZvD)XN(@.  [   \]!/GXW[      
        / ))G8\NMN%W%%%%%%%%%%8KK@WZKYK*ZG5KMi,-   vi[NZGM[      
       i\!(44Y8K%8%%%**~YZYZ@%%%%%4KWZ/PKN)ZDZ7   c=//WZK%!      
      ,\v\YtMZW8W%%f`,`.t/bNZZK%%W%%ZXb*K(K5DZ   -c\\/KM48       
      -|c5PbM4DDW%f  v./c\[tMY8W%PMW%D@KW)Gbf   -/(=ZZKM8[       
      2(N8YXWK85@K   -'c|K4/KKK%@  V%@@WD8e~  .//ct)8ZK%8`       
      =)b%]Nd)@KM[  !'\cG!iWYK%%|   !M@KZf    -c\))ZDKW%`        
      YYKWZGNM4/Pb  '-VscP4]b@W%     'Mf`   -L\///KM(%W!         
      !KKW4ZK/W7)Z. '/cttbY)DKW%     -`  .',\v)K(5KW%%f          
      'W)KWKZZg)Z2/,!/L(-DYYb54%  ,,`, -\-/v(((KK5WW%f           
       \M4NDDKZZ(e!/\7vNTtZd)8\Mi!\-,-/i-v((tKNGN%W%%            
       'M8M88(Zd))///((|D\tDY\\KK-`/-i(=)KtNNN@W%%%@%[           
        !8%@KW5KKN4///s(\Pd!ROBY8/=2(/4ZdzKD%K%%%M8@%%           
         '%%%W%dGNtPK(c\/2\[Z(ttNYZ2NZW8W8K%%%%YKM%M%%.          
           *%%W%GW5@/%!e]_tZdY()v)ZXMZW%W%%%*5Y]K%ZK%8[          
            '*%%%%8%8WK\)[/ZmZ/Zi]!/M%%%%@f\ \Y/NNMK%%!          
              'VM%%%%W%WN5Z/Gt5/b)((cV@f`  - |cZbMKW%%|          
                 'V*M%%%WZ/ZG\t5((+)L\'-,,/  -)X(NWW%%           
                      `~`MZ/DZGNZG5(((\,    ,t\\Z)KW%@           
                         'M8K%8GN8\5(5///]i!v\K)85W%%f           
                           YWWKKKKWZ8G54X/GGMeK@WM8%@            
                            !M8%8%48WG@KWYbW%WWW%%%@             
                              VM%WKWK%8K%%8WWWW%%%@`             
                                ~*%%%%%%W%%%%%%%@~               
                                   ~*MM%%%%%%@f`                 
                                       '''''
```

**速率 5.0/5.0**

```
// If I from the future read this I'll back in time and kill myself.
```

**速率 4.6/5.0**

```
// TODO: Fix this.  Fix what?
```

**速率 5.0/5.0**

```
using namespace std;            // So sue me
```

**速率 4.3/5.0**

```
//        .==.        .==.          
//       //`^\\      //^`\\         
//      // ^ ^\(\__/)/^ ^^\\        
//     //^ ^^ ^/6  6\ ^^ ^ \\       
//    //^ ^^ ^/( .. )\^ ^ ^ \\      
//   // ^^ ^/\| v""v |/\^ ^ ^\\     
//  // ^^/\/ /  `~~`  \ \/\^ ^\\    
//  -----------------------------
/// HERE BE DRAGONS
```

**速率 4.6/5.0**

```
return null; //Not really null
```

**速率 4.5/5.0**

```
/* You are not meant to understand this */
```

**速率 4.9/5.0**

```
// drunk, fix later
```

**速率 4.3/5.0**

```
// This function has been here since 1987\. DON'T FXXKING TOUCH IT
```

**速率 4.0/5.0**

```
/***
 * You may think you know what the following code does.
 * But you dont. Trust me.
 * Fiddle with it, and youll spend many a sleepless
 * night cursing the moment you thought youd be clever
 * enough to "optimize" the code below.
 * Now close this file and go play with something else.
 */
/***
 * 你可能会认为你读得懂以下的代码。但是你不会懂的，相信我吧。
 * 要是你尝试玩弄这段代码的话，你将会在无尽的通宵中不断地咒骂自己为什么会认为自己聪明到可以优化这段代码。
 * 现在请关闭这个文件去玩点别的吧。
 */
```

**速率 5.0/5.0**

```
// I will give you two of my seventy-two virgins if you can fix this.
```

**评分 4.6/5.0(试试谷歌翻译**😁 **)**

```
* 程序员 1（于 2010 年 6 月 7 日）：在这个坑临时加入一些调料
 * 程序员 2（于 2011 年 5 月 22 日）：临你个屁啊
 * 程序员 3（于 2012 年 7 月 23 日）：楼上都是狗屎，鉴定完毕
 * 程序员 4（于 2013 年 8 月 2 日）：fuck 楼上，三年了，这坑还在！！！
 * 程序员 5（于 2014 年 8 月 21 日）：哈哈哈，这坑居然坑了这么多人，幸好我也不用填了，系统终止运行了，you're died
```

**速率 5.0/5.0**

```
// This function has been here since 1987\. DON'T FXXKING TOUCH IT
```

**速率 4.9/5.0**

```
// no comments for you
// it was hard to write
// so it should be hard to read
```

**速率 4.3/5.0**

```
// This comment is self explanatory.
```

**速率 4.4/5.0**

```
// Autogenerated, do not edit. All changes will be undone.
```

**速率 4.7/5.0**

```
#Christmas tree initializer  
    toConnect = []  
    toRead =   [  ]  
    toWrite = [    ]   
    primes = [      ]  
    responses = {}  
    remaining = {}
```

**速率 4.9/5.0**

```
// Remove this if you wanna be fired
```

# 开发者的作品作为注释

meditating buddha

Buddha said: I am paralysed by bugs! — translated

The high mountain stops, and the Jingxing stops. Although it can’t be reached, I yearn for it — translated

Face inside Code

The above is the magic map — translated

Look at the source code again, look at your sister! — translated

18+ . LOL !

Now doze off, okay, don’t change the code below, go to sleep — translated the last Line. LOL!

Again 18+

检查实际的 [**github**](https://github.com/Blankj/awesome-comment) 回购更像上面那些。你可以在评论中添加你的上衣。我将在后面的原文中补充这一点。

## 这里是[第一版](https://medium.com/javascript-in-plain-english/30-funny-code-comments-that-will-make-you-laugh-1c1b54d4ab00)。我希望你也会喜欢。🍿

# **来自简明英语团队的通知**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！**

**我们还推出了一个 YouTube，希望你能通过 [**订阅我们的英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****

# ****感谢阅读。🍻****