
### Routing and Navigation
Using <Link/>

### Metadata
Path

**src/app/ content**
- layout.tsx - for header and footer
- page_name folder
	- page.tsx - page content
	- loading.tsx
	- middleware.ts
### Styling 
Tailwind 
### Images
<Image/> - component improved image management
### Client vs Server side components
- Data fetching using GET requests
```js
//allows to fetch inside of the component using async method
export default async function PostPage(){
	const response = await fetch('/app/data');
}
```

```js
"use client" // makes component rendered on the client side 
//server side rendering could be done when page hase big dependency components

```

### Server Actions
- POST/PUT/DELETE requests

```js
const addPost = (formData)=>{
	"use server"
	await prisma.post.create({
		data:{
			title: "some title",
			"body": "some body"
		}
	});
	revalidatePath("/posts"); // rerenders page 
}
```

### Suspense and streaming
Automatically uses loading.tsx when waiting for data retrieving
Works as:
```html
<Suspens fallback="Loading...">
		<PostsList/>
</Suspense>
```

### Caching


### Static and Dynamic rendering
static - prerendered in resulting build, uses cached data
To make page dynamic use 
```js
export const dynamic = 'force-dynamic'
//or
const response = await fetch('/app/data',
	{
		cache:"no-cache" // doesn't cahe request
	}
	//or
	{
		next: {
			revalidate: 3600, // after one hour no caching, new request is made
		},
	}
);
```

### Middleware
