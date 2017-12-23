---
title: 'MVC4 Quick Tip #3–Removing the XML Formatter from ASP.Net Web API'
author: ely
type: post
date: 2012-03-05T14:09:30+00:00
url: /post/mvc4-quick-tip-3-removing-the-xml-formatter-from-asp-net-web-api/
dsq_thread_id:
  - 998617389

---
ASP.Net Web API provides a powerful new feature called content negotiation that will automatically format of your requests based on what the client asks for.&#160; For instance, if the client sends ‘application/xml’ in the Content-Type HTTP header, then the format of your response will be XML.&#160; The two default formatters included in Web API are the XML formatter, and the JSON formatter. The beauty of the automatic content negotiation is that you don’t need to write any code to map your models into these formats, it is taken care of for you by the formatters.&#160; You can even create your own formatters to serve up different types as well, such as images, vCards, iCals, etc.

If all you are doing with your Web API is serving up JSON to either web or mobile clients, you might not feel the need to offer up any other format besides JSON.&#160; This might also be helpful for quickly testing your APIs when hitting them with a web browser as some browsers send application/xml as their default content type.

**_Disclaimer: This code is based on MVC 4 Beta 1, and might change in future releases._**

To remove the XML formatter is really easy, and I found this tip from <a href="http://codebetter.com/glennblock/2012/02/26/disabling-the-xml-formatter-in-asp-net-web-apithe-easy-way-2/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+CodeBetter+%28CodeBetter.Com%29" target="_blank">Glenn Block</a>, who conveniently posted this to his blog while I was searching for a way to do this.&#160; In order to do so, put the following code somewhere in your Application_Start method in the global.asax.cs file:

<pre class="brush: csharp">GlobalConfiguration.Configuration.Formatters.XmlFormatter.SupportedMediaTypes.Clear()</pre>