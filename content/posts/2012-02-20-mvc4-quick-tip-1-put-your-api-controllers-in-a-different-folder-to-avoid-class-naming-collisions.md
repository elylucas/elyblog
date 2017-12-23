---
title: 'MVC4 Quick Tip #1–Put your API Controllers in a different folder to avoid class naming collisions'
author: ely
type: post
date: 2012-02-20T15:23:16+00:00
url: /post/mvc4-quick-tip-1-put-your-api-controllers-in-a-different-folder-to-avoid-class-naming-collisions/
dsq_thread_id:
  - 1146228455

---
This post is the start of a series of quick tips for Asp.Net MVC 4, which was released in beta form last week.&#160; You can find out more about MVC4 and download the beta from [www.asp.net/mvc/mvc4][1].

**_Disclaimer: This code is based on MVC 4 Beta 1, and might change in future releases._**

MVC 4 ships with the new Asp.net Web APIs, which allow you to create RESTful services from within your web projects (Web Forms or MVC).&#160; In previous versions of MVC, people commonly used Controllers to return JSON to front end websites and other clients.&#160; While this worked for many scenarios, it was difficult to create services that were truly RESTful in nature, taking advantage of all that HTTP had to offer.

With Web APIs, you can now create “API” Controllers.&#160; While these controllers don’t require you to follow REST, their default behavior encourages you to do so.

When starting up a new MVC 4 project, you might be tempted to put new API Controllers in the same folder&#160; (or more accurately, namespace) as your MVC Controllers.&#160; While this will work, you are limited in that you cannot have two classes with the same name in the same namespace.&#160; It will be a common scenario when you have a MVC Controller with the same name as a API Controller.&#160; For instance, you can have a MVC AccountController to serve views for Accounts in your system, and an API AccountController to respond to XHR requests from your website (or mobile devices, other services, etc..).&#160; One way I have found to get around this limitation is to put your API Controllers in a folder called API (or whatever folder/namespace you wish), like so:

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="controller" border="0" alt="controller" src="http://www.elylucas.net/image.axd?picture=controller_thumb.png" width="204" height="151" />][2]

Default routes in MVC 4 will respond to MVC controllers with the route of /{controller},&#160; and the default API route will respond to routes of /api/{controller}.&#160; Since the AccountsController in the API folder inherits from APIController, it is okay that you have two classes named AccountsController in your project, the routing engine will know to use the correct one in the API folder.

 [1]: http://www.asp.net/mvc/mvc4
 [2]: http://www.elylucas.net/image.axd?picture=controller.png