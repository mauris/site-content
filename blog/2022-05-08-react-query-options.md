---
title: "React-Query: abstracting query options"
date: "2022-05-08"
coverImage: https://images.unsplash.com/photo-1515674447568-09bbb507b96c?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&dl=possessed-photography-Xlx80tr5bEE-unsplash.jpg&w=1920
coverImageCaption: Photo by [Possessed Photography](https://unsplash.com/@possessedphotography) on Unsplash
---

I've found [react-query](https://react-query.tanstack.com/) to be pleasantly straightforward and easy to use in managing server-side state on React and React Native apps, with plenty of support and resources from the likes of [TkDodo's blog](https://tkdodo.eu/blog/practical-react-query) and [online courses](https://ui.dev/react-query?coupon_code=aQcnd).

React Query deals with, you guessed it, [queries](https://react-query.tanstack.com/guides/queries). As you build up your React application, you will find yourself facing some of these scenarios:

- you have queries that execute depending on the data returned from an earlier query: you can run conditional queries like in the guide on [Dependent Queries](https://react-query.tanstack.com/guides/dependent-queries).
- you have queries that can run in parallel: [`useQueries()`](https://react-query.tanstack.com/guides/parallel-queries) is there to solve parallel queries
- you have the same query that will be requested by multiple components in the same render cycle: It's more favourable to request for the same query multiple times than to perform [props drilling](https://blogs.perficient.com/2021/12/03/understanding-react-context-and-property-prop-drilling/), as you can configure [`staleTime`](https://react-query.tanstack.com/guides/important-defaults) to your needs

TkDodo has written about [creating custom hooks](https://tkdodo.eu/blog/practical-react-query#create-custom-hooks). Using custom hooks makes reading a component's code a breeze, because `useQuery` and how data is fetched gets colocated together in a custom hook. For example, a custom hook to fetch the list of todo items:

```typescript
export const useTodoListQuery = (filter: Filter = {}) =>
  useQuery(["todos", filter], () => fetchTodoList(filter));
```

As I encountered more of those scenarios I described earlier, I found the need to reuse queries written in `useQuery` to also be used in `useQueries` for parallel execution. To make things more manageable, I decided to write utility function for constructing query options:

```typescript
export const buildTodoListQueryOptions = (filter: Filter) => ({
  queryKey: ["todos", filter],
  queryFn: () => fetchTodoList(filter),
});

export const useTodoListQuery = (filter: Filter = {}) =>
  useQuery(buildTodoListQueryOptions(filter));
```

In the event that I need to build a portion of User Interface that needed to run the `useTodoListQuery` in parallel with something else, say loading calendar items, I can do this:

```typescript
export const buildTodoListQueryOptions = (filter: Filter) => ({
  // ...
});

export const useTodoListQuery = // ...

export const buildCalendarItemsQueryOptions = (dateRange: DateRange) => ({
  queryKey: ["calendar", dateRange],
  queryFn: () => fetchCalendarItems(dateRange),
});

export const useCalendarItemsQuery = // ...

export const useLandingInfoQuery = (
  dateRange: DateRange,
  filter: Filter = {}
) =>
  useQueries([
    buildCalendarItemsQueryOptions(dateRange),
    buildTodoListQueryOptions(filter),
  ]);
```

This makes it easier to build composable queries. That said, I don't think everyone should start going around and pick out their query options from whereever `useQuery` appeared. The abstraction should only occur when there is a good enough reason to do so, like when a particular query needs to be used in multiple instances of `useQuery` and `useQueries` to enhance user experience and loading time.

What do you think?
