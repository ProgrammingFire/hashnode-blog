---
title: "Get Started With React TypeScript"
seoDescription: "Why Use Typescript In React?
Typescript brings some awesome features that extend JavaScript in powerful ways, including the ability to define..."
datePublished: Wed Dec 22 2021 13:07:07 GMT+0000 (Coordinated Universal Time)
cuid: ckxhjvyq001oxess1bfzx7dya
slug: react-typescript-getting-started
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650273948840/PiORWM5tE.png
tags: javascript, reactjs, typescript, frontend-development, create-react-app

---

## Why Use Typescript In React?

Typescript brings some awesome features that extend JavaScript in powerful ways, including the ability to define the structure of an object in a variety of ways.

## Create React App Using [create-react-app](https://create-react-app.dev/)

When you normally create a react app you run :

```bash
# with npm
npx create-react-app my-app
# with yarn
yarn create react-app my-app
```

But, if you want to create a react app with **Typescript** run :

```bash
# with npm
npx create-react-app my-app --template typescript
# with yarn
yarn create react-app my-app --template typescript
```

## Defining Props

### Create A New Component

Components in React Typescript are different from React Javascript. To define a component in React Typescript create a file with the **.tsx** extension let's just create **Component/User.tsx** with the following code

```tsx
import React from "react";
export const User: React.FC = ({}) => {
  return <div>User Component</div>;
};
```

Now we have a user component, now go to **App.tsx** and render the User component with the following code

```tsx
import React from "react";
import { User } from "./Component/User";
export const App: React.FC = ({}) => {
  return (
    <div>
      App Component
      <User />
    </div>
  );
};
export default App;
```

### Run The App On Localhost

Now we can run our app on localhost to see if it's working

```bash
# with npm
npm run start
# with yarn
yarn start
```

On your browser open **https://localhost:3000/** and you are going to see this

![image](https://cdn.hashnode.com/res/hashnode/image/upload/v1640178078970/S-lLxuIvg.png)

To take props in the User component add your props in <> like this

```tsx
export const User: React.FC<{firstName: string, lastName:string, userId:number}>
```

But, a better way to do this is to create an [Interface](https://www.typescriptlang.org/docs/handbook/2/objects.html) like this

```tsx
interface Props = {
  firstName: string,
  lastName: string,
  userId: number
}
export const User:React.FC<Props>
```

Now if you want to use these props pass them in function like this

```tsx
interface Props = {
  firstName: string,
  lastName: string,
  userId: number
}
export const User:React.FC<Props> = ({firstName, lastName, userId}) => {
  return (
    <h1>User Info</h1>
    <div>First Name: {firstName}</div>
    <div>Last Name: {lastName}</div>
    <div>User ID: {userId}</div>
  )
}
```

Because now our User component wants these props where we are rendering this component we need to pass the props like in **App.tsx**

```tsx
<User firstName="Nouman" lastName="Rahman" userId={12} />
```

Now our app looks like this

![image](https://cdn.hashnode.com/res/hashnode/image/upload/v1640178113276/1aCgaR5oW.png)

### That's It Our Post End Right Here