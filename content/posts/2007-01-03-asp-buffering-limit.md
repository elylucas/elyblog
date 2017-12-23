---
title: ASP Buffering Limit
author: ely
type: post
date: 2007-01-03T07:05:00+00:00
url: /post/asp-buffering-limit/
dsq_thread_id:
  - 1722416700

---
I'm sure this isn't new to any blogs anywhere, but I wanted a quick place to find it in case I need it again in the future.&nbsp; We had a problem this morning with an asp page giving the following error: 

&nbsp;Response object error 'ASP 0251 : 80004005' 

Response Buffer Limit Exceeded 

/page.asp, line 0 

Execution of the ASP page caused the Response Buffer to exceed its configured limit. 

Turns out that IIS6 sets a limit on how much data can be stored in the response buffer when you are creating a page.&nbsp; On the particular page that was giving us problems this morning, the total output was over 12MB, and the default for the ASPBufferLimit is only 4MB.&nbsp; Sure, the 12MB of html being sent back to the client is a problem that will have to be fixed through redesigning the page, but I needed to get the size of the buffer increased so this page would work now.&nbsp; So, to do so we increase the ASPBufferLimit through the adsutil utility: 

adsutil set w3svc/aspbufferinglimit "4194304" 

This will set the ASPBufferingLimit on the global web service that all the virtual directories inherit from, meaning that all the sites configured on this box now have this limit.&nbsp; The number is the amount of bytes for the buffer limit, 4194304 (4MB) being the default.