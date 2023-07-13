---
title: "Why Svelte is becoming the next React and I love it"
seoDescription: "Nowadays, I see Svelte becoming more and more popular. I thought to try it and I quickly realized that it's awesome, but then, I wonder that..."
datePublished: Sat Dec 31 2022 10:39:49 GMT+0000 (Coordinated Universal Time)
cuid: clcbtb44g000108mr0snf867b
slug: why-svelte-is-becoming-the-next-react-and-i-love-it
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1672483009952/1011e7ff-c252-40d3-a3ab-218dc66a86ac.png
tags: javascript, web-development, svelte, sveltekit, svelte-vs-react

---

Nowadays, I see Svelte becoming more and more popular. I thought to try it and I quickly realized that it's awesome, but then, I wonder that **"is Svelte gonna replace React?".** We will find that out in this article, so let's get started.

# Why Svelte is the Next Big Thing in Web Development

Before the rise of React and Vue, creating large and maintainable web applications was quite a challenge. To name but a few reasons, JavaScript files were difficult to organize and deploy to production, and maintaining UI was also very hard.

This has undoubtedly opened the doors to new developments. The best proof is that, **while there were only 4 JS frameworks in 2016, this number increased a lot by 2022**. While new frameworks are coming to create a full-fledged web application, there are also frameworks coming to make simple and fast JAM Stack web applications

And while React and Vue have turned frontend developers into productivity stars, a whole new perspective was proposed by **Rich Harris, the creator of Svelte**.

### What is Svelte?

Like React and Vue, Svelte is a JavaScript framework/library. In general, it’s a set of components, tools, and rules for creating the structure of websites and applications with the use of JavaScript. Svelte, however, has developed **its own, original way to let developers build what they need and want**. Due to this fresh approach, Svelte is sometimes described as a **compiler.**

React and Vue uses a virtual DOM in the browser, which leads to certain performance shortcomings. The DOM (Document Object Model) is an interface for web pages. It is an API that allows programs to read and manipulate the page content, structure, and styles. You can compare it to temporary memory storage for interface changes.

**Svelte, however, doesn’t use a virtual DOM,** instead compiling code into tiny, pure Vanilla JS. As a result, the code works much faster from the beginning, making a potential app product much lighter and more user-friendly. This is the first and most notable difference that Svelte has presented to the world of web development.

![svelte_compiler](https://d33wubrfki0l68.cloudfront.net/ddef76997644095f7a172720e9efe3adb7404cfa/d0a51/assets/images/svelte_easy.webp align="left")

## **FAQs**

1. **Which is faster Svelte or React?**  
    Svelte is remarkably faster than traditional JavaScript frameworks. Unlike React, which functions as a conventional JavaScript library, Svelte works as a compiler. Svelte achieves faster compilation speeds due to its smaller size, and its compiler does not rely on VDOM diffing.
    
2. **Is Svelte getting more popular?**  
    Svelte usage has increased to 20% in 2022, from 8% in 2020. Awareness of Svelte has risen to 94%, from 75%. Svelte has ranked at the top, as the most satisfactory JavaScript framework, in the past three years.
    
3. **Can I use Svelte with React?**  
    Svelte components can be used with React and Vue components with this adapter module. [https://github.com/pngwn/svelte-adapter](https://github.com/pngwn/svelte-adapter) is the link to the GitHub repository of the code. The only limitation of this adapter is that it currently doesn’t support passing children/slots to Svelte components.
    

## Learning Curve and Ease of Use

If you’re deciding between learning React or Svelte, the ease of use of these two frameworks is an important factor to consider.

While more widely used, learning React can be daunting when you have to learn things like JSX and CSS-in-JS to build even the most basic applications.

Compared to React, **Svelte is simpler to understand and get started with**, because the major portion of Svelte is plain JavaScript, HTML, and CSS. Svelte also sticks closely to JavaScript’s classic web development models, and introduces only a few extensions to HTML, making it much easier to learn.

To demonstrate, here’s an example of a Svelte component. All we have here is basic HTML, CSS, and JavaScript:

```xml
<style>
  h1 {
    color: green;
  }
</style>

<script>
  let name = 'Nouman';
</script>

<h1>Hello, {name}!</h1>
```

As you can see, this is very easy to understand even if you know the basics of HTML, CSS, and JavaScript.

## Bundle Size and Performance

**Svelte produces smaller bundles than Reactjs**. Svelte's bundle size is 1.6KB gzipped version while Reactjs bundled size is 42.2KB, this is due to its compile-time approach. Also, Reactjs tends to generate more code than Svelte as it needs to maintain the virtual DOM.

According to user tests, Svelte is approximately **30%** faster than the rest of the frameworks in a showdown, Svelte vs. React vs. Vue.