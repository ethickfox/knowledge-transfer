GraphQL offers a solution to situations when the client needs data from multiple resources simultaneously, such as requesting a blog post and comments. Typically, this is solved by having the client make multiple requests or having the server supply extra data that might not always be required, leading to larger response sizes. It allows the client to specify exactly what data it desires, including navigating child resources in a single request and allows for multiple queries in a single request.

It also works in a much more RPC manner, using named queries and mutations instead of a standard mandatory set of actions. **This works to put the control where it belongs, with the API developer specifying what's possible and the API consumer specifying what's desired.**

The GraphQL server exposes a schema describing the API. This schema consists of type definitions. Each type has one or more fields, each taking zero or more arguments and returning a specific type.

The graph is derived from the way these fields are nested with each other. Note that the graph doesn't need to be acyclic, cycles are perfectly acceptable, but it is directed. The client can get from one field to its children, but it can't automatically get back to the parent unless the schema defines this explicitly.

**The root query needs to have specially annotated methods to handle the various fields in this root query**.

We need to **annotate the handler methods with** _**@QueryMapping**_ **annotation and place these inside standard** _**@Controller**_ **components**

```java
@Controller
public class PostController {

    private PostDao postDao;

    @QueryMapping
    public List<Post> recentPosts(@Argument int count, @Argument int offset) {
        return postDao.getRecentPosts(count, offset);
    }
}
```

**The** _**@SchemaMapping**_ **annotation maps the handler method to a field with the same name in the schema** and uses it as the DataFetcher for that field.

```java
@SchemaMapping
public Author author(Post post) {
    return authorDao.getAuthor(post.getAuthorId());
}
```
