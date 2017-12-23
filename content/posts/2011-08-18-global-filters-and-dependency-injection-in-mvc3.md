---
title: Global Filters and Dependency Injection in MVC3
author: ely
type: post
date: 2011-08-18T01:07:46+00:00
url: /post/global-filters-and-dependency-injection-in-mvc3/
dsq_thread_id:
  - 1587328022

---
I’ve been struggling with finding a good solution to resolve dependencies in global filters lately.&#160; Since it does not appear that global filters are created through the FilterAttributeFilterProvider (I don’t get any results back from base.GetFilters, hit me up if I’m doing something wrong here), my global filters that had resolve attributes on its properties were never being created by the provider, and thus, not getting those properties resolved.

It then dawned on me that the global filters are actually being instantiated in the global.asax.cs file (or wherever your app startup exists at).&#160; So instead of newing up the object directly, I just let Ninject do it for me:

private static void RegisterGlobalFilters(GlobalFilterCollection filters, IKernel kernel)   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; {   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; filters.Add(new HandleErrorAttribute());   
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; filters.Add(kernel.TryGet(typeof(UserProviderFilterAttribute)));   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; }

At first this felt like a giant hack, but after awhile I warmed up to it and realized that since this is the area of responsibility for creating global filters, that using Ninject to resolve its dependencies does in fact belong here.

If you have other methods for doing resolving your dependencies in global filters, or if I am way off base, I would love to hear any comments or suggestions.