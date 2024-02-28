

==indicaciones de Gemini== 

In your case, where the frontend and backend are on different hosts and you're not using HTTPS, you can follow these steps:

1. **Set `SameSite` attribute to `'None'`**: This allows the cookie to be sent on all cross-origin requests. However, this requires the `Secure` attribute, which in turn requires your site to be served over HTTPS. Since you're not using HTTPS, you can't use this option.

2. **Use a reverse proxy**: If you have control over your server environment, you can set up a reverse proxy that routes requests to the appropriate backend service based on the request path. This makes it appear as if your frontend and backend are on the same domain, even though they're actually on different hosts. This way, you can use cookies with `SameSite='Lax'` or `SameSite='Strict'`.

3. **Use a different method for sharing state**: If neither of the above options work for you, you might need to use a different method for sharing state between your frontend and backend. For example, you could use JSON Web Tokens (JWTs) and store them in local storage on the frontend. However, this approach has its own security considerations.

Here's how you can set up a reverse proxy using Nginx:

1. Install Nginx on your server.

2. Create a new Nginx configuration file. In this file, you'll specify how requests should be routed:

```nginx
server {
    listen 80;

    location /api {
        proxy_pass http://192.168.20.100:8003;
    }

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

In this configuration, requests to `http://your-server.com/api` are routed to `http://192.168.20.100:8003`, and all other requests are routed to `http://localhost:3000`.

3. Save the configuration file and restart Nginx.

Now, you can access your frontend at `http://your-server.com` and your backend at `http://your-server.com/api`. Since they're both under `http://your-server.com`, they're considered the same site, and cookies can be shared between them.

Remember to replace `http://your-server.com`, `http://192.168.20.100:8003`, and `http://localhost:3000` with the actual URLs of your server, backend, and frontend, respectively.


