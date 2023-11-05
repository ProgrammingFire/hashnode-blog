---
title: "Do we really need Backend now? Can we build applications without backend with Next.js 14?"
seoDescription: "In the rapidly evolving landscape of web development, the role of the backend has been a critical component for many years. However, with the advent of..."
datePublished: Sun Nov 05 2023 07:10:46 GMT+0000 (Coordinated Universal Time)
cuid: clol4whor000008l37dto5j3m
slug: do-we-really-need-backend-now-can-we-build-applications-without-backend-with-nextjs-14
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1699168176072/a86f9b83-17fe-44ae-b64c-054e2654033b.png
tags: web-development, backend, typescript, graphql, nextjs

---

In the rapidly evolving landscape of web development, the role of the backend has been a critical component for many years. However, with the advent of modern frameworks like Next.js 14, the question arises: "Do we really need a Backend now? Can we build applications without backend with Next.js 14?"

## Introduction

So we know that for a lot of years, I have been a very enthusiastic full-stack developer. Back in the day, I used to enjoy writing backend more than frontend. This may be something I still like right now! Yeah, it's true, I enjoy writing backend more than frontend. But with the evolving Next.js Server Components and Server Actions. Do we still need a backend? Well, in my opinion, **We don't.**

## Why we don't need a backend?

When I say that we don't need a backend, I mean that a lot of applications that we are building right now (SaaS, AI Tools, etc.) don't really need a separate backend. That said, I do think that a lot of large-scale applications still need a custom backend, **Why?**

## Why do we need a backend?

So now let's think more and talk about why we need a backend. For me, it's simple that syncing a lot of data with the database and other external APIs just sucks, that's why I like to have a custom backend for any large-scale applications. Let's take it a step further by explaining to you with code examples.

## How do you really build an application without a backend with Next.js 14?

So first, How do you really build an application without a backend with Next.js 14? Simply, you use a lot of third-party APIs to do so, like Clerk or Auth.js for Authentication, Stripe for payments, etc., you integrate a database into it using Prisma, and you use tRPC to build Type-safe APIs on the fly.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699164274259/c9a030e3-7194-4c38-9c65-d0ffefefb870.png align="center")

This seems like a good architecture but this comes with a lot of problems, one problem that is like you have to always solve is the external APIs you are using. You have to sync them with your database, like, when a user registers in your app and you are using Clerk, you then have to store the same user object in your database if you want to do something else with that user, it's also the case when you want to add some more properties to the user like you want to add a `bio` property on the user, you will still have to add the user in the database. This just doesn't stay limited to the user object in Clerk, but with other APIs like if you are using Sanity for content management, you still wanna sync the content with the database. To be honest, **this sucks for any large-scale application.**

Another problem is **scalability** this is the biggest problem because you're going to deploy your Next.js application on something like Vercel. Which we can say is amazing but not that much, like think about it, you are deploying a whole server to a platform that is built for deploying frontend.

But then there is one thing that I absolutely love when building these types of applications and that is the flow of data fetching, yeah, the flow is amazing. If you wanna fetch something from the database, no problem, here you go:

```typescript
import { db } from "@/lib/db"

async function Page() {
    const posts = await db.posts.findMany({}) // it's type safe 🚀
    
    return (
        <ul>
            {posts.map(post => (
                <li key={post.id}>{post.title}</li>
            )}
        </ul>
    )
}

export default Page
```

If you wanna create a tRPC query to query something from the server and then use it in client components, no problem, here you go:

```typescript
// @/server/index.ts

import { publicProcedure, router } from "./trpc";
import { db } from "@/lib/database";
import { TRPCError } from "@trpc/server";

export const appRouter = router({
  getAllPosts: publicProcedure.query(async () => {
    const posts = await db.posts.findMany({
      orderBy: { createdAt: "desc" },
    });

    if (!posts) throw new TRPCError({ 
        code: "NOT_FOUND", ,
        message: "There are no posts"
    });

    return { posts };
  }),
});

export type AppRouter = typeof appRouter;
```

Now use this query to fetch this data on a client component:

```typescript
"use client";

import Link from "next/link";
import { trpc } from "./_trpc/client";
import Loading from "@/components/Loading";

export default function Home() {
  const { data, isLoading } = trpc.getPosts.useQuery(); // it's also type-safe 🚀

  if (isLoading) return <Loading />

  return (
    <main className="py-12 flex flex-col items-center">
        {posts.map(post => (
            <Link href={`/posts/${post.slug}`}>{post.title}</Link>
        ))}
    </main>
  );
}
```

I absolutely love this flow, for the simplest thing you don't have to create another endpoint on the backend and then fetch it on the client. You can either get it directly using Server Components or write a tRPC query to get it on a client component. Now let's talk about how you build an application with a backend and what I personally like.

## How do you build an application with a backend?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699167034706/0ff34e6e-98e1-432a-9be4-4a2c0cab6603.png align="center")

Now here's where things get interesting for me, I absolutely love a few backend technologies specifically **Nest.js, Express, Prisma, and obviously GraphQL.** I have written a lot of articles related to these technologies, but, right now let's talk about some major stuff. The first thing that I like about building my own backend is, **that it's mine**, I have full control over it, and I can do anything I want in it, I am not limited to any point. I can build my own Authentication system, I can build my own API endpoints or queries/mutations. I can use anything I want. I am going to link some of my articles here so you can learn more about the backend:

* [https://programmingfire.com/infrastructure-as-code-iac-with-terraform-a-complete-guide](https://programmingfire.com/infrastructure-as-code-iac-with-terraform-a-complete-guide)
    
* [https://programmingfire.com/why-devops-is-essential-for-modern-app-development](https://programmingfire.com/why-devops-is-essential-for-modern-app-development)
    
* [https://programmingfire.com/why-docker-is-just-pure-magic-how-to-work-with-it](https://programmingfire.com/why-docker-is-just-pure-magic-how-to-work-with-it)
    
* [https://programmingfire.com/what-is-the-best-way-to-implement-microservices-and-is-it-really-worth-it](https://programmingfire.com/what-is-the-best-way-to-implement-microservices-and-is-it-really-worth-it)
    
* [https://programmingfire.com/the-top-6-amazing-devops-and-cloud-tools-to-try-once](https://programmingfire.com/the-top-6-amazing-devops-and-cloud-tools-to-try-once)
    
* [https://programmingfire.com/how-to-deploy-a-fullstack-app-to-the-moon](https://programmingfire.com/how-to-deploy-a-fullstack-app-to-the-moon)
    
* [https://programmingfire.com/why-serverless-doesnt-make-any-sense](https://programmingfire.com/why-serverless-doesnt-make-any-sense)
    
* [https://programmingfire.com/deploy-nodejs-application-to-kubernetes](https://programmingfire.com/deploy-nodejs-application-to-kubernetes)
    
* [https://programmingfire.com/create-docker-image-for-nodejs](https://programmingfire.com/create-docker-image-for-nodejs)
    
* [https://programmingfire.com/graphql-in-nodejs-with-apollo-typegraphql](https://programmingfire.com/graphql-in-nodejs-with-apollo-typegraphql)
    

and there are much more articles that I have written on backend technologies, you can check them out on this blog...