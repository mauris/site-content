---
title: Next.js + TypeScript in a Monorepo
date: '2022-03-31'
coverImage: https://images.unsplash.com/photo-1606390311490-96189b1504bd?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&dl=federica-bisso-Vif-nHWdiPE-unsplash.jpg&w=1920
coverImageCaption: Photo by [Federica Bisso](https://unsplash.com/@fedebisso) on Unsplash
---

It's been many years since I last blogged anything, especially on programming, software engineering and tech.

Watching [Yang Shun](https://github.com/yangshun)'s GitHub activities, I couldn't help but notice that he's been dabbling with _Next.js_ - a name that I came across occasionally but never really looked into seriously. Since I had some time on hand this week, I took the opportunity to read up and understand what Next.js was trying to achieve.

# Solving Common React Pain Points

As I dug, it turned out, Next.js was already solving common problems that I often encounter when building React apps:

- Deciding how files, pages, components are structured in the project folder
- Deciding how routing is done (despite everyone's using `react-router`, there's variance in flavour and how routers are structured)
- Deciding how code is transpiled (e.g. configuring Webpack, TypeScript etc.)
- Figuring how to do Server-Side Rendering (SSR) right
- Figuring how to get asset optimization done right

Next.js, as a framework, has structured and simplified the development workflow to get web developers straight into building the app.

# Learning Next.js

For me, the [tutorial](https://nextjs.org/learn/foundations/from-react-to-nextjs) was easy to follow as I had some background in React. If you only have experience in vanilla JavaScript, the [tutorial comes with steps](https://nextjs.org/learn/foundations/from-javascript-to-react) to help you get up to speed.

Once I was through with the tutorial, I figured that I'll try out Next.js by creating a blog that's similar to the tutorial, yet incorporating some of the niceties I'm familiar with, like:

- [Yarn Monorepo](https://semaphoreci.com/blog/javascript-monorepos-yarn-workspaces) - breaking down apps into reusable and composable packages sparks joy. In my blog's monorepo, I have two types of workspaces: `apps` and `libs`
- [TypeScript](https://nextjs.org/learn/excel/typescript) - I find typings help with santiy checks and sometimes make code easier to read
- [Tailwind CSS](https://tailwindcss.com/docs/guides/nextjs) - I could have gone with [MUI](https://mui.com/), but at this point I find it simpler to just have utility CSS classes rather than importing a full blown component library. I also like how Tailwind CSS makes it easy to incorporate dark/light theming
- [Sass](https://nextjs.org/docs/basic-features/built-in-css-support#sass-support) - expressive CSS rules, which goes hand in hand with CSS Modules in Next.js

# Additional Resources

After spending about 3 days on it, I was able to piece together [the blog](https://github.com/mauris/mauris.github.io) you see here, with much thanks to the following resources:

- The full tutorial to create a Next.js app: <https://nextjs.org/learn/basics/create-nextjs-app>
- An example of how to setup a monorepo with Next.js: <https://github.com/belgattitude/nextjs-monorepo-example>
- [Julia](https://bionicjulia.com/) gave some guidelines on setup (picked up `react-markdown` and `react-syntax-highlighter` from here, without having to explicitly write code to parse Markdown using `remark`): <https://bionicjulia.com/blog/setting-up-nextjs-markdown-blog-with-typescript>
- Guide on using GitHub Pages to host the exported Next.js app as a static webapp: <https://wallis.dev/blog/deploying-a-next-js-app-to-github-pages>
- Using `next-transpile-modules` to ensure workspace dependency packages/libraries get transpiled properly: <https://www.npmjs.com/package/next-transpile-modules>
- Using GitHub Actions to magically update GitHub Pages `gh-pages` branch whenever there are changes made on the `main` branch: <https://gregrickaby.blog/article/nextjs-github-pages>

# Conclusion

I must say it was quite a joy to work with Next.js and it took away some cognitive load I mentioned earlier. With that, I might continue working on more features for this blog (e.g. pagination, search) in my free time. Hopefully this will encourage me to write and reflect more upon my software engineering career and journey.
