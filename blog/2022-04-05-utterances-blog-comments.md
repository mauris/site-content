---
title: "Utterances: Blog Comments with GitHub Issues"
date: "2022-04-05"
coverImage: https://images.unsplash.com/photo-1648598037844-84ca8cb3bc88?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&dl=davis-patton-qkoLsnNsJMg-unsplash.jpg&w=1920
coverImageCaption: Photo by [Davis Patton](https://unsplash.com/@davispatton) on Unsplash
tags:
  - utterances
  - giscus
---

Awhile back when I was reading up on [TkDodo's commentaries of react-query](https://tkdodo.eu/blog/practical-react-query), I noticed that TkDodo's blog has a nice comments section that looked very familiar. Upon closer inspection, I realised it was a widget called [Utterances](https://utteranc.es/), which allowed comments to be stored on a GitHub repository's issues.

This was neat for a fully static blog like mine, where I just don't want to host a database somewhere just to store comments. So I decided to try adding it to my built-time generated blog.

The setup was straightforward, I simply fill up the form in the `configuration` section, and it populates my input into the `<script>` tag below. I thought it would be a straightforward copy-paste, but TypeScript came complaining about `repo` being an invalid attribute for the `<script>` tag.

I looked around GitHub for React implementation or integration of Utterances, and found these two:

- <https://github.com/wicksome/utterances-react/blob/master/packages/component/src/Utterances.js>
- <https://github.com/b6pzeusbc54tvhw5jgpyw8pwz2x6gs/react-utterances/blob/master/packages/component/src/ReactUtterances.js>

So the trick was to dynamically assign the special attributes (like `repo`) to get by TypeScript complains for `<script>`. Along the way, I encountered the issue of the injected `<script>` tag being removed while the `<iframe>` tag as being injected asynchronously via `insertAdjacentHTML` (See [line 77](https://github.com/utterance/utterances/blob/36695bf89ffb2ea10eaa26f1b5cbc90477704b9f/src/client.ts) of the utterances source code).

My solution? To enable proper re-render of the Utterances widget, [I wrote a copy of the component](https://github.com/mauris/mauris.github.io/blob/main/apps/main/components/Utterances.tsx) taking references from the two other implementations mentioned above. My implementation takes care to track the number of script instances being loaded, and once the last instance has loaded, it will remove all duplicated instances, leaving only the last "up-to-date" instance in the DOM. This works neatly with changes in the site theme and current post.

I was quite pleased at how easy and straightforward Utterances can be integrated to provide comments and discussions into static sites. I hope to hear from people who's come by to read what I've written and give their thoughts too.

Thank you to the folks behind [utterances](https://github.com/utterance) for making the comments widget that made sense.
