# nginx-javascript-modules-demo

1. `brew install nginx`
2. Update the public dir in `nginx.conf`
3. `sudo nginx -c $(pwd)/nginx.conf`
4. Open browser to `http://localhost`
5. When you're done: `sudo nginx -s quit`

In the JavaScript console you should see:

```
otherModule was loaded!
myModule:1 myModule was loaded!
deepModule:1 deepModule was loaded!
myModule:7 {deep: true}
```

nginx.conf:

```conf
events { }

http {
    types {
        application/javascript  mjs;
        text/html   html;
    }

    server {
        root public;

        location / {
            try_files $uri $uri/index.html index.html;
        }

        location /node_modules/ {
            try_files $uri $uri/index.mjs $uri/src/index.mjs index.mjs;
        }
    }
}
```

index.html:

```html
<h1>Nginx .mjs Module Loading Test</h1>

<script type="module" src="/node_modules/myModule"></script>
```