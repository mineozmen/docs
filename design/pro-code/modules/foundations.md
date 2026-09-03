---
description: Core react, redux and router features can be imported directly
---

# Foundations

## React

The only module where named imports work.

```jsx
import React, { useState, useEffect, useMemo, useCallback, useRef, useContext } from "react";
```

You get the application's own React instance. That is deliberate: it is what lets your component sit inside the platform's contexts and use hooks without the "invalid hook call / two copies of React" class of failures.

The full React 18 surface is available. See the list in Imports & rules.

`react-dom` is not available. You should not need it: render through JSX and let the platform mount your component.

***

## Redux

```jsx
import ReactRedux from "react-redux";
const { useSelector, useDispatch, useStore, batch, shallowEqual } = ReactRedux;
```

Your component already sits inside the application's Redux provider, so these hooks read the real store.

You will rarely need them. Lists and editors already receive `dispatch` as a prop and data access should go through the query hooks in `store` rather than through `useSelector` over raw state.

***

## Router

```jsx
import nextrouter from "next/router";
const useRouter = nextrouter.useRouter;
const Router = nextrouter.default;      // the imperative router singleton
const withRouter = nextrouter.withRouter;
```

Note the shape: `useRouter` is a property and the imperative router is at `.default`.

```jsx
const router = useRouter();

router.query;      // parsed query object
router.asPath;     // "/app/foo/common/bar?id=123"
router.push({ query: { ...router.query, id } });
router.replace({ query: nextQuery }, undefined, { shallow: true });
```

#### Use the router to read, use props to write

The platform already keeps the URL in sync with the selected record: `onSelect` updates `?id=` and `onRoute` writes filters and paging into the query string. Driving navigation yourself on top of that causes the two to fight.

So: read state from the router, write it through the props.

#### Reading the query string reliably

`router.query` can lag behind on shallow updates. The reliable pattern is to parse `asPath`:

```jsx
const [, currentQueryString = ""] = (router.asPath || "").split("?");
const params = new URLSearchParams(currentQueryString);
const activeId = params.get("id");
```
