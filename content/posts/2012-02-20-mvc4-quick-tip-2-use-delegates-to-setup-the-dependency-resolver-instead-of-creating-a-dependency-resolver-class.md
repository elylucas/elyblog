---
title: 'MVC4 Quick Tip #2–Use Delegates to setup the Dependency Resolver instead of creating a Dependency Resolver class'
author: ely
type: post
date: 2012-02-20T15:29:11+00:00
url: /post/mvc4-quick-tip-2-use-delegates-to-setup-the-dependency-resolver-instead-of-creating-a-dependency-resolver-class/
dsq_thread_id:
  - 1116504221

---
I’m a big fan of cutting out unneeded or unnecessary code, so here is a tip that isn’t new in MVC4, but I just discovered it and thought it was really cool and worth sharing.&#160; When using IOC in MVC, you setup a class that implements IDependencyResolver, which has methods to return services for a given type.&#160; While these classes where simple, it always seemed like a bit of ceremony was required whenever starting up a new MVC app.

The following example shows what a DependencyResolver class looks like using my favorite IOC framework, Ninject:

<pre class="brush: csharp">public class NinjectDependencyResolver : IDependencyResolver
         {
             private readonly IKernel _kernel;
      
             public NinjectDependencyResolver(IKernel kernel)
             {
                 _kernel = kernel;
             }
      
            public object GetService(Type serviceType)
            {
                return _kernel.TryGet(serviceType);
            }
     
            public IEnumerable&lt;object> GetServices(Type serviceType)
            {
                return _kernel.GetAll(serviceType);
            }
        }
</pre>

Nothing terribly complicated, but a lot of fluff.

I saw in a demo in last week’s C4MVC talk, that the DependencyResolver that was setup to create the APIController classes had some overrides I never saw before, and low and behold, the standard MVC DependencyResolver also has these overrides:

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="ioc" border="0" alt="ioc" src="http://www.elylucas.net/image.axd?picture=ioc_thumb.png" width="620" height="114" />][1]

So instead of providing the SetResolver method with a DependencyResolver class, you can just provide it two delegate methods that would normally be in the DependencyResolver (GetService, and GetServices):

<pre class="brush: csharp">DependencyResolver.SetResolver(
                     x => kernel.TryGet(x),
                     x => kernel.GetAll(x));

</pre>

POW!&#160; Cut that 20+ line class down to a method that takes two parameters.&#160; BAM!

 [1]: http://www.elylucas.net/image.axd?picture=ioc.png