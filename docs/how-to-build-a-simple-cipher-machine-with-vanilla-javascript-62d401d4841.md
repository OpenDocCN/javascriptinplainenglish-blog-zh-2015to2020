# 如何用普通的 JavaScript 构建一个简单的密码机

> 原文：<https://javascript.plainenglish.io/how-to-build-a-simple-cipher-machine-with-vanilla-javascript-62d401d4841?source=collection_archive---------1----------------------->

加密和解密与凯撒的算法在一个简单的方式

![](img/9760e4e08f9ea120b2e97c2e4f0d0f7b.png)

A woman with a background code by [ThisIsEngineering](https://www.pexels.com/es-es/@thisisengineering?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) en [Pexels](https://www.pexels.com/es-es/foto/mujer-internet-tecnologia-hembra-3861969/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

在这篇文章中，我将构建简单的加密和解密函数，对一部分文本进行编码，然后在客户端对编码后的字符串进行解码。

我要用凯撒密码。这是已知最早也是最简单的密码之一。这是一种替代密码，其中，为了对文本进行编码，原始文本中的每个字母都被“移位”到字母表的某些位置，而为了对文本进行解码，你只需进行相反的过程。例如，移位 1 时，“A”将被替换为“B”，移位 2 时，“A”将被替换为“c”

你可以在一个在线 HTML 文本编辑器中尝试最后一个例子，比如 [jsbin](https://jsbin.com) 。

## 凯撒密码算法

首先，我们要定义我们的字母表。您可以使用您的语言字符:

```
const alphabet = [
      'A’,’B’,’C’,’D’,’E’,’F’,
      'G’,’H’,’I’,’J’,’K’,’L’,
      'M’,’N’,’O’,’P’,’Q’,’R’,
      'S’,’T’,’U’,’V’,’W’,’X’,
      'Y’,’Z’
];
```

让我们定义我们的加密函数。我们使用一个条件语句，使用 includes()数组方法检查参数是否是字母表的一部分。我们还使用 toUpperCase()方法将输入字符转换为大写，因为字母表数组中的所有字母都是大写的。

现在，我们的 encrypt 函数使用 indexOf 函数获得了每个字符在字母表中的位置。例如，字符“A”在第 0 位，字母“Z”在第 25 位。然后，我们用移位值增加位置变量的值，并使用新位置从字母数组中获取字符。

例如，如果我们使用移位值 1，在字符“A”的位置增加 1 之后，我们获得值为 1(0+1)的新位置，它对应于字母数组中的字符“B”。

注意，我们使用模操作符“%”来做模除法。在我们的例子中，我们的字母数组大小是 26(0 到 25)，这意味着任何超过 25 的值都将“回送”到数组的开头:26 将到达 0、28 到 2 或 29 到 3。例如，移位 2 的字符“Y”将回到字母表的开头，并映射到字母“a”

```
function encrypt(char, shift) {
  let include =        
   alphabet.includes(
      char.toUpperCase()); 

  if (include){      
   let position =         
    alphabet.**indexOf**(
     char.toUpperCase());

 **let newPosition = 
    (position + shift) %  
      alphabet.length;** return alphabet[newPosition];
 } else  return char;
}
```

我们的 encrypText helper 函数读取我们在表单中引入的文本和移位值，并将其转换为数字。然后使用 map 函数为每个字符调用加密函数。为此，首先，我们使用 spread 操作符将输入字符串 sourceTest 从字符串转换为数组。最后，我们将所有字母连接起来，得到最终结果:

```
function encryptText() {

  const form = document.forms[0];

  let title=
   document.getElementById("titleId");  

  title.innerHTML = "Encrypted text";

  let shift= Number(form.shift.value); 

  let sourceText =  
    form.sourceText.value;       

 **form.sourceText.value 
    = [... sourceText ].map(char =>
      encrypt(char, shift)).join(’’);**
 }
```

decryptText 辅助函数类似于我们的 encryptText 函数，但是这里我们从字母表的长度中减去移位值以获得新的偏移值:

```
function decryptText() { const form = document.forms[0]; let title = document.getElementById("titleId");       

 title.innerHTML = "Plain text";

 let shift =   
  Number(form.shift.value); let sourceText = 
  form.sourceText.value;    

 shift = 
 (**alphabet.length - shift**) %  
   alphabet.length;

 form.sourceText.value =
    [...sourceText].map(char => 
       ** encrypt(char ,shift))**.join(’’);
 }
```

用一些 Html 来测试的结果代码是:

```
<!DOCTYPE html>
<html>

<head>
 <meta charset="utf-8">
 <meta name="viewport"    
  content="width=device-width">

 <title>Cipher machine</title>

 <script>  

  const alphabet = [
      'A','B','C','D','E','F',
      'G','H','I','J','K','L',
      'M','N','O','P','Q','R',
      'S','T','U','V','W','X',
      'Y','Z' 
  ];

 function encryptText() {

  const form = document.forms[0];

  let title=
   document.getElementById("titleId");  

  title.innerHTML = "Encrypted text";

  let shift= Number(form.shift.value); 

  let sourceText =  
    form.sourceText.value;       

  form.sourceText.value 
    = [... sourceText ].map(char =>
      encrypt(char, shift)).join('');
 }

 function decryptText() {
  const form = document.forms[0]; let title = document.getElementById("titleId");       

  title.innerHTML = "Plain text";

  let shift =   
    Number(form.shift.value); let sourceText = 
    form.sourceText.value;    

  shift = 
     (alphabet.length - shift) %  
      alphabet.length;

  form.sourceText.value 
      = [... sourceText ].map(char => 
        encrypt(char,    
        shift)).join('');
 }

 function encrypt(char, shift) {
  let include =        
   alphabet.includes(
    char.toUpperCase()); 

  if (include){      
   let position =         
    alphabet.indexOf(
     char.toUpperCase());

  let newPosition = 
    (position+shift) %  
      alphabet.length; return alphabet[newPosition];
 }else  return char;
}        
</script>
</head><body>
  <form>       
   <h1>My Cipher machine</h1>   
   <p id="titleId">Plain text</p>

   <div>          
    <textarea name="sourceText" 
       rows="8"
       cols="50"         
       spellcheck="false"
       value="">                       
    </textarea>
   </div>

   <div>      
    <label for="shift">Shift:</label>
    <select id="shift" name="shift">
     <option value="1">1</option>
     <option value="2">2</option>
     <option value="5">5</option>
     <option value="10">10</option>
    </select> 
  </div>

  <div>      
    <input type="button" id="decrypt" 
       value="Encrypt" 
       onclick="encryptText();"> <input type="button" id="decrypt" 
       value="Decrypt the text" 
       onclick="decryptText();">    
  </div>

  </form>
</body>
</html>
```

## 结论

如果您正在寻找可靠的加密算法，您可以使用 AES 或其他类型的加密。例如，在 JavaScript 中，你有 CryptoJS。CryptoJS 是一个标准和安全的加密集合，具有一致和简单的接口。在本文中，我想展示现有的快速加密文本的最简单算法之一，但是对于真正的开发，建议使用标准库，而不要重复发明。

## 感谢您花时间阅读这篇文章。希望你觉得有用！

## **用简单的英语写的 JavaScript 的注释:**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**