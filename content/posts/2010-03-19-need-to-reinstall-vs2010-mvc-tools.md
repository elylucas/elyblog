---
title: Need to Reinstall VS2010 MVC Tools?
author: ely
type: post
date: 2010-03-19T18:36:24+00:00
url: /post/need-to-reinstall-vs2010-mvc-tools/
dsq_thread_id:
  - 1734626296

---
Last week, <a href="http://haacked.com/archive/2010/03/11/aspnet-mvc2-released.aspx" target="_blank">MVC 2</a> was released, and like many ecstatic developers, I grabbed the bits right away and downloaded them.&#160; First, however, I uninstalled all the MVC2 RC bits I had, including the tools for VS2008 and VS2010.&#160; Then I installed RTM, and… came to realize that this release was for VS2008 only.&#160; Ok, no biggie, I can wait till VS2010 RTMs to use the final MVC2 bits, so I uninstalled RTM and reinstalled RC.&#160; However, the bits that includes all the templates and tooling for VS2010 is not included in the RC release, only the tools.&#160; Phil Haack had a post about it <a href="http://haacked.com/archive/2010/02/10/installing-asp-net-mvc-2-rc-2-on-visual-studio.aspx" target="_blank">here</a>.&#160; 

So I needed to get the tools reinstalled.&#160; I knew I could probably uninstall VS2010 and reinstall, but I wanted to avoid that if I could.&#160; Fortunately, in the comment section from the above blog post, there was a quick solution that worked.&#160; Simply run <dvd>\WCU\ASPNETMVC\VS2010ToolsMVC2.msi, and the tools will be reinstalled.&#160; Yay for saving time.