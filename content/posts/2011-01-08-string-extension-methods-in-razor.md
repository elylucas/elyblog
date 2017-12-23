---
title: String Extension Methods in Razor
author: ely
type: post
date: 2011-01-08T17:16:08+00:00
url: /post/string-extension-methods-in-razor/
dsq_thread_id:
  - 2384902976

---
The new Razor view engine contains several handy extension methods you can use in your views.&#160; These methods mostly deal with either detecting if a string value can be converted to a particular type, or converting string values to a particular type.&#160; For instance, the following code checks to see if a querystring value is a decimal, and if so, to format it as a currency:

<pre><br />@{<br />&#160;&#160;&#160; if (Request.QueryString["amount"].IsDecimal())<br />&#160;&#160;&#160; {<br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; @Request.QueryString["amount"].AsDecimal().ToString("c")W<br />&#160;&#160;&#160; }<br />&#160;&#160;&#160; else <br />&#160;&#160;&#160; { <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; @: Amount is not a valid number<br />&#160;&#160;&#160; }<br />}</pre>

There are Is(Type), and As(Type) methods for bools, datetimes, decimals, floats, and ints. All of the As(Type) conversion methods take in an optional second parameter that can serve as the default value in case the conversion is not successful.

These extension methods are handy and can save some time and lines of code when dealing with strings.&#160; They are in the System.Web.WebPages namespace, which new projects in MVC3 and Webmatrix automatically reference, so you do not need to worry about adding any using statements into your views in order to take advantage of them.&#160; If you want to take advantage of them in your views, you can simply add a using statement to System.Web.WebPages.