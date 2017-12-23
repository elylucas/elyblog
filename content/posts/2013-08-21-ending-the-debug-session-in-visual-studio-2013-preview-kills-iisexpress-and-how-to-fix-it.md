---
title: Ending the debug session in Visual Studio 2013 Preview kills IISExpress, and how to fix it
author: ely
type: post
date: 2013-08-21T18:51:28+00:00
url: /post/ending-the-debug-session-in-visual-studio-2013-preview-kills-iisexpress-and-how-to-fix-it/
dsq_thread_id:
  - 1624900915

---
It seems that by default when you end the debugging session of an ASP.Net app in Visual Studio 2013 Preview, it will kill the IISExpress instance as well. This is fairly annoying as my workflow usually involves leaving the browser window open and just doing a new build, and then refreshing the browser. Fortunately it was an easy fix, all you need to do is go the the project properties for the web application, go to the web tab, and uncheck &#8220;Enable Edit and Continue&#8221;. 

I hope that this is just a bug and will be fixed in the final release of VS2013, as the idea of finally getting edit and continue back is cool, but not at the expense of killing IISExpress.