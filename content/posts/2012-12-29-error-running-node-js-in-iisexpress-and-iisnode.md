---
title: Error running Node JS in IISExpress and IISNode
author: ely
type: post
date: 2012-12-29T22:07:52+00:00
url: /post/error-running-node-js-in-iisexpress-and-iisnode/
dsq_thread_id:
  - 999340587

---
I recently started playing around with Node JS, and have been going through TekPub&#8217;s BackBone JS tutorials (which uses Node for the backend). Â In the tutorial, Rob uses Webmatrix on Windows8, and I followed along, however, I ran into a problem the first time I tried to run the site using IISExpress and IISNode:

&#8220;The iisnode module is unable to start the node.exe process. Make sure the node.exe executable is available at the location specified in theÂ <a href="https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config" rel="nofollow">system.webServer/iisnode/@nodeProcessCommandLine</a>element of web.config. By default node.exe is expected to be installed in %ProgramFiles%\nodejs folder on x86 systems and %ProgramFiles(x86)%\nodejs folder on x64 systems.&#8221;

I went into the web.config file, and there is a commented out section for a bunch of different options for IISNode. Â One of them is nodeProcessCommandLine, which is the path to node.exe. Â On 64 bit systems, this is installed in Program Files, and even though the path was correct, IISNode refused to start. Â I did some searching and found this post onÂ <a href="http://stackoverflow.com/questions/13079199/error-running-node-app-in-webmatrix" target="_blank">StackOverlow</a>, which suggested that there is a current bug in IISNode where it always looks for node in the 32 bit path (Program Files (x86)). Â The solution was to go back to Node&#8217;s website, and download and install the 32 bit version. Â This worked for me, but I was surprised that more people haven&#8217;t run into this issue, and the default install from Node is the x64 version. Â Hope this helps anyone else that runs into this problem.