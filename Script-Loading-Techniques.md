# Script loading techniques

Initially when JavaScript was only used as functions, best practice was to combine all your JavaScript and load from an external source. In the last few years, JavaScript is often used as framework that creates sites with heavy front-end scripting. To simply load everything once is no longer a one size fits all solution. This article talks about some basic script loading and coupling techniques.

[[http://lh3.ggpht.com/_Ivh-Q9a0IUw/TQ9kgpGRspI/AAAAAAAAAG0/7B_0xZKWib4/s576/chart.png]]

*Attribution: “Even Faster Web Sites, by Steve Souders. Copyright 2009 Steve Souders, 978-0-596-52230-8.”*

## Script Loading
Normally, most browsers download components in parallel, but that’s not the case for external scripts. When the browser starts downloading an external script, it won’t start any additional downloads until the script has been completely downloaded, parsed, and executed.

### Normal Script Src
The nice thing about loading the external script the normal way is that execution order is preserved across all browsers. The external script is downloaded, parsed, and executed. After that, the inlined code is parsed and executed. There are no race conditions.
<a href="http://stevesouders.com/cuzillion/?ex=10008&title=Scripts+Block+Downloads">Demo</a>

### XHR Eval
This technique loads scripts in parallel, but it doesn't preserve execution order. Therefore, the inlined code is executed before the external script finishes downloading. This results in an undefined symbol error because the inlined code references EFWS, which isn't yet defined. <a href="http://stevesouders.com/cuzillion/?ex=10009&title=Load+Scripts+using+XHR+Eval">Demo</a>

### XHR Injection
When the response is received, it's injected into the page by setting the text of a dynamically created script element. This technique loads scripts in parallel, but it doesn't preserve execution order. <a href="http://stevesouders.com/cuzillion/?ex=10015&title=XHR+Injection">Demo</a>

### Fetch Injection
When a `fetch` response is received, it's injected into the page by setting the text of a dynamically created script element. This technique loads scripts in parallel, and, unlike XHR Injection, may be used to preserve execution order. [Introduction](https://habd.as/post/managing-async-dependencies-javascript/). [Implementation](https://git.habd.as/jhabdas/fetch-inject/). [Demo](https://codepen.io/jhabdas/pen/MpVeOE?editors=0012)

### Script in Iframe
The script is loaded via an iframe. The external script's code is put inside an HTML document, and that HTML document is loaded as an iframe. This succeeds in loading the scripts in parallel, but, similar to the XHR techniques, execution order is not preserved. Therefore, the inlined code is executed before the external script finishes downloading. <a href="http://stevesouders.com/cuzillion/?ex=10012&title=Script+in+Iframe">Demo</a>

### Script DOM Element
This technique succeeds in loading the scripts in parallel across all major browsers, but the execution order is preserved only in Firefox and Opera. In other browsers, an undefined symbol error occurs because the inlined code references EFWS, which isn't yet defined. <a href="http://stevesouders.com/cuzillion/?ex=10010&title=Script+Dom+Element">Demo</a>

### Script Defer
The DEFER attribute is part of the HTML 4 specification, but it is only supported in IE, and even there execution order is not preserved, resulting in undefined symbol errors. In other browsers, it will have no effect and the external script will still block downloads and rendering in the page. <a href="http://stevesouders.com/cuzillion/?ex=10013&title=Script+Defer">Demo</a>

### document.write Script Tag
This technique successfully loads scripts in parallel in IE, Safari 4, and Opera, and it preserves execution order. But it has some drawbacks. Although scripts are loaded in parallel, in IE and Opera other types of resources (images, stylesheets, etc.) are blocked from downloading, elements in the page below the scripts are blocked from rendering, and it doesn't preserve execution order. <a href="http://stevesouders.com/cuzillion/?ex=10014&title=document.write+Script+Tag">Demo</a>

## Coupling
When you use race conditions with inline script, there might be a problem. This technique loads scripts in parallel, but it doesn't preserve execution order. Therefore, the inlined code is executed before the external script finishes downloading. This results in an undefined symbol error because the inlined code references to the objects defined in external script, which aren't yet defined. Below are some of the coupling techniques:

### Script Onload
This works across browsers. It causes the inlined code to be invoked as soon as possible after the external script is done loading. It has minimal overhead. <a href="http://stevesouders.com/efws/script-onload.php?t=1292857258">Demo</a>
