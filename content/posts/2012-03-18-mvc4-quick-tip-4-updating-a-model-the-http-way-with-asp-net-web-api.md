---
title: 'MVC4 Quick Tip #4â€“Updating a model the HTTP way with ASP.Net Web API'
author: ely
type: post
date: 2012-03-18T15:50:04+00:00
url: /post/mvc4-quick-tip-4-updating-a-model-the-http-way-with-asp-net-web-api/
dsq_thread_id:
  - 998583436

---
**_Disclaimer: This code is based on MVC 4 Beta 1, and might change in future releases._**

The [www.asp.net/web-api][1] website has a few good videos and decent examples, however, I didnâ€™t notice a concrete way of doing an update using ASP.Net Web API.Â So I thought I would quickly document how I implemented it for anyone else looking how to do so, and will welcome any comments or suggestions if I am going about this the wrong way.

ASP.Net Web API embraces HTTP and all that comes along with it, and that is one of the main differences between APIControllers and the regular controllers found in MVC.Â In MVC, you would normally do all your CUD (Create, Update, Delete) through POST actions to the controller.Â In ASP.Net Web API, the recommended way is to use the POST verb for creating objects, PUT for updating objects, and DELETE (you guessed it) for deleting objects.Â So instead of having a controller decorated with HTTP Verbs attributes, APIControllers uses a convention in the way you name your methods.Â If the method begins with Get, it must be a get request, Post, a create request,and so on.

Below is a quick example of doing a PUT request to an APIController.Â Your update logic will undoubtedlyÂ Â vary, but it is the outline of the method that is important:

<pre class="brush: csharp">HttpResponseMessage&lt;Person> PutPerson(Person person)
         {
             _repo.Attach(person);
                 _unitOfWork.Commit();
                var response = new HttpResponseMessage&lt;Person>(person, HttpStatusCode.OK);
                response.Headers.Location = new Uri(Request.RequestUri, "/api/person/" + person.Id);
                return response;
         } 
</pre>

The jQuery AJAX call for this method is as follows:

<pre class="brush: js">$.ajax("/api/person", {
                 data: person,
                 type: "PUT",
                 contentType: "application/json",
                 statusCode: {
                     200: function (data) {
                         //success
                     },
                     500: function (data) {
                        //error
                    }
                }
            });
</pre>

 [1]: http://www.asp.net/web-api