---
title: Using a grid that can sort, page, and filter in Asp.Net MVC3–Part 1–Using the WebGrid WebHelper
author: ely
type: post
date: 2011-03-07T16:11:54+00:00
url: /post/using-a-grid-that-can-sort-page-and-filter-in-asp-net-mvc3-part-1-using-the-webgrid-webhelper/
dsq_thread_id:
  - 998585000

---
If you are coming from Asp.Net WebForms development, you are probably use to controls that include a lot of base functionality out of the box.&#160; Some of these controls were the DataGrid and GridView, which would construct an html table for you without writing a single line of html.&#160; While these controls offered productivity gains because you did not need to mess with the html, css, or JavaScript in order to get them to run, they gave you little control on what the output was, and often times, the output was less than desirable.

In Asp.Net MVC development, you have complete control of your html output.&#160; This is great news for those that want this type of power, but for those that still look for the productivity gains of some type of grid control, using MVC had its drawbacks.&#160; There have been a few grid controls provided by the community (such as the one in <a href="http://mvccontrib.codeplex.com/" target="_blank">MvcContrib</a>), however, there is now one available in MVC3 out of the box.&#160; With the release of MVC3 and WebMatrix, there is now a suite of web helpers, one of which is the WebGrid, which we will be using in Part 1 of this series to create a grid that can sort, page, and filter data on an html page.

The WebGrid WebHelper is located in the System.Web.WebHelpers assembly, which is referenced by default in a new MVC3 application.&#160; 

**Getting the MvcWebGrid app setup**

To begin, create a new MVC3 application called MvcWebGrid.&#160; Make sure you select Empty for the template, and that the view engine is set to Razor.&#160; Next, right click on the Controllers folder and select add->New Controller.&#160; Name the controller HomeControler, and leave the checkbox to create the CRUD methods unchecked.

For simplicity sake, we will not connect to a real database in this sample.&#160; Instead, I have created a class that returns a lists of music albums (generated from the MvcMusicStore tutorial at <a href="http://mvcmusicstore.codeplex.com" target="_blank">mvcmusicstore.codeplex.com</a>).&#160; You can find this class in the code download (linked at the end of this article), or <a href="https://gist.github.com/859514" target="_blank">here</a> on github.&#160; This is a simple static class with a single method called GetAlbums, which returns an IEnumerable<Album>.&#160; Add this class to your models folder.

In your home controller, modify your Index action method to like like so:

<div class="csharpcode">
  <div class="csharpcode">
    <pre class="alt"><span class="lnum">   1:  </span><span class="kwrd">public</span> ActionResult Index()</pre>
    
    <pre><span class="lnum">   2:  </span>{</pre>
    
    <pre class="alt"><span class="lnum">   3:  </span>    var albums = AlbumRepo.GetAlbums();</pre>
    
    <pre><span class="lnum">   4:  </span>    <span class="kwrd">return</span> View(albums);</pre>
    
    <pre class="alt"><span class="lnum">   5:  </span>}</pre></p>
  </div>
</div>

Nothing fancy happening here, we are just returning the whole list of albums to our view.&#160; We still need to create our index view, but before we do, lets update the _Layout.cshtml file in the Views/Shared folder so our site has a bit more of a look to it.&#160; Modify the file like so:

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span><span class="kwrd">&lt;!</span><span class="html">DOCTYPE</span> <span class="attr">html</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   2:  </span><span class="kwrd">&lt;</span><span class="html">html</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">   3:  </span><span class="kwrd">&lt;</span><span class="html">head</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   4:  </span>    <span class="kwrd">&lt;</span><span class="html">title</span><span class="kwrd">&gt;</span>@ViewBag.Title<span class="kwrd">&lt;/</span><span class="html">title</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">   5:  </span>    <span class="kwrd">&lt;</span><span class="html">link</span> <span class="attr">href</span><span class="kwrd">="@Url.Content("</span>~/<span class="attr">Content</span>/<span class="attr">Site</span>.<span class="attr">css</span><span class="kwrd">")"</span> <span class="attr">rel</span><span class="kwrd">="stylesheet"</span> <span class="attr">type</span><span class="kwrd">="text/css"</span> <span class="kwrd">/&gt;</span></pre>
  
  <pre><span class="lnum">   6:  </span>    <span class="kwrd">&lt;</span><span class="html">script</span> <span class="attr">src</span><span class="kwrd">="@Url.Content("</span>~/<span class="attr">Scripts</span>/<span class="attr">jquery-1</span>.<span class="attr">4</span>.<span class="attr">4</span>.<span class="attr">min</span>.<span class="attr">js</span><span class="kwrd">")"</span> <span class="attr">type</span><span class="kwrd">="text/javascript"</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">   7:  </span>    <span class="kwrd">&lt;/</span><span class="html">script</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   8:  </span><span class="kwrd">&lt;/</span><span class="html">head</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">   9:  </span>&#160;</pre>
  
  <pre><span class="lnum">  10:  </span><span class="kwrd">&lt;</span><span class="html">body</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  11:  </span>    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">id</span><span class="kwrd">="header"</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  12:  </span>        Welcome to my awesome music site where </pre>
  
  <pre class="alt"><span class="lnum">  13:  </span>        a deep dish of awesome stew is served up daily</pre>
  
  <pre><span class="lnum">  14:  </span>        <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">/&gt;</span> <span class="kwrd">&lt;</span><span class="html">p</span> <span class="kwrd">/&gt;&lt;</span><span class="html">p</span> <span class="kwrd">/&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  15:  </span>    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  16:  </span>    </pre>
  
  <pre class="alt"><span class="lnum">  17:  </span>    @RenderBody()</pre>
  
  <pre><span class="lnum">  18:  </span>&#160;</pre>
  
  <pre class="alt"><span class="lnum">  19:  </span>    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">id</span><span class="kwrd">="footer"</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  20:  </span>        <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">/&gt;</span> <span class="kwrd">&lt;</span><span class="html">p</span> <span class="kwrd">/&gt;&lt;</span><span class="html">p</span> <span class="kwrd">/&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  21:  </span>        You have reached the bottom of this page, now what?</pre>
  
  <pre><span class="lnum">  22:  </span>    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  23:  </span><span class="kwrd">&lt;/</span><span class="html">body</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  24:  </span><span class="kwrd">&lt;/</span><span class="html">html</span><span class="kwrd">&gt;</span></pre>
</div>

**Adding the WebGrid to the app**

Now that we have a basic layout for our site, lets create the index view.&#160; Go back to the home controller, right click anywhere within the index action method, and select add view.&#160; Keep all the defaults and click Add.

Change the new view to look like the following:

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span>@model IEnumerable<span class="kwrd">&lt;</span><span class="html">Album</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   2:  </span>&#160;</pre>
  
  <pre class="alt"><span class="lnum">   3:  </span>@{</pre>
  
  <pre><span class="lnum">   4:  </span>    ViewBag.Title = "MVC Music Store Grid";</pre>
  
  <pre class="alt"><span class="lnum">   5:  </span>}</pre>
  
  <pre><span class="lnum">   6:  </span>&#160;</pre>
  
  <pre class="alt"><span class="lnum">   7:  </span>@using (Html.BeginForm())</pre>
  
  <pre><span class="lnum">   8:  </span>{</pre>
  
  <pre class="alt"><span class="lnum">   9:  </span>    <span class="kwrd">&lt;</span><span class="html">fieldset</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  10:  </span>        <span class="kwrd">&lt;</span><span class="html">legend</span><span class="kwrd">&gt;</span>Search<span class="kwrd">&lt;/</span><span class="html">legend</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  11:  </span>&#160;</pre>
  
  <pre><span class="lnum">  12:  </span>        <span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  13:  </span>            Album Id:</pre>
  
  <pre><span class="lnum">  14:  </span>        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  15:  </span>        <span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  16:  </span>            <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="text"</span> <span class="attr">id</span><span class="kwrd">="albumid"</span> <span class="attr">name</span><span class="kwrd">="albumid"</span> <span class="kwrd">/&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  17:  </span>        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  18:  </span>&#160;</pre>
  
  <pre class="alt"><span class="lnum">  19:  </span>        <span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  20:  </span>            Album Name:</pre>
  
  <pre class="alt"><span class="lnum">  21:  </span>        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  22:  </span>        <span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  23:  </span>            <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="text"</span> <span class="attr">id</span><span class="kwrd">="albumName"</span> <span class="attr">name</span><span class="kwrd">="albumName"</span> <span class="kwrd">/&gt;</span></pre>
  
  <pre><span class="lnum">  24:  </span>        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  25:  </span>&#160;</pre>
  
  <pre><span class="lnum">  26:  </span>        <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  27:  </span>            <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="submit"</span> <span class="attr">value</span><span class="kwrd">="Search"</span> <span class="kwrd">/&gt;</span></pre>
  
  <pre><span class="lnum">  28:  </span>        <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  29:  </span>    <span class="kwrd">&lt;/</span><span class="html">fieldset</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  30:  </span>}</pre>
  
  <pre class="alt"><span class="lnum">  31:  </span>&#160;</pre>
  
  <pre><span class="lnum">  32:  </span><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">id</span><span class="kwrd">="myGrid"</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  33:  </span>    @Html.Partial("_grid", Model)</pre>
  
  <pre><span class="lnum">  34:  </span><span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
</div>

We will be separating the WebGrid into its own partial view, so right click on the Views/Home folder, select add, and then view.&#160; Name the view _grid, and check the “Create as a partial view” checkbox, then click add.&#160; Prepending the name of our Razor view with a underscore tells Asp.Net that this file should not be served if requested directly, which is perfect for our partial views.

In our grid partial view, we will now be using the new WebGrid helper.&#160; We start off creating an instance of the WebGrid (passing in our model into the constructor), and then call the GetHtml method which generates the html for the grid and outputs it to the screen:

<div class="csharpcode">
  <div class="csharpcode">
    <pre class="alt"><span class="lnum">   1:  </span>@model IEnumerable&lt;Album&gt;</pre>
    
    <pre><span class="lnum">   2:  </span>&#160;</pre>
    
    <pre class="alt"><span class="lnum">   3:  </span>@{</pre>
    
    <pre><span class="lnum">   4:  </span>    var grid = <span class="kwrd">new</span> WebGrid(Model);</pre>
    
    <pre class="alt"><span class="lnum">   5:  </span>    @grid.GetHtml();</pre>
    
    <pre><span class="lnum">   6:  </span>}</pre></p>
  </div>
</div>

When we run the app, we get a web page that looks like so:

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://www.elylucas.net/image.axd?picture=image_thumb.png" width="653" height="421" />][1]

If you inspect the html for the new grid, you will see that it is clean markup, with no added styles or nasty viewstate that we have seen in the past with WebForms.&#160; You might also notice that the table html is semantically correct, where the header is in the thead section, the content in the tbody section, and the paging in the tfoot section.&#160; Having your tables formatted properly will go a long ways if you want to try to extend your table functionality using JavaScript libraries such as jQuery, where it was often difficult to achieve this in WebForms.

Another bonus is that the paging and sorting already work!&#160; Out of the box, the WebGrid provides these functions by doing Get requests and passing in a few values over the query string.&#160; The WebGrid will see these values in the new request, and adjust the output accordingly.&#160; This means that the WebGrid expects the entire set of data in each view in order to generate the paging properly.&#160; For small sets of data this is not a problem, but even in our sample app you can notice a lag when hitting the links.&#160; Fixing this issue is out of the scope of this article, but just be aware that you will need to come up with a different solution if your data sets are large.

The WebGrid provides a lot of options for configuration.&#160; These options are provided by passing in parameters to the WebGrid’s constructor, as well as the GetHtml method.

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://www.elylucas.net/image.axd?picture=image_thumb_1.png" width="657" height="381" />][2]

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://www.elylucas.net/image.axd?picture=image_thumb_2.png" width="657" height="410" />][3]

We will modify some of these options to add some Ajax functionality to the screen (so that paging and sorting don’t refresh the entire screen), as well as change how some of the columns appear.&#160; Modify _grid.cshtml to look like so:

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span>@model IEnumerable<span class="kwrd">&lt;</span><span class="html">Album</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   2:  </span>&#160;</pre>
  
  <pre class="alt"><span class="lnum">   3:  </span>@{</pre>
  
  <pre><span class="lnum">   4:  </span>    var grid = new WebGrid(Model, rowsPerPage: 15, ajaxUpdateContainerId: "myGrid");</pre>
  
  <pre class="alt"><span class="lnum">   5:  </span>    @grid.GetHtml(columns: grid.Columns(</pre>
  
  <pre><span class="lnum">   6:  </span>                        grid.Column("AlbumId", "Album Id"),</pre>
  
  <pre class="alt"><span class="lnum">   7:  </span>                        grid.Column("Title", "Title"),</pre>
  
  <pre><span class="lnum">   8:  </span>                        grid.Column("Artist", "Artist")</pre>
  
  <pre class="alt"><span class="lnum">   9:  </span>                        ));</pre>
  
  <pre><span class="lnum">  10:  </span>}</pre>
</div>

In the constructor, we setting the size of the page to handle 15 albums at a time, as well as setting the container id of the div in the index.cshtml page that holds our grid.&#160; The simple act of setting the ajaxUpdateContainerId will automatically enable our screen to use Ajax to update the screen on pages and sorts.&#160; All we need to do is make sure jQuery is included in the project, which it is by default in the _Layout.cshtml page.

The GetHtml method take a columns param, where we pass in a list of column objects we want to use on the grid.

One thing I don’t particularly care for is how the Ajax is inserted into the page.&#160; If you take a look at the source, all of the links now have onclick methods inserted into them.&#160; This gets away from the unobtrusive JavaScript theme the rest of MVC3 strived so hard to achieve.&#160; We will talk some more about unobtrusive JavaScript in the next section.

**Filtering the grid**

You probably noticed that our index.cshtml page has had a form in it that we will be using to filter the data in the grid.&#160; This form is simple by design, and will allow us to filter the form either by the Album Id, or the Album Name.&#160; Before we do anything with the form, lets add a new action method in our home controller that will let us filter the data:

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span>[HttpPost]</pre>
  
  <pre><span class="lnum">   2:  </span><span class="kwrd">public</span> ActionResult Index(<span class="kwrd">int</span>? albumId, <span class="kwrd">string</span> albumName)</pre>
  
  <pre class="alt"><span class="lnum">   3:  </span>{</pre>
  
  <pre><span class="lnum">   4:  </span>    var albums = AlbumRepo.GetAlbums();</pre>
  
  <pre class="alt"><span class="lnum">   5:  </span>&#160;</pre>
  
  <pre><span class="lnum">   6:  </span>    <span class="kwrd">if</span> (!<span class="kwrd">string</span>.IsNullOrEmpty(albumName))</pre>
  
  <pre class="alt"><span class="lnum">   7:  </span>        albums = albums.Where(a =&gt; a.Title.Contains(albumName)).ToList();</pre>
  
  <pre><span class="lnum">   8:  </span>&#160;</pre>
  
  <pre class="alt"><span class="lnum">   9:  </span>    <span class="kwrd">if</span> (albumId.HasValue)</pre>
  
  <pre><span class="lnum">  10:  </span>        albums = albums.Where(a =&gt; a.AlbumId == albumId).ToList();</pre>
  
  <pre class="alt"><span class="lnum">  11:  </span>&#160;</pre>
  
  <pre><span class="lnum">  12:  </span>    <span class="kwrd">return</span> View(albums);</pre>
  
  <pre class="alt"><span class="lnum">  13:  </span>}</pre>
</div>

<div class="csharpcode">
  &#160;
</div>

<div class="csharpcode">
  <font face="Verdana">This new action method has the same name as our other action method, but has the HttpPost attribute which tells MVC that this method will only respond to post requests.&#160; We accept two params into the method that will be used to filter the data. (Note: the code in this action method is not a good way to filter data, it is just demo code, so don’t reuse it in production).</font>
</div>

<div class="csharpcode">
  <font face="Verdana"></font>
</div>

<div class="csharpcode">
  <font face="Verdana">Now when we run the app, we can filter the data.&#160; However, our site is doing a complete post and refresh of the page every time we do a search.&#160; We will now add Ajax to the form.</font>
</div>

<div class="csharpcode">
  <font face="Verdana"></font>
</div>

<div class="csharpcode">
  <font face="Verdana">We will use the new unobtrusive Ajax support that is in MVC3 to accomplish this, and do it without having to write a single line of JavaScript.&#160; The Ajax.BeginForm has been around for awhile in MVC, but it has been changed to now use jQuery and not the Microsoft Ajax library.&#160; Also, there is no more JavaScript inserted into the screen on your behalf (unlike the WebGrid Ajax support shown above).&#160; Instead, MVC3 uses HTML5 data attributes to mark the form, and a special script that is used to wire the Ajax functionality up at runtime.</font>
</div>

<div class="csharpcode">
  <font face="Verdana"></font>
</div>

<div class="csharpcode">
  <font face="Verdana">In index.cshtml, change the Html.BeginForm line to the following:</font>
</div>

<div class="csharpcode">
  <font face="Verdana"></font>
</div>

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span>@using (Ajax.BeginForm(new AjaxOptions </pre>
  
  <pre><span class="lnum">   2:  </span>    { InsertionMode = InsertionMode.Replace, UpdateTargetId = "myGrid" }))</pre>
  
  <pre class="alt"><span class="lnum">   3:  </span>{</pre>
</div>

<div class="csharpcode">
  <font face="Verdana"></font>
</div>

<div class="csharpcode">
  <font face="Verdana">The AjaxOptions parameter tells our form how to handle the Ajax reqeuest.&#160; In this case, we are telling it we want the InsertionMode to replace the current contents in the myGrid div tag (much like we did with the paging and sorting on the grid).&#160; All that is left to do is add the JavaScript file that will make this magic happen.&#160; In the _Layout.cshtml file, add the following line directly below the line that includes jQuery:</font>
</div>

<div class="csharpcode">
  <font face="Verdana"></font>
</div>

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span><span class="kwrd">&lt;</span><span class="html">script</span> <span class="attr">type</span><span class="kwrd">="text/javascript"</span> <span class="attr">src</span><span class="kwrd">="../../Scripts/jquery.unobtrusive-ajax.min.js"</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   2:  </span><span class="kwrd">&lt;/</span><span class="html">script</span><span class="kwrd">&gt;</span>  </pre>
</div>

Make sure you don’t include any of the JavaScript files that start with Microsoft, as those are just included for backwards compatibility.&#160; In MVC3, you want to be using the scripts who’s filenames start with jQuery.

If you run the app now, you might notice a strange oddity when filtering the data.&#160; The entire response is copied into the myDiv tag, meaning that we get another copy of the form, along with the WebGrid table again.&#160; When using Ajax.BeginForm, the response that is sent is expected to only be the content that changes, not the entire page.&#160; We can easily fix this and return just the updated grid by changing the index action method that accepts the HttpPost to return a partial view of only the grid and not the entire index view (this is why we separated the grid out from the index view in the first place).

Update the index action method for the filtering to return a partial view by modifying the last line as such:

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span><span class="kwrd">return</span> PartialView(<span class="str">"_grid"</span>, albums);</pre>
  
  <p>
    <strong></strong></div> 
    
    <p>
      Now when you enter an a AlbumId or a partial Album name, the grid will filter and display properly.
    </p>
    
    <p>
      <strong>Summary</strong>
    </p>
    
    <p>
      In this post, I went over how the new WebGrid html helper can be used to quickly generate html tables with paging, sorting, and filtering.&#160; I also showed how to enable the Ajax features of the WebGrid, as well as using the new unobtrusive Ajax features in MVC3.&#160; If you are familiar with webforms development, but new to MVC, then you can use these techniques to easily add rich functionality to your MVC apps.&#160; These techniques did not require deep knowledge of html or JavaScript to accomplish our desired results.
    </p>
    
    <p>
      In my next artilce, I will show how to do the same thing using straight html and JavaScript (via jQuery).&#160; We won’t rely on the framework to generate any of our code, but will instead dig deep and get close to the metal.&#160; I think you will discover it is not as scary as it sounds.
    </p>
    
    <p>
      You can download the code via github (click the Download button to download all the code in a zip file):<a href="https://github.com/elylucas/MvcWebGrid-Part1">https://github.com/elylucas/MvcWebGrid-Part1</a>
    </p>

 [1]: http://www.elylucas.net/image.axd?picture=image.png
 [2]: http://www.elylucas.net/image.axd?picture=image_1.png
 [3]: http://www.elylucas.net/image.axd?picture=image_2.png