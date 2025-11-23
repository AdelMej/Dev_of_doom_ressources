# 🪓 JavaScript Redirection Cheat Sheet

## 🔥 1. Redirect to a URL (most common)

``` js
window.location.href = "https://example.com";
```

## 🧹 2. Redirect WITHOUT keeping history

User cannot go back.

``` js
window.location.replace("https://example.com");
```

## ⚡ 3. Redirect in the same tab

(Like clicking a link programmatically)

``` js
location.assign("https://example.com");
```

## 💥 4. Redirect after a delay

``` js
setTimeout(() => {
    window.location.href = "/home";
}, 2000); // 2 seconds
```

## 🎯 5. Redirect with URL parameters

``` js
const placeId = 42;
window.location.href = `/place.html?id=${placeId}`;
```

## 🧠 6. Reload the page

``` js
location.reload();
```

## 🔐 7. Redirect only if not authenticated

``` js
if (!document.cookie.includes("access_token")) {
    window.location.href = "/login.html";
}
```

## 🪟 8. Open link in a new tab

``` js
window.open("https://example.com", "_blank");
```

## 🧭 9. Go back in browser history

``` js
history.back();
```

## 🦍 Unga-Bunga Tip

If confused, this works 99% of the time:

``` js
window.location.href = "URL";
```
