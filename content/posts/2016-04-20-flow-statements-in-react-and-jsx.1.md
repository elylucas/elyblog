---
title: Flow statements in React and JSX
author: ely
type: post
date: 2016-04-20T16:03:40+00:00
url: /post/flow-statements-in-react-and-jsx/
dsq_thread_id:
  - 4868415035

---
Attempting to use an if statement in JSX will quickly be met with lots of resistance. For instance, trying to conditionally render some content like so:
```jsx
<div>
  {
    if(true) {
      <div>some conditional text</div>;
    }
  }
</div>
```

This will generate errors by the JSX transpiler. As discussed in the React docs, this is because JSX transpiles into a series of JavaScript function calls and object creation, where you can&#8217;t legally use an if statement. So how do you go about hiding and showing content based on some type of condition?

There are a couple of examples on how you might achieve this in the docs. One common and well excepted option is to use a JavaScript expression:

```jsx
{ shouldShow && <div>conditional content</div> }
```

This method takes advantage of short circuting in JavaScript. If the conditional is false, then the JSX on the right (which just compiles down to a React.createElemeent call) never gets executed.

At first this took awhile for me to get use to seeing in React codebases. But once I was use to it, it became second nature.

To take this example another step further, you can also use ternary expressions inside of JSX like so:

```jsx
{ showShow ? <div>Content A</div> : <div>Content B</div> }
```

Ternarys become ugly quickly inside of JSX, and I wouldn&#8217;t recommend using them for less trivial examples.

However, we sometimes need if/else conditiona, so what are we to do?

We could define variables and set them conditionally outside of our return statement where conditional statements are perfectly legal. While this approach works well, I feel that it scatters the view code in multiple places, and gets hard to follow.

The React docs also recommend using an inline iife to hide the conditional inside of a function like so:

```jsx
<div>
  {
    (()=>{
      if(false) {

		<div>{test('truthy')}truthy</div>

      } else{

		<div>{test('falsey')}falsey</div>

      }
    })()
  }
</div>
```

The iife method works well in most cases, but at the same time it feels hacky and adds some script to our html.

I have also seen approaches of wrapping the component in another conditional component and conditionally showing the inner components like so:

```jsx
<Maybe show={shouldShow}>
  <div>Content</div>
</Maybe>
```

The Maybe component would then check the show prop and conditionally render it. While I like this syntax, the drawback to this method is that the internal component will always be executed, no matter the value of show. Thie is because JSX will turn the child components into React.createElement calls, and then execute those calls, and pass the result as children to the Maybe component. This isn&#8217;t a big deal for just showing an hiding text content, but could have unintended performace consequences if you were passing in other React components.

There is a promising project that lets you write you statements like the wrapped component and takes care of not executing the children if the statement is falsey. Its called <a href="https://github.com/AlexGilleran/jsx-control-statements" target="_blank">JSX-Control-Statements</a>, and they do the magic by being a babel plugin. Check it out if none of the above solutions seem to work for you.