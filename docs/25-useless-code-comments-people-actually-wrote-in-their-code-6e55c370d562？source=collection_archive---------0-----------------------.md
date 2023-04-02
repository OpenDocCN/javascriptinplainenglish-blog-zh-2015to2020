# 人们实际上在代码中写了 25 个以上无用的代码注释。

> 原文：<https://javascript.plainenglish.io/25-useless-code-comments-people-actually-wrote-in-their-code-6e55c370d562?source=collection_archive---------0----------------------->

## 我们都被困在里面了，所以你不妨读读这个自娱自乐吧！

![](img/1ecf62f47b6bc5a5b524675dd66d021a.png)

Photo by [Safar Safarov](https://unsplash.com/@codestorm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

代码注释！有人说其丑陋，有人说其标准和做法做得好。

> “代码从不说谎，但注释有时会。”——罗恩·杰弗里斯

好吧，这篇文章将向你展示代码注释也是无用的。我列出了一些人们在生产代码中遇到的无用代码注释。

注:此片是人家实际写的的 [32+搞笑代码评论的续集。](https://medium.com/javascript-in-plain-english/30-funny-code-comments-that-will-make-you-laugh-1c1b54d4ab00)

**TL；博士这里没什么严重的！读这个只是为了好玩！**

```
/*Added because someone asked me to add it*/
```

```
// Increase i by one
i++;
```

```
// Times New Roman? What was wrong with the old Romans?
```

```
// Do not remove this comment else compilation will fail.
```

```
/**
 * @author SomeUserWhoShouldKnowBetter
 *
 * To change this generated comment edit the template variable "typecomment":
 * Window>Preferences>Java>Templates.
 * To enable and disable the creation of type comments go to
 * Window>Preferences>Java>Code Generation.
 */
```

```
// Steve did not send attribute so happy creativity 
// lets recreate it using available information
// this logic should nod be here and I am waiting impatiently to //throw it away// TODO: DAN to fix this. Not Wes. No sir. Not Wes.
```

```
/*The Knuth books are not in the office only for show; they help make
loops 30% faster and 20% as readable. */
```

```
//The truth is not out there. It is here
  #define TRUE (1==1)
  #define FALSE (!TRUE)
```

```
// Had to do this because DB is in Abbie Normal Form
```

```
// Times New Roman? What was wrong with the old Romans?
```

```
#include <stdio.h>
//why isn't this working!
```

```
//???
```

```
// return
return;
```

```
$i = 0; //set i to 0

$i++; //use sneaky trick to add 1 to i!

if ($i==$j) { // I made sure to use == rather than = here to avoid a  // bug
```

```
// Undocumented
```

```
private
  //PRIVATE means PRIVATE so no comments for you
  function LoadIt(IntID: Integer): Integer;
```

```
// Return value does not matter
return 0;
```

```
try
{
  ...
}
catch
{
  // TODO: something catchy
}
```

```
// do nothing
```

```
// TODO: DAN to fix this.  Not Wes.  No sir.  Not Wes.
```

```
//I know that this is very ugly, but I am tired and in a hurry. 
//You would do the same if you were me...
//...
//[A piece of nasty code here]
```

```
// Magic
menu.Visible = False
menu.Visible = True
```

```
// My class!
Class myclass 
{
    //Default constructor
    public myClass()
    {
       ...
    }
}
```

```
// secret sauce
```

```
// remember to comment code
```

```
try
{
...some code...
}
catch
{
// Just don't crash, it wasn't that important anyway.
}
```

```
if(some_condition){
    do_stuff();
}
else{
    //An error occurred!
}
```

```
// TODO: Documentation.
```

```
// This method takes two integer values and adds them together via the built-in
// .NET functionality. It would be possible to code the arithmetic function
// by hand, but since .NET provides it, that would be a waste of time
private int Add(int i, int j) // i is the first value, j is the //second value
{
    // add the numbers together using the .NET "+" operator
    int z = i + j;

    // return the value to the calling function
    // return z;

    // this code was updated to simplify the return statement, //eliminating the need
    // for a separate variable.
    // this statement performs the add functionality using the + //operator on the two
    // parameter values, and then returns the result to the calling //     function
    return i + j;
}
```

```
//if you get here then you really f**ked
```

```
/* our second do loop */
do {
```

```
/// <summary>
/// Toes the foo.
/// </summary>
/// <returns></returns>
public Foo ToFoo()
```

```
/**
 * Method declaration
 *
 *
 * @param table
 * @param row
 *
 * @throws SQLException
 */
void addTransactionDelete(Table table, Object row[]) throws SQLException {
```

```
// Don't know why we have to do this
```

```
//@TODO: Rewrite this, it sucks. Seriously.
```

```
/**
 * @author SomeUserWhoShouldKnowBetter
 *
 * To change this generated comment edit the template variable "typecomment":
 * Window>Preferences>Java>Templates.
 * To enable and disable the creation of type comments go to
 * Window>Preferences>Java>Code Generation.
 */
```

```
Thread.Sleep(1000); // this will fix .NET's crappy threading implementation
```

```
/*Added because someone asked me to add it*/
```

```
// yes, this is going to break in 2089, but, one, I'll be dead, and two, we really ought to be using
// a different system by then
if (yearPart >= 89)
{
    // naughty bits removed....
}
```

```
/* Do the business */
```

```
// this is a comment - don't delete
```

```
// TODO: this is basically a copy of the code at line 743!!!
```

```
//SFD Start
...code...
//SFD End
```

```
//This causes a crash for some reason. I know the real reason but it // doesn't fit on this line.
```

```
/**
 * Implements the PaymentType interface.
 */
public class PaymentTypePo implements PaymentType
```

```
/// <summary>
/// Disables the web part. (Virtual method)
/// </summary>
public virtual void EnableWebPart() { /* nothing - you have to override it */ }
```

```
//TODO: Remove this comment.
```

```
if (someFlag)
{
    // YES
    DoSomething();
}
else
{
    // NO
    DoSomethingElse();
}
```

```
/******************************************************************************
   NAME:       (repeat the trigger name)
   PURPOSE:    To perform work as each row is inserted or updated.
   REVISIONS:
   Ver        Date        Author           Description
   ---------  ----------  ---------------  ------------------------------------
   1.0        27.6.2000             1\. Created this trigger.
   PARAMETERS:
   INPUT:
   OUTPUT:
   RETURNED VALUE:
   CALLED BY:
   CALLS:
   EXAMPLE USE:
   ASSUMPTIONS:
   LIMITATIONS:
   ALGORITHM:
   NOTES:
******************************************************************************/
```

```
/* Hmmm. A bit tricky. */
```

```
/**
 * Gets the something
 *
 * @param num The num
 * @param offset The offset
 */
public void getSomething(int num, bool offset)
```

```
dim J
J = 0 'magic
J = J 'more magic
for J=1 to 100
...do stuff...
```

```
/** function header comments required to pass checkstyle */
```

```
/*
   MAB 08-05-2004: Who wrote this routine? When did they do it? Who should 
   I call if I have questions about it? It's worth it to have a good header
   here. It should helps to set context, it should identify the author 
   (hero or culprit!), including contact information, so that anyone who has
   questions can call or email. It's useful to have the date noted, and a 
   brief statement of intention. On the other hand, this isn't meant to be 
   busy work; it's meant to make maintenance easier--so don't go overboard.

   One other good reason to put your name on it: take credit! This is your
   craft
*/
```

```
try
{
  someSeeminglyTrivialMethod();
}
catch (Exception e)
{
  //Ignore. Should never happen.
}
```

```
;--- if LOGERR=1, errors are logged but debugging is difficult
;--- if LOGERR=0, errors are not logged but debugging is easy
LOGERR=1
```

```
if (returnValue ==0)
  doStuff();
else
  System.out.println("Beware of you, the Dragons are coming!");
```

```
// initialise the static variable to 0
count = 1;
```

```
..."AI code"...
if(something)
   goto MyAwesomeLabel;   //Who's gonna be the first to dump crap on me for this?
..."More Ai code"...

MyAwesomeLabel:
   //It wasn't that hard to get here, right?
   ..."Even more AI code"...
```

```
// Its stupid but it work
```

```
/* http://youtube.com/watch?v=oHg5SJYRHA0 */
```

```
[some code]
// [a commented out code line]
// this line added 2004-10-24 by JD.
// removed again 2004-11-05 by JD.
// [another commented out code line]
[some more code]
```

```
// it's a kind of magic (number)
$descr_id = 2;
$url_id = 34;
```

```
/* this is a hack.
 ToDo: change this code */
```

```
//the system can only handle 5 people right now. make sure where not //over
if(num_people>20){
```

```
# Let them send messages to the world
#del self.irc_PRIVMSG  # By deleting the method? Hello?
```

```
// Return value does not matter
return 0;
```

```
/// <summary>
/// Disables the web part. (Virtual method)
/// </summary>
public virtual void EnableWebPart() { /* nothing - you have to override it */ }
```

```
/*========================================================================

FUNCTION 
    DtFld_SetMin

DESCRIPTION
    This local function sets a nMin member of the Dtfld struct.

DEPENDENCIES
    None

ARGUMENTS
    [in]me
        Pointer to the Dtfld struct.
    [in]val
        Value to set

RETURN VALUE
    None.

SIDE EFFECTS
    None

NOTE
    None
========================================================================*/
/**
 @brief This local function sets a nMin member of the Dtfld struct..
 @param [in] me  Pointer to the Dtfld struct.
 @param [in]val Value to set
 @retval None 
 @note None
 @see None
*/

static __inline void DtFld_SetMin(DtFld *me, int val)
{
  me->nMin = val;
}
```

```
// Good luck
```

```
//TODO: Remove this comment.
```

```
#contentWrapper{
  position:absolute;
  top: 150px; //80 = 30 + 50 where 50 is margin and 30 is the height   //of the header
 }
```

```
/// <summary>
/// The Page_Load runs when the page loads
/// </summary>
private void Page_Load(Object sender, EventArgs e) {}
```

```
#region This is ugly but a mas has to do what a man has to do
Initialization of a gigantic array (...)
#endregion 
// Aren't you glad this has ended?
```

```
/******************************************************************************
   NAME:       (repeat the trigger name)
   PURPOSE:    To perform work as each row is inserted or updated.
   REVISIONS:
   Ver        Date        Author           Description
   ---------  ----------  ---------------  ------------------------------------
   1.0        27.6.2000             1\. Created this trigger.
   PARAMETERS:
   INPUT:
   OUTPUT:
   RETURNED VALUE:
   CALLED BY:
   CALLS:
   EXAMPLE USE:
   ASSUMPTIONS:
   LIMITATIONS:
   ALGORITHM:
   NOTES:
******************************************************************************/
```

```
//Whoa, now that's a big if condition.
```

```
if(some_condition){
    do_stuff();
}
else{
    //An error occurred!
}
```

```
// This method takes two integer values and adds them together via the built-in
// .NET functionality. It would be possible to code the arithmetic function
// by hand, but since .NET provides it, that would be a waste of time
private int Add(int i, int j) // i is the first value, j is the second value
{
    // add the numbers together using the .NET "+" operator
    int z = i + j;

    // return the value to the calling function
    // return z;

    // this code was updated to simplify the return statement, eliminating the need
    // for a separate variable.
    // this statement performs the add functionality using the + operator on the two
    // parameter values, and then returns the result to the calling function
    return i + j;
}
```

```
# Below is stub documentation for your module. You’d better edit it
```

```
//URGENT TODO: Reimplement this shit, the old code is as broken as hell… and we tought we solved all the problems
```

```
// HACK HACK HACK. REMOVE THIS ONCE MARLETT IS AROUND
```

```
[some code]
// [a commented out code line]
// this line added 2004-10-24 by JD.
// removed again 2004-11-05 by JD.
// [another commented out code line]
[some more code]
```

```
//we trick it, if forbidden, as if it had already existed
```

```
/* this is a hack.
 ToDo: change this code */
```

```
// this is messed up, and no one actually knows how it works anymore..
```

```
//Forgive me if you can!
```

```
/* FIXME: documentation for the bellow functionality - and why are we doing it this way */
```

```
// Had to do this because DB is in Abbie Normal Form
```

```
//Array where widgets live. Should have been called WidgetLand
  tWidget Widgets[MAX_WIDGETS];
```

```
# should never ever ever ever happen
	DEBUG and print "AYYAYAAAAA at line ", __LINE__, "\n";
	die "SPORK 512512!";
```

```
public class A {
	protected<type> foo;
	public void setFoo(<type> fooVal);
	public <type> getFoo();
	public void doSomething() {
	. . . 
	foo = x.munge();
	. . .
	}
  };

  public class B extends A {
	/* redeclared here for clarity */
	protected foo;
	public void doSomething() {
	. . . 
	foo = x.munge();
	. . .
	}
```

```
CRect  rectUm;	//  darn near killed'em
```

```
// Since this file is loaded on every page, and since 
  // I want the following function on every page, I'm going to 
  // cheat and include it here even though it has nothing to 
  // do with the other things in this file. 
  // If you don't like it, bite me!
```

```
* Single bit errors are detected and corrected
 * Double bit errors are detected
 * Undetected errors are ignored
```

```
// Hocus Pocus, grab the focus
  winSetFocus(...)
```

```
//if nothing else can be found to run, will go feed the ducks.
```

```
//What the F**K are you looking at?
```

```
# All your space are belong to perl
 $customer =~ s/ //g;
```

```
// below here be scary parsing related things
```

```
//WORKAROUND compiler bug with following code.
  //static final Class[] OIS_ARGS = {ObjectInpuStream.class};
  //static final Class[] OOS_ARGS = {ObjectOutpuStream.class};
```

```
// Shouldn't ever happen... so close display if it does
  ecr_close_display();
  break;
```

```
// according to the Win98 docs, this should be 1
// according to the WinNT docs, this should be 2
// they are both wrong!
```

```
// Return a status block suitable for inclusion in the reply
// buffer to Control.  Note: this code sucks.
```

```
if ((eIOObjectType == Prototype_  && HR_FAILED(SchemaProcessor.Validate[AndOr](https://wiki.c2.com/?AndOr)Create())) ||
	(eIOObjectType == Production_ && HR_FAILED(SchemaProcessor.ValidateOnly())) )
  {
	ASSERT(FALSE);
	// Don't put an assertion here; there are enough assertions  in the
	// schema processor to kill a horse already.
	return HR_Failure;
  }
```

```
// Ad Index scheming and plotting - Those with
// heart conditions are advised to not continue
```

```
// Bits 6, 5, and 4 must be 0, 1, and 0 respectively.
// Otherwise, the oscillator burns crazy evil crack.
```

```
//The following code was written by <developer name>.
//Unless it doesn't work, then I have no idea who wrote it.
```

```
/* And now, ladies and gentlemen, the chord from HELL! */
```

这是另一篇 [30 多条人们在代码](https://medium.com/javascript-in-plain-english/17-piece-of-art-code-comment-people-wrote-in-code-60a4284e0d92)中写的艺术代码评论，你可能也会喜欢。

检查实际的 [**堆栈溢出**](https://stackoverflow.com/questions/143429/whats-the-least-useful-comment-youve-ever-seen?page=1&tab=active#tab-top) 问题，以获得更多类似上面的问题。你可以在评论中添加你的上衣。我将在后面的原文中补充这一点。

## 用简单英语写的 JavaScript 的注释

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的爱:[](https://medium.com/ai-in-plain-english)**[**UX**](https://medium.com/ux-in-plain-english)[**Python**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！****

****我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。******

# ******感谢阅读。**🍻********