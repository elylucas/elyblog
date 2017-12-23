---
title: JavaScript Formatting in Visual Studio 2008 sp1
author: ely
type: post
date: 2010-02-08T10:27:00+00:00
url: /post/javascript-formatting-in-visual-studio-2008-sp1/
dsq_thread_id:
  - 1669952972

---
The formatting of JavaScript in VS 2008 has finally drove me nuts.&nbsp; Particularly, when pressing enter after a statement, the IDE will decrease the indent of the line above.&nbsp; Why it does this makes no sense to me, but there is a way to get around it.

In VS2008, go to Tools>Options>Text Editor>JScript>Formatting.&nbsp; Uncheck &ldquo;Format completed line on Enter&rdquo;.&nbsp; This will keep the IDE from changing the formatting when adding new lines of code to a function/code block in JavaScript.