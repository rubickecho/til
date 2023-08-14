# How to check website pre-rendering

Check That Pre-rendering Is Happening
You can check that pre-rendering is happening by taking the following steps:

Disable JavaScript in your browser. (Here’s how in Chrome).
Try accessing this page (the final result of this tutorial).
You should see that your app is rendered without JavaScript. That’s because Next.js has pre-rendered the app into static HTML, allowing you to see the app UI without running JavaScript.

Note: You can also try the above steps on localhost, but CSS won’t be loaded if you disable JavaScript.

If your app is a plain React.js app (without Next.js), there’s no pre-rendering, so you won’t be able to see the app if you disable JavaScript. For example:

Enable JavaScript in your browser and check out this page. This is a plain React.js app built with Create React App.
Now, disable JavaScript and access the same page again.
You won’t see the app anymore — instead, it’ll say “You need to enable JavaScript to run this app.” This is because the app is not pre-rendered into static HTML.

[Nextjs pre-rendering](https://nextjs.org/learn/basics/data-fetching/pre-rendering)