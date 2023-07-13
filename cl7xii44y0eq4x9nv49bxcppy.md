---
title: "Introduction to Backend as a Service (BaaS)"
seoDescription: "Backend-as-a-Service (BaaS) allows developers to focus on the front end of their applications and leverage backend services without building them."
datePublished: Sun Sep 11 2022 15:49:44 GMT+0000 (Coordinated Universal Time)
cuid: cl7xii44y0eq4x9nv49bxcppy
slug: introduction-to-backend-as-a-service
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1662911277512/YE5kc3nbT.png
tags: javascript, server, firebase, backend, backend-as-a-service

---

## What is BaaS?
Backend-as-a-Service (BaaS) allows developers to focus on the front end of their applications and leverage backend services without building or maintaining them. BaaS and serverless computing share some similarities, and many providers offer both, but the two models have several differences.


![Concept of IceBerg](https://cdn.hashnode.com/res/hashnode/image/upload/v1662909581413/H8h5yz5gQ.png align="left")

## What is included in BaaS?
BaaS providers offer a number of server-side capabilities. For instance:

- Database management
- Cloud storage (for user-generated content)
- User authentication
- Push notifications
- Remote updating
- Hosting
- Other platforms- or vendor-specific functionalities; for instance, Firebase offers Google search indexing

Think of developing an application without using a BaaS provider to direct a movie. A film director is responsible for overseeing or managing camera crews, lighting, set construction, wardrobe, actor casting, and the production schedule, in addition, to actually filming and directing the scenes that will appear in the movie. Now imagine if there was a service that took care of all the behind-the-scenes activities so that all the director had to do was direct and shoot the scene. That's the idea of BaaS: The vendor takes care of the 'lights' and the 'camera' (or, the server-side* functionalities) so that the director (the developer) can just focus on the 'action' – what the end user sees and experiences.

BaaS enables developers to focus on writing the front-end application code. Via APIs (which are a way for a program to make a request of another program) and SDKs (which are kits for building software) offered by the BaaS vendor, they are able to integrate all the backend functionality they need, without building the backend themselves. They also don't have to manage servers, virtual machines, or containers to keep the application running. As a result, they can build and launch mobile and web applications (including single-page applications) more quickly.

> Server-side refers to everything hosted on or that takes place on a server instead of on a client in the Internet client-server model.

## Firebase
Firebase is the most popular Backend as a Service developed by Google for creating mobile and web applications. It was originally an independent company founded in 2011. In 2014, Google acquired the platform and it is now their flagship offering for app development.

**Features**:
- **Cloud Firestore**: Build serverless, secure apps at a global scale. Store app data in the cloud, sync data across online and offline devices and retrieve it with expressive queries.

- **Hosting**: Deploy fast-loading, secure websites that are backed by a global CDN without all of the hassles.

- **Cloud Storage**: Store and serve user-generated content with ease as your app grows from prototype to production-ready.

- **Cloud Functions**: Write and run app logic server-side without needing to set up your own server.

- **Authentication**: Add an end-to-end identity solution to your app for easy user authentication, sign-in, and onboarding in just a few lines of code.

- **Cloud Messaging**: Get infrastructure to reliably send and receive push messages between your server and devices, across platforms at no cost.

- **Crashlytics**: Track, prioritize and fix stability issues that erode app quality, in real time.

- **Test Lab**: Spot errors before release by testing and validating your app on physical and virtual devices that simulate actual environments.

- **App Distribution**: Firebase App Distribution allows developers to send pre-release versions of their app to trusted testers from the console or using command line tools, as well as to manage testers in one place.

> **Firebase Extensions**:  Designed to increase productivity, Firebase Extensions provide extended functionality to your apps without the need to research, write, or debug code on your own. With Firebase Extensions, you provide the configuration parameters for your extension that are unique to your needs. You can also review the APIs enabled, resources created, and access granted to the extension.
