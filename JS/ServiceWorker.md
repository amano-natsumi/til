## キャッシュ

```javascript
    if('serviceWorker' in navigator) {
      navigator.serviceWorker.register('sw.js');
    };
```

```javascript
var CACHE_NAME = "aaa";

self.addEventListener('install', function(event) {
  console.log('[Service Worker] Install');
  event.waitUntil(self.skipWaiting());
});

self.addEventListener('activate', (event) => {
  event.waitUntil(self.clients.claim());
});

self.addEventListener('fetch', function(event) {
  console.log('Handling fetch event for', event.request.url);

  event.respondWith(
    caches.open(CACHE_NAME).then(function(cache) {
      return cache.match(event.request).then(function(response) {
        if (response) {
          console.log('hit cache', event.request.url);
          return response;
        }

        return fetch(event.request.clone()).then(function(response) {

          if (response.status < 400 && event.request.url.match(/^https:\/\/api\.nhk\.or\.jp/i)) {
            cache.put(event.request, response.clone());
            console.log('put');
          } else {
            console.log('  Not caching the response to', event.request.url);
          }
          return response;
        });
      }).catch(function(error) {
        console.error('  Error in fetch handler:', error);

        throw error;
      });
    })
  );
});

```
