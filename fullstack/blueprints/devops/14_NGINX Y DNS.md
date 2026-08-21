
Great, this output clarifies a lot.

## Current architecture

Right now you have:

```plain text
Browser
  -> dev.tamayo.dev
      -> VM / k3s NodePort 32322
          -> absencesbo-dev-nginx service
              -> absencesbo-dev-nginx pod
                  -> frontend/backend ClusterIP services
```


You are **not currently using Traefik Ingress** because:

```shell script
kubectl get ingress -A
No resources found
```


So yes: **your custom nginx is currently the routing authority for this app**.

Your nginx service is exposed as:

```plain text
absencesbo-dev-nginx   NodePort   80:32322/TCP
```


So some external DNS / VM-level rule is probably sending `dev.tamayo.dev` traffic to the VM and then to this NodePort.

---

## Current nginx behavior

Your active nginx config has this:

```
location / {
    proxy_pass http://absencesbo-dev-frontend:80;
}
```


That means:

```plain text
https://dev.tamayo.dev/             -> frontend service
https://dev.tamayo.dev/absencesbo   -> frontend service with path /absencesbo
https://dev.tamayo.dev/assets/...   -> frontend service with path /assets/...
https://dev.tamayo.dev/api/...      -> backend service
```


So your current behavior is expected:

```plain text
dev.tamayo.dev
```


works because `/` goes to the frontend.

But:

```plain text
dev.tamayo.dev/absencesbo
```


goes to the frontend as `/absencesbo`, and your React/Vite SPA probably is not configured to run under that base path, so you get a 404 or routing failure.

---

# Recommended decision

Since this is a **Vite/React SPA**, and your desired public route is:

```plain text
https://dev.tamayo.dev/absencesbo
```


I recommend doing **both** of these:

1. **nginx should mount the app under `/absencesbo/`**
2. **Vite/React should know its base path is `/absencesbo/`**

This gives the cleanest long-term setup.

---

## Why both nginx and Vite need changes

nginx controls server-side routing:

```plain text
/absencesbo/anything -> frontend service
```


But Vite controls generated asset paths.

If Vite is built with the default base `/`, it may generate assets like:

```html
<script src="/assets/index-abc123.js"></script>
```


That points to:

```plain text
https://dev.tamayo.dev/assets/index-abc123.js
```


But for your desired setup, you usually want:

```plain text
https://dev.tamayo.dev/absencesbo/assets/index-abc123.js
```


So Vite should be configured with:

```javascript
base: '/absencesbo/'
```


Then nginx can route `/absencesbo/assets/...` correctly to the frontend.

---

# Important issue in your CORS config

You currently have:

```
map $http_origin $cors_origin {
    default "";
    "http://localhost" "http://localhost";
    "http://localhost:8041" "http://localhost:8041";
    "https://dev.tamayo.dev/absencesbo" "https://dev.tamayo.dev/absencesbo";
    "https://stg.tamayo.dev/absencesbo" "https://stg.tamayo.dev/absencesbo";
    "https://apps.tamayo.dev/absencesbo" "https://apps.tamayo.dev/absencesbo";
}
```


This is incorrect because the browser `Origin` header **does not include the path**.

The origin will be:

```plain text
https://dev.tamayo.dev
```


Not:

```plain text
https://dev.tamayo.dev/absencesbo
```


So this should become:

```
map $http_origin $cors_origin {
    default "";
    "http://localhost" "http://localhost";
    "http://localhost:8041" "http://localhost:8041";
    "https://dev.tamayo.dev" "https://dev.tamayo.dev";
    "https://stg.tamayo.dev" "https://stg.tamayo.dev";
    "https://apps.tamayo.dev" "https://apps.tamayo.dev";
}
```


---

# Recommended nginx config shape

Given your current Kubernetes services:

```plain text
frontend: absencesbo-dev-frontend:80
backend:  absencesbo-dev-backend:8080
nginx:    absencesbo-dev-nginx NodePort 32322
```


I would make nginx behave like this:

```plain text
/                         -> landing placeholder for now
/absencesbo               -> redirect to /absencesbo/
/absencesbo/              -> absences frontend
/absencesbo/assets/...    -> absences frontend assets
/absencesbo/api/...       -> absences backend
/healthz                  -> nginx health check
```


Example:

```
worker_processes auto;

events {
    worker_connections 1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log main;
    error_log   /var/log/nginx/error.log warn;

    sendfile on;
    keepalive_timeout 65;

    map $http_origin $cors_origin {
        default "";
        "http://localhost" "http://localhost";
        "http://localhost:8041" "http://localhost:8041";
        "https://dev.tamayo.dev" "https://dev.tamayo.dev";
        "https://stg.tamayo.dev" "https://stg.tamayo.dev";
        "https://apps.tamayo.dev" "https://apps.tamayo.dev";
    }

    server {
        listen 80;
        server_name _;

        location = /healthz {
            access_log off;
            default_type text/plain;
            return 200 "ok\n";
        }

        # Temporary landing route.
        # Later this can proxy to another landing frontend project/service.
        location = / {
            default_type text/plain;
            return 200 "Landing page not deployed yet\n";
        }

        # Normalize path.
        location = /absencesbo {
            return 301 /absencesbo/;
        }

        # Backend API for the absences app.
        # Public:
        #   /absencesbo/api/...
        # Internal backend receives:
        #   /api/...
        location /absencesbo/api/ {
            proxy_pass http://absencesbo-dev-backend:8080/api/;

            proxy_http_version 1.1;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header X-Forwarded-Prefix /absencesbo;

            add_header Access-Control-Allow-Origin "$cors_origin" always;
            add_header Vary "Origin" always;
            add_header Access-Control-Allow-Methods "GET, POST, OPTIONS, PUT, DELETE" always;
            add_header Access-Control-Allow-Headers "DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization" always;

            if ($request_method = OPTIONS) {
                add_header Access-Control-Allow-Origin "$cors_origin" always;
                add_header Vary "Origin" always;
                add_header Access-Control-Allow-Methods "GET, POST, OPTIONS, PUT, DELETE" always;
                add_header Access-Control-Allow-Headers "DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization" always;
                add_header Content-Length 0;
                add_header Content-Type "text/plain; charset=utf-8";
                return 204;
            }
        }

        # Vite/React built assets.
        # Public:
        #   /absencesbo/assets/...
        # Internal frontend receives:
        #   /assets/...
        location /absencesbo/assets/ {
            proxy_pass http://absencesbo-dev-frontend:80/assets/;

            proxy_http_version 1.1;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header X-Forwarded-Prefix /absencesbo;

            add_header Cache-Control "public, max-age=31536000, immutable" always;
        }

        # Absences frontend SPA.
        # Public:
        #   /absencesbo/
        #   /absencesbo/some/client/route
        # Internal frontend receives:
        #   /
        #   /some/client/route
        location /absencesbo/ {
            proxy_pass http://absencesbo-dev-frontend:80/;

            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header X-Forwarded-Prefix /absencesbo;

            add_header Cache-Control "no-cache, no-store, must-revalidate" always;
        }

        # Anything else at root should not accidentally go to absences app.
        location / {
            return 404;
        }
    }
}
```


---

## Important note about `proxy_pass` trailing slashes

This line:

```
location /absencesbo/ {
    proxy_pass http://absencesbo-dev-frontend:80/;
}
```


means nginx strips `/absencesbo/`.

So:

```plain text
/absencesbo/
```


is sent internally as:

```plain text
/
```


And:

```plain text
/absencesbo/assets/index.js
```


is sent internally as:

```plain text
/assets/index.js
```


That is probably what you want because your frontend service likely serves files from its own root `/`.

---

# Vite config recommendation

In your Vite frontend project, configure the app base.

Usually in `vite.config.js` or `vite.config.ts`:

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  base: '/absencesbo/',
});
```


Or TypeScript:

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  base: '/absencesbo/',
});
```


This ensures built assets reference:

```plain text
/absencesbo/assets/...
```


instead of:

```plain text
/assets/...
```


---

# React Router recommendation

If you use `BrowserRouter`, also set the basename:

```javascript
import { BrowserRouter } from 'react-router-dom';

export function App() {
  return (
    <BrowserRouter basename="/absencesbo">
      {/* routes */}
    </BrowserRouter>
  );
}
```


Without this, React Router may interpret `/absencesbo` as an actual app route instead of the mounting prefix.

---

# API URL recommendation

You need to decide whether the frontend calls:

```plain text
/api/...
```


or:

```plain text
/absencesbo/api/...
```


For your target architecture, I recommend:

```plain text
/absencesbo/api/...
```


That keeps this app self-contained under its prefix.

So your frontend environment should use something like:

```plain text
VITE_API_BASE_URL=/absencesbo/api
```


Then browser calls go to:

```plain text
https://dev.tamayo.dev/absencesbo/api/...
```


And nginx forwards them to:

```plain text
http://absencesbo-dev-backend:8080/api/...
```


---

# One thing to verify

Your backend location currently uses:

```
location /api/ {
    proxy_pass http://absencesbo-dev-backend:8080;
}
```


In nginx, this can produce slightly different upstream paths depending on trailing slashes.

If your backend expects `/api/...`, use:

```
location /absencesbo/api/ {
    proxy_pass http://absencesbo-dev-backend:8080/api/;
}
```


If your backend expects `/...` without `/api`, use:

```
location /absencesbo/api/ {
    proxy_pass http://absencesbo-dev-backend:8080/;
}
```


Based on your existing config, I assume your backend expects `/api/...`, so this is likely correct:

```
proxy_pass http://absencesbo-dev-backend:8080/api/;
```


---

## Final recommendation

For your current setup, I would **not introduce Traefik Ingress yet** unless you want to standardize k3s ingress routing now.

The fastest and cleanest path is:

1. Keep `absencesbo-dev-nginx` as the public NodePort entrypoint.
2. Change nginx so `/` no longer proxies to the absences frontend.
3. Mount the frontend under `/absencesbo/`.
4. Move API under `/absencesbo/api/`.
5. Configure Vite with `base: '/absencesbo/'`.
6. Configure React Router with `basename="/absencesbo"`.
7. Fix CORS origins to remove the path portion.

That gives you this desired behavior:

```plain text
https://dev.tamayo.dev
```


returns landing placeholder now, future landing app later.

```plain text
https://dev.tamayo.dev/absencesbo
```


redirects to:

```plain text
https://dev.tamayo.dev/absencesbo/
```


and loads the absences frontend app.

```plain text
https://dev.tamayo.dev/absencesbo/api/...
```


routes to the absences backend.

