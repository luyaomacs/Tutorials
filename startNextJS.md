

# Create your own NextJS project

## Get Started

### Install dependency packages

First make sure you have installed Node Package Manager (npm) for Node.js and Node Package Execute (npx), use `npm -v` and `npx -v` to check if you have installed them.

Then make sure you have installed Next.js. Use `npm list next` to check if you have installed. If not, use `npm install next` to install.



### Create project framework

Then, use the following command to create a Next.js project framework.

```shell
npx create-next-app@latest <appname>
```

In the project folder, use the following command to run.

```shell
npm run dev
```

After running this command, you can visit your project on `localhost:3000`. (You can see a starter page)



### Get an empty page

In `app/` directory, first you can remove all the things in `page.tsx` and just leave an empty main function.

```tsx
import image from "next/image";

export default function Home() {
    return <main></main>;
}
```

💭 What is `.tsx` file?

> The `.tsx` file extension is used for files containing TypeScript code combined with JSX syntax.
>
> TypeScript is a superset of JavaScript that adds type-checking capabilities, making it possible to catch errors and bugs before runtime. JSX, on the other hand, is a syntax extension for JavaScript that allows you to write HTML-like elements and components within your JavaScript code. 

Remove everything in `global.css`, just keep the tailwind utilities.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```



Now you can see an empty page in `localhost:3000`. And we will start edit your own page ✌



## Develop your own page



```shell
npm i styled-components # install styled-components library
```



We will provide an example of how to create a TODO app. 

#### Create sidebar and buttons

For a TODO app, usually we need a sidebar and some buttons. So under `app/` directory, we create `app/Components/` directory. And under `Components/`, we create `Components/Sidebar/` and `Components/Button/`.

File Tree:

```
mytasks
└──app
   │  favicon.ico
   │  globals.css
   │  layout.tsx
   │  page.tsx
   │  
   ├─Components
   │  ├─Button
   │  └─Sidebar
   │          Sidebar.tsx
   │          
   └─providers

```



🤔 Why "Content" in `page.tsx` and "Sidebar" in `layout.tsx` can be shown in the same page?



If you need a user interaction, then the component should be a client component.

```tsx
"use client";
```

This tells Next.js that the component or module containing `"use client";` is a Client Component, meaning it should only be executed on the client-side (in the browser) and not during server-side rendering.






In JavaScript or TypeScript (`.js`, `.ts`, `.tsx` file), using `export` is for modular programming.

```tsx
//file: Sidebar.tsx
export default function Sidebar() {}
//file: layout.tsx
import Sidebar from "./Components/Sidebar/Sidebar";
```





React Hooks

```tsx
import React, {createContext, useState, useContext} from "react";
```

> `createContext`: creates a Context object. Context provides a way to share values between components without having to explicitly pass props through every level of the component tree. The return value of `createContext` is an object containing two React components: `Provider` and `Consumer`, which allow you to share state across different levels of your component hierarchy.
>
> `useState`: A hook that lets you add state to React function components. Previously, state could only be used within class components. `useState` takes an initial state and returns a pair: the current state and a function that updates it. 
>
> `useContext`: Also a hook, `useContext` lets you subscribe to the current value of a Context object created by `createContext`. This hook takes a context object (the return value of `createContext`) and returns the current context value. It offers a cleaner way to subscribe to React Context without having to write nested `<Context.Consumer>` components.





## Some Key Info

### App Router

App router is the biggest feature of Next.js, we don't have to worry about routing.

App router allows us to navigate between pages easily.

In React, we need some libraries and configurations to do that.

In Next.js, we just do it in the `app` folder.

The main structure of the App router. (For dynamic router, we use square parenthesis `[]`).

![diagram-app-router](./image/diagram-app-router.png)





Dynamic Router: `[]`

Group the pages: use `()`, and we don't have to write the group name in url



### Layout

Use Layout: we don't need to use the components in every single page.

Change the `layout.tsx` !!! It works.



Map function, Add Links to the page.

```jsx
<div>
    {links.map((link=>(
        <Link href={link.path} key={link.title}>{link.title}</Link>
    )))}
</div>
```

`page.tsx` template

```tsx
const Loading = () => {
    return (
        <div>Loading</div>
    );
};

export default Loading;
```

What we need:

```
page.tsx
layout.tsx
loading.tsx
error.tsx		# for something like database access
not-found.tsx
```



### Styling: .css and .module.css

in CSS, we can use:

```css
:root{
    --bg: #0d0c22;
    --bgsoft: #2d2b42;
    --text: white;
    --textsoft: #e5e5e5;
    --btn: #3673fd;
}
/* We can use these variables in any component or page, bcz it is the parent css */

*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
/* The universal selector */
/*It matches elements of any type within the document.*/

body{
    background-color: var(--bgsoft);
    color:var(--textsoft);
}
```



Next.js allows us to use module CSS, which can solve className conflicts (className in globals.css be the same as className in some Navbar.css).

```tsx
import styles from "./Navbar.module.css"

<div className={styles.container}> ...
```



To highlight the link selected, we need to change the style of the selected link.

🤔 If path name == link title, then give it a different style.

If current path name == link path, then give the link a different style.

How to get the path name? We need a ***Hook***  

```jsx
"use client";

import Link from "next/link";
import styles from "./navLink.module.css";
import {usePathname} from "next/navigation";

// Give the selected link a different style.
const NavLink = ({item}) => {

    const pathName = usePathname();

    return (
        <Link 
            href={item.path} className={`${styles.container} 
            ${pathName === item.path && styles.active
            }`}
        >
            {item.title}
        </Link>
    )
}

export default NavLink;
```



### Responsive Layout

Make the page responsive!!! 🎊 

Create a responsive menu.

When adjust the window size, if there's enough room for the Navbar, then show the whole Navbar, otherwise, show something like a sidebar.

How to do this? Use Media query.

```css
@media (max-width: 768px){
    .links{
        display: none;
    }
}
```



### Show Image: `<Image />`

```jsx
//Not Next.js
<img src="/about.png"/>
// Next.js, need to provide the width and height.
<Image src="/about.png" alt="about" width={500} height={500}/>
// remember to import
import Image from "next/image";
```

***Next.js*** does not show the image directly, instead it's caching it first and ***optimizing*** it for device screen sizes. (You can click "open image in new tab to see the URL") You don't need to manually edit the size of every image.

Usually the cached image is much smaller than the original.

In the code below, `fill` means the image would fill in the container.

```tsx
<div className={styles.imgContainer}>
<Image src="/about.png" alt="Image: about" fill/>
</div>
```

But Next.js does not allow us to use any external image URL.

In `next.config.mjs`, you can modify the config using:

```js
const nextConfig = {
    images:{
        remotePatterns: [
            {
                protocol: 'https',
                hostname: "images.pexels.com",
            }
        ],
    }
};
```

And then we can use external image URL, like:

```tsx
<Image src="https://images.pexels.com/photos/7945944/pexels-photo-7945944.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2" alt="" fill/>
```



### Form

```tsx
<form className={styles.form}>
    <input type="text" placeholder="Name and Surname" />
    <input type="email" placeholder="Email Address" />
    <input type="text" placeholder="Phone Number (Optional)" />
    <textarea name="" id="" cols={30} rows={10} placeholder="Message"></textarea>
    <button>Send</button>
</form>
// button is part of the form (click the button and it would refresh the form)
```

Adjust the style of form components in `css`: 

```css
.form {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.form input,
.form textarea { 
    padding: 20px;
    border-radius: 5px;
    border: none;
    outline: none;
    background-color: var(--bgsoft);
    color: var(--text);
}

.form button {
    padding: 20px;
    background-color: var(--btn);
    color: var(--text);
    font-weight: bold;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}
```



### Rendering: SSR and CSR

We can choose to render in the Server side or in the Client side

SSR vs CSR

![rendering](./image/diagram-rendering.png)

#### SSR

Pros: 

 + The initial page load is faster

 + Better old devices and slow internet connection

 + Better SEO (Search Engine Optimization)

	SSR generates the full HTML for each page on the server before sending it to the client's browser, ensuring that search engine bots can index the content effectively

Cons:

+ Less interaction
+ Increased Server Loads
+ Slower Subsequent Page Loads
+ State Management complexity

#### CSR

Pros:

- Better Performance after initial load
- Less server load
- Best for the user interactivity

Cons:

- May affect SEO
- Slower initial load
- Dependency on client resources

In React, the default approach is use the Client side rendering.

In Next.js, it use server side rendering. When you need some interactivity:

We can combine the server and client components like this:

![useclient](figure-useclient.png)

Write `"use client"` directive on the top of the client components.

After this directive, we can use React Hooks and we can interact with the user.



Prove that the Next.js components work on the server side:

​	Write `console.log("It works here!")` in the `.js` code and open the server terminal and the console on the browser. To see where it works.

​	In JS file with `use client`, this log would appear in the user browser console. Otherwise it would appear in the server console.

NOTE: Even if you write `"use client"` directive, the initial render will be on the server, and you will most probably encounter hydration problems. Because Next.js checks your components on the server side first and it expects that you have the same component on the client side. 

```jsx
const a = Math.random()
console.log(a)
// The server console and the user browser console shows different numbers.
// But why CAN't I see any number in the server console when "use client"!!!😭
```

Hydration problem: Text content does not match server-rendered HTML.

Solutions:

- Using a user fact Hook, make sure the component runs on the client side.

	```jsx
	const [isClient, setIsClient] = useState(false);
	
	useEffect(()=> {
	    setIsClient(true);
	}, []);
	
	const a = Math.random();
	
	console.log(a);
	
	{isClient && a} // it's gonna be rendered only when it's client.
	```

- Disabling server side rendering on specific components.

	```jsx
	// We have a HydrationTest component in hydrationTest.jsx
	// instead of import HydrationTest from "@/components/hydrationTest";
	// we do this:
	import dynamic from "next/dynamic";
	const HydrationTestNoSSR = dynamic( () => import("@/components/hydrationTest"), {ssr: false} )
	
	// And we can call this component later.
	<HydrationTestNoSSR/>
	```

- Suppress Hydration Warning

	```jsx
	<div suppressHydrationWarning>{a}</div>
	// not going to show in the browser
	```

If I wrap my server side component A with a client side component B, then A is still a server side component.



### Navigation

```tsx
<Link href="/" prefetch={false}>Click here</Link>
```

Link component would prefetch the page. Allow us to see the referring page much faster.

But if you have many links in the component, you might want to disable some prefetch.

```tsx
import {useRouter} from 'next/navigation';

const router = useRouter() //useRouter hook

const handleClick = () => {
    console.log("clicked")
    router.push("/") // it performs a client-side navigation to the provided router
}
```

`router.push()` add a new entry to the browser history stack. (When click the `return` button of the browser, it will go back to the last page.)

`router.replace()` would not add a new entry to the browser history stack.

`router.refresh()` would refresh the current route and make a new request to the server.

```tsx
router.push("/")
router.replace("/")
router.refresh()
router.back()
router.forward()
```

This is the ***router hook***

Hook: even if we don't have any click event, the component has to be a client side component. (bcz we are using a hook)



***Query***: `http://localhost:3000/navigationtest?q=test$sort=desc`

You can set a new query using `set()` method.

```tsx
const searchParams = useSearchParams() // hook

const q = searchParams.get("q")
const sort = searchParams.get("sort")

console.log(q) // test
console.log(sort) // desc
```



Useful Hooks

```tsx
import { useRouter, usePathname, useSearchParams } from 'next/navigation';
```



### Data Fetching

Create our own blogs, fetch them using our database

Use API: we get the data from an external API (like a link)

```tsx
const getData = async () => {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts") //data fetch

    if(!res.ok){
        throw new Error("Failed to fetch data")
    }

    return res.json()
}

const BlogPage = async () => {

    const posts = await getData();

    return(
        <div className={styles.container}>
            {posts.map( (post) => (
                <div className={styles.post}>
                <PostCard post={post}/>
                </div>
            ))}
        </div>
    );
};
```

By default, Next.js caches the API responses to increase performance

```tsx
//default
const res = await fetch("https://jsonplaceholder.typicode.com/posts", {cache:"force-cache"});
```

If you don't want to cache, 

```tsx
//do not want to cache
const res = await fetch("https://jsonplaceholder.typicode.com/posts", {cache:"no-store"});
```

If you have some data that changes constantly in your database, you'd better use `no-store`

Otherwise, it's better to use cache. 

Or you can set a revalidate time (like refresh every one hour)

```tsx
//refresh regularly
const res = await fetch("https://jsonplaceholder.typicode.com/posts", {next:{revalidate:3600}});
```



### React Suspense

Show an indicator like loading text or a skeleton component

To do so, use React Suspense

```tsx
import {Suspense} from "react";

<Suspense fallback={<div>LoadingData...</div>}>
    <PostUser userID={post.userId}/>
</Suspense>
```

Or you can create a skeleton component and show that instead of this loading text



Since we are using Next.js 14, we don't need an API to fetch data.

Fetch data in `@/lib/data.js`



### Fetch data using Mongo DB

Create database using MongoDB Atlas

Get the database URL:

```
mongodb+srv://luyaomacs:<password>@blogdatabase.fgrftnd.mongodb.net/?retryWrites=true&w=majority&appName=BlogDatabase
```

We need to hide this.

create a `.env` file

```
MONGO = mongodb+srv://luyaomacs:13722729856Mly!@blogdatabase.fgrftnd.mongodb.net/next14tutorial?retryWrites=true&w=majority&appName=BlogDatabase
```

In the `.gitignore`, make sure the `.env` file is in `local env files`

When you push the project into github, it would not be visible.

```
createAt doesn't work
BUT createdAt works
??? WHY ???
```



## What you have finished 🎊 😊 

https://youtu.be/vCOSTG10Y4o?feature=shared

> [!NOTE]
>
> Understand Folder Structure
>
> App Route
>
> Layouts
>
> Components
>
> Loading, Error, Not Found Layouts
>
> Styling (CSS Modules and Tailwind)
>
> Navbar Link
>
> Responsive Navbar Design
>
> Responsive Container Layout
>
> Image
>
> Rendering
>
> Navigation
>
> Data Fetching
>
> Suspense
>
> Fetch data without using API
>
> MongoDB Full Stack App
>
> ====
>
> no-store: stop caching data
>
> SEO (static and dynamic SEO)
>
> Server Actions 
>
> API Route
>
> Server Actions vs API Routes
>
> Authentication with Next-Auth & Server Actions
>
> Auth Social Media Sign in
>
> useFormState Hook
>
> NEXT_REDIRECT Error and the Solution
>
> Middleware Explained | Next.js Auth.js (Next-Auth) How to Protect Routes?
>
> Admin Dashboard Tutorial with Server Actions

