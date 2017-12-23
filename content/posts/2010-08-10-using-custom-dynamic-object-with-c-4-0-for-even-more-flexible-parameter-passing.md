---
title: 'Using custom dynamic object with C# 4.0 for even more flexible parameter passing'
author: ely
type: post
date: 2010-08-10T09:10:00+00:00
url: /post/using-custom-dynamic-object-with-c-4-0-for-even-more-flexible-parameter-passing/
dsq_thread_id:
  - 1588219980

---
A few days back, Rob Conery <a href="http://blog.wekeroad.com/2010/08/06/flexible-parameters-with-csharp" target="_blank">wrote a post</a> comparing C# and Ruby, and how using the dynamic keyword in C# lets you pass in arguments into a C# method in a way similar that it is done in Ruby land.&nbsp; I&rsquo;m not a Rubyist, but the same ideas are in JavaScript as well.&nbsp; I have worked on .Net applications that take in a dictionary parameter of named value pairs, and sometimes it has been a bit of a bear to maintain, but I agree with Rob that it can be very flexible, especially if you don&rsquo;t have your API nailed down quite yet and you are constantly changing your method signature.

Rob gives an example of using the dynamic keyword in C# to pass in arguments like so:

<pre style="border: 1px solid #cecece; padding: 5px; background-color: #fbfbfb; min-height: 40px; width: 650px; overflow: auto;"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  1: DoStuff(<span style="color: #0000ff">new</span> { Message = "<span style="color: #8b0000">Hello Monkey</span>" });
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  3: <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> DoStuff(dynamic args) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  4:     Console.WriteLine(args.Message);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  5: }</pre>


<p>
  <code>However, I quickly saw a problem with this method, and that is that all the arguments used inside of the DoStuff method are now required to be defined in the dynamic object being passed in.&nbsp; So if I had a DoStuff method like so:</code>
</p>


<pre style="border: 1px solid #cecece; padding: 5px; background-color: #fbfbfb; min-height: 40px; width: 650px; overflow: auto;"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  1: <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> DoStuff(dynamic args) { 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  2:     Console.WriteLine(args.Message); 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  3:     Console.WriteLine(args.Name);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  4: }</pre>


<p>
  <code>This code would crash at runtime unless you passed in the Name as well.&nbsp; In JavaScript, we can check to see if a value has been defined on an object like so:</code>
</p>


<pre style="border: 1px solid #cecece; padding: 5px; background-color: #fbfbfb; min-height: 40px; width: 650px; overflow: auto;"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  1: <span style="color: #0000ff">if</span>(obj.Name){
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  2:     <span style="color: #008000">//do something with obj.Name</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  3: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  4: <span style="color: #0000ff">else</span> {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  5:     <span style="color: #008000">//Ignore because obj.Name is not defined.</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  6: }</pre>


<p>
  <code>It would be really helpful if we could do the same thing in C#, so I set out on a mission to try to see if this was possible.&nbsp; After much digging on the web (and many thanks to this &lt;a href="http://www.codeproject.com/KB/cs/dynamicincsharp.aspx" target="_blank">excellent article&lt;/a>from Anoop Madhusudanan) I found a method that gets close, but not quite there.&nbsp; Instead of passing in the argument to the if statement, we pass in a Has(Member Name) property.&nbsp; For instance, if we wanted to check to see if a Name property was on our dynamic object, we call if(obj.HasName), and if we were looking for age if(obj.HasAge), etc..</code>
</p>


<p>
  <code>We accomplish this by creating a new class called DynamicParam that derives from DynamicObject, and overload TryGetMember, TrySetMember, and TryInvokeMember.&nbsp; A dictionary member variable keeps track of all the dynamic members we have defined, as well as stores the values for these members.&nbsp; Elsewhere, I have read that internally, this is how the ExpandoObject works.</code>
</p>


<p>
  I&rsquo;m not saying this is necessarily a good practice, but it is interesting use of using the dynamic features of C#, and you might find it useful in your application.&nbsp; The full source for the sample console app is below, and you can find it on gist here <a title="http://gist.github.com/517890" href="http://gist.github.com/517890">http://gist.github.com/517890</a>.
</p>


<pre style="border: 1px solid #cecece; padding: 5px; background-color: #fbfbfb; min-height: 40px; width: 650px; overflow: auto;"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  4: <span style="color: #0000ff">using</span> System.Text;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  5: <span style="color: #0000ff">using</span> System.Dynamic;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  6: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  7: <span style="color: #0000ff">namespace</span> ConsoleApplication2
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  8: {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;">  9:     <span style="color: #0000ff">class</span> Program
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 10:     {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 11:         <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> Main(<span style="color: #0000ff">string</span>[] args)
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 12:         {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 13:             dynamic param = <span style="color: #0000ff">new</span> DynamicParam();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 14:             param.Message = "<span style="color: #8b0000">butt</span>";
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 15:             param.SayHi = <span style="color: #0000ff">new</span> Action&lt;<span style="color: #0000ff">string</span>&gt; (s =&gt; Console.WriteLine(s));
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 16:             
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 17:             DoStuff(param);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 18:             Console.ReadLine();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 19:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 20: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 21:         <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> DoStuff(dynamic args)
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 22:         {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 23:             Console.WriteLine(args.Message);  <span style="color: #008000">//This param is required since we don't check if exists or not</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 24:             <span style="color: #0000ff">if</span>(args.HasTitle)  <span style="color: #008000">//Check to see if args Has a Title param, since we don't this statement doesn't execute</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 25:                 Console.WriteLine(args.Title);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 26:             <span style="color: #0000ff">if</span>(args.HasSayHi) <span style="color: #008000">//Check to see if args Has a SayHi method</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 27:                 args.SayHi("<span style="color: #8b0000">h22i</span>");
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 28:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 29:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 30: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 31:     <span style="color: #0000ff">class</span> DynamicParam : DynamicObject
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 32:     {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 33:         <span style="color: #0000ff">private</span> Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">object</span>&gt; _members = <span style="color: #0000ff">new</span> Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">object</span>&gt;();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 34: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 35:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">bool</span> TryGetMember(GetMemberBinder binder, <span style="color: #0000ff">out</span> <span style="color: #0000ff">object</span> result)
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 36:         {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 37:             <span style="color: #0000ff">if</span> (binder.Name.StartsWith("<span style="color: #8b0000">Has</span>"))
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 38:             {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 39:                 var name = binder.Name.Substring(3);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 40:                 <span style="color: #0000ff">if</span> (_members.ContainsKey(name))
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 41:                     result = <span style="color: #0000ff">true</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 42:                 <span style="color: #0000ff">else</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 43:                     result = <span style="color: #0000ff">false</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 44:                 <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 45:             }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 46:             <span style="color: #0000ff">else</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 47:             {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 48:                 <span style="color: #0000ff">if</span> (_members.ContainsKey(binder.Name))
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 49:                 {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 50:                     result = _members[binder.Name];
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 51:                     <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 52:                 }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 53:                 <span style="color: #0000ff">else</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 54:                 {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 55:                     result = <span style="color: #0000ff">false</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 56:                     <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 57:                 }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 58:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 59:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 60: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 61:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">bool</span> TrySetMember(SetMemberBinder binder, <span style="color: #0000ff">object</span> <span style="color: #0000ff">value</span>)
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 62:         {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 63:             <span style="color: #0000ff">if</span> (!_members.ContainsKey(binder.Name))
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 64:                 _members.Add(binder.Name, <span style="color: #0000ff">value</span>);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 65:             <span style="color: #0000ff">else</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 66:                 _members[binder.Name] = <span style="color: #0000ff">value</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 67:             <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;  
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 68:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 69:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 70: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px;"> 71: </pre>