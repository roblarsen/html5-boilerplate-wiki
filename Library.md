# Better web development

## HTML

* Learn HTML5 elements better: [HTML5 Doctor Glossary](http://html5doctor.com/element-index/) and [Dive Into HTML5 : semantic elements](http://diveintohtml5.info/semantics.html#new-elements)

* [HTML5 Layout with ARIA roles](http://pastie.org/877825) by Jonathan Neal

* [HTML5, ARIA Roles, and Screen Readers](http://www.accessibleculture.org/research/html5-aria/) for Web Accessibility 


## CSS

* [Color considerations for printing](http://www.merttol.com/articles/css/too-light-for-print.html)

* Using border-radius with a border in Webkit? Try [-webkit-background-clip](http://tumble.sneak.co.nz/post/928998513/fixing-the-background-bleed) to fix the bleed.

* Write [efficient CSS](http://css-tricks.com/efficiently-rendering-css/) for optimum browser performance.

* Know your available [browser CSS hacks](http://paulirish.com/2009/browser-specific-css-hacks).

* Style input placeholders with [vendor-prefixed extensions](http://stackoverflow.com/questions/2610497/change-an-inputs-html5-placeholder-color-with-css)

* If `hr` has a width, make sure you have `text-align: left` also set for consistent rendering across all browsers. IE8 renders the `hr` in the center while others align it to the left. (see [#297](https://github.com/h5bp/html5-boilerplate/issues/297))

* Consider less visually intense colors for text selection when the [window is inactive](http://css-tricks.com/window-inactive-styling/).

* Using `text-decoration: underline;` for anchors cause some issues specially for languages that have dots under some letters. Use `border-bottom: 1px solid currentcolor;` in such cases. Note: consider that `text-shadow` does not effect borders.

* Consider [not setting :hover states for touch devices](https://gist.github.com/1275792)

* Be aware of [issues with disabled field display in IE <= 9](http://tjvantoll.com/2012/03/17/Styling-Disabled-Form-Fields/) (see [#1022](https://github.com/h5bp/html5-boilerplate/pull/1022)).

### Working with CSS Media Queries

* [[Essential Considerations for Crafting Media Queries|http://zomigi.com/blog/essential-considerations-for-crafting-quality-media-queries/]]

* [[“Mobile first” CSS and getting Sass to help with legacy IE|http://nicolasgallagher.com/mobile-first-css-sass-and-ie/]]

* [[Community discussion about Mobile First development|https://github.com/h5bp/html5-boilerplate/issues/816]]

* [[Respond.js Media Query polyfill|https://github.com/scottjehl/Respond]]

## JavaScript

* [[Script Loading Techniques|script-loading-techniques]]

* [[The Essentials of Writing High Quality JavaScript|http://net.tutsplus.com/tutorials/javascript-ajax/the-essentials-of-writing-high-quality-javascript/]]

* [[JavaScript: The Good Parts|http://www.yuiblog.com/blog/2007/06/08/video-crockford-goodstuff/]]

* [[JavaScript: The Good Parts (Book)|http://oreilly.com/catalog/9780596517748]]

* [[JavaScript Patterns (Book)|http://oreilly.com/catalog/9780596806767]]

* [[Smooth JavaScript Animation|http://velocityjs.org]]


## Embedded media

* Things to consider while [[using PNG images|using-PNG]]


## Mobile

* [Reference Sheet for Mobile Platforms](https://github.com/h5bp/mobile-boilerplate/wiki/Mobile-Matrices)

* [45 most useful guidelines for mobile web design & development](http://www.mobilexweb.com/blog/guidelines-mobile-web-design)

**Gmail for Mobile HTML5 Series:**

1. <a href="http://googlecode.blogspot.com/2009/04/gmail-for-mobile-html5-series-using.html" target="_blank">Using AppCache to Launch Offline - Part 1</a><br />
2. <a href="http://googlecode.blogspot.com/2009/05/gmail-for-mobile-html5-series-part-2.html" target="_blank">Using AppCache to Launch Offline - Part 2</a><br />
3. <a href="http://googlecode.blogspot.com/2009/05/gmail-for-mobile-html5-series-part-3.html" target="_blank">Using AppCache to Launch Offline - Part 3</a><br />
4. <a href="http://googlecode.blogspot.com/2009/05/gmail-for-mobile-html5-series-common.html" target="_blank">A Common API for Web Storage</a><br />
5. <a href="http://googlecode.blogspot.com/2009/06/gmail-for-mobile-html5-series.html" target="_blank">Suggestions for Better Performance</a><br />
6. <a href="http://googlecode.blogspot.com/2009/06/gmail-for-mobile-html5-series-cache.html" target="_blank">Cache Pattern For Offline HTML5 Web Applications</a><br />
7. <a href="http://googlecode.blogspot.com/2009/07/gmail-for-mobile-html5-series-using.html" target="_blank">Using Timers Effectively</a><br />
8. <a href="http://googlecode.blogspot.com/2009/07/gmail-for-mobile-html5-series.html" target="_blank">Autogrowing Textareas</a><br />
9. <a href="http://googlecode.blogspot.com/2009/09/gmail-for-mobile-html5-series-reducing.html" target="_blank">Reducing Startup Latency</a><br />
10. <a href="http://googlecode.blogspot.com/2009/09/gmail-for-mobile-html5-series-css.html" target="_blank">CSS Transforms and Floaty Bars</a><br />

<a href="http://davidbcalhoun.com/" target="_blank">David B. Calhoun - Developer Blog</a> - David Calhoun, frontend engineer working for Yahoo! Mobile team.<br />
<a href="http://waynepan.com/" target="_blank">Wayne Pan</a> - Wayne Pan, engineer working for Google AdMob


## Misc

* [iOS fonts](http://www.iosfonts.com/)

* [Common fonts on desktop devices](http://www.codestyle.org/css/font-family/sampler-CombinedResults.shtml)

* [Building Better Font Stacks](http://www.codestyle.org/css/font-family/BuildBetterCSSFontStacks.shtml)

* Consider some opinionated [web do's and web dont's](http://webdosanddonts.com/)

* Get your team hooked using the same [front-end coding standards and best practices](http://molecularvoices.molecular.com/standards/).

* What should a developer know before building a public web site? [Stack Overflow](http://stackoverflow.com/questions/72394/what-should-a-developer-know-before-building-a-public-web-site) great summaries.
