---
title: Enabling a submit button when a textbox has value in jQuery
author: ely
type: post
date: 2011-03-25T10:44:00+00:00
url: /post/enabling-a-submit-button-when-a-textbox-has-value-in-jquery/
dsq_thread_id:
  - 1629496433

---
A common pattern on the web is to only enable a submit button on a form if a value is filled out in a textbox.&#160; The other day I was asked by a colleague how to do this in jQuery, and while I gave the basic pseudo code, I was curious on the exact method, so I put this together real quick and decided to share it with the world.

The premise is simple: We want to check the textbox after every time a change is made to it to see if there is any value in it.&#160; If there is, we enable the button (by removing the disabled attribute), and if there isn’t, then we make sure the button stays disabled.&#160; We initially load the form with the submit button disabled.

The trick to accomplishing this was in coming up with the right event to fire off the textbox.&#160; At first I thought the change event would work, but that only fires when the textbox loses focus (not what we want here).&#160; Then I tried keypress, but it seems that event gets fired before the textbox is updated, which gives us the previous value, not the current one we need.&#160; Finally, I tried the keyup event, and that seems to work just fine.

Below is the code to accomplish this.&#160; It might not be the best way to do this, but it works in this case.&#160; Also make sure you always validate on the server side as well.

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span><span class="kwrd">&lt;!</span><span class="html">DOCTYPE</span> <span class="attr">html</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   2:  </span><span class="kwrd">&lt;</span><span class="html">html</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">   3:  </span><span class="kwrd">&lt;</span><span class="html">head</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   4:  </span>    <span class="kwrd">&lt;</span><span class="html">title</span><span class="kwrd">&gt;&lt;/</span><span class="html">title</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">   5:  </span><span class="kwrd">&lt;/</span><span class="html">head</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   6:  </span>&#160;</pre>
  
  <pre class="alt"><span class="lnum">   7:  </span><span class="kwrd">&lt;</span><span class="html">script</span> <span class="attr">src</span><span class="kwrd">="Scripts/jquery-1.4.4.min.js"</span> <span class="attr">type</span><span class="kwrd">="text/javascript"</span><span class="kwrd">&gt;&lt;/</span><span class="html">script</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">   8:  </span>&#160;</pre>
  
  <pre class="alt"><span class="lnum">   9:  </span>&lt;script type=<span class="str">"text/javascript"</span>&gt;       </pre>
  
  <pre><span class="lnum">  10:  </span>    $(document).ready(<span class="kwrd">function</span> () {     </pre>
  
  <pre class="alt"><span class="lnum">  11:  </span>        $(<span class="str">"#name"</span>).keyup(<span class="kwrd">function</span> (data) {              </pre>
  
  <pre><span class="lnum">  12:  </span>            <span class="kwrd">if</span> ($(<span class="kwrd">this</span>).val() != <span class="str">""</span>) {  </pre>
  
  <pre class="alt"><span class="lnum">  13:  </span>                $(<span class="str">"#enter"</span>).removeAttr(<span class="str">"disabled"</span>); </pre>
  
  <pre><span class="lnum">  14:  </span>            } </pre>
  
  <pre class="alt"><span class="lnum">  15:  </span>            <span class="kwrd">else</span> {  </pre>
  
  <pre><span class="lnum">  16:  </span>                $(<span class="str">"#enter"</span>).attr(<span class="str">"disabled"</span>, <span class="str">"disabled"</span>); </pre>
  
  <pre class="alt"><span class="lnum">  17:  </span>            }  </pre>
  
  <pre><span class="lnum">  18:  </span>        });</pre>
  
  <pre class="alt"><span class="lnum">  19:  </span>    });  </pre>
  
  <pre><span class="lnum">  20:  </span><span class="kwrd">&lt;/</span><span class="html">script</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  21:  </span>&#160;</pre>
  
  <pre><span class="lnum">  22:  </span><span class="kwrd">&lt;</span><span class="html">body</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  23:  </span>    <span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span>        </pre>
  
  <pre><span class="lnum">  24:  </span>        <span class="kwrd">&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="name"</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  25:  </span>            Name<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span></pre>
  
  <pre><span class="lnum">  26:  </span>        <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="text"</span> <span class="attr">id</span><span class="kwrd">="name"</span> <span class="attr">name</span><span class="kwrd">="name"</span> <span class="kwrd">/&gt;</span>        </pre>
  
  <pre class="alt"><span class="lnum">  27:  </span>        <span class="kwrd">&lt;</span><span class="html">br</span> <span class="kwrd">/&gt;&lt;</span><span class="html">br</span> <span class="kwrd">/&gt;</span>        </pre>
  
  <pre><span class="lnum">  28:  </span>        <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="submit"</span> <span class="attr">disabled</span><span class="kwrd">="disabled"</span> <span class="attr">value</span><span class="kwrd">="Enter"</span> <span class="attr">id</span><span class="kwrd">="enter"</span> <span class="kwrd">/&gt;</span>       </pre>
  
  <pre class="alt"><span class="lnum">  29:  </span>    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>    </pre>
  
  <pre><span class="lnum">  30:  </span><span class="kwrd">&lt;/</span><span class="html">body</span><span class="kwrd">&gt;</span></pre>
  
  <pre class="alt"><span class="lnum">  31:  </span><span class="kwrd">&lt;/</span><span class="html">html</span><span class="kwrd">&gt;</span></pre>
</div>