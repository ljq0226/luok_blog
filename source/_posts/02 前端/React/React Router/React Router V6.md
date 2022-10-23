
# Ultimate React Router v6 Guide


![](https://obs-pic-1309372570.cos.ap-chongqing.myqcloud.com/React%20Router%20V6.png)


## React Router Basics

### Configuring The Router
To get React Router working in your app, you need to render a router element at or near the root of your element tree. We provide several different routers depending on where your app is running.

 `<BrowserRouter>` or `<HashRouter>` should be used when running in a web browser (which one you pick depends on the style of URL you prefer or need)

These routers provide the context that React Router needs to operate in a particular environment. Each one renders a `<Router>` internally, which you may also do if you need more fine-grained control for some reason. But it is highly likely that one of the built-in routers is what you need.
#### BrowserRouter
 `<BrowserRouter>` is the **recommended** interface for running React Router in a web browser. A `<BrowserRouter>` stores the current location in the browser's address bar using clean URLs and navigates using the browser's built-in history stack.
 `<BrowserRouter window>` defaults to using the current document's `defaultView`, but it may also be used to track changes to another window's URL, in an `<iframe>`, for example.
```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { BrowserRouter } from "react-router-dom";

ReactDOM.render(
  <BrowserRouter>
	{/* The rest of your app goes here */}
  </BrowserRouter>,
  root
);
```
```ad-tip
We strongly recommend you do not use `HashRouter` unless you absolutely have to.
```
#### HashRouter
 `<HashRouter>` is for use in web browsers when the URL should not (or cannot) be sent to the server for some reason. This may happen in some shared hosting scenarios where you do not have full control over the server. In these situations, `<HashRouter>` makes it possible to store the current location in the `hash` portion of the current URL, so it is never sent to the server.

 `<HashRouter window>` defaults to using the current [document's `defaultView`](https://developer.mozilla.org/en-US/docs/Web/API/Document/defaultView), but it may also be used to track changes to another window's URL, in an `<iframe>`, for example.

### Defining Routes
Routing is the process of deciding which React elements will be rendered on a given page of your app, and how they will be nested. React Router provides two interfaces for declaring your routes.

+ `<Routes>`and `<Route>` if you're using JSX
+ `useRoutes` if you'd prefer a JavaScript object-based config

[[router-v6-detailed#Routing|more details]]

#### Routes And Route
 `<Routes>` and `<Route>` are the primary ways to render something in React Router based on the current [`location`](https://reactrouter.com/en/v6.3.0/api#location). You can think about a `<Route>` kind of like an `if` statement; if its `path` matches the current URL, it renders its `element`! The `<Route caseSensitive>` prop determines if the matching should be done in a case-sensitive manner (defaults to `false`).

Whenever the location changes, `<Routes>` looks through all its `children` `<Route>` elements to find the best match and renders that branch of the UI. `<Route>` elements may be nested to indicate nested UI, which also correspond to nested URL paths. Parent routes render their child routes by rendering an [`<Outlet>`](https://reactrouter.com/en/v6.3.0/api#outlet).
```tsx
<Routes>
  <Route path="/" element={<Dashboard />}>
    <Route
      path="messages"
      element={<DashboardMessages />}
    />
    <Route path="tasks" element={<DashboardTasks />} />
  </Route>
  <Route path="about" element={<AboutPage />} />
</Routes>
```
The default `<Route element>` is an `<Outlet>`. This means the route will still render its children even without an explicit element prop, so you can nest route paths without nesting UI around the child route elements.

For example, in the following config the parent route renders an `<Outlet>` by default, so the child route will render without any surrounding UI. But the child route's path is /users/:id because it still builds on its parent.
#### useRoutes()
The useRoutes hook is the **functional equivalent** of `<Routes>`, but it uses JavaScript objects instead of 	 `<Route>` elements to define your routes. These objects have the same properties as normal `<Route> `elements, but they don't require JSX.
```tsx
import * as React from "react";
import { useRoutes } from "react-router-dom";

function App() {
  let element = useRoutes([
    {
      path: "/",
      element: <Dashboard />,
      children: [
        {
          path: "messages",
          element: <DashboardMessages />,
        },
        { path: "tasks", element: <DashboardTasks /> },
      ],
    },
    { path: "team", element: <AboutPage /> },
  ]);

  return element;
}
```
The return value of useRoutes is either a valid React element you can use to render the route tree, or null if nothing matched.
### Handling Navigation

-   [`<Link>`](https://reactrouter.com/en/v6.3.0/api#link) and [`<NavLink>`](https://reactrouter.com/en/v6.3.0/api#navlink) render an accessible `<a>` element, or a `TouchableHighlight` on React Native. This lets the user initiate navigation by clicking or tapping an element on the page.
-   [`useNavigate`](https://reactrouter.com/en/v6.3.0/api#usenavigate) and [`<Navigate>`](https://reactrouter.com/en/v6.3.0/api#navigate) let you programmatically navigate, usually in an event handler or in response to some change in state

#### Link
A `<Link>` is an element that lets the user **navigate to another page** by clicking or tapping on it. In `react- router-dom`, a `<Link>` renders an accessible `<a>` element with a real `href` that points to the resource it's linking to. This means that things like right-clicking a `<Link>` work as you'd expect. You can use `<Link reloadDocument>` to skip client side routing and let the browser handle the transition normally (as if it were an `<a href>`).
```tsx
import * as React from "react";
import { Link } from "react-router-dom";

function UsersIndexPage({ users }) {
  return (
    <div>
      <h1>Users</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            <Link to={user.id}>{user.name}</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}
```
A relative `<Link to>` value (that does not begin with `/`) resolves relative to the parent route, which means that it builds upon the URL path that was matched by the route that rendered that `<Link>`. It may contain `..` to link to routes further up the hierarchy. In these cases, `..` works exactly like the command-line `cd` function; each `..` removes one segment of the parent path.
```ad-note
`<Link to>` with a `..` behaves differently from a normal `<a href>` when the current URL ends with `/`. `<Link to>` ignores the trailing slash, and removes one URL segment for each `..`. But an `<a href>` value handles `..` differently when the current URL ends with `/` vs when it does not.
```

#### NavLink
A `<NavLink>` is a special kind of [`<Link>`](https://reactrouter.com/en/v6.3.0/api#link) that knows whether or not it is "**active**". This is useful when building a navigation menu such as a **breadcrumb** or **a set of tabs** where you'd like to show which of them is currently selected. It also provides useful context for assistive technology like screen readers.

By default, an `active` class is added to a `<NavLink>` component when it is active. This provides the same simple styling mechanism for most users who are upgrading from v5. One difference as of `v6.0.0-beta.3` is that  `activeClassName` and `activeStyle` have been removed from `NavLinkProps`. Instead, you can pass a function to either `style` or `className` that will allow you to customize the inline styling or the class string based on the component's active state. you can also pass a function as children to customize the content of the `<NavLink>` component based on their active state, specially useful to change styles on internal elements.

```tsx
import * as React from "react";
import { NavLink } from "react-router-dom";
function NavList() {
  // This styling will be applied to a <NavLink> when the
  // route that it links to is currently selected.
  let activeStyle = {
    textDecoration: "underline"
  };

  let activeClassName = "underline"

  return (
    <nav>
      <ul>
        <li>
          <NavLink
            to="messages"
            style={({ isActive }) =>
              isActive ? activeStyle : undefined
            }
          >
            Messages
          </NavLink>
        </li>
        <li>
          <NavLink
            to="tasks"
            className={({ isActive }) =>
              isActive ? activeClassName : undefined
            }
          >
            Tasks
          </NavLink>
        </li>
        <li>
          <NavLink
            to="tasks"
          >
            {({ isActive }) => (
              <span className={isActive ? activeClassName : undefined}>
                Tasks
              </span>
            ))}
          </NavLink>
        </li>
      </ul>
    </nav>
  );
}
```

#### Navigate

A `<Navigate>` element changes the current location **when it is rendered**. It's a component wrapper around [`useNavigate`](https://reactrouter.com/en/v6.3.0/api#usenavigate), and accepts all the same arguments as props.

```tsx
import * as React from "react";
import { Navigate } from "react-router-dom";

class LoginForm extends React.Component {
  state = { user: null, error: null };

  async handleSubmit(event) {
    event.preventDefault();
    try {
      let user = await login(event.target);
      this.setState({ user });
    } catch (error) {
      this.setState({ error });
    }
  }

  render() {
    let { user, error } = this.state;
    return (
      <div>
        {error && <p>{error.message}</p>}
        {user && (
          <Navigate to="/dashboard" replace={true} />
        )}
        <form
          onSubmit={(event) => this.handleSubmit(event)}
        >
          <input type="text" name="username" />
          <input type="password" name="password" />
        </form>
      </div>
    );
  }
}
```

#### useNavigate()
The `useNavigate` hook returns a function that lets you navigate programmatically, for example after a form is submitted.

```tsx
import { useNavigate } from "react-router-dom";

function SignupForm() {
  let navigate = useNavigate();

  async function handleSubmit(event) {
    event.preventDefault();
    await submitForm(event.target);
    navigate("../success", { replace: true });
  }

  return <form onSubmit={handleSubmit}>{/* ... */}</form>;
}
```

The `navigate` function has two signatures:
-   Either pass a `To` value (same type as `<Link to>`) with an optional second `{ replace, state }` arg or
-   Pass the delta you want to go in the history stack. For example, `navigate(-1)` is equivalent to hitting the back button.


## Advanced Route Definitions
This is where React Router really gets interesting. There is a lot of cool stuff you can do with routing to make more complex routes, easier to read, and overall much more functional. 

### Index Routes
Index routes can be thought of as "default child routes". When a parent route has multiple children, but the URL is just at the parent's path, you probably want to render something into the outlet.
```tsx
function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Activity />} />
        <Route path="invoices" element={<Invoices />} />
        <Route path="activity" element={<Activity />} />
      </Route>
    </Routes>
  );
}
```
Now at "/" the `<Activity>` element will render inside the outlet.
You can have an index route at any level of the route hierarchy that will render when the parent matches but none of its other children do.

### NotFound Routes
When no other route matches the URL, you can render a "not found" route using `path="*"`. This route will match any URL, but will have the weakest precedence so the router will only pick it if no other routes match.
```tsx
function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="dashboard" element={<Dashboard />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```



### Dynamic Routing

```tsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/books" element={<BookList />} />
  <Route path="/books/:id" element={<Book />} />
</Routes>
```
The final route in the above example is a dynamic route that has a dynamic parameter of :id. Defining dynamic routes in React Router is as simple as putting a colon in front of whatever you want the dynamic part of your route to be. In our case *our dynamic route will match any URL that starts with `/book` and ends with some value.* For example, `/books/1`, `/books/bookName`, and `/books/literally-anything` will all match our dynamic route.

Pretty much always when you have a dynamic route like this you want to access the dynamic value in your custom component which is where the *useParams* hook comes in.
#### useParams()
The `useParams` hook returns an object of key/value pairs of the dynamic params from the current URL that were matched by the `<Route path>`. Child routes inherit all params from their parent routes.
```tsx
import * as React from 'react';
import { Routes, Route, useParams } from 'react-router-dom';

function ProfilePage() {
  // Get the userId param from the URL.
  let { userId } = useParams();
  // ...
}

function App() {
  return (
    <Routes>
      <Route path="users">
        <Route path=":userId" element={<ProfilePage />} />
        <Route path="me" element={...} />
      </Route>
    </Routes>
  );
}
```
### Routing Priority

```tsx
<Route path="teams/:teamId" element={<Team />} />
<Route path="teams/new" element={<NewTeamForm />} />
```

In previous versions of React Router you had to order your routes a certain way to get the right one to render when multiple routes matched an ambiguous URL. V6 is a lot smarter and *will pick the most specific match* so you don't have to worry about that anymore. For example, the URL `/teams/new `matches both of these route,But `teams/new` is a more specific match than `/teams/:teamId`, so `<NewTeamForm />` will render.

### Nested Routes
This is one of the most powerful features of React Router making it so you don't have to mess around with complicated layout code. The vast majority of your layouts are coupled to segments of the URL and React Router embraces this fully.
Routes can be nested inside one another, and their paths will nest too (child inheriting the parent).
```tsx
function App() {
  return (
    <Routes>
      <Route path="invoices" element={<Invoices />}>
        <Route path=":invoiceId" element={<Invoice />} />
        <Route path="sent" element={<SentInvoices />} />
      </Route>
    </Routes>
  );
}
```
This route config defined three route paths:
+ "/invoices"
+ "/invoices/sent"
+ "/invoices/:invoiceId"
Notice the inner component that changed with the URL (`<SentInvoices>` and `<Invoice>`). The parent route (`<Invoices>`) is responsible for making sure the matching child route is rendered with `<Outlet>`.
#### Outlet
An `<Outlet>` should be used in parent route elements to render their child route elements. This allows nested UI to show up when child routes are rendered. If the parent route matched exactly, it will render a child index route or nothing if there is no index route.
```tsx
import { Routes, Route, Outlet } from "react-router-dom";

function App() {
  return (
    <Routes>
      <Route path="invoices" element={<Invoices />}>
        <Route path=":invoiceId" element={<Invoice />} />
        <Route path="sent" element={<SentInvoices />} />
      </Route>
    </Routes>
  );
}

function Invoices() {
  return (
    <div>
      <h1>Invoices</h1>
      <Outlet />//when there is a nested route,it will render to be nested router Element
    </div>
  );
}
```
#### Shared Layouts
1. 
```tsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/books" element={<BooksLayout />}>
    <Route index element={<BookList />} />
    <Route path=":id" element={<Book />} />
    <Route path="new" element={<NewBook />} />
  </Route>
  <Route path="*" element={<NotFound />} />
</Routes>
```
2. 
```tsx
<Route element={<OtherLayout />}>
    <Route path="/contact" element={<Contact />} />
    <Route path="/about" element={<About />} />
  </Route>
```
#### useOutletContext()
Often parent routes manage state or other values you want shared with child routes. You can create your own context provider if you like, but this is such a common situation that it's built-into `<Outlet />`:
```tsx 
function Parent() {
  const [count, setCount] = React.useState(0);
  return <Outlet context={[count, setCount]} />;
}
```
```tsx
import { useOutletContext } from "react-router-dom";
function Child() {
  const [count, setCount] = useOutletContext();
  const increment = () => setCount((c) => c + 1);
  return <button onClick={increment}>{count}</button>;
}
```
If you're using TypeScript, we recommend t**he parent component provide a custom hook for accessing the context value**. This makes it easier for consumers to get nice typings, control consumers, and know who's consuming the context value. Here's a more realistic example:
```tsx
import * as React from "react";
import type { User } from "./types";
import { Outlet, useOutletContext } from "react-router-dom";

type ContextType = { user: User | null };

export default function Dashboard() {
  const [user, setUser] = React.useState<User | null>(null);

  return (
    <div>
      <h1>Dashboard</h1>
      <Outlet context={{ user }} />
    </div>
  );
}

export function useUser() {
  return useOutletContext<ContextType>();
}
```
```tsx
import { useUser } from "../dashboard";

export default function DashboardMessages() {
  const { user } = useUser();
  return (
    <div>
      <h2>Messages</h2>
      <p>Hello, {user.name}!</p>
    </div>
  );
}
```
### Multiple Routes

Although you should only ever have a single `<Router>` in an app, you may have as many [`<Routes>`](https://reactrouter.com/en/v6.3.0/api.md#routes) as you need, wherever you need them. Each `<Routes>` element operates independently of the others and picks a child route to render.
```tsx
function App() {
  return (
    <div>
      <Sidebar>
        <Routes>
          <Route path="/" element={<MainNav />} />
          <Route
            path="dashboard"
            element={<DashboardNav />}
          />
        </Routes>
      </Sidebar>

      <MainContent>
        <Routes>
          <Route path="/" element={<Home />}>
            <Route path="about" element={<About />} />
            <Route path="support" element={<Support />} />
          </Route>
          <Route path="dashboard" element={<Dashboard />}>
            <Route path="invoices" element={<Invoices />} />
            <Route path="team" element={<Team />} />
          </Route>
          <Route path="*" element={<NotFound />} />
        </Routes>
      </MainContent>
    </div>
  );
}
```



equivalent 等效的
assistive 辅助的

mechanism 机制