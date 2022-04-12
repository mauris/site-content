---
title: "Giscus: Blog Comments with GitHub Discussions"
date: "2022-04-11"
coverImage: https://images.unsplash.com/photo-1648819955157-a9a96e307d56?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&dl=siarhei-palishchuk-um_LMDP8pSY-unsplash.jpg&w=1920
coverImageCaption: Photo by [Siarhei Palishchuk](https://unsplash.com/@smeshny) on Unsplash
tags:
  - utterances
  - giscus
---

Last week, I integrated [Utterances](https://utteranc.es) to my blog over a couple of hours and shortly after integration, I found out about [Giscus](https://giscus.app/) and [laymonage](https://github.com/laymonage) came over to tell me that Giscus comes "with a ready-to-use [component library](https://github.com/giscus/giscus-component)".

Being "heavily inspired by utterances", Giscus is similar in many aspects (client-side integration, custom themes, reactions etc.) leveraged on [GitHub Discussions](https://docs.github.com/en/discussions) to host comments rather than GitHub Issues. Since I was familiar with how Utterances worked by then, I was able to quickly switch over to Giscus in an hour or so. Additionally, Giscus allows you to:

- add reactions to the page itself - these are stored on the discussion original post and can be disabled by option
- replies to comments, nesting - this is a feature I understand to be not supported by GitHub Issues
- sorting of comments by oldest/newest
- configuring if the comment box should be placed above all comments - I preferred not to scroll through the long list of comments before adding my comment, though this is purely the user's preference.

One thing I was facinated about, was that the comments widget did not reload fully when themes changed. I dug into the component library's source code and found that Giscus's component library leverages on [message passing to the iframe](https://github.com/giscus/giscus-component/blob/main/web/src/giscus.ts#L202) to update changes occurred in the component props. When using Utterances, any change in the props would result in a full reload of the script and iframe.

![](https://raw.githubusercontent.com/mauris/site-content/main/assets/d3c0caadbf.gif)

Giscus worked nicely, and I guess it's here to stay on my blog. Kudos to the Giscus team for building on the goodness that Utterances brought about.
