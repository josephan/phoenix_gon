# PhoenixGon [![Hex.pm](https://img.shields.io/hexpm/v/plug.svg)](https://hex.pm/packages/phoenix_gon) [![Build Status](https://travis-ci.org/khusnetdinov/phoenix_gon.svg?branch=master)](https://travis-ci.org/khusnetdinov/phoenix_gon)
## Your Phoenix variables in your JavaScript.

![img](http://res.cloudinary.com/dtoqqxqjv/image/upload/v1492849051/github/gon.png)

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed
by adding `phoenix_gon` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [{:phoenix_gon, "~> 0.2.0"}]
end
```

## Usage

### Three steps configuration:

1) You need add plug to `lib/project/endpoint.ex` before you router plug.

```elixir
defmodule Project.Endpoint do

  ...

  plug PhoenixGon.Pipeline
  plug Project.Router
end
```

Plug accepts options:

    - `:env` - this option for hard overloading Mix.env.
    - `:namespace` - namescpase for javacript object in global window space.
    - `:assets` - map for keeping permament variables in javascript.

2) Add possibility to use view helper by adding `use PhoenixGon.View` in templates in `web/views/layout_view.ex` file:

```elixir
defmodule Project.LayoutView do
  ...
  import PhoenixGon.View
  ...
end

```

3) Add helper `render_gon_script` to you layout in `/web/templates/layout/app.html.eex` before main javascript file:

```elixir

  ...

  <%= render_gon_script(@conn) %>
  <script src="<%= static_path(@conn, "/js/app.js") %>"></script>
</body>
```

Now you can read phoenix variables in browser console and javascript code.


### Phoenix controllers

For using gon in controllers just add:

```elixir
defmodule Project.Controller do
  ...
  import PhoenixGon.Controller
  ...
end
```

#### Controller methods:

All controller variables are kept in `assets` map.

- `put_gon` - Put variable to assets.
- `update_gon` - Update variable in assets.
- `drop_gon` - Drop variable in assets.
- `get_gon` - Get variable from assets.

Example:

```elixir
  def index(conn, _params) do
    conn = put_gon(conn, controller: variable)
    render conn, "index.html"
  end
```

### JavaScript

Gon object is kept in `window`.

#### Browser

Now you can access to you variables in console:

```javascript
// browser console

Gon.assets()

// Object {controller: "variable"}
```

#### JavaScript assets

```JavaScript
// Somewhere in javascript modules

window.Gon.assets()

```

#### JavaScript methods:

Phoenix env methods:

  - `getEnv()` - Returns current phoenix env.
  - `isDev()` - Returns boolean if development env.
  - `isProd()` - Returns boolean if production env.
  - `isCustomEnv(env)` - Return bollean if custom env.

Assets variables methods:

  - `assets()` - Returns all variables setting in config and controllers.
  - `getAssets(key)` - Returns variable by key.

## Contributors

Special thanks to Andrey Soshchenko @getux.

## License

The library is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
