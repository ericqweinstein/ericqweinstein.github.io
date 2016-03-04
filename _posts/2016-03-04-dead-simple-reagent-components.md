---
layout: post
title: Dead Simple Reagent Components
date: 2016-03-04 14:00:00
summary: Creating UI components with Reagent.
categories: programming clojure clojurescript reagent react
---

I've been using [ClojureScript](https://github.com/clojure/clojurescript) + [Reagent](https://github.com/reagent-project/reagent) for a couple of years now, and I've found the combination (with [Figwheel](https://github.com/bhauman/lein-figwheel) for live reloading) to be absolutely delightful. It's been in production for the [CityShelf web](http://www.cityshelf.com) front end since May of 2015, and I've been continually impressed at how simple it is to write small, composable components that lead to code that's easy to reason about and maintain.

A good example is the component CityShelf uses to wrap `<img />`s. The search API returns whatever cover image the bookstores in a given city provide, and occasionally these images are broken or missing. We felt it was a bad experience to pass a broken image along to CityShelf users, so I decided to make a lightweight wrapper around image tags to include additional behavior: specifically, an `onError` handler to swap in our preferred default book image.

The canonical JavaScript + jQuery solution to the problem looks something like this:

```js
function handleImageError(img) {
  img.onerror = '';
  img.src = '/path/to/default/image.jpg';
  return true;
}
```

So you could do the following in your markup:

```html
<img src="image.jpg" onerror="handleImageError(this);" />
```

This may not immediately seem suboptimal as compared to ClojureScript: JavaScript (and therefore ClojureScript) is single-threaded, and the [lack of STM in ClojureScript](https://en.wikipedia.org/wiki/Software_transactional_memory) means that state mutation isn't really safer (read: transactional) as compared to plain JS. However, I think there are several disadvantages to the JavaScript approach:

1. **Transient, mutable data structures**. The `HTMLImageElement` we're passing into `handleImageError()` is modified directly; we have no access to previous versions of the data structure. The ClojureScript implementation (see below) accepts a map of the form `{:src (reagent/atom "image.jpg")}` and returns a *new map* with the modified `:src`; because ClojureScript data structures are [persistent](https://en.wikipedia.org/wiki/Persistent_data_structure), we only need to allocate space commensurate with the changed data (in this case, the new `src` attribute). POJOs are neither immutable nor persistent.
2. **Trivialized state mutation**. We so cavalierly mutate state in JavaScript that nothing about the language causes alarm bells to ring in our heads when we're potentially doing something bad (*i.e.* mutating state and passing it around). In this case we're not necessarily passing anything around, but ClojureScript's `atom`s do make us (or at least, me) pause and think about what we're doing.
3. **Broken encapsulation**. There's an argument to be made here for separation of concerns, but I think separating markup and behavior via templating is actually an antipattern: in this case, we have to remember to set the inline `onerror` handler for every `<img />`. Reagent + ClojureScript (or plain [React](https://facebook.github.io/react/) + [JSX](https://facebook.github.io/react/docs/jsx-in-depth.html)) has moved us closer to [web components](http://webcomponents.org/), which I ultimately think are the Right Way™ to think about and write web applications.

The ClojureScript + Reagent component (which I think addresses the above concerns nicely) looks like this:

```cljs
(ns ^{:doc "Image component."
      :author "Eric Weinstein <eric.q.weinstein@gmail.com>"}
  cityshelf.components.image)

(defn image
  "Image component. Takes a map of options and creates an
  <img /> from the supplied :src; if the source is broken
  or missing, swaps in a default book image instead.

  Example:

  [image {:src (reagent/atom \"book-cover.jpg\")}]"
  [options]
  (let [src (:src options)]
    [:img {:className (:className options)
           :src @src
           :onError #(reset! src "/img/defaultbook_green.png")}]))
```

The production code actually works slightly differently than this&mdash;it uses a helper function to keep track of the root URL so we always serve the default book image correctly.

The nice thing about this component is that it can be used all over the application&mdash;for instance, we use it both for the search results gallery view and the individual product detail view. Even better, we could use it in a host of other scenarios where a default image is needed, since it would be trivial to parameterize the component with a custom default image. You can also include other default behaviors that you might want for all images, such as lazy loading, but it's important to remember not to try to do too much in your components. The more they do, the more likely they are to become special snowflakes that never see significant reuse.
