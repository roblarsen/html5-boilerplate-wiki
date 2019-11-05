# Offline support

There are a couple of interesting techniques that can be used to create web sites and applications that continue to work even if your internet connection goes offline. Making use of these techniques may also result in performance improvements.

The [HTML5 Rocks Offline Tutorial](http://www.html5rocks.com/en/tutorials/offline/whats-offline/) lists the following technologies:

* Application Cache
* `localStorage` and `sessionStorage`
* Web SQL Database (deprecated) & Indexed Database API
* Online/Offline Events

## Application Cache

As mentioned in [A Beginner’s Guide to Using the Application Cache](http://www.html5rocks.com/en/tutorials/appcache/beginner/), using the appcache interface gives your application three advantages:

* Offline browsing: users can navigate your full site when they’re offline;
* Speed: cached resources are local, and therefore load faster;
* Reduced server load: the browser will only download resources from the server that have changed.

The Application Cache (or AppCache) allows a developer to specify which files the browser should cache and make available to offline users. Your app will load and work correctly, even if the user presses the refresh button while they’re offline.

However, using Application Cache is usually more complex than it initially seems. Find out about the [pitfalls of working with Application Cache](http://www.alistapart.com/articles/application-cache-is-a-douchebag/)
