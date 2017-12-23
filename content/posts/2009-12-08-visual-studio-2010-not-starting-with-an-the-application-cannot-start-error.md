---
title: Visual Studio 2010 Not Starting with an “The Application Cannot Start” error?
author: ely
type: post
date: 2009-12-08T12:19:23+00:00
url: /post/visual-studio-2010-not-starting-with-an-the-application-cannot-start-error/
dsq_thread_id:
  - 1608470523

---
I started getting this error out of the blue trying to startup VS 2010 tonight.&#160; A quick bingle search lead me to the <a href="https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=499244&wa=wsignin1.0" target="_blank">Microsoft Connect</a> site where somebody has already filed it as a bug, and a quick workaround to get VS to start again was to run “devenv /resetuserdata” in the C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE directory.&#160; The problem seems to stem from having a corrupted profile (at least it did for me), and running that command fixed the problem.&#160; 

If you are having this issue, give this a try, and head over to the Connect site and vote up the bug to get fixed.