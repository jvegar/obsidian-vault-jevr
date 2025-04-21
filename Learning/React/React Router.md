React Router is a multi-strategy router for React bridging the gap from React 18 to React 19. You can use it maximally as a React framework or as minimally as you want.

## Getting Started
There are three primary ways, or "modes", to use it in your app, so there are three guides to get you started.

- [Declarative](https://reactrouter.com/start/declarative/installation)
- [Data](https://reactrouter.com/start/data/custom)
- [Framework](https://reactrouter.com/start/framework/installation)

# Picking a Mode

React Router is a multi-strategy router for React. There are three primary ways, or "modes", to use it in your app. Across the docs you'll see these icons indicating which mode the content is relevant to:
![[Pasted image 20250329100913.png]]
The features available in each mode are additive, so moving from Declarative to Data to Framework simply adds more features at the cost of architectural control. So pick your mode based on how much control or how much help you want from React Router.

The mode depends on which "top level" router API you're using:

**Declarative**
Declarative mode enables basic routing features like matching URLs to components, navigating around the app, and providing active states with APIs like `<Link>`, `useNavigate`, and `useLocation`.

```jsx
import { BrowserRouter } from "react-router"; 
ReactDOM.createRoot(root).render( <BrowserRouter> <App /> </BrowserRouter> );
```

**Data**
By moving route configuration outside of React rendering, Data Mode adds data loading, actions, pending states and more with APIs like `loader`, `action`, and `useFetcher`.

```jsx
import {
  createBrowserRouter,
  RouterProvider,
} from "react-router";

let router = createBrowserRouter([
  {
    path: "/",
    Component: Root,
    loader: loadRootData,
  },
]);

ReactDOM.createRoot(root).render(
  <RouterProvider router={router} />
);

```

**Framework**
Framework Mode wraps Data Mode with a Vite plugin to add the full React Router experience with:

- typesafe `href`
- typesafe Route Module API
- intelligent code splitting
- SPA, SSR, and static rendering strategies
- and more