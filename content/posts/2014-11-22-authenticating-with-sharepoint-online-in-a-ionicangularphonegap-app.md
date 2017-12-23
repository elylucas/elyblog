---
title: Authenticating with SharePoint Online in an Ionic/Angular/PhoneGap app
author: ely
type: post
date: 2014-11-22T16:30:44+00:00
url: /post/authenticating-with-sharepoint-online-in-a-ionicangularphonegap-app/
dsq_thread_id:
  - 3252249379

---
I am working on a side project to update lists in a SharePoint online instance. The project is a hybrid mobile app using PhoneGap and the Ionic framework. I have been playing around with Ionic for a couple of months now, and so far I am really enjoying it. For starters, it uses AngularJS, which is my fav MV* framework of the moment. Also, Ionic provides great tooling to help you build and package up your app. If you haven&#8217;t checked it out yet, do so <a href="http://ionicframework.com/" title="ionicframework.com/" target="_blank">http://ionicframework.com/</a>.

The first hurdle in the app was that I am no SharePoint expert and I don&#8217;t know the first thing about their APIs or how to work with them. I needed to figure out how to actually authenticate with the SharePoint online instance. Doing a Google search returned back so many results and different ways to accomplish authentication in SP that it was very confusing which to try. I tried a few different ways and none of them seemed to work. Then I found <a href="http://allthatjs.com/2012/03/28/remote-authentication-in-sharepoint-online/" target="_blank">this</a> great blog post. It turns out that there appears to be a difference when trying to authenticate with SharePoint Online versus an on-prem SharePoint 2013 instance, and most of the articles I was reading was regarding authenticating with an on-prem instance. 

The post details how you need to send a SAML token to the Microsoft ID Service, which is the provider of authentication for nearly every MS property on the web, from Office365 to Xbox. While the post didn&#8217;t show exactly how to do this in JavaScript, it was fairly easy to make it work.

Since my Ionic app is just a web app running on a local device, I can take advantage of reusing the cookies that come from authenticating with the Microsoft ID service. The following Angular service does just that. Once you authenticate, you will have two cookies called rtFA and FedAuth. Every request you make into the Sharepoint REST APIs will now have these cookies and you will be authenticated.



The service first constructs a SAML token using a userId, password, and url of the SP Online instance. It then calls into Microsoft&#8217;s login service to obtain a authentication token. From that token, you need to extract the Bearer Token, and then pass that into Sharepoints Forms based authentication. Once that is done, you will have the needed cookies to make future API requests.