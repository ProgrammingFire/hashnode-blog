# Explore the New Era of Static Sites Generation: Astro

Do you know what is the hottest stuff going on in the Web Dev Community, it's JAM Stack, Static Sites, and stuff like that. People are more into creating their portfolios, blogs, podcasts, etc. where you don't need a lot of business-level technologies like Angular, .NET, and MySQL. This is where most people use more content-driven technologies like a Static Site Generator, a Content Management System, and more...

## What is Astro in general?

Astro is an **all-in-one** **web framework** for building **fast,** **content-focused** websites.

### **Key Features**

*   **Component Islands:** A new web architecture for building faster websites.
    
*   **Server-first API design:** Move expensive hydration off of your users’ devices.
    
*   **Zero JS, by default:** No JavaScript runtime overhead to slow you down.
    
*   **Edge-ready:** Deploy anywhere, even a global edge runtime like Deno or Cloudflare.
    
*   **Customizable:** Tailwind, MDX, and 100+ other integrations to choose from.
    
*   **UI-agnostic:** Supports React, Preact, Svelte, Vue, Solid, Lit and more.
    

In general, It supports many UI JavaScript Frameworks altogether, like one of your components can be React and another can be Vue and the other one can be Svelte. This makes Astro very useful because of this feature. Another extremely powerful feature of Astro is that **it ships no Javascript** on the page load.

## Get started with Astro

To create a new Astro project you need to run this `npm` command:

```bash
# create a new project with npm
npm create astro@latest
```

You can run `create-astro` anywhere on your machine, so there’s no need to create a new empty directory for your project before you begin. If you don’t have an empty directory yet for your new project, the wizard will help create one for you automatically.

If all goes well, you should see a “Ready for liftoff!” message followed by some recommended “Next steps”. `cd` into your new project directory to begin using Astro.

If you skipped the `npm install` step during the `create-astro` wizard, then be sure to install your dependencies before continuing.

# Add your first page to your site

# **Pages**

**Pages** are files that live in the `src/pages/` subdirectory of your Astro project. They are responsible for handling routing, data loading, and overall page layout for every page in your website.

## **Supported page files**

Astro supports the following file types in the `src/pages/` directory:

*   `.astro`
    
*   `.md`
    
*   `.mdx` (with the [MDX Integration installed](https://docs.astro.build/en/guides/integrations-guide/mdx/#installation))
    
*   `.html`
    
*   \[`.js`/`.ts`\] (as [endpoints](https://docs.astro.build/en/core-concepts/endpoints/))
    

To create a new page you have to create a new file in the `src/pages/` directory:

```xml
<html lang="en">
  <head>
    <title>My Homepage</title>
  </head>
  <body>
    <h1>Welcome to my website!</h1>
  </body>
</html>
```

To avoid repeating the same HTML elements on every page, you can move common `<head>` and `<body>` elements into your layout components. You can use as many or as few layout components as you’d like.

```xml
---
import MySiteLayout from '../layouts/MySiteLayout.astro';
---
<MySiteLayout>
  <p>My page content, wrapped in a layout!</p>
</MySiteLayout>
```

You can also use markdown for your pages like this:

```markdown
---
layout: '../layouts/MySiteLayout.astro'
title: 'My Markdown page'
---
# Title

This is my page, written in **Markdown.**
```

Congratulations! You have created your first Astro website. If you want to learn more about let me know that in the comments. If you like this article be considerate about sponsoring me, it really gives motivations. You can also reach me on Twitter ([@programmingfire](https://twitter.com/ProgrammingFire))