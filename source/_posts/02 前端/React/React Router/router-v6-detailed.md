
## SetUp
-   [`<BrowserRouter>`](https://reactrouter.com/en/v6.3.0/api#browserrouter) or [`<HashRouter>`](https://reactrouter.com/en/v6.3.0/api#hashrouter) should be used when running in a web browser (which one you pick depends on the style of URL you prefer or need)
-   [`<StaticRouter>`](https://reactrouter.com/en/v6.3.0/api#staticrouter) should be used when server-rendering a website
-   [`<NativeRouter>`](https://reactrouter.com/en/v6.3.0/api#nativerouter) should be used in [React Native](https://reactnative.dev/) apps
-   [`<MemoryRouter>`](https://reactrouter.com/en/v6.3.0/api#memoryrouter) is useful in testing scenarios and as a reference implementation for the other routers
-   [`<unstable_HistoryRouter>`](https://reactrouter.com/en/v6.3.0/api#unstable_historyrouter) is used with your own [`history`](https://github.com/remix-run/history) instance.

## Routing

-   [`matchPath`](https://reactrouter.com/en/v6.3.0/api#matchpath) - matches a path pattern against a URL pathname
-   [`matchRoutes`](https://reactrouter.com/en/v6.3.0/api#matchroutes) - matches a set of routes against a [location](https://reactrouter.com/en/v6.3.0/api#location)
-   [`createRoutesFromChildren`](https://reactrouter.com/en/v6.3.0/api#createroutesfromchildren) - creates a route config from a set of React elements (i.e. [`<Route>`](https://reactrouter.com/en/v6.3.0/api#routes-and-route) elements)

## Navigation

-   [`useResolvedPath`](https://reactrouter.com/en/v6.3.0/api#useresolvedpath) - resolves a relative path against the current [location](https://reactrouter.com/en/v6.3.0/api#location)
-   [`useHref`](https://reactrouter.com/en/v6.3.0/api#usehref) - resolves a relative path suitable for use as a `<a href>`
-   [`useLocation`](https://reactrouter.com/en/v6.3.0/api#uselocation) and [`useNavigationType`](https://reactrouter.com/en/v6.3.0/api#usenavigationtype) - these describe the current [location](https://reactrouter.com/en/v6.3.0/api#location) and how we got there
-   [`useLinkClickHandler`](https://reactrouter.com/en/v6.3.0/api#uselinkclickhandler) - returns an event handler to for navigation when building a custom `<Link>` in `react-router-dom`
-   [`useLinkPressHandler`](https://reactrouter.com/en/v6.3.0/api#uselinkpresshandler) - returns an event handler to for navigation when building a custom `<Link>` in `react-router-native`
-   [`resolvePath`](https://reactrouter.com/en/v6.3.0/api#resolvepath) - resolves a relative path against a given URL pathname

