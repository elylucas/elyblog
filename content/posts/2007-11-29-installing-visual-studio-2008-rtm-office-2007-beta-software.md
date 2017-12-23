---
title: Installing Visual Studio 2008 RTM – Office 2007 Beta Software
author: ely
type: post
date: 2007-11-29T05:11:00+00:00
url: /post/installing-visual-studio-2008-rtm-office-2007-beta-software/
dsq_thread_id:
  - 1660811352

---
So Visual Studio 2008 RTM was released last week.&nbsp; I'm a bit behind the game and just downloaded it from MSDN.&nbsp; After the long download, I burnt the ISO to disk and began the install, and what happened?&nbsp; Installation failure.&nbsp; I went and did some googling and it seems there are a lot of people having installation problems with RTM.&nbsp; I followed some of the suggestions I found and nothing seemed to work for me.&nbsp; So, I had to go and do the research myself in order to get this thing installed. 

My installation was failing on the Microsoft Visual Studio Web Authoring Component, which is the third item installed if you do the standard installation (after the .Net framework 3.5 and Microsoft Document Explorer 2008).&nbsp; I went into the D:\WCU\WebDesignerCore folder and used winzip to extract WebDesignerCore.EXE into another folder, then I attempted to do the installation of the Web Authoring Components manually to see what happened.&nbsp; I received the following error: 

"_Setup is unable to proceed due to the following error(s): The 2007 Microsoft Office system does not support upgrading from a prerelease version of the 2007 Microsoft Office system. You must first uninstall any prerelease versions of the 2007 Microsoft Office system products and associates technologies._" 

Which was a bit strange to me since I am still on Office 2003 and haven't installed any Office 2007 betas to my knowledge.&nbsp; I dove back into my Add Remove Programs to make sure I didn't have any betas hanging around.&nbsp; What I did find was that I had the Compatablity Pack for the 2007 Office System (Beta) installed.&nbsp; I got this a few months back so that I could open up the new Word docx file format using Word 2003.&nbsp; Seems that when Microsoft first released this patch, it was using Office 2007 beta software.&nbsp; I uninstalled this package and tried the Web Component install again.&nbsp; Worked like a charm this time. 

I cancelled the installation and began fresh installing VS2008 once again.&nbsp; This time it successfully went through the Web Components and continued with the rest of the installation with no issues. 

Uninstalling the Compatability Pack worked for my issue.&nbsp; Microsoft has since released a new version of the Compatablity Pack that does not use Office 2007 beta software.&nbsp; You can find that <a href="http://www.microsoft.com/downloads/details.aspx?FamilyId=941b3470-3ae9-4aee-8f43-c6bb74cd1466&displaylang=en" target="_blank">here</a>. 

&nbsp;