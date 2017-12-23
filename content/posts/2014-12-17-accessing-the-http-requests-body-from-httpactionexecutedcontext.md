---
title: Accessing the Http Request’s body from HttpActionExecutedContext
author: ely
type: post
date: 2014-12-17T05:16:53+00:00
url: /post/accessing-the-http-requests-body-from-httpactionexecutedcontext/
dsq_thread_id:
  - 3333635941

---
If you have ever tried to directly access the body of a Http request on a POST or PUT in Web Api, you might have run into some roadblocks. Unlike the MVC pipeline, Web Api accesses the request of the body through a stream for performance reasons. Once the stream is read during the model binding phase, the stream is at the end, and if you try to read it at a later time, you might notice the body is just a blank string.

Today I was writing a ExceptionFilter and wanted to log the body on certain error conditions. The HttpActionExecutedContext had the Request object, which had the Content property, but when I tried reading it (using context.Request.ReadAsStringAsync()), the result was the aforementioned blank string. This is because when the stream was read earlier in the process, the stream was left at the end. In order to get the body, you need to reset the position in the stream back to zero and then read it. The following function will return the content of the body as a string that you can use in many of Web Apis filter attributes:

<pre class="brush: csharp; title: ; notranslate" title="">private string GetBodyFromRequest(HttpActionExecutedContext context)
{
    string data;
    using (var stream = context.Request.Content.ReadAsStreamAsync().Result)
    {
        if (stream.CanSeek)
        {
            stream.Position = 0;
        }
        data = context.Request.Content.ReadAsStringAsync().Result;
    }
    return data;
}
</pre>