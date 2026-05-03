var CACHE_NAME = 'pomodoro-v1';
var urlsToCache = ['./', './index.html'];

self.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open(CACHE_NAME).then(function(cache) {
      return cache.addAll(urlsToCache);
    }).then(function() {
      return self.skipWaiting();
    })
  );
});

self.addEventListener('activate', function(event) {
  event.waitUntil(
    caches.keys().then(function(names) {
      return Promise.all(
        names.filter(function(n) { return n !== CACHE_NAME; }).map(function(n) { return caches.delete(n); })
      );
    }).then(function() { return self.clients.claim(); })
  );
});

self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request).then(function(response) {
      return response || fetch(event.request).then(function(resp) {
        return caches.open(CACHE_NAME).then(function(cache) {
          cache.put(event.request, resp.clone());
          return resp;
        });
      });
    }).catch(function() {
      return caches.match('./');
    })
  );
});
