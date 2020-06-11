<a name="top"></a>

# React Router API Reference

React Router is a collection of [React components](https://reactjs.org/docs/components-and-props.html), [hooks](#https://reactjs.org/docs/hooks-intro.html) and utilities that make it easy to build multi-page applications with [React](https://reactjs.org).

If you [installed](./installation) React Router as a global (using a `<script>` tag), you can find the library on the `window.ReactRouterDOM` object. If you installed it from npm, you can simply `import` the pieces you need. The examples in this reference all use `import` syntax.

<a name="overview"></a>

## Overview

The various components, hooks, and methods in the React Router API fall into a few major categories, grouped by purpose.

<a name="packages"></a>

### Packages

React Router is published to npm in three different packages:

- [`react-router`](https://npm.im/react-router) contains most of the core functionality of React Router including the route matching algorithm and most of the core components and hooks
- [`react-router-dom`](https://npm.im/react-router-dom) includes everything from `react-router` and adds a few DOM-specific APIs like [`<BrowserRouter>`](#browserrouter), [`<HashRouter>`](#hashrouter), [`<Link>`](#link), etc.
- [`react-router-native`](https://npm.im/react-router-native) includes everything from `react-router` and adds a few APIs that are specific to React Native including [`<NativeRouter>`](#nativerouter) and [a native version of `<Link>`](#link-native)

Both `react-router-dom` and `react-router-native` automatically include `react-router` as a dependency when you install them. When you `import` stuff, you should always import from either `react-router-dom` or `react-router-native` and never directly from `react-router`.

<a name="setup"></a>

### Setup

To get React Router working in your app, you need to render a router element at or near the root of your element tree. We provide several different routers, depending on where you're running your app.

- [`<BrowserRouter>`](#browserrouter) or [`<HashRouter>`](#hashrouter) should be used when running in a web browser. Which one you pick depends on the style of URL you prefer or need.
- [`<StaticRouter>`](#staticrouter) should be used when server-rendering a website
- [`<NativeRouter>`](#nativerouter) should be used in [React Native](https://reactnative.dev/) apps
- [`<MemoryRouter>`](#memoryrouter) is useful in testing scenarios and as a reference implementation for the other routers

These routers provide the context that React Router needs to operate in a particular environment. Each one renders [a `<Router>`](#router) internally, which you may also do if you need more fine-grained control for some reason. But it is highly likely that one of the built-in routers is what you need.

<a name="routing"></a>

### Routing

Routing is the process of deciding which React elements will be rendered on a given page of your app, and how they will be nested. React Router provides two interfaces for declaring your routes.

- [`<Routes>` and `<Route>`](#routes-and-route) if you're using JSX
- [`useRoutes`](#useroutes) if you'd prefer a JavaScript object-based config

<a name="navigation"></a>

### Navigation

React Router's navigation interfaces let you change the currently rendered page by modifying the current [location](#TODO). There are two main interfaces for navigating between pages in your app, depending on what you need.

- [`<Link>`](#link) and [`<NavLink>`](#navlink) render an accessible `<a>` element, or a `TouchableHighlight` on React Native. This lets the user initiate navigation by clicking or tapping an element on the page.
- [`useNavigate`](#usenavigate) and [`<Navigate>`](#navigate) let you programmatically navigate, usually in an event handler or in response to some change in state

<a name="confirming-navigation"></a>

### Confirming Navigation

Sometimes you need to confirm navigation before it actually happens. For example, if the user has entered some data into a form on the current page, you may want to prompt them to save the data before they navigate to a different page.

- [`usePrompt` and `<Prompt>`](#useprompt-and-prompt) trigger a platform-native confirmation prompt when the user tries to navigate away from the current page
- [`useBlocker`](#useblocker) is a low-level interface that lets you keep the user on the same page and execute a function that will be called when they try to navigate away

<a name="search-parameters"></a>

### Search Parameters

Access to the URL [search parameters](https://developer.mozilla.org/en-US/docs/Web/API/URL/searchParams) is provided via [the `useSearchParams` hook](#usesearchparams).

----------

<a name="reference"></a>

## Reference

This is an alphabetical list of all components, hooks, and other methods that are part of React Router. APIs that are specific to building web apps are contained in the `react-router-dom` package, while React Native APIs are contained in the `react-router-native` package. Both packages re-export the entire core library, so you only ever need to install one of them.

> [!Note:]
>
> You should only ever `import` from one React Router package. If you're making
> a web app, you should import everything from `react-router-dom`. Otherwise, in
> a React Native app import everything from `react-router-native`. You never
> need to import anything from `react-router`.

<a name="browserrouter"></a>

### `<BrowserRouter>`

```tsx
declare function BrowserRouter(props: BrowserRouterProps): React.ReactElement;

interface BrowserRouterProps {
  children?: React.ReactNode;
  window?: Window;
}
```

`<BrowserRouter>` is the recommended interface for running React Router in a web browser. A `<BrowserRouter>` stores the current location in the browser's address bar using clean URLs and navigates using the browser's built-in history stack.

`<BrowserRouter window>` defaults to using the current [document's `defaultView`](https://developer.mozilla.org/en-US/docs/Web/API/Document/defaultView), but it may also be used to track changes to another's window's URL, in an `<iframe>`, for example.

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
  <BrowserRouter>
    {/* The rest of your app goes here */}
  </BrowserRouter>,
  root
);
```

<a name="hashrouter"></a>

### `<HashRouter>`

```tsx
declare function HashRouter(props: HashRouterProps): React.ReactElement;

interface HashRouterProps {
  children?: React.ReactNode;
  window?: Window;
}
```

`<HashRouter>` is for use in web browsers when the URL should not (or cannot) be sent to the server for some reason. This may happen in some shared hosting scenarios where you do not have full control over the server. In these situations, `<HashRouter>` makes it possible to store the current location in the `hash` portion of the current URL, so it is never sent to the server.

`<HashRouter window>` defaults to using the current [document's `defaultView`](https://developer.mozilla.org/en-US/docs/Web/API/Document/defaultView), but it may also be used to track changes to another window's URL, in an `<iframe>`, for example.

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import { HashRouter } from 'react-router-dom';

ReactDOM.render(
  <HashRouter>
    {/* The rest of your app goes here */}
  </HashRouter>,
  root
);
```

<a name="nativerouter"></a>

### `<NativeRouter>`

```tsx
declare function NativeRouter(props: NativeRouterProps): React.ReactElement;

interface NativeRouterProps {
  children?: React.ReactNode;
  initialEntries?: InitialEntry[];
  initialIndex?: number;
}
```

`<NativeRouter>` is the recommended interface for running React Router in a [React Native](https://reactnative.dev) app.

- `<NativeRouter initialEntries>` defaults to `["/"]` (a single entry at the root `/` URL)
- `<NativeRouter initialIndex>` defaults to the last index of `props.initialEntries`

```tsx
import React from 'react';
import { NativeRouter } from 'react-router-native';

function App() {
  return (
    <NativeRouter>
      {/* The rest of your app goes here */}
    </NativeRouter>
  );
}
```

<a name="memoryrouter"></a>

### `<MemoryRouter>`

```tsx
declare function MemoryRouter(props: MemoryRouterProps): React.ReactElement;

interface MemoryRouterProps {
  children?: React.ReactNode;
  initialEntries?: InitialEntry[];
  initialIndex?: number;
}
```

A `<MemoryRouter>` stores its locations internally in an array. Unlike `<BrowserHistory>` and `<HashHistory>`, it isn't tied to an external source, like the history stack in a browser. This makes it ideal for scenarios where you need complete control over the history stack, like testing.

- `<MemoryRouter initialEntries>` defaults to `["/"]` (a single entry at the root `/` URL)
- `<MemoryRouter initialIndex>` defaults to the last index of `props.initialEntries`

> [!Tip:]
>
> Most of React Router's tests are written using a `<MemoryRouter>` as the
> source of truth, so you can see some great examples of using it by just
> [browsing through our tests](https://github.com/ReactTraining/react-router/tree/dev/packages/react-router/__tests__).

```tsx
import React from 'react';
import { create } from 'react-test-renderer';
import { MemoryRouter, Routes, Route } from 'react-router-dom';

describe('My app', () => {
  it('renders correctly', () => {
    let renderer = create(
      <MemoryRouter initialEntries={['/users/mjackson']}>
        <Routes>
          <Route path="users" element={<Users />}>
            <Route path=":id" element={<UserProfile />}>
          </Route>
        </Routes>
      </MemoryRouter>
    );

    expect(renderer.toJSON()).toMatchSnapshot();
  });
});
```

<a name="link"></a>

### `<Link>`

> [!Note:]
>
> This is the web version of `<Link>`. For the React Native version,
> [go here](#link-native).

```tsx
declare function Link(props: LinkProps): React.ReactElement;

interface LinkProps
  extends Omit<React.AnchorHTMLAttributes<HTMLAnchorElement>, 'href'> {
  replace?: boolean;
  state?: State;
  to: To;
}
```

A `<Link>` is an element that lets the user navigate to another page by clicking or tapping on it. In `react-router-dom`, a `<Link>` renders an accessible `<a>` element with a real `href` that points to the resource it's linking to. This means that things like right-clicking a `<Link>` work as you'd expect.

```tsx
import React from 'react';
import { Link } from 'react-router-dom';

function UsersIndexPage({ users }) {
  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            <Link to={user.id}>{user.name}</Link>
          </li>
        ))}
      </ul>
    </div>
  )
}
```

A relative `<Link to>` value (that does not begin with `/`) resolves relative to the parent route, which means that it builds upon the URL path that was matched by the route that rendered that `<Link>`. It may contain `..` to link to routes further up the hierarchy. In these cases, `..` works exactly like the command-line `cd` function; each `..` removes one segment of the parent path.

> [!Note:]
>
> `<Link to>` with a `..` behaves differently from a normal `<a href>` when the
> current URL ends with `/`. `<Link to>` ignores the trailing slash, and removes
> one URL segment for each `..`. But an `<a href>` value handles `..` differently
> when the current URL ends with `/` vs when it does not.

<a name="link-native"></a>

### `<Link>` (React Native)

> [!Note:]
>
> This is the React Native version of `<Link>`. For the web version,
> [go here](#link).

```tsx
declare function Link(props: LinkProps): React.ReactElement;

interface LinkProps extends TouchableHighlightProps {
  onPress?: (event: GestureResponderEvent) => void;
  replace?: boolean;
  state?: State;
  to: To;
}
```

A `<Link>` is an element that lets the user navigate to another view by tapping it, similar to how `<a>` elements work in a web app. In `react-router-native`, a `<Link>` renders a `TouchableHighlight`.

TODO: example

<a name="navlink"></a>

### `<NavLink>`

```tsx
declare function NavLink(props: NavLinkProps): React.ReactElement;

interface NavLinkProps extends LinkProps {
  activeClassName?: string;
  activeStyle?: object;
  caseSensitive?: boolean;
  end?: boolean;
}
```

A `<NavLink>` is a special kind of [`<Link>`](#link) that knows whether or not it is "active". This is useful when building a navigation menu such as a breadcrumb or a set of tabs where you'd like to show which of them is currently selected.

Both `<NavLink activeClassName>` and/or `<NavLink activeStyle>` are applied to the underlying `<Link>` when the route it links to is currently active.

```tsx
import React from 'react';
import { NavLink } from 'react-router-dom';

function NavList() {
  // This styling will be applied to a <NavLink> when the
  // route that it links to is currently selected.
  let activeStyle = {
    textDecoration: 'underline'
  };

  return (
    <nav>
      <ul>
        <li>
          <NavLink to="messages" activeStyle={activeStyle}>Messages</NavLink>
        </li>
        <li>
          <NavLink to="tasks" activeStyle={activeStyle}>Tasks</NavLink>
        </li>
      </ul>
    </nav>
  );
}
```

<a name="navigate"></a>

### `<Navigate>`

TODO

<a name="outlet"></a>

### `<Outlet>`

```tsx
declare function Outlet(): React.ReactElement | null;
```

An `<Outlet>` should be used in parent route elements to render their child route elements. This allows nested UI to show up when child routes are rendered. If the parent route matched exactly, it will render nothing.

```tsx
function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>

      {/* This element will render either <DashboardMessages> when the URL is
          "/messages", <DashboardTasks> at "/tasks", or null if it is "/"
      */}
      <Outlet />
    </div>
  );
}

function App() {
  return (
    <Routes>
      <Route path="/" element={<Dashboard />}>
        <Route path="messages" element={<DashboardMessages />} />
        <Route path="tasks" element={<DashboardTasks />} />
      </Route>
    </Routes>
  )
}
```

<a name="router"></a>

### `<Router>`

```tsx
declare function Router(props: RouterProps): React.ReactElement;

interface RouterProps {
  action?: Action;
  children?: React.ReactNode;
  location: Location;
  navigator: Navigator;
  pending?: boolean;
  static?: boolean;
}
```

`<Router>` is the low-level interface that is shared by all router components ([`<BrowserRouter>`](#browserrouter), [`<HashRouter>`](#hashrouter), [`<StaticRouter>`](#staticrouter), [`<NativeRouter>`](#nativerouter), and [`<MemoryRouter>`](#memoryrouter)). In terms of React, `<Router>` is a [context provider](https://reactjs.org/docs/context.html#contextprovider) that supplies routing information to the rest of the app.

You probably never need to render a `<Router>` manually. Instead, you should use one of the higher-level routers depending on your environment. You only ever need one router in a given app.

<a name="routes-and-route"></a>
<a name="routes"></a>
<a name="route"></a>

### `<Routes>` and `<Route>`

```tsx
declare function Routes(props: RoutesProps): React.ReactElement | null;

interface RoutesProps {
  basename?: string;
  children?: React.ReactNode;
}

declare function Route(props: RouteProps): React.ReactElement | null;

interface RouteProps {
  caseSensitive?: boolean;
  children?: React.ReactNode;
  element?: React.ReactElement | null;
  path?: string;
  preload?: RoutePreloadFunction;
}

type RoutePreloadFunction = (
  params: Params,
  location: Location,
  index: number
) => void;
```

`<Routes>` and `<Route>` are the primary ways to render something in React Router based on the current [`location`](#TODO). You can think about a `<Route>` kind of like an `if` statement; if its `path` matches the current URL, it renders its `element`!

Whenever the location changes, `<Routes>` looks through all its `children` `<Route>` elements to find the best match and renders that branch of the UI. `<Route>` elements may be nested to indicate nested UI, which also correspond to nested URL paths.

```tsx
<Routes>
  <Route path="/" element={<Dashboard />}>
    <Route path="messages" element={<DashboardMessages />} />
    <Route path="tasks" element={<DashboardTasks />} />
  </Route>
  <Route path="about" element={<AboutPage />} />
</Routes>
```

`<Route element>` defaults to an [`<Outlet />`](#outlet). This means the route will still render its children even without an `element` prop, so you can nest route paths without nesting UI around the child routes.

For example, in the following config the parent route renders an `<Outlet />` by default, so the child route will render without any surrounding UI. But the route paths will still nest.

```tsx
<Route path="users">
  <Route path=":id" element={<UserProfile />} />
</Route>
```

If you'd prefer to define your routes as regular JavaScript objects instead of in JSX, [try `useRoutes` instead](#useroutes).

<a name="staticrouter"></a>

### `<StaticRouter>`

```tsx
declare function StaticRouter(props: StaticRouterProps): React.ReactElement;

interface StaticRouterProps {
  children?: React.ReactNode;
  location?: Path | LocationPieces;
}
```

`<StaticRouter>` is used to render a React Router web app in [node](https://nodejs.org). Provide the current location via the `location` prop.

- `<StaticRouter location>` defaults to `"/"`

```tsx
import http from 'http';
import React from 'react';
import ReactDOMServer from 'react-dom/server';
import { StaticRouter } from 'react-router-dom/server';

function requestHandler(req, res) {
  let html = ReactDOMServer.renderToString(
    <StaticRouter location={req.url}>
      {/* The rest of your app goes here */}
    </StaticRouter>
  );

  res.write(html);
  res.end();
}

http.createServer(requestHandler).listen(3000);
```

<a name="usenavigate"></a>

### `useNavigate`

```tsx
declare function useNavigate(): NavigateFunction;

interface NavigateFunction {
  (to: To, options?: { replace?: boolean; state?: State }): void;
  (delta: number): void;
}
```

The `useNavigate` hook returns a function that lets you navigate programmatically, for example after a form is submitted.

```tsx
import { useNavigate } from 'react-router-dom';

function SignupForm() {
  let navigate = useNavigate();

  async function handleSubmit(event) {
    event.preventDefault();
    await submitForm(event.target);
    navigate('../success', { replace: true });
  }

  return (
    <form onSubmit={handleSubmit}>
      {/* ... */}
    </form>
  );
}
```

The `navigate` function has two signatures:

- Either pass a `To` value (same type as `<Link to>`) with an optional second `{ replace, state }` arg or
- Pass the delta you want to go in the history stack. For example, `navigate(-1)` is equivalent to hitting the back button.

<a name="useroutes"></a>
<a name="partialrouteobject"></a>

### `useRoutes`

```tsx
declare function useRoutes(
  partialRoutes: PartialRouteObject[],
  basename = ''
): React.ReactElement | null;

interface PartialRouteObject {
  path?: string;
  caseSensitive?: boolean;
  element?: React.ReactNode;
  preload?: RoutePreloadFunction;
  children?: PartialRouteObject[];
}

type RoutePreloadFunction = (
  params: Params,
  location: Location,
  index: number
) => void;
```

The `useRoutes` hook is the functional equivalent of [`<Routes>`](#routes), but it uses JavaScript objects instead of `<Route>` elements to define your routes. These objects have the same properties as normal [`<Route>` elements](#route), but they don't require JSX.

The return value of `useRoutes` is either a valid React element you can use to render the route tree, or `null` if nothing matched.

```tsx
import React from 'react';
import { useRoutes } from 'react-router-dom';

function App() {
  let element = useRoutes([
    { path: '/', element: <Dashboard />, children: [
      { path: 'messages': element: <DashboardMessages /> },
      { path: 'tasks', element: <DashboardTasks /> }
    ]},
    { path: 'team', element: <AboutPage /> }
  ]);

  return element;
}
```
