# PWA-Workshop
### Reference code for PWA event

# First Step 

* https://darksky.net/dev
* search on glitch searchbar "pwa-init"
* Remix with that
* Add dark sky api key ""

# Second Step

* Manifest.json include
```
<link rel="manifest" href="/manifest.json">
```

* Look at Audits

* IOS meta tag
```
   <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black">
  <meta name="apple-mobile-web-app-title" content="Weather PWA">
  <link rel="apple-touch-icon" href="/images/icons/icon-152x152.png">
```
* Description and theme color add
```
<meta name="description" content="A sample weather app">
<meta name="theme-color" content="#2F3BA2" />

```

# Third Step (Offline experience)

* Register Service Worker
```
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js')
        .then((reg) => {
          console.log('Service worker registered.', reg);
        });
  });
}

```
* Add files to cache
```
'/offline.html',
```

* Install event
```
evt.waitUntil(
    caches.open(CACHE_NAME).then((cache) => {
      console.log('[ServiceWorker] Pre-caching offline page');
      return cache.addAll(FILES_TO_CACHE);
    })
);

```
* Activate event
```
evt.waitUntil(
    caches.keys().then((keyList) => {
      return Promise.all(keyList.map((key) => {
        if (key !== CACHE_NAME) {
          console.log('[ServiceWorker] Removing old cache', key);
          return caches.delete(key);
        }
      }));
    })
);
```
* Fetch Event
```
if (evt.request.mode !== 'navigate') {
  // Not a page navigation, bail.
  return;
}
evt.respondWith(
    fetch(evt.request)
        .catch(() => {
          return caches.open(CACHE_NAME)
              .then((cache) => {
                return cache.match('offline.html');
              });
        })
);
```