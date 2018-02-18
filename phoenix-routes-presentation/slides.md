theme: Simple,6

# Phoenix Routing:

## A High Level Introduction

---
# The parts of a URL

### `https://twitter.com/ElixirGirls`

---
# The parts of a URL

### **The protocol**: `https://`

---
# The parts of a URL

### **The host**: `twitter.com`

---
# The parts of a URL

### **The path**: `/ElixirGirls`

---
# HTTP methods (or verbs)

- `GET` - Eg, when you type a URL in to the browser and hit enter
- `POST` - Eg, when you are submitting a form
- `PATCH`
- `PUT`
- `DELETE`
- `OPTIONS`
- `HEAD`

---
# What is routing in Phoenix

Telling the phoenix framework (in `router.ex`) what code to run for a combination of a *URL path* and the *HTTP method/verb*.

---
# Listing routes

Run the following from the root of your project

```sh
mix phx.routes        # 1.3+
mix phoenix.routes    # 1.2 and below
```

---
# Example results

```sh
     page_path  GET     /                        BlogElixirGirlsWeb.PageController :index
     post_path  GET     /posts                   BlogElixirGirlsWeb.PostController :index
     post_path  GET     /posts/:id/edit          BlogElixirGirlsWeb.PostController :edit
     post_path  GET     /posts/new               BlogElixirGirlsWeb.PostController :new
     post_path  GET     /posts/:id               BlogElixirGirlsWeb.PostController :show
     post_path  POST    /posts                   BlogElixirGirlsWeb.PostController :create
     post_path  PATCH   /posts/:id               BlogElixirGirlsWeb.PostController :update
                PUT     /posts/:id               BlogElixirGirlsWeb.PostController :update
     post_path  DELETE  /posts/:id               BlogElixirGirlsWeb.PostController :delete
post_post_path  POST    /posts/:post_id/comment  BlogElixirGirlsWeb.PostController :add_comment
```

Notice the *HTTP methods*, and *URL paths* in each result.

---
# Declaring a single route

- `get "/posts", PostController, :index`
- `post "/posts", PostController, :create`

In other words:

- `http_method "/url/path", ModuleName, :function_name`

---
# Declaring resources

`resources "/posts", PostController`

---
# Declaring resources

Is the same as declaring:

- `get "/posts", PostController, :index`
- `get "/posts/:id/edit", PostController, :edit`
- `get "/posts/new", PostController, :new`
- `get "/posts/:id", PostController, :show`

continued...

---
# Declaring resources

- `post "/posts", PostController, :create`
- `patch "/posts/:id", PostController, :update`
- `put "/posts/:id", PostController, :update`
- `delete "/posts/:id", PostController, :delete`

---
# Route parameters

Some of the examples that we have seen have a route parameter:

`get "/posts/:id", PostController, :show`

The `:id` param will be passed to the `show` function in the controller:

```elixir
def show(conn, %{"id" => id}) do
  post = Blog.get_post!(id)
  changeset = Blog.change_comment(%Comment{})
  render(conn, "show.html", post: post, changeset: changeset)
end
```

---
# Path helpers

Declaring routes will create path helpers behind the scenes:

The route

`get "/posts/:id", PostController, :show`

will have a path helper function called `post_path`. This can be used to generate a path for the route, for example `post_path(conn, :show, post)`.

*Note*: Listing routes via `mix` will include the path helper name.

---
# Scopes

Scopes are a way to group routes under a common path prefix, for example `"/"` or `"/api"`

```elixir
scope "/", BlogElixirGirlsWeb do
  pipe_through :browser # Use the default browser stack

  get "/", PageController, :index
  resources "/posts", PostController do
    post "/comment", PostController, :add_comment
  end
end
```

---
# Channel and routing

Channels work differently and so are not routed in the same way. Declaring a `channel` will tell phoenix what code to run based on the name of the channel, for example:

`channel "random:lobby", Slackir.RandomChannel`

---
# Resources

- [HTTP methods / verbs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- [Routing cheat sheet](https://devhints.io/phoenix-routing)
- [Phoenix routing guide](https://hexdocs.pm/phoenix/routing.html#content)
- [@matt_furness on Twitter](https://twitter.com/matt_furness)
