theme: Simple,6

# Things to talk about

- Recent addition to get started, so felt like a good time to talk about it.
  - Touch on what is in the guide
  - Focused on local debugging
- inspect, and Inspect.Opts
  - inspect returns what you pass it
  - with options can make diff colours for diff processes
- inspect binding()
- pry in code
- break
  - don't have to modify code
  - can pattern match
  - number of calls is a thing
  - whereami()
- debugger (using elixir ls)
  - launch.json is key
- observer
  - GUI
  - cmd line


---
# Good 'ol `IO.inspect/2`

- Inspects and writes its first argument to the screen
  - As long as it implements the [Inspect protocol](`https://hexdocs.pm/elixir/Inspect.html#content` )
- It is great in the middle of a pipeline because it returns its first argument unchanged
  - Don't forget about it's options, labeling by `pid` can be handy

---
# Good 'ol `IO.inspect/2`

```elixir
[1, 2, 3]
|> IO.inspect(label: "#{Kernel.inspect(self())} before", syntax_colors: [number: :blue])
|> Enum.map(&(&1 * 2))
|> IO.inspect(label: "#{Kernel.inspect(self())} after", syntax_colors: [number: :red])
|> Enum.sum
```
Results in:

```
#PID<0.84.0> before: [1, 2, 3]
#PID<0.84.0> after: [2, 4, 6]
12
```

---

# `Kernel.binding`

Don't forget about `Kernel.binding` it will show you the bound variables in a context:

```elixir
iex> foo = "a"
"a"
iex> bar = "b"
"b"
iex> binding()
[bar: "b", foo: "a"]
```

___

# `IEx.pry`

- If you have used `Pry` in ruby it will feel familiar
- You will be able to access all variables, as well as imports and aliases from the code, directly From IEx
- `IEx.whereami/0` comes in really handy
  - Prints the current location and stacktrace in a pry session

---

# `IEX.break!/1`

- No need to modify code
- Can break on code that isn't yours
- No stops is important to remember
- Can pattern match - awesome!
- If the module is recompiled, all breakpoints are lost
- Aliases and imports from the source code won’t be available in the shell
- Pending stops is important

---

# Break points in vscode

- You need the [Elixir LS](https://github.com/JakeBecker/vscode-elixir-ls) extension for vs code
- Documentation could be better
- You need a properly configured [`launch.json`](https://github.com/mattfurness/elixir-girls-mentoring-blog/blob/master/.vscode/launch.json)
- Still early days, and will likely keep inproving

---

# Observer

- You can really dig into the details with `:observer.start()`
